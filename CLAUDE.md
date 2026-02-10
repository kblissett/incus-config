# Project Guidelines

## Command Examples

- **Prefer scripts:** Put reusable commands in `bin/` scripts rather than inline in the README. Scripts work regardless of the user's shell. Run `shellcheck` on scripts before committing.

- **Copy-paste friendly:** When inline commands are necessary, place `<placeholder>` style placeholders at the end of command strings so users can easily select and delete them. Use variables for values that repeat across commands.

- **Shell compatibility:** When inline commands are necessary, include both bash and fish instructions wherever syntax differs (e.g., environment variables, command substitution).
