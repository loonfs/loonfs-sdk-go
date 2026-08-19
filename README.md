# LoonFS Go SDK

Go client for the LoonFS HTTP API.

## Status

This SDK is pre-release. This repository is private until the first release.

## Install

```sh
go get github.com/loonfs/loonfs-sdk-go
```

This command works once the repository is public.

## Usage

```go
package example

import (
	"context"

	loonfs "github.com/loonfs/loonfs-sdk-go"
	"github.com/loonfs/loonfs-sdk-go/client"
	"github.com/loonfs/loonfs-sdk-go/option"
)

func listRoot(ctx context.Context) error {
	loonfsClient := client.NewClient(
		option.WithBaseURL("https://api.example.com"),
		option.WithToken("your-token"),
	)

	_, err := loonfsClient.Filesystem.ListPathEntries(ctx, &loonfs.ListPathEntriesRequest{
		NamespaceID: "your-namespace-id",
		Path:        "/",
	})
	return err
}
```

See [reference.md](./reference.md) for the full API reference.

## Tests

Some generated tests send requests to a WireMock server. Start it first:

```sh
docker compose -f wiremock/docker-compose.test.yml up -d --wait
WIREMOCK_URL="http://localhost:$(docker compose -f wiremock/docker-compose.test.yml port wiremock 8080 | cut -d: -f2)" go test ./...
```

## Generated code

The code is generated with Fern from the LoonFS OpenAPI spec (`docs/specs/openapi.json` in `github.com/loonfs/loonfs`). Regeneration runs from the `fern/` config in that repository (`scripts/generate-sdks.sh`). Do not edit generated files by hand.

## License

Apache-2.0.
