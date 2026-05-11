# 100x-bot Skills Repository

This repository contains specialized AI skills for 100x-bot, designed to provide domain expertise and step-by-step guidance for automating various websites and applications.

## Structure

Skills are organized by domain:
`skills/<domain>/<skill-name>.json`

Example: `skills/github.com/repo-maintenance.json`

## Skill Format

Each skill is a JSON document containing:
- `name`: Unique identifier for the skill.
- `description`: What the skill helps the agent achieve.
- `instructions`: Detailed prompt templates and guidance.
- `domain`: The target website/application.
- `tags`: Keywords for discovery.

## Contributing

1. Create a new branch for your skill.
2. Follow the domain-based folder structure.
3. Submit a Pull Request for review.
