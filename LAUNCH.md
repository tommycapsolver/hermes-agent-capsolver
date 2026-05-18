# Início e Verificação 🚀

Este é o passo crucial onde iniciamos o navegador com a extensão e verificamos a ligação.

## Passo 3: Iniciar o Chrome com a extensão e CDP ativado
Iniciamos o Chrome **uma vez**, separadamente do Hermes, com três flags cruciais:
- `--remote-debugging-port=9222`: expõe o protocolo DevTools para o Hermes se ligar.
- `--load-extension=...`: pré-carrega a extensão do CapSolver.
- `--user-data-dir=...`: usa um perfil dedicado para não colidir com o seu Chrome pessoal.

### Opção A: Início manual (bom para testes rápidos)
```bash
/caminho/para/chrome-for-testing/chrome \
  --remote-debugging-port=9222 \
  --remote-debugging-address=127.0.0.1 \
  --user-data-dir="$HOME/.hermes/chrome-debug" \
  --load-extension="$HOME/.hermes/capsolver-extension" \
  --disable-extensions-except="$HOME/.hermes/capsolver-extension" \
  --no-first-run \
  --no-default-browser-check \
  --no-sandbox
```
Substitua `/caminho/para/chrome-for-testing/chrome` pelo seu binário real.

### Opção B: Processo persistente em segundo plano (recomendado)
Crie um script em `~/.hermes/chrome-debug.sh`:
```bash
#!/usr/bin/env bash
CHROME_BIN="$HOME/.cache/ms-playwright/chromium-1200/chrome-linux64/chrome"
EXT_DIR="$HOME/.hermes/capsolver-extension"
USER_DATA_DIR="$HOME/.hermes/chrome-debug"

export DISPLAY=:99   # para Linux headless

exec "$CHROME_BIN" \
  --remote-debugging-port=9222 \
  --remote-debugging-address=127.0.0.1 \
  --user-data-dir="$USER_DATA_DIR" \
  --load-extension="$EXT_DIR" \
  --disable-extensions-except="$EXT_DIR" \
  --no-first-run \
  --no-default-browser-check \
  --no-sandbox \
  --disable-dev-shm-usage \
  --disable-features=Translate
```
Inicie-o com `nohup`:
```bash
nohup ~/.hermes/chrome-debug.sh > /tmp/chrome-debug.log 2>&1 &
```

### Servidores Headless
Se estiver num servidor Linux sem ecrã físico (VPS, EC2, etc.), consulte a secção de *Melhores Práticas* para a configuração do `Xvfb`. O subsistema de extensões do Chrome exige um contexto de ecrã.

---

## Verificação

O Hermes inclui um comando de diagnóstico integrado:
```bash
hermes doctor
```
Procure estes sinais:
```
◆ Tool Availability
  ✓ browser-cdp        ← A ligação CDP está ativa
  ✓ browser
```
Se o `browser-cdp` aparecer, o Hermes detetou o seu endpoint CDP e a integração está configurada corretamente.

Também pode confirmar que o Chrome está acessível diretamente:
```bash
curl -s http://127.0.0.1:9222/json/version
```

---

## Melhores Práticas para Produção

### Configuração do Xvfb para Servidores Headless
Em servidores sem GPU/monitor, use o `Xvfb` para fornecer um ecrã virtual:
```bash
sudo apt-get install xvfb
Xvfb :99 -screen 0 1024x768x24 &
export DISPLAY=:99
```

### Supervisão com systemd
Crie um ficheiro em `~/.config/systemd/user/chrome-debug.service`:
```ini
[Unit]
Description=Chrome equipado com CapSolver para Hermes Agent
After=network.target

[Service]
ExecStart=%h/.hermes/chrome-debug.sh
Restart=always
RestartSec=5

[Install]
WantedBy=default.target
```
Depois:
```bash
systemctl --user daemon-reload
systemctl --user enable --now chrome-debug
```
