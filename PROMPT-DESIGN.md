# Prompt para o Claude Design — Redesign do app NARU PRIME (Reservas)

> Cole o bloco abaixo no Claude Design. Ajuste a seção **Direção estética** se quiser outro clima visual.

---

## Contexto

Preciso do **redesign visual** de um web app de reservas para um restaurante japonês sofisticado chamado **NARU PRIME** (Alto da Lapa, São Paulo). O app já existe e funciona — quero elevar o acabamento visual, mantendo toda a estrutura e o fluxo. Entregue **um único arquivo `index.html` autossuficiente** (HTML + CSS + JS inline, sem bibliotecas nem assets externos), **mobile-first**, tema **escuro e elegante**, pronto pra deploy estático (Netlify).

O app tem **dois painéis** que convivem no mesmo arquivo, alternados por abas no topo:
1. **Reservar** — fluxo do cliente em formato de **quiz** (uma pergunta por tela).
2. **Administração** — painel de gestão do restaurante (protegido por senha).

## Marca e identidade

- **Nome:** NARU PRIME · selo/logo com o kanji **成** num quadrado.
- **Posicionamento:** alta gastronomia japonesa, pratos tradicionais elaborados, ingredientes gourmet, coquetéis. Ticket R$ 100–180/pessoa.
- **Endereço:** R. Dr. José Elias, 399 — Alto da Lapa, São Paulo · Tel (11) 3641-6679.
- **Tom:** sofisticado, calmo, premium, minimalista. Nada "app genérico" — deve parecer um restaurante fino.

## Objetivo do redesign

Manter a arquitetura e melhorar: hierarquia tipográfica, uso de espaço em branco (conceito japonês de *ma*), microinterações, refinamento de cores e sombras, sensação tátil premium. Deve ficar impecável no **celular** e bonito no desktop.

## Requisitos técnicos

- 1 arquivo `.html`, tudo inline, **sem CDN/fontes externas** (use font stack do sistema ou @font-face embutida em base64 se necessário; pode usar fontes web-safe elegantes).
- **Mobile-first**, responsivo até desktop (max-width ~1120px, quiz centralizado ~640px).
- Tema escuro com `color-scheme: dark` (inputs, selects e date-picker nativos precisam ter texto claro e legível — nada de fundo preto com letra preta).
- Persistência em `localStorage` (não precisa backend).
- Acessível: contraste AA, alvos de toque ≥ 44px, foco visível.

---

## ESTRUTURA E TELAS

### Cabeçalho (global, nas duas abas)
- Logo (kanji 成 num selo) + nome **NARU PRIME** + subtítulo "ALTO DA LAPA · SÃO PAULO".
- Abas: **Reservar** | **Administração**.
- No celular: logo em cima centralizado, abas ocupando a largura toda embaixo.

### PAINEL 1 — QUIZ DO CLIENTE (8 telas, com barra de progresso)

Barra de progresso no topo mostrando "Etapa X de 6" (as telas de boas-vindas e sucesso ficam fora da contagem). Cada tela tem: um *kicker* em japonês/maiúsculas, título grande, subtítulo, o conteúdo, e navegação **← Voltar / Continuar →**.

1. **Boas-vindas** — selo 成 grande, headline "Vamos reservar sua mesa", subtítulo curto, botão "Começar reserva →". Sem barra de progresso.
2. **Quando (data)** — seletor de data (mínimo hoje); mostra o dia da semana. Valida se o restaurante abre no dia.
3. **Quantas pessoas** — "chips" tocáveis de 1 a 8 + um **menu dropdown** "Grupo maior?" (9 a 20 e "Mais de 20 / evento"). Mostra o total selecionado.
4. **Onde (ambiente)** — cartões de seleção: **Balcão 1** (🍶, 10 lugares), **Balcão 2** (🍣, 28 lugares), **Mesas** (🍽️, 40 lugares). Cada cartão com ícone, nome, descrição e status de disponibilidade. Nota explicativa: balcões só reservam nos primeiros horários (resto por ordem de chegada); mesas em qualquer horário.
5. **Que horas** — grade de horários em blocos por serviço (**Almoço** e **Jantar**), de 30 em 30 min. Cada slot mostra os **lugares livres** (verde = tranquilo, âmbar = enchendo, "lotado" = bloqueado). Nos balcões, os horários fora da janela de reserva aparecem em estilo distinto (tracejado, rótulo "chegada"), **não clicáveis** = ordem de chegada.
6. **Quem (contato)** — nome, telefone/WhatsApp e observação opcional.
7. **Confira (revisão)** — resumo de tudo (restaurante, data, horário, ambiente, pessoas, nome, telefone, obs). **Aviso de tolerância de 10 minutos** em destaque. Botão "Enviar reserva ✓".
8. **Sucesso** — selo ✓ animado, "Reserva recebida!", **código da reserva** em destaque (ex.: A3F9K), pílula pulsante **"Aguarde — iremos confirmar sua reserva"**, aviso de tolerância de 10 min, e instrução de que a confirmação virá pelo telefone. Botão "Nova reserva".

