# minhas repetições

**🇧🇷 [Português](README.md) · 🇺🇸 [English](README.en.md) · 🇪🇸 [Español](README.es.md)**

[![Minhas repetições](guia/assets/banner.jpg)](https://inematds.github.io/minhas-repeticoes/guia/)

🎬 Vídeo: https://www.youtube.com/watch?v=3aanGE9Jpz8 · Short: https://youtube.com/shorts/cEHKKEnX4GU

## O que é

Um kit gratuito que lê o seu histórico do **Claude Code** e do **Codex** e mostra o que você digita de novo e de novo.
Para cada repetição ele diz se ela vira **atalho** (texto curto colado com um gatilho), **skill** (procedimento com
passos), se é uma **checagem** ("qual a situação?", que pede um aviso de fim, não um atalho) ou se é só uma
**resposta** ("sim", "continue"). De quebra mede para qual modelo vão os seus
pedidos, com que esforço, quantas vezes você bateu no limite e quantos turnos passaram de 10 minutos.

Roda na sua máquina, só com Python 3 (biblioteca padrão). Não chama API e não manda nada para lugar nenhum.

## 📖 Guia de uso

Guia completo (landing + passo a passo): **https://inematds.github.io/minhas-repeticoes/guia/**

## Por quê

Você não percebe o que repete, porque cada vez parece a primeira. Numa máquina de produção real, em 30 dias: **2.287
pedidos digitados, 12,6% repetidos**, e o campeão, "qual a situação", apareceu **68 vezes**. Nesse mesmo mês os robôs
fizeram 13.505 turnos, quase 6 para cada pedido digitado. Esses números estão em
`exemplos/numeros-maquina-inema-30dias.json`, que só tem contagens, sem nenhum texto.

## O processo, do início ao fim

| # | Passo | Comando | Quem |
|---|---|---|---|
| 1 | Baixar o kit | `git clone https://github.com/inematds/minhas-repeticoes && cd minhas-repeticoes` | 🧠 você |
| 2 | Conferir se roda | `python3 -m unittest discover -s testes` (15 testes, dados inventados) | ⚙️ script |
| 3 | Ler o seu histórico | `python3 repeticoes.py` (padrão: 30 dias, `~/.claude/projects` e `~/.codex/sessions`) | ⚙️ script |
| 4 | Ver o relatório | abrir `relatorio/relatorio.html` no navegador | 🧠 você |
| 5 | Ligar os atalhos | copiar `relatorio/atalhos-espanso.yml` para a pasta `match` do Espanso (Linux `~/.config/espanso/match/`, Mac `~/Library/Application Support/espanso/match/`) ou colar do `atalhos.md` no seu expansor | 🧠 você |
| 6 | Virar skill o que tem passos | revisar `relatorio/skills-rascunho/<nome>/SKILL.md`, escrever os passos e mover para `~/.claude/skills/`. Para o agente fazer com você: `mkdir -p ~/.claude/skills && cp -r skill/minhas-repeticoes ~/.claude/skills/ && cp repeticoes.py DICAS.md ~/.claude/skills/minhas-repeticoes/` | 🧠 você (ou 🤖 o agente, com a skill do kit) |
| 7 | Ajustar modelo e esforço | ler `modelo` e `modelo\|esforço` no relatório e seguir as dicas 6–9 de `DICAS.md` | 🧠 você |
| 8 | Medir de novo em 30 dias | `python3 repeticoes.py --saida relatorio-mes2` e comparar o `numeros.json` | ⚙️ script |

Opções: `--dias 7` · `--saida <pasta>` · `--claude <pasta>` · `--codex <pasta>` · `--sem-codex`.

## O que sai

| Arquivo | O que tem | Pode mostrar a alguém? |
|---|---|---|
| `relatorio.html` | gráficos: repetição, grupos, modelo, esforço, limite, turnos longos | ❌ tem trechos seus |
| `relatorio.md` | o mesmo em texto | ❌ |
| `atalhos.md` / `atalhos-espanso.yml` | um atalho por pedido curto repetido | ❌ |
| `skills-rascunho/` | um rascunho de SKILL.md por pedido longo repetido | ❌ |
| `numeros.json` | só contagens (nenhum texto) | ✅ |

## Como ele decide

- **Só o que você digitou.** Conta o turno com `turnOrigin: human` (Claude Code) ou a sessão iniciada pelo usuário
  (Codex). `claude -p`, vigias, notificações, subagentes e `codex exec` entram à parte, como turnos automáticos.
- **Sessão retomada não conta duas vezes** (o mesmo `uuid` é lido uma vez só).
- **Parecido é igual.** Junta pedidos com as mesmas palavras (cortadas em 5 letras: publica/publique/publicou) e aceita
  erro de digitação nos curtos ("sistuacao" = "situacao"). Repetição = 3 vezes ou mais.
- **Atalho × skill:** até ~120 caracteres é atalho; acima disso, com passos, é skill. Até 10 caracteres ("sim", "1")
  é resposta. Pergunta de acompanhamento (situação, andamento, rodando, status) é checagem e não vira atalho.
  Link colado sozinho é `link`; `/comando` que você já usa é `ja-e-atalho`.
- **Limite:** o mesmo aviso ("resets 9:10pm") em várias sessões conta 1 batida. No Codex conta a sessão que chegou a 100%.

## Conteúdo

- `repeticoes.py`: o kit (um arquivo, sem dependências)
- `DICAS.md`: 12 dicas do que fazer com o que você repete
- `skill/minhas-repeticoes/SKILL.md`: skill para o Claude Code rodar o kit e transformar os grupos em atalho/skill com você
- `testes/`: 15 testes com histórico inventado (`gerar_dados.py`)
- `exemplos/`: `relatorio-inventado/` (saída com dados inventados) e `numeros-maquina-inema-30dias.json` (números reais, sem texto)

## Limites

- O relatório sai em português.
- Lê o formato de log do Claude Code e do Codex de novembro de 2026. Logs antigos sem `turnOrigin` são tratados pela
  regra antiga (CLI interativa = você).
- Esforço só aparece quando o Claude Code grava o pedido de API (`?` = sem registro).
- O agrupamento é por palavras, não por sentido: "posta no portal" e "publique no portal" podem ficar separados.

Grátis no [inema.club](https://inema.club), direto na internet.
