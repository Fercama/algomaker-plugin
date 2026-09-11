# AlgoMaker — plugin para Claude Code

**AlgoMaker is a Model Context Protocol (MCP) server for quantitative trading.** Your AI agent
downloads market data, mines and validates algorithmic trading strategies (out-of-sample gates,
walk-forward, Monte Carlo), builds a portfolio and paper-trades it — with the engine running on
your own machine, not on our servers. Orders with real money and broker API keys never pass
through the agent: they happen on a screen of yours.

| | |
|---|---|
| Remote MCP endpoint | `https://algomakers.com/mcp` (streamable HTTP) |
| Official MCP Registry | `com.algomakers/algomaker` |
| Claude Code | `/plugin marketplace add https://algomakers.com/claude/marketplace.json` |
| Engine (41 tools) | `uvx --python 3.11 --from https://algomakers.com/claude/mm_engine-0.0.153-cp311-none-any.whl mm-engine --mcp` |
| Manual | <https://algomakers.com/claude/manual> |

*The rest of this file, and the product itself, are in Portuguese.*

---


Instala em qualquer Claude Code (Windows, macOS, Linux):

    /plugin marketplace add https://algomakers.com/claude/marketplace.json
    /plugin install algomaker@algomakers

O que entra:

- **Conector remoto** `algomaker-onboarding` (3 ferramentas): o que é o AlgoMaker, como instalar,
  como a licença funciona. Não precisa de nada instalado.
- **Motor** `algomaker-motor` (41 ferramentas): dados, minerar, validar, compor carteira, simular
  em papel, ativar licença — roda na sua máquina via `uvx` (Mac, Linux, Windows). Precisa do
  `uv` instalado antes: `curl -LsSf https://astral.sh/uv/install.sh | sh` (Mac/Linux) ou
  `winget install astral-sh.uv` (Windows). Sem `uv`, só este servidor falha; o resto do plugin
  continua.
- **Skill `algomaker`**: a doutrina — as três frases que iniciam tudo, o que o agente nunca faz,
  e como ler o Monitor.

No Windows com o app AlgoMaker instalado, o próprio app registra o motor ao abrir; o do plugin
não conflita (a ponte usa o app quando ele está aberto).

Claude Desktop / claude.ai (sem plugin): *Configurações → Conectores → Adicionar conector
personalizado* com a URL `https://algomakers.com/mcp`; o motor, nesse caso, pela linha do `uvx`
que `algomaker_instalar` devolve.

## O que o plugin faz na sua máquina

O conector remoto só devolve texto: ele não alcança seu computador, não confere chave e não
executa nada. O motor roda **local**, no seu processador, e é ele que baixa dados, minera,
testa e simula. Ordem com dinheiro real e cadastro de chave de corretora **não** passam pelo
agente: acontecem numa tela sua, no app ou no painel local em `127.0.0.1`.

## Privacy Policy

Política de privacidade: <https://algomakers.com/privacidade> · Termos:
<https://algomakers.com/termos>

Em resumo, e medido no código:

- **Fica na sua máquina:** dados de mercado baixados, estratégias mineradas, bancos, carteiras,
  configurações e as chaves de corretora que você cadastrar. Nada disso é enviado para nós.
- **Sai da sua máquina apenas:** a ativação da licença (chave + identificador da máquina, para
  travar a licença em um computador) e, **se você aceitar no popup**, estatísticas agregadas de
  execução em percentual. Nunca valores em dinheiro, nunca dados de conta, nunca chaves.
- **O conector remoto** (`algomakers.com/mcp`) recebe apenas o que você digitar na conversa ao
  chamá-lo; ele não guarda perfil nem histórico.
- **Terceiros:** o motor fala com a corretora que você escolher, com as suas credenciais, direto
  da sua máquina.

Suporte: <https://algomakers.com/contato>

## Divulgação

O link de abertura de conta em corretora é de parceria remunerada, e a ferramenta que o devolve
diz isso na mesma resposta, sempre.
