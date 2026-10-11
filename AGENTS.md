# Instructions for AI agents

Use `README.md` for the project overview, `HOW_TO_USE.md` for commands, `GLOSSARY.md` for terms, and `problem.yaml` for structured facts. `AI_CONTEXT.md` states the validation limits and rules for changing the patch.

Keep changes narrow to USB device `31b2:0011`, capture direction, and the case where ACP did not specify a format. Do not generalize the observed XRUN to other devices or systems. The patch targets PipeWire commit `77b148060acb2dca1da0480c3063d1d536a6a681`; verify it against any different base before updating it.
