# Reference
<details><summary><code>client.Capabilities() -> *loonfs.CapabilityDocument</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a summary of supported features and limits.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
client.Capabilities(
    context.TODO(),
)
```
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## system
<details><summary><code>client.System.Health() -> string</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns `ok` when the server is running and can accept requests.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
client.System.Health(
    context.TODO(),
)
```
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.System.GetMetrics() -> string</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns this process's metrics in Prometheus text exposition format 0.0.4. Unlike `/health` and `/readiness`, the route requires the deployment's bearer token.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
client.System.GetMetrics(
    context.TODO(),
)
```
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.System.Readiness() -> string</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns `ready` while the server admits new work. Once shutdown begins and publisher admission closes, answers 503 `shutting_down` so load balancers can drain the instance. `/health` stays the liveness probe: it only reports that the process is up.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
client.System.Readiness(
    context.TODO(),
)
```
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## admin
<details><summary><code>client.Admin.ListCheckpoints(NamespaceID) -> *loonfs.ListCheckpointsResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Lists one page of active checkpoints in checkpoint-id order. Expired checkpoints remain visible until collection releases them. Released checkpoints are omitted. The cursor resumes a live listing and does not create a snapshot.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.ListCheckpointsRequest{
    NamespaceID: "namespace_id",
}
client.Admin.ListCheckpoints(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**limit:** `*int` — Maximum page size
    
</dd>
</dl>

<dl>
<dd>

**cursor:** `*string` — Opaque checkpoint-list page cursor
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Admin.CreateCheckpoint(NamespaceID, request) -> *loonfs.CreateCheckpointResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a named, user-owned checkpoint record pinning the current namespace view. Every call mints a new record under a new id; the name is a label, not a key. The record is a garbage-collection root until it is released, so routine maintenance should flush the WAL instead. This is a maintenance/admin operation, not a file mutation.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.CreateCheckpointRequest{
    NamespaceID: "namespace_id",
    Name: "name",
}
client.Admin.CreateCheckpoint(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**name:** `string` 

Label recorded on the checkpoint record. A label, not a key: several
records may carry the same name over different bases.
    
</dd>
</dl>

<dl>
<dd>

**ttlMs:** `*int64` 

Optional lifetime; the server computes the record's expiry from its
own clock. Absent means the pin holds until explicitly released.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Admin.ReleaseCheckpoint(NamespaceID, CheckpointID) -> *loonfs.ReleaseCheckpointResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Releases a user-owned checkpoint pin by id. Idempotent: releasing an already-released or reaped record succeeds. The record is reaped by a later garbage-collection pass; its pinned data becomes collectable only on the pass after that.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.ReleaseCheckpointRequest{
    NamespaceID: "namespace_id",
    CheckpointID: "checkpoint_id",
}
client.Admin.ReleaseCheckpoint(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**checkpointID:** `string` — Checkpoint id
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Admin.GetNamespaceDiagnostics(NamespaceID) -> *loonfs.NamespaceDiagnostics</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns namespace state together with the current manifest and visible WAL tail.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.GetNamespaceDiagnosticsRequest{
    NamespaceID: "namespace_id",
}
client.Admin.GetNamespaceDiagnostics(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Admin.GetGrepIndexStatus(NamespaceID) -> *loonfs.GrepIndexStatusResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns whether the namespace's grep index is `disabled`, `backfilling`, or `active`, including build progress when available. A namespace that has never enabled the index is `disabled`. This operation requires a deployment that maintains grep indexes and does not change the index.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.GetGrepIndexStatusRequest{
    NamespaceID: "namespace_id",
}
client.Admin.GetGrepIndexStatus(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Admin.DisableGrepIndex(NamespaceID) -> *loonfs.GrepIndexStatusResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Disables the namespace's grep root and clears its segment references with one durable compare-and-swap; index maintenance stops on its own once a step reads the disabled root. Explicit grep garbage collection later reclaims the segments. Idempotent. Requires this deployment to maintain the grep index.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.DisableGrepIndexRequest{
    NamespaceID: "namespace_id",
}
client.Admin.DisableGrepIndex(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Admin.EnableGrepIndex(NamespaceID) -> *loonfs.GrepIndexStatusResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Enables the namespace's grep root and asks this deployment's maintenance runner for the backfill's first step. The response reports the lifecycle and bookkeeping read after the transition: a fresh enable is `backfilling` with the sequence its checkpoint captured, while an already-enabled namespace answers with its current status. Idempotent. Requires this deployment to maintain the grep index.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.EnableGrepIndexRequest{
    NamespaceID: "namespace_id",
}
client.Admin.EnableGrepIndex(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Admin.GcGrepIndex(NamespaceID, request) -> *loonfs.GrepGcResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Runs one explicit garbage-collection pass over only this namespace's grep-owned extension keyspace. A tombstoned or absent namespace has aged extension state reaped; no grep garbage collection runs implicitly. `max_objects` bounds the reads the pass spends and returns a `next_cursor` when keys remain; resuming re-reads liveness and the grep root, so a cursor only skips enumeration. Requires this deployment to maintain the grep index.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.GrepGcRequest{
    NamespaceID: "namespace_id",
}
client.Admin.GcGrepIndex(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**cursor:** `*string` 

Opaque resume token returned as `next_cursor` by an earlier pass
against the same namespace.
    
</dd>
</dl>

<dl>
<dd>

**maxObjects:** `*int64` 

Reads this pass may spend before returning with a `next_cursor`.
Omit to take the same per-pass default the runtime's own collection
takes.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Admin.MaintenanceStep(NamespaceID, request) -> *loonfs.MaintenanceStepResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Runs one bounded maintenance step. The body selects the actions by naming them: `metadata` folds the WAL tail once it reaches the threshold and merges one bounded reorganization unit, `advance_retention: true` advances the retention floor, and `gc` runs one bounded garbage-collection pass. Selected actions run in that order, each reports separately, and an absent report means the body did not select that action. A body that selects nothing is rejected. Nothing surrenders replay history or sweeps objects unless the body asked for it. A deleted namespace accepts a step that selects `gc` alone, which is how its reclaimable state is collected; any other selection is refused. Step-driven GC defaults to 1024 candidates and returns its cursor for a later step rather than looping internally. Losing the root race is an outcome, not an error.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.MaintenanceStepRequest{
    NamespaceID: "namespace_id",
}
client.Admin.MaintenanceStep(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**advanceRetention:** `*bool` 

Advance the retention floor to the flushed manifest head. Nothing
surrenders replay history unless this is true.
    
</dd>
</dl>

<dl>
<dd>

**gc:** `*loonfs.GcRequest` 

Run one bounded mark-and-sweep garbage-collection pass. Nothing
sweeps unless this is present.
    
</dd>
</dl>

<dl>
<dd>

**metadata:** `*loonfs.MetadataMaintenanceRequest` 

Flush the visible WAL tail into metadata tables, then run one bounded
reorganization step.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Admin.ProbeStore(request) -> *loonfs.StoreProbeResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Proves the configured object store honours the create-if-absent, compare-and-swap, visibility, listing, and ranged-read semantics LoonFS depends on, and reports what it found check by check. Nothing runs this implicitly: a probe writes and deletes objects, all of them under a scratch prefix that is not a durable object family, and its last check deletes them and proves the prefix empty. A store that fails a check answers 200 with that check reported `failed` — the probe ran, and the answer is that the store is wrong. Optional capabilities a store declares it lacks answer `unsupported`, which is an answer rather than a fault. This route does not decide whether the deployment may serve presigned direct uploads: that trust comes from the endpoint allowlist, because a probe exercises the server's own request path and never a presigned capability handed to a client.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := map[string]any{
    "key": "value",
}
client.Admin.ProbeStore(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `loonfs.StoreProbeRequest` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## namespaces
<details><summary><code>client.Namespaces.CreateNamespace(request) -> *loonfs.Namespace</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a new empty namespace.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.CreateNamespaceRequest{
    NamespaceID: "demo",
}
client.Namespaces.CreateNamespace(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `loonfs.NamespaceID` — Durable namespace id to create.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Namespaces.GetNamespace(NamespaceID) -> *loonfs.Namespace</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the current head and retention state for a namespace.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.GetNamespaceRequest{
    NamespaceID: "namespace_id",
}
client.Namespaces.GetNamespace(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Namespaces.DeleteNamespace(NamespaceID) -> *loonfs.DeleteNamespaceResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Marks a namespace as deleted.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.DeleteNamespaceRequest{
    NamespaceID: "namespace_id",
}
client.Namespaces.DeleteNamespace(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**expectedHeadSeq:** `*loonfs.ChangeSeq` — Delete only if the namespace head is still at this sequence
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Namespaces.ForkNamespace(NamespaceID, request) -> *loonfs.Namespace</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a new namespace as a fork from the source namespace's current durable view.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.ForkNamespaceRequest{
    NamespaceID: "namespace_id",
    NewNamespaceID: "demo",
}
client.Namespaces.ForkNamespace(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Source namespace id
    
</dd>
</dl>

<dl>
<dd>

**newNamespaceID:** `loonfs.NamespaceID` — Durable namespace id for the fork target.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## filesystem
<details><summary><code>client.Filesystem.ListChanges(NamespaceID) -> *loonfs.ChangesResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns committed changes from the write-ahead log. Callers can use this feed to keep another projection synchronized with WAL history.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.ListChangesRequest{
    NamespaceID: "namespace_id",
    AfterSeq: int64(1000000),
}
client.Filesystem.ListChanges(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**afterSeq:** `loonfs.ChangeSeq` — Return committed changes after this sequence
    
</dd>
</dl>

<dl>
<dd>

**limit:** `*int` — Maximum page size
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Filesystem.ApplyCommit(NamespaceID, request) -> *loonfs.CommitResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Applies one commit: an ordered, non-empty list of path operations that commit together as one logical commit, under one commit id that makes retries idempotent. A single-operation call is the one-element case. The first operation that fails aborts the whole request, and a request carrying more than one operation names that operation's position in `details.operation_index`.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.CommitRequest{
    NamespaceID: "namespace_id",
    Actor: &loonfs.ActorRef{
        ID: "usr_8f3c",
        Kind: loonfs.ActorKindUser,
    },
    CommitID: "c_f3a9c2d4b6e8417a90c5d2f8e1b7a6c0",
    Operations: []*loonfs.FilesystemOperation{
        &loonfs.FilesystemOperation{
            CreateDirectory: &loonfs.FsOpCreateDirectory{
                Path: "/docs/report.txt",
            },
        },
    },
}
client.Filesystem.ApplyCommit(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**actor:** `*loonfs.ActorRef` — Actor responsible for the commit, as supplied by the application.
    
</dd>
</dl>

<dl>
<dd>

**commitID:** `loonfs.CommitID` — Caller-supplied idempotency key for the whole request.
    
</dd>
</dl>

<dl>
<dd>

**contentTokens:** `[]*loonfs.ContentToken` 

Proofs for any new external content refs introduced by this request.
One proof covers every operation that names its content ref.
    
</dd>
</dl>

<dl>
<dd>

**message:** `*string` 

Caller annotation recorded on the commit and reported by the change
feed. Part of the commit's identity: reusing `commit_id` with a
different message is a `commit_id_reuse_conflict`, exactly as it is
for an explicit commit.
    
</dd>
</dl>

<dl>
<dd>

**operations:** `[]*loonfs.FilesystemOperation` 

Ordered operations to apply. Must be non-empty; they commit all
together or not at all.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Filesystem.GetFileBytes(NamespaceID) -> string</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns file bytes for the current revision at a path, or for a specific retained revision when `revision_no` is provided.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.GetFileBytesRequest{
    NamespaceID: "namespace_id",
    Path: "path",
}
client.Filesystem.GetFileBytes(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**path:** `string` — Absolute file path
    
</dd>
</dl>

<dl>
<dd>

**revisionNo:** `*loonfs.RevisionNo` — Optional prior revision number
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Filesystem.BeginDownload(NamespaceID, request) -> *loonfs.BeginDownloadResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Authorizes one direct read of a file's content object and returns a short-lived presigned GET capability, the resolved revision, and the content reference the client checks the arriving bytes against. `Range` is outside the signature, so one grant serves ranged, resumed, and parallel reads. Deployments that cannot presign answer 501 `not_supported`; the proxied `GET /filesystem/content` route stays available and is capped by `download.max_content_bytes`.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.BeginDownloadRequest{
    NamespaceID: "namespace_id",
    Path: "/docs/report.txt",
}
client.Filesystem.BeginDownload(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**path:** `loonfs.AbsolutePath` — Absolute path of the file to read.
    
</dd>
</dl>

<dl>
<dd>

**revisionNo:** `*loonfs.RevisionNo` — Revision to read, or `None` for the path's current revision.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Filesystem.ListPathEntries(NamespaceID) -> *loonfs.ListPathEntriesResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Lists a directory at the current namespace head.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.ListPathEntriesRequest{
    NamespaceID: "namespace_id",
    Path: "path",
}
client.Filesystem.ListPathEntries(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**path:** `string` — Absolute filesystem path
    
</dd>
</dl>

<dl>
<dd>

**limit:** `*int` — Maximum page size
    
</dd>
</dl>

<dl>
<dd>

**cursor:** `*string` — Opaque directory-list page cursor
    
</dd>
</dl>

<dl>
<dd>

**includeAttributes:** `*bool` — Project each entry's attribute map and revision (`true` or `false`). Defaults to `false`: a page holds many entries and each map may be 64 KiB, so a listing does not carry them unless asked.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Filesystem.ListFileRevisions(NamespaceID) -> *loonfs.ListFileRevisionsResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Resolves the current path to a file inode and returns revisions for that file. If the file could be renamed, use the inode revision API for stable identity.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.ListFileRevisionsRequest{
    NamespaceID: "namespace_id",
    Path: "path",
}
client.Filesystem.ListFileRevisions(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**path:** `string` — Absolute file path
    
</dd>
</dl>

<dl>
<dd>

**limit:** `*int` — Maximum page size
    
</dd>
</dl>

<dl>
<dd>

**cursor:** `*string` — Opaque file-revisions page cursor
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Filesystem.StatPath(NamespaceID) -> *loonfs.AuthoritativePathEntry</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the current metadata for a path, including inode identity, kind, display name, file content metadata, and the inode's attributes.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.StatPathRequest{
    NamespaceID: "namespace_id",
    Path: "path",
}
client.Filesystem.StatPath(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**path:** `string` — Absolute filesystem path
    
</dd>
</dl>

<dl>
<dd>

**includeAttributes:** `*bool` — Project the inode's attribute map and revision (`true` or `false`). Defaults to `true`: a stat answers for one path and a map is capped at 64 KiB.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Filesystem.ListTrash(NamespaceID) -> *loonfs.ListTrashResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the namespace's recoverable deletions, oldest deletion first. Entries never age out at the retention floor; each carries the inode id and deletion sequence undelete needs, plus the deleted name when the delete recorded one.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.ListTrashRequest{
    NamespaceID: "namespace_id",
}
client.Filesystem.ListTrash(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**limit:** `*int` — Maximum page size
    
</dd>
</dl>

<dl>
<dd>

**cursor:** `*string` — Opaque trash page cursor
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## inodes
<details><summary><code>client.Inodes.StatInode(NamespaceID, InodeID) -> *loonfs.AuthoritativePathEntry</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the current path entry for a visible inode. Unknown or hidden inodes answer `inode_not_found`.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.StatInodeRequest{
    NamespaceID: "namespace_id",
    InodeID: "ino_123",
}
client.Inodes.StatInode(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**inodeID:** `string` — Inode ID
    
</dd>
</dl>

<dl>
<dd>

**includeAttributes:** `*bool` — Project the inode's attribute map and revision (`true` or `false`). Defaults to `true`: a stat answers for one path and a map is capped at 64 KiB.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Inodes.ListFileRevisionsByInode(NamespaceID, InodeID) -> *loonfs.ListFileRevisionsResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns retained revisions for a file inode without requiring a current path.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.ListFileRevisionsByInodeRequest{
    NamespaceID: "namespace_id",
    InodeID: "ino_123",
}
client.Inodes.ListFileRevisionsByInode(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**inodeID:** `string` — File inode ID
    
</dd>
</dl>

<dl>
<dd>

**limit:** `*int` — Maximum page size
    
</dd>
</dl>

<dl>
<dd>

**cursor:** `*string` — Opaque file-revisions page cursor
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Inodes.GetFileRevisionBytesByInode(NamespaceID, InodeID, RevisionNo) -> string</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Reads and verifies one retained file revision by inode ID and revision number.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.GetFileRevisionBytesByInodeRequest{
    NamespaceID: "namespace_id",
    InodeID: "inode_id",
    RevisionNo: int64(1000000),
}
client.Inodes.GetFileRevisionBytesByInode(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**inodeID:** `string` — File inode ID
    
</dd>
</dl>

<dl>
<dd>

**revisionNo:** `loonfs.RevisionNo` — Revision number
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Inodes.BeginDownloadByInode(NamespaceID, InodeID, RevisionNo, request) -> *loonfs.BeginDownloadByInodeResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Authorizes a direct read of one retained inode revision. The request body is `{}` and the response does not include a path.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.BeginDownloadByInodeBody{
    NamespaceID: "namespace_id",
    InodeID: "ino_123",
    RevisionNo: int64(1000000),
    Body: map[string]any{
        "key": "value",
    },
}
client.Inodes.BeginDownloadByInode(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**inodeID:** `string` — File inode ID
    
</dd>
</dl>

<dl>
<dd>

**revisionNo:** `loonfs.RevisionNo` — Revision number
    
</dd>
</dl>

<dl>
<dd>

**request:** `loonfs.BeginDownloadByInodeRequest` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## query
<details><summary><code>client.Query.Grep(NamespaceID, request) -> *loonfs.GrepResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Searches file content with a regular expression, accelerated by the namespace's grep index. Matches are verified against the real pattern and returned in ascending `(inode_id, byte_offset)` order; revisions committed after the index watermark are scanned exhaustively unless `allow_stale` skips them. Requires this deployment to serve grep and the namespace to carry a materialized active grep root.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.GrepRequest{
    NamespaceID: "namespace_id",
    Pattern: "pattern",
}
client.Query.Grep(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**allowScan:** `*bool` 

Permit a capped exhaustive scan when the pattern yields no required
grams. Refused beyond the server's scan budget.
    
</dd>
</dl>

<dl>
<dd>

**allowStale:** `*bool` 

When the unindexed tail exceeds the scan budget, return
indexed-only results (reported via `tail_scanned: false`) instead
of failing with `index_lagging`.
    
</dd>
</dl>

<dl>
<dd>

**caseInsensitive:** `*bool` 

Match case-insensitively. Verification is exact; the index remains
consulted through its case-folded grams.
    
</dd>
</dl>

<dl>
<dd>

**cursor:** `*string` 

Resume cursor from a previous page. The cursor resumes strictly
after the last candidate the issuing page finished scanning and is
bound to that page's request; each page is evaluated against the
namespace head at page time.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `*int` — Maximum matches per page.
    
</dd>
</dl>

<dl>
<dd>

**pathPrefix:** `*loonfs.AbsolutePath` 

Restrict matches to files under this complete absolute path, resolved
to a directory inode before candidates are filtered.
    
</dd>
</dl>

<dl>
<dd>

**pattern:** `string` 

The pattern, in the Rust `regex` crate's dialect (no backreferences
or lookaround). Patterns that require no literal bytes are rejected
with `query_unindexable` unless `allow_scan` is set.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## uploads
<details><summary><code>client.Uploads.BeginUpload(NamespaceID, request) -> *loonfs.BeginUploadResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Starts an upload session for content that may later be attached to a file. Service-proxied uploads send bytes through the server; direct-put uploads return object-store presigned credentials.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.BeginUploadBody{
    NamespaceID: "namespace_id",
    Body: &loonfs.BeginUploadRequest{
        ServiceProxied: &loonfs.BeginUploadServiceProxied{},
    },
}
client.Uploads.BeginUpload(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**request:** `*loonfs.BeginUploadRequest` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Uploads.GetUploadStatus(NamespaceID, UploadID) -> *loonfs.UploadSessionResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns an upload session. A completed session includes a new content token so the client can retry the commit without uploading the content again.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.GetUploadStatusRequest{
    NamespaceID: "namespace_id",
    UploadID: "upload_id",
}
client.Uploads.GetUploadStatus(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**uploadID:** `string` — Upload session id
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Uploads.AbortUpload(NamespaceID, UploadID) -> *loonfs.UploadSessionResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Ends an upload session without selecting content and deletes the object it was writing. Repeating it succeeds; a session that already completed cannot be aborted.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.AbortUploadRequest{
    NamespaceID: "namespace_id",
    UploadID: "upload_id",
}
client.Uploads.AbortUpload(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**uploadID:** `string` — Upload session id
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Uploads.CompleteUpload(NamespaceID, UploadID, request) -> *loonfs.UploadSessionResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Completes an upload. The request mode must match the mode used to start the session. Direct-multipart requests also include the content claim and completed parts.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.CompleteUploadBody{
    NamespaceID: "namespace_id",
    UploadID: "upload_id",
    Body: &loonfs.CompleteUploadRequest{
        ServiceProxied: &loonfs.CompleteUploadServiceProxied{},
    },
}
client.Uploads.CompleteUpload(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**uploadID:** `string` — Upload session id
    
</dd>
</dl>

<dl>
<dd>

**request:** `*loonfs.CompleteUploadRequest` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Uploads.SignUploadParts(NamespaceID, UploadID, request) -> *loonfs.SignUploadPartsResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns one short-lived, checksum-bound upload capability per requested part of an open direct_multipart session. Asking again for a part is how a client retries it.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```go
request := &loonfs.SignUploadPartsRequest{
    NamespaceID: "namespace_id",
    UploadID: "upload_id",
    Parts: []*loonfs.UploadPartChecksumClaim{
        &loonfs.UploadPartChecksumClaim{
            Checksum: &loonfs.Checksum{
                Algorithm: loonfs.ChecksumAlgorithmSha256,
                Value: "value",
            },
            PartNumber: 1,
        },
    },
}
client.Uploads.SignUploadParts(
    context.TODO(),
    request,
)
```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**namespaceID:** `string` — Namespace id
    
</dd>
</dl>

<dl>
<dd>

**uploadID:** `string` — Upload session id
    
</dd>
</dl>

<dl>
<dd>

**parts:** `[]*loonfs.UploadPartChecksumClaim` 

Parts to authorize, each with the checksum the provider will enforce
on it. Asking again for a part already uploaded is how a client
retries one: a repeated part is last-write-wins at the provider.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

