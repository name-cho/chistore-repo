# chistore-repo

Package repository for [Chistore-go](https://github.com/name-cho/Chistore-go).
Chistore-go 2.2 and newer connect to it by default.

- GitGud: <https://gitgud.io/name-cho/chistore-repo>
- GitHub: <https://github.com/name-cho/chistore-repo>

[Русская версия](README.ru.md)

## Packages

| Package | Description |
| --- | --- |
| `deskgen` | create and edit `.desktop` files |
| `envsnap` | save system and environment state, compare snapshots |
| `netdiag` | check IP, ping, DNS, ports and speed |
| `pystat` | system monitor with a TUI |

## GitHub mirror

The default config points to GitGud. To add the GitHub mirror:

```bash
chistore repo add github https://raw.githubusercontent.com/name-cho/chistore-repo/main --priority 20
chistore repo refresh
```

Priority 20 means the GitGud copy still wins when both repositories have the same
package.

## Raw URLs

The raw base address is not a file, so a browser gets 404:

```text
https://gitgud.io/name-cho/chistore-repo/-/raw/main             -> 404
https://gitgud.io/name-cho/chistore-repo/-/raw/main/index.json -> 200
```

Chistore asks for `index.json` first, then downloads packages like
`<base>/envsnap/envsnap`. The 404 on the base address does not affect it.

To browse the repository, use the web pages above.

## Layout

```text
chistore-repo/
├── index.json
├── <package>/
│   ├── info.md
│   └── <payload>
```

`index.json` is written by `chistore pkg index`. `info.md` is the source file for
the package directory; the client does not download it during install.

## Adding a package

```bash
chistore pkg init hello
# edit hello/info.md, replace hello/hello
chistore pkg validate
chistore pkg index
```

Then push to GitGud and to GitHub.

Keep `.gitattributes` with `* text=auto eol=lf`. `pkg index` hashes the bytes Git
serves, so a CRLF checkout on Windows would produce hashes that fail on install.

## Notes

`Notes` is optional. Add one `## Notes:` line per note in `info.md`:

```markdown
## Notes: Run hello --init once
## Notes: Config lives in ~/.config/hello
```

`pkg index` copies them into `index.json`. Chistore 2.4 and newer show them in
`chistore info` and after a successful install or upgrade. The manager only prints
the text; it does not run it.
