# Agent Control Metalayer Skill

skills.sh-compatible skill for initializing repositories with a control-system metalayer for autonomous code agents.

## Install

```bash
npx skills add broomva/control-metalayer --skill control-metalayer-loop
```

## Main Wizard

```bash
python3 .agents/skills/control-metalayer-loop/scripts/control_wizard.py init . --profile governed
python3 .agents/skills/control-metalayer-loop/scripts/control_wizard.py init . --profile autonomous
python3 .agents/skills/control-metalayer-loop/scripts/control_wizard.py audit . --strict
python3 .agents/skills/control-metalayer-loop/scripts/control_wizard.py status .
```
