```

██╗    ██╗██╗███╗   ██╗██╗      █████╗ ██████╗
██║    ██║██║████╗  ██║██║     ██╔══██╗██╔══██╗
██║ █╗ ██║██║██╔██╗ ██║██║     ███████║██████╔╝
██║███╗██║██║██║╚██╗██║██║     ██╔══██║██╔══██╗
╚███╔███╔╝██║██║ ╚████║███████╗██║  ██║██████╔╝
 ╚══╝╚══╝ ╚═╝╚═╝  ╚═══╝╚══════╝╚═╝  ╚═╝╚═════╝

███████╗██╗  ██╗██╗██╗     ██╗     ███████╗
██╔════╝██║ ██╔╝██║██║     ██║     ██╔════╝
███████╗█████╔╝ ██║██║     ██║     ███████╗
╚════██║██╔═██╗ ██║██║     ██║     ╚════██║
███████║██║  ██╗██║███████╗███████╗███████║
╚══════╝╚═╝  ╚═╝╚═╝╚══════╝╚══════╝╚══════╝

```

# WinLab Skills

The WinLab Agent Skills collection.

## Available Skills

### [winlab-slides-guidelines](winlab-slides-guidelines/SKILL.md)

Review and guide WinLab presentation slides creation following WinLab guidelines.

**Use when:**

- Creating, reviewing, or editing WinLab presentation slides
- Processing WinLab PowerPoint files
- Asking about WinLab slide formatting, design standards, or presentation best practices
- Review my WinLab slides
- Check my WinLab slides formatting
- Help me create WinLab slides

## Installation

```bash
npx skills add nycu-winlab/winlab-skills
```

or using [bun](https://bun.sh/):

```bash
bunx skills add nycu-winlab/winlab-skills
```

## Usage

After installation, the skills will be automatically available. When AI detects related tasks, the corresponding skills will be automatically used.

**Examples:**

```
Review my presentation slides for WinLab
```

```
Help me create slides following WinLab guidelines
```

```
Check if my slides comply with WinLab standards
```

## Contributing

To add a new WinLab-specific skill:

1. Create a new skill directory in the project root (e.g., `skill-name/`)
2. Create a `SKILL.md` file in the directory
3. Follow the [Agent Skills format](https://agentskills.io/)
4. Update the skill list in this README

## Resources

- [Agent Skills format specification](https://agentskills.io/)
- [skills CLI](https://www.npmjs.com/package/skills)
- [Cursor Skills documentation](https://cursor.com/zh-Hant/docs/context/skills)
