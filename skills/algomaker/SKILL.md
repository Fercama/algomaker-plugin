---
name: algomaker
description: Como operar a fábrica de estratégias AlgoMaker pelo chat — o que é grátis, as três frases que iniciam tudo, o que o agente nunca faz, e como distinguir "app não instalado" de "app instalado". Use quando o usuário mencionar AlgoMaker, minerar estratégias, carteira quantitativa, Bybit, licença do AlgoMaker, ou quando as ferramentas algomaker_* / mine_* / portfolio_* / exec_* estiverem disponíveis.
---

# AlgoMaker — doutrina para o agente do cliente

## Em que passo ele está (decida ANTES de responder)

- Só existem ferramentas `algomaker_*` → o motor **não está ligado** nesta máquina. Chame
  `algomaker_comecar`; se ele perguntar de instalação, `algomaker_instalar` (passe `sistema`:
  em macOS/Linux a resposta é UMA linha de terminal com `uvx`, sem instalador).
- Existem `mine_preflight`, `portfolio_estrutura`, `licenca_status`, `exec_status` → o app está
  instalado e o conector completo registrado. Este skill vale para os dois casos.

## O que é grátis e o que pede licença

Grátis: baixar dados, minerar, backtest, robustez (walk-forward, Monte Carlo, estresse de
custo), compor carteira, operar em PAPEL, e desenhar/simular um GRID para o bot gratuito da
corretora. Diga isso cedo — a objeção mais comum é achar que precisa comprar para experimentar.

Licença: executar com dinheiro real, exportar código (.mq5/Python) e .alg. Chave: o cliente
cola no chat e `licenca_ativar` valida na máquina dele. **Nunca adivinhe, gere ou complete uma
chave.** Sem chave, o caminho é https://algomakers.com/comprar.

## As quatro frases que iniciam tudo

1. **Perfil** — "meu capital é X, aguento Y% de queda, quero crescimento|consistência,
   horizonte Z meses". Sem perfil a fábrica não minera (é proposital): `perfil_set`.
2. **Carteira** — "monta uma carteira pra mim". Pergunte ONDE vai executar (`destino`:
   `bybit_api` | `mt5`) antes de `mine_preflight` → `mine_strategies` → `portfolio_run` →
   `deploy_paper`. Minerar no destino errado não dá erro: dá backtest aprovado com o custo da
   corretora errada.
3. **Acompanhamento** — "como está minha carteira?" → `exec_status`. Leia TRÊS coisas antes
   de concluir "conservador": `kpis.retorno_sobre_risco_pct` (P&L sobre o capital em risco),
   `ocupacao` (desenho vs modelo vs corretora — `gap_execucao_pct` alto = ordem não entra) e
   `bloqueios_corretora` (recusa que exige ação humana, com a ação escrita).
4. **Grid** — "quero rodar um grid em SOL" → `grid_desenhar` (mede a faixa no histórico e
   dimensiona a escada) e `grid_simular` ("e se eu apertar a faixa?"). A Bybit não expõe os
   bots dela por API: o cliente digita faixa/grades/capital no bot GRATUITO da corretora. Leia
   `fora_da_faixa` ANTES do lucro — grid ganha oscilando dentro da faixa e perde quando sai.
   Entradas por ordem limite/stop (`order_entry`) também rodam ao vivo desde o 0.0.150.

## O que o agente NUNCA faz — por arquitetura, não por promessa

- Armar modo LIVE, cadastrar chave de corretora, mexer nas travas de risco: só o dono, na
  tela do app (ou na página local de confirmação). Duas camadas bloqueiam.
- Destravar um kill-switch. Encerrar tudo em emergência PODE (`exec_kill`).
- Enviar ordem por conta própria, mesmo "de teste".

## Honestidade > promessa

- A fábrica RECUSA campanha que produziria lixo, com o motivo em números. É o produto
  funcionando.
- Aprovada de verdade é rara (> 99% descartadas). Poucas e sólidas.
- Simule antes de armar: o backtest é conservador por construção (custos medidos do book
  real). A simulação existe para provar isso ao cliente.
- Respostas grandes: peça `campos: [...]`; `_omitido` significa que o dado existe e foi
  cortado para caber, não que falta.

## A tela: `painel_abrir`

O motor serve a interface completa do app (Monitor, Fábrica, Databank, Conexões, VPS) no
navegador, em `127.0.0.1`, em qualquer sistema. Quando o cliente disser "abre o painel" — ou
quando algo for recusado com "na tela" — chame `painel_abrir` e **entregue o link** (uso
único, 5 min). Você nunca abre o link, nunca pede chave de corretora no chat: chave, armar
dinheiro real e fechar posição são feitos por ele, na tela. O painel continua rodando depois
da conversa; se o app Windows estiver aberto, a resposta diz que a tela é o app.
