# Goal — minhas-repeticoes (15/11/2026)

Pedido do Nei: construir o kit "seus prompts viram atalhos" (lê o histórico do Claude Code/Codex, acha o que você
repete, sugere atalho ou skill, mede modelo/esforço/limite), guia PT/EN/ES, portal, tópico INEMA.AGENTES,
explicavideo 16:9 + 9:16 no lives7. Sem citar autor/fonte de referência.

## Pronto quando (nível 3+)
- `python3 -m unittest discover -s testes` → OK, ≥ 10 testes (função, filtro humano×robô, limite, Codex, agrupamento, privacidade).
- `python3 repeticoes.py --claude testes/dados/claude --codex testes/dados/codex --saida /tmp/x` → cria relatorio.html, relatorio.md, atalhos.md, numeros.json; numeros.json sem nenhum texto de prompt.
- Rodada real na máquina do Nei (30 dias) → números só de lá no FATOS.md do vídeo.
- grep pelos nomes do material de referência (lista só na sessão, fora do repo) → vazio antes de cada push.
- Guia https://inematds.github.io/minhas-repeticoes/guia/ (+en/ +es/) responde 200; portal com push; tópico no INEMA.AGENTES + MENU; vídeo lives7 via continua.
