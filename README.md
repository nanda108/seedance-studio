# Claude Skills

Personal Claude Code skills collection.

## Skills

### seedance-studio
Modular production agent for Seedance 2 video generation.

Routes between stages based on what you need:
- **Idea → storyboard** — break down concept into shots with cinematography
- **Storyboard → prompts** — write Seedance 2 prompts for each shot
- **QA review** — validate prompts against 9-point checklist
- **Final package** — assemble everything for generation

Knowledge base includes:
- Hollywood cinematographers techniques (Deakins, Lubezki, van Hoytema, Willis, Hall, Zsigmond)
- Lens selection matrix by genre and emotion
- Visual styles & color grading presets
- Seedance 2 prompting rules and templates
- Keyframe and reference management

**Usage:** `/seedance-studio` in Claude Code

## Install

```
# Package and install a skill
cd skill-creator
python -Xutf8 -m scripts.package_skill "C:\! skills\seedance-studio"
```
