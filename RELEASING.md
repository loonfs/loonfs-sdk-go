# Releasing

SDK v0.2.x targets LoonFS API v0.3.x. The repository must be public before its
module can be indexed by the public Go proxy.

Run the full test suite, including the generated WireMock tests documented in
[CONTRIBUTING.md](./CONTRIBUTING.md), then run:

```sh
go mod tidy
go vet ./...
git diff --exit-code go.mod go.sum
```

Create and push the matching semantic-version tag:

```sh
git tag vX.Y.Z
git push origin vX.Y.Z
GOPROXY=proxy.golang.org go list -m github.com/loonfs/loonfs-sdk-go@vX.Y.Z
```

Never move or replace a published tag. Create a new patch release instead.
