# team-up-with-codey

Codey, the implementation specialist for
[team-up](https://github.com/Bumblebiber/team-up).

One repository, one specialist. This one holds a manifest, instructions, two
skills and an eval suite — no code, no model names, no install hooks.

## What Codey does

Implements exactly one ticket against a spec, writes the tests that cover it,
and runs the project's test action.

Several Codeys run at once on one spec, each in its own full `git clone` of the
repository, created for the run and thrown away afterwards. That is why a Codey
may use git freely inside its clone, and why it never sees what the others are
doing: overlapping edits surface later as a merge conflict, which is loud,
rather than as a lost write, which is silent.

It does not change the spec, do adjacent work, review its own change
authoritatively, or deploy anything.

### One thing to know before running it

Codey holds an unbounded `Write`. A path specifier on the `Write` tool does not
scope anything in the Claude CLI (measured against 2.1.250 — path rules work
for `Read` and not for `Write`), so what keeps a Codey inside its clone is the
disposable clone itself, its instructions, and the OS sandbox where that
engages. Treat it as a trusted process, not a contained one.

## Install

```bash
git clone https://github.com/Bumblebiber/team-up-with-codey
team-up specialist inspect ./team-up-with-codey     # read-only, always first
team-up specialist install ./team-up-with-codey
team-up specialist approve coding.codey@0.1.0 --project /abs/path/to/project
```

Approval needs a `.team-up/commands.json` in that project declaring the
`project-test` action, or it fails with `COMMAND_POLICY_MISSING`. Every project
Codey works in needs its own:

```json
{
  "schema_version": 1,
  "commands": {
    "project-test": {
      "argv": ["npm", "test"],
      "cwd": ".",
      "timeout_seconds": 900,
      "environment": {}
    }
  }
}
```

The argv is fixed and takes no arguments from the specialist — that is the
point of the command broker, which also denies the native shell.

## Permissions

| | |
|---|---|
| filesystem | `project` |
| writes | `true` |
| network | `false` |
| commands | `project-test` |

## License

MIT
