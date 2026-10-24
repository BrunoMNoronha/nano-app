# nano-app

> **Aplicativos web mobile-first e offline-first em arquivo único — HTML, CSS e JavaScript modernos, sem build, sem dependências.**

---

## Sobre

`nano-app` é uma coleção de aplicativos web autocontidos para tarefas pessoais do dia a dia. Cada app vive num único arquivo `.html`, roda offline depois do primeiro carregamento, é pensado primeiro para o celular e usa apenas HTML, CSS e JavaScript modernos — sem build tools, sem frameworks pesados, sem dependências para instalar. A ideia é que você possa abrir qualquer arquivo direto no navegador, ou hospedá-los gratuitamente em GitHub Pages, sem etapa intermediária.

## Filosofia

A restrição é o produto. Cada app caber em um único `.html` parece arbitrário, mas é uma escolha deliberada que organiza várias virtudes ao mesmo tempo.

**Legibilidade do todo.** Você abre o arquivo, lê de cima a baixo, e entende a aplicação inteira. Não há cinco pastas, sete arquivos de configuração e uma cadeia de imports a percorrer. Isso transforma cada nano-app numa peça pedagógica: serve tanto para usar quanto para estudar.

**Distribuição trivial.** Um único arquivo se envia por e-mail, se hospeda em qualquer lugar, se abre direto do disco. Nenhum servidor de build precisa estar de pé para que o app funcione daqui a três anos.

**Resistência ao tempo.** Projetos com dezenas de dependências de NPM começam a apodrecer no instante em que param de ser mantidos — versões quebram, vulnerabilidades aparecem, builds antigas deixam de compilar. Vanilla puro envelhece muito melhor: HTML e JavaScript modernos continuam rodando dez anos depois sem ajustes.

**Mobile-first como padrão.** A maioria das pessoas acessa a internet do celular a maior parte do tempo. Tratar o desktop como o caso geral e o mobile como adaptação é desonesto com o uso real. Aqui a inversão é total: o layout, os alvos de toque, o teclado virtual e as áreas seguras vêm primeiro; o desktop é o ajuste.

**Offline-first como respeito.** Conexão ruim, plano de dados limitado, modo avião — são realidades comuns. Um app que precisa de internet para mostrar dados que você mesmo gerou é um app mal projetado. Aqui, persistência é local por padrão.

## Apps disponíveis

No momento o repositório contém os seguintes apps:

`nano-meds.html` — Gestão de remédios com marcação de doses diárias. Você cadastra cada remédio (nome, dosagem, frequência), e o app gera cápsulas tocáveis para registrar cada dose tomada ao longo do dia. As cápsulas resetam automaticamente na virada do dia, e o histórico recente fica armazenado localmente para futuras visualizações de adesão.

`nano-water.html` — Registro de hidratação diária com meta configurável. Botões rápidos (200/300/500 ml) ou valor livre, anel de progresso do dia, desfazer o último registro e barras dos últimos 7 dias; o histórico de 30 dias fica salvo localmente.

