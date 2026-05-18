# Configuração ⚙️

Siga estes passos para configurar as chaves e a ligação entre o Hermes e o navegador.

## Passo 2: Configurar a sua chave API do CapSolver
Abra o ficheiro de configuração da extensão em `~/.hermes/capsolver-extension/assets/config.js` e substitua o valor de `apiKey` pelo seu:

```javascript
export const defaultConfig = {
  apiKey: 'CAP-XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX',  // ← a sua chave aqui
  useCapsolver: true,
  enabledForRecaptcha: true,
  enabledForRecaptchaV3: true,
  // ... resto da configuração
};
```
Pode obter a sua chave API no seu [painel CapSolver](https://www.capsolver.com/?utm_source=official&utm_medium=blog&utm_campaign=hermes).

## Passo 4: Dizer ao Hermes para se ligar via CDP
Edite a sua configuração do Hermes em `~/.hermes/config.yaml`. Procure a secção `browser:` (que normalmente só tem `inactivity_timeout`) e adicione um `cdp_url`:

```yaml
browser:
  inactivity_timeout: 120
  cdp_url: http://127.0.0.1:9222
```

Essa linha única diz à ferramenta `browser_cdp` do Hermes para encaminhar cada operação do navegador através da instância do Chrome que iniciámos, em vez de iniciar a sua própria.

**Reversibilidade**: Esta é a *única* alteração no Hermes. Para reverter, elimine a linha `cdp_url`. O Hermes voltará a qualquer fornecedor de navegador padrão que estivesse a usar (Browserbase, Browser Use, etc.) sem outros efeitos secundários.

## Passo 5: Reiniciar o Hermes
Se o Hermes já estiver a correr, reinicie-o para que ele assuma o novo `cdp_url`:
```bash
# Se correr diretamente:
hermes gateway run
```
Ou reinicie através do gestor de processos que utiliza.
