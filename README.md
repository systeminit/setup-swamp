# setup-swamp

A GitHub Action to install [swamp](https://github.com/systeminit/swamp) and
optionally authenticate with [swamp.club](https://swamp.club).

## Usage

```yaml
- uses: systeminit/setup-swamp@v1
  with:
    api-key: ${{ secrets.SWAMP_API_KEY }}
```

### Inputs

| Input            | Required | Default  | Description                                        |
|------------------|----------|----------|----------------------------------------------------|
| `version`        | No       | `latest` | Swamp version to install (release tag)             |
| `api-key`        | No       |          | API key for authenticating with swamp.club         |
| `swamp-club-url` | No       |          | Override the swamp.club server URL                 |
| `repo-init`      | No       | `false`  | Run `swamp repo init` after setup                  |

### Outputs

| Output    | Description                          |
|-----------|--------------------------------------|
| `version` | The version of swamp that was installed |

### Examples

**Install latest and authenticate:**

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: systeminit/setup-swamp@v1
    with:
      api-key: ${{ secrets.SWAMP_API_KEY }}
  - run: swamp model list
```

**Install a specific version:**

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: systeminit/setup-swamp@v1
    with:
      version: v0.5.0
```

**Install, authenticate, and initialize the repo:**

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: systeminit/setup-swamp@v1
    with:
      api-key: ${{ secrets.SWAMP_API_KEY }}
      repo-init: true
  - run: swamp workflow run my-workflow
```

## How it works

1. Downloads the swamp binary for the runner's OS and architecture from GitHub
   releases
2. Verifies the downloaded binary against the release checksums
3. Adds swamp to `PATH`
4. If `swamp-club-url` is provided, sets `SWAMP_CLUB_URL` as an environment
   variable for all subsequent steps
5. If `api-key` is provided, masks the value in logs, sets `SWAMP_API_KEY` as
   an environment variable for all subsequent steps, and verifies authentication
   with `swamp auth whoami`
6. If `repo-init` is `true`, runs `swamp repo init`

## Supported platforms

- Linux (x86_64, aarch64)
- macOS (x86_64, aarch64)

## License

AGPLv3 - see [LICENSE](LICENSE) for details.
