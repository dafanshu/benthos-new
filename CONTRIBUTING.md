Contributing to Benthos
=======================

Joining Club Blob by contributing to the Benthos project is a selfless, boring and occasionally painful act. As such any contributors to this project will be treated with the respect and compassion that they deserve.

Please be dull, please be respecting of others and their efforts, please do not take criticism or rejection of your ideas personally.

## Reporting Bugs

If you find a bug then please let the project know by opening an issue after doing the following:

- Do a quick search of the existing issues to make sure the bug isn't already reported
- Try and make a minimal list of steps that can reliably reproduce the bug you are experiencing
- Collect as much information as you can to help identify what the issue is (project version, configuration files, etc)

## Suggesting Enhancements

Having even the most casual interest in Benthos gives you honorary membership of Club Blob, entitling you to give a reserved (and hypothetical) tickle of the projects' toes in order to steer it in the direction of your whim.

Please don't abuse this entitlement, the poor blobfish can only gobble so many features before it starts to droop beyond repair. Enhancements should roughly follow the general goals of Benthos and be:

- Common use cases
- Simple to understand
- Simple to monitor

You can help us out by doing the following before raising a new issue:

- Check that the feature hasn't been requested already by searching existing issues
- Try and reduce your enhancement into a single, concise and deliverable request, rather than a general idea
- Explain your own use cases as the basis of the request

## Adding Features

Pull requests are always welcome. However, before going through the trouble of implementing a change it's worth creating an issue. This allows us to discuss the changes and make sure they are a good fit for the project.

### Building locally

Run commands from the repository root. `go.mod` declares Go 1.21; the test and lint workflow in `.github/workflows/test.yml` uses Go 1.22.x. Use Go 1.22.x to match CI, and install GNU Make for the targets below.

There is no `make build` target. Build the CLI from `cmd/benthos` and check its available commands with:

```shell
go build -o ./benthos ./cmd/benthos
./benthos --help
```

### Testing and formatting

Before opening a pull request, run the checks relevant to your change and report the commands you actually ran, their results, and any checks you could not run.

```shell
make test
make test-race
make lint
make fmt
```

- `make test` runs `go test -timeout 3m ./...`, then CLI template linting, then configuration tests with `go run ./cmd/benthos test ./config/test/...`. It stops on the first failing command. Template paths come from `TEMPLATE_FILES`, which is unset by default; supply paths when testing templates.
- `make test-race` runs the Go tests with the race detector. It requires CGO and a C compiler; unlike the CI test job, do not disable CGO for this target.
- `make lint` runs `go vet ./...` and `golangci-lint` over `internal/...` and `public/...`. Install `golangci-lint` v1.57.1 on your `PATH` to match the version pinned in CI. The CI lint job also includes `cmd/...`.
- `make fmt` rewrites Go files with `gofmt` and `goimports`, then runs `go mod tidy`. It requires `goimports` on your `PATH` (the module currently uses `golang.org/x/tools` v0.16.1). Review the resulting diff and keep unrelated formatting and dependency changes out of your PR.

For example, lint a template that uses the components included in this CLI:

```shell
go run ./cmd/benthos template lint ./config/template_examples/processor_log_message.yaml
```

`make deps` runs `go mod tidy`; CI runs it and checks that tracked files remain unchanged before running `make test`. It is a dependency consistency check, not a dependency upgrade command.

This checkout has no `make test-integration` or `make docs` target and no `website/` directory. For connector changes, add or run relevant tests in the affected packages and describe any external services required in your PR. For documentation changes, edit the relevant files directly and review their rendered Markdown.

### Adding New Components

The APIs for adding new components (inputs, outputs, processors, caches, etc) has recently been simplified. If you are planning to create a new component you should use the latest implementations within `./internal/impl` as inspiration.

### Plugins

The core components within Benthos (inputs, processors, conditions and outputs) are all easily pluggable. If you are interested in adding new components please raise a ticket and we can discuss whether it's a good fit for the project.

If not then it's still easy to build your own version of Benthos with custom components. For guidance take a look at [this example repo](https://github.com/benthosdev/benthos-plugin-example).
