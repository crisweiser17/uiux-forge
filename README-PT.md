<div align="center">

<img src="assets/logo.png" alt="uiux-forge" width="160" />

# uiux-forge

**Repensa uma tela até ela contar a história numa olhada — _depois_ deixa bonita.**

[![Stars](https://img.shields.io/github/stars/crisweiser17/uiux-forge?style=flat&color=DAA520)](https://github.com/crisweiser17/uiux-forge/stargazers)
[![Forks](https://img.shields.io/github/forks/crisweiser17/uiux-forge?style=flat)](https://github.com/crisweiser17/uiux-forge/network/members)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-2D6BFF.svg)](./LICENSE)
[![Claude Code plugin](https://img.shields.io/badge/Claude%20Code-plugin-FF6B4A.svg)](https://docs.claude.com/en/docs/claude-code)

[English](./README.md) | **Português**

</div>

---

## ⚡ A ideia

Quase toda ferramenta de "melhorar UI" vai direto pra tinta: cor melhor, canto redondo, um
gradiente. Mas tela bonita que ainda confunde não está pronta — e polir uma tela mal-estruturada é
pintar parede torta.

O uiux-forge faz na ordem certa:

> **A lógica escolhe, você confirma, o unslop embeleza.**
>
> 1. Simula 4-6 layouts *de verdade* com os seus dados *reais* e escolhe o mais claro por critério.
> 2. Mostra os mockups ranqueados — vencedor em cima, **cada descartado com o motivo de ter perdido**.
> 3. Só depois do seu "pode seguir" roda o passe estético que tira a cara genérica de AI.

Estrutura primeiro. Pele depois. Claro merece bonito.

## 🧠 Por que lógica antes de beleza

O erro que ele mata: pegar o primeiro layout que vem à cabeça, polir e entregar. A primeira ideia
quase nunca é a mais clara — só dá pra saber comparando lado a lado com os mesmos dados. Então o
uiux-forge *compara antes de escolher*, decide por uma régua de "menos dúvida" (responde a pergunta
numa olhada? o escopo está claro? os números alinham? aguenta o pior caso de volume? segura em
360px?), e te mostra o porquê em vez de entregar um resultado que você não pode questionar.

Aí — e só aí — pega emprestada a ideia do unslop: uma direção visual tirada do seu produto e passes
ordenados que tiram o gradiente roxo, o glow, o emoji-ícone e o espaçamento desencontrado.

## 🔄 Fluxo

| # | Fase | O que acontece |
|---|------|---------------|
| 1 | **Queixa → perguntas** | Vira "não me ajuda a entender" nas 2-4 perguntas que a pessoa faz à tela. Um exemplo concreto vira teste de aceitação. |
| 2 | **Estudar** | Ler a tela inteira, o domínio, as regras de negócio, as telas vizinhas; puxar dados reais. |
| 3 | **Simular 4-6 variantes** | Layouts estruturalmente diferentes, mesmos dados reais, print em 1280px e 360px. |
| 4 | **Julgar por critério** | Escolher o vencedor pela régua de "menos dúvida" — pelos prints, não pelo código. |
| 5 | ★ **Vitrine** | Página: vencedor em cima, cada descartado com *por que perdeu*. Você confirma antes de escrever. |
| 6 | ★ **Unslop no vencedor** | Direção visual + passes ordenados (espaçamento → cor → ícone → copy → tipo → layout → detalhe → visual → movimento) + checklist anti-slop. |
| 7 | **Implementar com teste primeiro** | Teste que reproduz o exemplo e a regra de agrupamento. Vermelho → verde. |
| 8 | **Conferir de verdade** | 1280px e 360px, claro e escuro, sem rolagem lateral. |
| 9 | **Entregar + antes/depois** | Antes e depois lado a lado; vídeo quadrado opcional pra postar. |

> **Atalho:** se a tela já está clara e só parece genérica, pula a simulação de variantes e faz
> 2-3 ajustes pontuais em vez de redesenhar.

## 🚀 Começar

Instale como plugin do [Claude Code](https://docs.claude.com/en/docs/claude-code):

```bash
/plugin marketplace add crisweiser17/uiux-forge
/plugin install uiux-forge@crisweiser17
```

Depois é só falar com ele:

```
essa tela tá bagunçada, me ajuda a repensar
melhora a UX dessa tela
simula uns layouts pra esse dashboard e me mostra por que escolheu um
```

## 📄 Licença e créditos

[CC BY 4.0](./LICENSE). A ideia do passe estético é adaptada do
[**unslop**](https://github.com/heydamir/unslop) de Damir Sokolovsky. O uiux-forge acrescenta o
método estrutura-primeiro: simulação de variantes com dados reais, a vitrine "me mostra por que
cada um perdeu", testes de aceitação e conferência mobile/escuro.

<div align="center"><sub>Feito por <a href="https://github.com/crisweiser17">Cris Weiser</a>.</sub></div>
