I want you to Create 3 global Claude Code protective hooks in ~/.claude/hooks/:

1. [block-destructive-git.sh](http://block-destructive-git.sh/) — PreToolUse hook on "Bash" matcher. Block commands matching: git push --force/-f, git reset --hard, git clean -f, git checkout -- ., git branch -D, git stash drop.
2. [block-dangerous-bash.sh](http://block-dangerous-bash.sh/) — PreToolUse hook on "Bash" matcher. Block commands matching (case-insensitive): rm -rf, rm -f /, DROP TABLE, DROP DATABASE, TRUNCATE, kill -9, killall, chmod 777, mkfs, dd if=/dev/zero.
3. [block-secrets.sh](http://block-secrets.sh/) — PreToolUse hook on "Edit|Write" matcher. Block writes to .env files. Block content containing: BEGIN PRIVATE KEY, aws_secret_access_key, AKIA AWS key IDs.

Each script reads JSON from stdin via `cat`, extracts the command/content with jq, loops over patterns with grep -qiE, and on match outputs JSON with permissionDecision "deny" and a reason string. Exit 0 always.

Then create/update ~/.claude/settings.json with the hooks config pointing to these scripts with 5s timeouts. Make all scripts executable.
