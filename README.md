# Como Resolver CAPTCHA na Automação de Navegadores com Hermes Agent e CapSolver 🚀

[![GitHub stars](https://img.shields.io/github/stars/NousResearch/hermes-agent?style=social)](https://github.com/NousResearch/hermes-agent)
[![CapSolver](https://img.shields.io/badge/CapSolver-Integrado-blue)](https://www.capsolver.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

![Fluxo de trabalho de automação de navegador Hermes Agent integrado com CapSolver para resolução automática de CAPTCHA](https://assets.capsolver.com/prod/posts/hermes-agent-capsolver/mTOWzB7OuNai-51dc95ca0b12964d1370ff6c038086bc.jpeg)

Quando o seu agente de IA navega na web por si, os **CAPTCHAs** são o obstáculo número um. Páginas protegidas bloqueiam o agente, formulários recusam-se a ser submetidos e as tarefas param à espera de intervenção humana.

O **Hermes Agent** da Nous Research é um agente de IA que melhora de forma autónoma e corre em qualquer lugar — desde um VPS de $5 até um cluster de GPUs — e chega até si em todos os canais que já utiliza: Telegram, Discord, Slack, WhatsApp, Signal e e-mail. Também pode conduzir um navegador para navegar em páginas, clicar em botões, preencher formulários e extrair dados em seu nome. Mas, como qualquer agente que conduz um navegador, fica bloqueado nos CAPTCHAs.

O **[CapSolver](https://www.capsolver.com/?utm_source=official&utm_medium=blog&utm_campaign=hermes)** muda isto completamente. Ao carregar a extensão do Chrome do CapSolver no navegador ao qual o Hermes se liga, os CAPTCHAs são resolvidos **automática e invisivelmente** em segundo plano. Sem código. Sem chamadas de API da sua parte. Sem ginástica de engenharia de prompts.

A melhor parte? **Nem precisa de mencionar os CAPTCHAs ao agente.** Basta dizer-lhe para esperar um momento antes de submeter — e quando ele clicar em Submeter, o CAPTCHA já estará resolvido.

---

## 📋 Tabela de Conteúdos
- [O que é o Hermes Agent?](#-o-que-é-o-hermes-agent)
- [O que é o CapSolver?](#-o-que-é-o-capsolver)
- [Por que esta integração é diferente?](#-por-que-esta-integração-é-diferente)
- [Pré-requisitos](#-pré-requisitos)
- [Configuração Passo a Passo](#-configuração-passo-a-passo)
- [Verificação](#-verificação)

---

## 🤖 O que é o Hermes Agent?

[**Hermes Agent**](https://github.com/NousResearch/hermes-agent) é um agente de IA autónomo de código aberto construído pela [Nous Research](https://nousresearch.com/). Foi concebido em torno de três princípios: **memória persistente** (lembra-se de si e dos seus projetos entre sessões), **criação autónoma de competências** (aprende procedimentos através da experiência e repete-os na próxima vez) e **flexibilidade de infraestrutura** (corra-o num pequeno VPS, num contentor Docker, numa sandbox serverless ou na sua própria máquina GPU).

### Características Principais
- **Gateway multicanal**: Fale com o seu agente via Telegram, Discord, Slack, WhatsApp, Signal, e-mail ou na sua própria interface de terminal.
- **Traga o seu próprio modelo**: OpenRouter (mais de 200 modelos), Nous Portal, NVIDIA NIM, Z.AI, o seu próprio endpoint — mude com `hermes model`.
- **Memória entre sessões**: Pesquisa de sessão FTS5 + resumo de LLM significa que o agente se lembra do que falaram na semana passada.
- **Sistema de competências**: Memória procedimental que o agente constrói sozinho, compatível com o padrão agentskills.io.
- **Sete backends de terminal**: Local, Docker, SSH, Singularity, Modal, Daytona, Vercel Sandbox.
- **Ferramenta de navegador integrada**: Conduz um Chromium real via Playwright + Chrome DevTools Protocol.

### A Ferramenta de Navegador
O Hermes pode conduzir um navegador Chromium para realizar trabalho real — navegar, ler o DOM, clicar, escrever, tirar capturas de ecrã, fazer scraping. A sua camada de ferramenta de navegador é invulgar num aspeto específico: em vez de o forçar a usar um único backend, o Hermes suporta **cinco fornecedores de navegador intercambiáveis**:

1. Browserbase (Nuvem)
2. Browser Use (Nuvem)
3. Firecrawl (Nuvem)
4. Camoufox (Local - Firefox stealth)
5. **CDP attach** (Local - qualquer Chromium)

Os fornecedores na nuvem não podem carregar extensões — você não controla o navegador remoto. O Camoufox é baseado em Firefox e não executará uma extensão Chrome MV3. O ponto de integração limpo é o quinto: **CDP attach**, onde o Hermes se liga a um Chromium que *você* iniciou separadamente. É aqui que o CapSolver entra.

---

## 🧩 O que é o CapSolver?

O [CapSolver](https://www.capsolver.com/?utm_source=official&utm_medium=blog&utm_campaign=hermes) é um serviço líder na resolução de CAPTCHAs que fornece soluções baseadas em IA para contornar os desafios modernos de CAPTCHA. Com suporte para todos os principais tipos de CAPTCHA e tempos de resposta rápidos, o CapSolver integra-se perfeitamente em fluxos de trabalho automatizados — quer esteja a conduzir um navegador via Playwright, a chamar a sua API diretamente ou, como neste guia, **a executar a sua extensão do Chrome dentro da sessão de navegador de um agente.**

---

## 💡 Por que esta integração é diferente?

A maioria das integrações de resolução de CAPTCHA exige que escreva código — criar chamadas de API, verificar resultados, injetar tokens em campos de formulário ocultos. É assim que funciona com ferramentas como Crawlee, Puppeteer ou Playwright.

**Hermes + CapSolver é fundamentalmente diferente:**

- **Tradicional (Baseado em Código)**: Exige escrever uma classe `CapSolverService`, chamar `createTask()` / `getTaskResult()`, injetar tokens via `page.$eval()`, e gerir erros, tentativas e tempos de espera no código.
- **Hermes (Linguagem Natural)**: Basta iniciar o Chrome uma vez com `--load-extension=...` e falar com o seu agente. A extensão trata de tudo automaticamente.

**A ideia-chave**: A extensão do Chrome do CapSolver corre dentro do navegador *ligado*. O Hermes liga-se a esse navegador via CDP e conduz o mesmo normalmente. Quando o agente navega para uma página com um CAPTCHA, a extensão — a correr no mesmo Chrome, completamente invisível para o agente — deteta o widget, chama a API do CapSolver e injeta o token de solução na página. Quando o agente clica em Submeter, o formulário já contém um token válido.

**Só precisa de lhe dar tempo.** Em vez de dizer ao agente para "resolver o CAPTCHA", basta dizer:
> "Vai a essa página, **espera 60 segundos**, depois clica em Submeter."

É tudo. O agente não precisa de saber que o CapSolver existe.

---

## ⚙️ Pré-requisitos
Consulte o [Guia de Instalação](INSTALL.md) para mais detalhes.

---

## 🚀 Configuração Passo a Passo
- [Passo 1: Descarregar a Extensão](INSTALL.md#passo-4-descarregar-a-extensão-do-capsolver)
- [Passo 2: Configurar a Chave API](CONFIG.md#passo-2-configurar-a-sua-chave-api-do-capsolver)
- [Passo 3: Iniciar o Chrome com CDP](LAUNCH.md)
- [Passo 4: Ligar o Hermes](CONFIG.md#passo-4-dizer-ao-hermes-para-se-ligar-via-cdp)

---

## ✅ Verificação
Consulte a secção de [Verificação](LAUNCH.md#verificação) para garantir que tudo está a funcionar corretamente.

---

## 📄 Licença
Este projeto está sob a Licença MIT. Consulte o arquivo [LICENSE](LICENSE) para mais detalhes.
