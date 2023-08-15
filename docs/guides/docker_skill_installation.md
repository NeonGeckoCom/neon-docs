# Docker Skill Installation
This guide summarizes how to add custom skills to a Docker deployment.

## Steps
1. Locate your `neon.yaml` configuration file.
2. Create or add to the configuration list `skills`, `default_skills`
3. Add any desired skill(s) to the list by PyPI name or URL starting with `git+`
4. Restart the `neon-skills` container

## Troubleshooting
- Make sure the `neon.yaml` file being changed is the one used by Docker containers.
- Make sure any skill package names or URLs are valid.
- Check the `neon-skills` container logs in Docker or `skills.log` file for errors.

## Tags
- skill installation
- install skill
- docker
- skill development