# Guia de Instalação 🛠️

Siga estes passos detalhados para preparar o seu ambiente. Nenhuma informação do artigo original foi omitida.

## 1. Instalar o Hermes Agent
Certifique-se de que tem o Hermes Agent instalado e o gateway em funcionamento. Pode encontrar as instruções oficiais no seu [repositório GitHub](https://github.com/NousResearch/hermes-agent#install).

## 2. Conta CapSolver
Precisa de uma conta CapSolver com uma chave API ativa. Pode [registar-se aqui](https://www.capsolver.com/?utm_source=official&utm_medium=blog&utm_campaign=hermes).

## 3. Navegador: Chromium, não Google Chrome
**IMPORTANTE**: O Google Chrome 137+ (lançado em meados de 2025) removeu silenciosamente o suporte para `--load-extension` em versões oficiais. Isto significa que as extensões do Chrome **não podem ser carregadas** em sessões automatizadas usando o Google Chrome padrão. Não há erro — a flag é simplesmente ignorada.

Isto afeta o Google Chrome e o Microsoft Edge. **Deve** usar uma destas alternativas:
- **Chrome for Testing** (Suportado e Recomendado)
- **Chromium (standalone)** (Suportado e Recomendado)
- **Chromium do Playwright** (Suportado e Recomendado)

### Como instalar o Chrome for Testing:
**Opção 1: Via Playwright (recomendado)**
```bash
npx playwright install chromium
```
O binário estará num caminho como:
- `~/.cache/ms-playwright/chromium-XXXX/chrome-linux64/chrome` (Linux)
- `~/Library/Caches/ms-playwright/chromium-XXXX/chrome-mac/Chromium.app/Contents/MacOS/Chromium` (macOS)

**Opção 2: Download direto**
Visite: [https://googlechromelabs.github.io/chrome-for-testing/](https://googlechromelabs.github.io/chrome-for-testing/) e descarregue a versão correspondente ao seu sistema operativo.

## 4. Descarregar a Extensão do CapSolver
Descarregue a extensão do Chrome do CapSolver e extraia-a para um local estável:
1. Vá aos [lançamentos da extensão no GitHub](https://github.com/capsolver/capsolver-browser-extension/releases).
2. Descarregue o ficheiro `CapSolver.Browser.Extension-chrome-vX.X.X.zip` mais recente.
3. Extraia o zip:
```bash
mkdir -p ~/.hermes/capsolver-extension
unzip CapSolver.Browser.Extension-chrome-v*.zip -d ~/.hermes/capsolver-extension/
```
4. Verifique a extração:
```bash
ls ~/.hermes/capsolver-extension/manifest.json
```
Deverá ver o `manifest.json` — isto confirma que a extensão está no local correto.

**Dica sobre caminhos**: Use um caminho absoluto e resolvido (não `~`) quando passar `--load-extension=...` para o Chrome mais tarde.