Outros apps estão planejados — veja a seção [Roadmap](#roadmap).

## Como usar

Há três caminhos, do mais simples ao mais profissional, e todos funcionam.

**1. Aberto direto do disco.** Clone o repositório e dê duplo clique em qualquer arquivo `.html`. Ele abre no navegador padrão e roda. Funciona via protocolo `file://` sem servidor algum.

```bash
git clone https://github.com/SEU-USUARIO/nano-app.git
cd nano-app
# Agora abra qualquer .html no navegador
```

**2. Servidor local para desenvolvimento.** Algumas APIs do navegador (Service Workers, por exemplo, em apps futuros) exigem `http://` ou `https://`. Para isso, qualquer servidor estático resolve:

```bash
# Com Python (já vem instalado em macOS e Linux)
python3 -m http.server 8000

# Ou com Node, se preferir
npx serve

# Acesse http://localhost:8000
```

**3. Publicação gratuita via GitHub Pages.** Em **Settings → Pages**, ative o Pages apontando para a branch principal e a raiz (`/`). Em poucos minutos cada arquivo `.html` vira uma URL pública. Por exemplo: `https://SEU-USUARIO.github.io/nano-app/nano-meds.html`. Adicione essa URL à tela inicial do celular e o app fica indistinguível de um aplicativo nativo instalado.

## Estrutura do repositório

A estrutura é deliberadamente plana. Cada nano-app é um arquivo na raiz, com nome no padrão `nano-[domínio].html`. Documentação adicional vai em `docs/`, mas não é obrigatória para nano-apps simples.

```
nano-app/
├── nano-meds.html        # Gestão de remédios e doses
├── docs/                 # Convenções e notas de design (opcional)
├── LICENSE
└── README.md
```

Manter tudo na raiz tem um propósito: o usuário final que clona o repo encontra os apps imediatamente, sem precisar caçar dentro de subpastas. A estrutura plana também impede que o projeto cresça acidentalmente para uma organização em monorepo, o que iria contra a filosofia de simplicidade.

## Stack técnica

A regra mestra é: tudo que um navegador moderno entrega nativamente está no jogo; tudo que precisa ser instalado, não está.

Em prática, isso significa **HTML5 semântico**, **CSS moderno** com custom properties, unidades dinâmicas (`dvh`, `svh`), `color-mix()`, container queries quando fizerem sentido, e seleção via `prefers-color-scheme` para tema escuro. JavaScript usa **ES2022 ou superior** rodando direto, sem transpilação — `optional chaining`, `nullish coalescing`, `logical assignment`, `crypto.randomUUID()`, `Object.fromEntries`, `Array.from` com mapper, e por aí vai. Persistência fica em `localStorage` para dados pequenos e simples, ou `IndexedDB` quando o app demandar consultas mais sofisticadas.

E aqui está a parte igualmente importante — o que **não** é usado, e por quê. **Frameworks** como React, Vue ou Svelte ficam de fora porque trazem build, dependências e curva de aprendizado que matam a ideia de "abrir o arquivo e entender". **Bundlers** como Vite, Webpack ou esbuild ficam de fora pelo mesmo motivo: o arquivo precisa ser executável como veio, sem etapa de compilação. **Gerenciadores de pacote** como npm, yarn e pnpm ficam de fora porque a instalação de dependências é exatamente o que o projeto se propõe a evitar. **TypeScript** fica de fora porque exige transpilação; em compensação, JSDoc bem escrito oferece grande parte dos benefícios de tipos sem custo de build. **Pré-processadores CSS** como Sass ou Less ficam de fora porque CSS moderno já incorporou variáveis, aninhamento parcial, e funções de cor — não há mais necessidade.

Isso não é dogma. É uma restrição produtiva que define um tipo específico de artefato e o protege de virar mais um boilerplate genérico.

## Compatibilidade

Os apps usam APIs nativas relativamente recentes. Como referência prática, navegadores lançados de 2023 em diante rodam tudo sem ajustes. Especificamente, são esperadas as seguintes capacidades: o elemento `<dialog>` (Chrome 37+, Firefox 98+, Safari 15.4+), `crypto.randomUUID()` (Chrome 92+, Firefox 95+, Safari 15.4+), unidades viewport dinâmicas como `dvh` (Chrome 108+, Firefox 101+, Safari 15.4+), e a função `color-mix()` em CSS (Chrome 111+, Firefox 113+, Safari 16.2+).

Se você precisar suportar navegadores significativamente mais antigos, polyfills resolvem boa parte do gap — mas adicionar polyfills vai contra o espírito do projeto, então a recomendação é simplesmente assumir um piso moderno. Os usuários-alvo (você mesmo e quem quiser usar o app) provavelmente já estão em navegadores recentes.

## Convenções para um nano-app

Para que um arquivo seja, de fato, um nano-app válido neste repositório, ele precisa seguir alguns princípios. Cada app **resolve um problema focado** — não há ambição de virar canivete suíço; quando o escopo crescer, vira outro nano-app. Cada app **cabe confortavelmente em um único arquivo**: como regra de bolso, abaixo de mil linhas; quando exceder isso de forma inevitável, é sinal de que o escopo está pedindo separação. Cada app **persiste dados localmente** via `localStorage` ou `IndexedDB`, nunca dependendo de servidor remoto. Cada app **funciona offline** depois do primeiro carregamento — qualquer recurso externo (fontes, ícones) deve ser cacheado pelo navegador ou inlinado no próprio HTML. E cada app é **utilizável com uma mão no celular**: alvos de toque mínimos de 44 pixels, áreas seguras (`safe-area-inset`) respeitadas, viewport configurado corretamente.

Há também convenções de organização interna do arquivo: o `<style>` no `<head>` agrupado por seções comentadas, o `<script>` no fim do `<body>` em modo `'use strict'`, e funções pequenas e nomeadas em vez de blocos longos. O objetivo é que outra pessoa (ou você mesmo daqui a seis meses) consiga ler o arquivo de cima a baixo e entender o app inteiro sem precisar de ferramentas externas.

## Como contribuir

Contribuições são bem-vindas, especialmente novos nano-apps que respeitem o espírito do projeto. O fluxo é direto. Faça um fork do repositório, crie uma branch descritiva (por exemplo, `add-nano-water`), adicione o arquivo `nano-[nome].html` na raiz seguindo as convenções acima, atualize a seção "Apps disponíveis" deste README com uma descrição de uma a duas linhas do novo app, e abra um Pull Request explicando o problema que o app resolve.

Antes de propor um app novo, vale fazer mentalmente algumas perguntas. **O escopo é realmente focado?** Se a resposta envolve "e também", talvez sejam dois apps. **A funcionalidade caberia melhor em um app existente?** Às vezes uma feature pequena melhora um nano-app já presente sem precisar criar outro. **O app resolve um problema concreto do dia a dia?** Demonstrações abstratas, embora educativas, talvez fiquem melhor em outro repositório.

Para ajustes em apps existentes (correções, melhorias de UX, otimizações), o fluxo é o mesmo — fork, branch, PR — e a régua é simplesmente que a mudança preserve a filosofia: se a correção exige adicionar uma dependência ou um build step, ela provavelmente está atacando o problema errado.

## Roadmap

A lista abaixo são ideias planejadas, sem ordem fixa nem compromisso de prazo. Servem mais para indicar a direção e convidar contribuições do que para prometer entregas.

`nano-meds` — pronto. `nano-water` — pronto. `nano-mood` — diário de humor em uma nota rápida por dia, com visualização semanal. `nano-habits` — checklist de hábitos diários com streak. `nano-budget` — registro de gastos pessoais ultraleve. `nano-timer` — pomodoro minimalista sem distração visual. Sugestões de novos apps são bem-vindas via Issues.

## Licença

Distribuído sob a licença **MIT**. Veja o arquivo `LICENSE` para o texto completo. Resumindo o espírito: você pode usar, copiar, modificar e redistribuir os arquivos, inclusive em projetos comerciais, contanto que mantenha o aviso de copyright e a licença nos derivados. Não há garantias.

---

*Mantido por [seu nome aqui].*
