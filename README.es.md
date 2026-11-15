# Minhas repetições

[![Minhas repetições](guia/assets/banner-es.jpg)](https://inematds.github.io/minhas-repeticoes/guia/es/)

**🇧🇷 [Português](README.md) · 🇺🇸 [English](README.en.md) · 🇪🇸 [Español](README.es.md)**

[![Minhas repetições](guia/assets/banner.jpg)](https://inematds.github.io/minhas-repeticoes/guia/es/)

<!-- VIDEO: entra el explicavideo de lives7 cuando se publique -->

## Qué es

Un kit gratuito que lee tu historial de **Claude Code** y **Codex** y muestra lo que escribes una y otra vez.
Para cada repetición indica si se convierte en un **atajo** (texto breve que se pega con un disparador), una **skill** (procedimiento con pasos), una **verificación** («¿cuál es la situación?», que requiere un aviso al final, no un atajo) o simplemente una **respuesta** («sí», «continúa»). Además, mide a qué modelo van tus solicitudes, con qué nivel de esfuerzo, cuántas veces llegaste al límite y cuántos turnos superaron los 10 minutos.

Se ejecuta en tu máquina, solo con Python 3 (biblioteca estándar). No llama a ninguna API ni envía nada a ningún lugar.

## 📖 Guía de uso

Guía completa (landing + paso a paso): **https://inematds.github.io/minhas-repeticoes/guia/es/**

## Por qué

No te das cuenta de lo que repites porque cada vez parece la primera. En una máquina de producción real, en 30 días: **2.287 solicitudes escritas, 12,6% repetidas**, y la más frecuente, «cuál es la situación», apareció **68 veces**. En ese mismo mes, los agentes hicieron 13.505 turnos, casi 6 por cada solicitud escrita. Esos números están en
`exemplos/numeros-maquina-inema-30dias.json`, que solo contiene conteos, sin ningún texto.

## El proceso, de principio a fin

| # | Paso | Comando | Quién |
|---|---|---|---|
| 1 | Descargar el kit | `git clone https://github.com/inematds/minhas-repeticoes && cd minhas-repeticoes` | 🧠 tú |
| 2 | Comprobar que funciona | `python3 -m unittest discover -s testes` (15 pruebas, datos inventados) | ⚙️ script |
| 3 | Leer tu historial | `python3 repeticoes.py` (predeterminado: 30 días, `~/.claude/projects` y `~/.codex/sessions`) | ⚙️ script |
| 4 | Ver el informe | abrir `relatorio/relatorio.html` en el navegador | 🧠 tú |
| 5 | Activar los atajos | copiar `relatorio/atalhos-espanso.yml` a la carpeta `match` de Espanso (Linux `~/.config/espanso/match/`, Mac `~/Library/Application Support/espanso/match/`) o copiar desde `atalhos.md` a tu expansor | 🧠 tú |
| 6 | Convertir en skill lo que tenga pasos | revisar `relatorio/skills-rascunho/<nome>/SKILL.md`, escribir los pasos y moverlo a `~/.claude/skills/`. Para que el agente lo haga contigo: `cp -r skill/minhas-repeticoes ~/.claude/skills/ && cp repeticoes.py ~/.claude/skills/minhas-repeticoes/` | 🧠 tú (o 🤖 el agente, con la skill del kit) |
| 7 | Ajustar el modelo y el esfuerzo | leer `modelo` y `modelo\|esforço` en el informe y seguir las sugerencias 6–9 de `DICAS.md` | 🧠 tú |
| 8 | Volver a medir en 30 días | `python3 repeticoes.py --saida relatorio-mes2` y comparar el `numeros.json` | ⚙️ script |

Opciones: `--dias 7` · `--saida <pasta>` · `--claude <pasta>` · `--codex <pasta>` · `--sem-codex`.

## Qué genera

| Archivo | Qué contiene | ¿Puedes mostrárselo a alguien? |
|---|---|---|
| `relatorio.html` | gráficos: repetición, grupos, modelo, esfuerzo, límite, turnos largos | ❌ contiene fragmentos tuyos |
| `relatorio.md` | lo mismo en texto | ❌ |
| `atalhos.md` / `atalhos-espanso.yml` | un atajo por cada solicitud breve repetida | ❌ |
| `skills-rascunho/` | un borrador de SKILL.md por cada solicitud larga repetida | ❌ |
| `numeros.json` | solo conteos (ningún texto) | ✅ |

## Cómo decide

- **Solo lo que escribiste.** Cuenta el turno con `turnOrigin: human` (Claude Code) o la sesión iniciada por el usuario
  (Codex). `claude -p`, monitores, notificaciones, subagentes y `codex exec` se cuentan aparte, como turnos automáticos.
- **Una sesión retomada no cuenta dos veces** (el mismo `uuid` se lee una sola vez).
- **Lo parecido cuenta como igual.** Agrupa solicitudes con las mismas palabras (reducidas a 5 letras: publica/publique/publicou) y acepta
  errores de escritura en las solicitudes breves ("sistuacao" = "situacao"). Repetición = 3 veces o más.
- **Atajo × skill:** hasta ~120 caracteres es un atajo; por encima de eso, si tiene pasos, es una skill. Hasta 10 caracteres ("sim", "1")
  es una respuesta. Las preguntas de seguimiento (situación, avance, en ejecución, estado) son verificaciones y no se convierten en atajos.
  Un enlace pegado solo es `link`; un `/comando` que ya usas es `ja-e-atalho`.
- **Límite:** el mismo aviso ("resets 9:10pm") en varias sesiones cuenta como 1 ocurrencia. En Codex cuenta la sesión que llegó al 100%.

## Contenido

- `repeticoes.py`: el kit (un archivo, sin dependencias)
- `DICAS.md`: 12 sugerencias sobre qué hacer con lo que repites
- `skill/minhas-repeticoes/SKILL.md`: skill para que Claude Code ejecute el kit y convierta los grupos en atajos/skills contigo
- `testes/`: 15 pruebas con historial inventado (`gerar_dados.py`)
- `exemplos/`: `relatorio-inventado/` (salida con datos inventados) y `numeros-maquina-inema-30dias.json` (números reales, sin texto)

## Límites

- El informe se genera en portugués.
- Lee el formato de registro de Claude Code y Codex de noviembre de 2026. Los registros antiguos sin `turnOrigin` se procesan según la regla anterior (CLI interactiva = tú).
- El esfuerzo solo aparece cuando Claude Code registra la solicitud de API (`?` = sin registro).
- La agrupación se basa en palabras, no en el significado: «publicar en el portal» y «publique en el portal» pueden quedar separados.

Gratis en [inema.club](https://inema.club), directamente en internet.