### PAINEL 2 — ADMINISTRAÇÃO

- **Login** — cartão central com cadeado, campo de senha, botão "Entrar".
- **Dashboard** — seletor de data + botões "+ Reserva manual" e "Exportar CSV". **4 KPIs**: Reservas ativas, Pessoas (covers), Pendentes, Lugares totais.
- **Reservas do dia** — lista das reservas ordenadas por horário. No desktop: tabela (Hora, Cliente, Pax, Ambiente, Obs., Status, Ações). No celular: **cards** com rótulos e botões grandes (✓ Confirmar / ✕ Cancelar / 🗑). Status com badge colorido (confirmada = verde, pendente = âmbar, cancelada = vermelho).
- **Ocupação por horário** — tabela por serviço mostrando lugares usados/capacidade por ambiente, com **barra de ocupação** (fica vermelha acima de 80%).
- **Modal "Reserva manual"** — nome, telefone, data, horário (select), pessoas, ambiente (select). Selects precisam de contraste perfeito.

### Componentes reutilizáveis
Chips de seleção, cartões de opção, slots de horário (estados: livre/enchendo/lotado/chegada/selecionado), botões (primário vermelho, fantasma/ghost, pequeno), badges de status, caixas de **aviso** (notice âmbar), **toasts** de feedback (sucesso/erro), barra de progresso, KPIs, tabela→card responsivo, modal.

### Regras de negócio que impactam a UI
- **Mesas:** reserváveis em qualquer horário de funcionamento.
- **Balcões (1 e 2):** reserva só nos **2 primeiros horários** de cada serviço (ex.: 12:00/12:30 no almoço, 19:00/19:30 no jantar); demais horários = **ordem de chegada** (mostrados como "chegada", não clicáveis).
- **Tolerância de 10 minutos:** avisar cliente que, após 10 min de atraso, a reserva pode ser cancelada.
- Horários de funcionamento variam por dia: Seg–Qui 12:00–15:00 e 19:00–22:30; Sex 12:00–15:00 e 19:00–23:00; Sáb 12:30–16:00 e 19:00–23:00; Dom 12:30–16:00 e 19:00–22:00.

---

## DIREÇÃO ESTÉTICA (personalize aqui se quiser outro clima)

- **Mood:** *fine dining* japonês — tinta sumi-e, laca urushi vermelha, ouro *kintsugi*, papel washi, muito espaço negativo e silêncio visual.
- **Paleta (base escura):**
  - Fundo: preto-carvão profundo (#0d0b0c) com leve brilho radial quente no topo.
  - Vermelho laca (ação/marca): ~#e11d2a → #b3121f.
  - Ouro/latão (acentos, kickers): ~#c8a45c.
  - Texto: branco-washi levemente quente (#f3ece7) + cinza fumê para secundário.
  - Estados: verde jade (livre/confirmado), âmbar (atenção/pendente), vermelho (lotado/cancelado).
- **Tipografia:** títulos em uma **serifada display de alto contraste** (ar editorial/refinado) e corpo em sans limpa e legível; kickers em maiúsculas com *letter-spacing* amplo. Boa escala tipográfica.
- **Textura & profundidade:** grão/ruído sutil, divisórias em fio de cabelo (hairline), sombras suaves e difusas, cantos arredondados médios (12–16px), possíveis linhas douradas finas remetendo a *kintsugi*.
- **Motion:** transições suaves entre telas do quiz (fade/slide leve), microinterações em hover/seleção, selo de sucesso com "pop", pílula de espera pulsando. Nada exagerado.
- **Layout:** generoso, respiração entre blocos, alinhamento consistente, foco em uma decisão por tela no quiz.

## Entregável
Um `index.html` completo e funcional (com dados de exemplo no `localStorage`), cobrindo **todas as telas e estados** acima, refinado visualmente e impecável no celular.
