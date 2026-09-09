# Reference
## Capabilities
<details><summary><code>client.Capabilities.Retrieve() -> *loonfs.CapabilityDocument</code></summary>
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
client.Capabilities.Retrieve(
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

## namespaces
<details><summary><code>client.Namespaces.Create(request) -> *loonfs.Namespace</code></summary>
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
client.Namespaces.Create(
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

<details><summary><code>client.Namespaces.Retrieve(NamespaceID) -> *loonfs.Namespace</code></summary>
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
client.Namespaces.Retrieve(
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

<details><summary><code>client.Namespaces.Delete(NamespaceID) -> *loonfs.DeleteNamespaceResponse</code></summary>
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
client.Namespaces.Delete(
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

<details><summary><code>client.Namespaces.Fork(NamespaceID, request) -> *loonfs.Namespace</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a new namespace from the source current head or a live snapshot.
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
client.Namespaces.Fork(
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

<dl>
<dd>

**snapshotID:** `*loonfs.CheckpointID` — Fork from this live snapshot instead of the current head.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Changes
<details><summary><code>client.Changes.List(NamespaceID) -> *loonfs.ListChangesResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns committed changes after a sequence. A snapshot limits the feed to its captured sequence.
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
    SnapshotID: loonfs.String(
        "pin_00000000000000000001-0000000000000002",
    ),
}
client.Changes.List(
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

<dl>
<dd>

**snapshotID:** `*loonfs.CheckpointID` — End the feed at this snapshot's captured sequence
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Commits
<details><summary><code>client.Commits.Create(NamespaceID, request) -> *loonfs.CommitResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Applies one commit: an ordered, non-empty list of path operations that commit together as one logical commit, under one commit id that makes retries idempotent. Request assertions check the pre-state after receipt resolution and before operations; a failed assertion names its position in `details.assertion_index`. A single-operation call is the one-element case. The first operation that fails aborts the whole request, and a request carrying more than one operation names that operation's position in `details.operation_index`.
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
            CopyPath: &loonfs.FilesystemOperationCopyPath{
                FromPath: "/docs/report.txt",
                ToPath: "/docs/report.txt",
            },
        },
    },
}
client.Commits.Create(
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

**assertions:** `[]*loonfs.CommitAssertion` — Ordered admission conditions evaluated before any operations.
    
</dd>
</dl>

<dl>
<dd>

**commitID:** `loonfs.CommitID` — Caller-supplied idempotency key for the whole request.
    
</dd>
</dl>

<dl>
<dd>

**contentTokens:** `[]*loonfs.ContentToken` — The proofs for new external content references in this request.
    
</dd>
</dl>

<dl>
<dd>

**message:** `*string` — The caller annotation that forms part of the commit identity.
    
</dd>
</dl>

<dl>
<dd>

**operations:** `[]*loonfs.FilesystemOperation` — The non-empty ordered operations to commit atomically.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Files
<details><summary><code>client.Files.Content(NamespaceID) -> string</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the current file bytes, a retained revision, or the revision captured by a live snapshot.
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
client.Files.Content(
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

**revisionNo:** `*loonfs.RevisionNo` — Optional prior revision number; cannot be combined with snapshot_id
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `*loonfs.CheckpointID` — Use the file revision captured by this snapshot
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Files.CreateDownload(NamespaceID, request) -> *loonfs.BeginDownloadResponse</code></summary>
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
    SnapshotID: loonfs.String(
        "pin_00000000000000000001-0000000000000002",
    ),
    Path: "/docs/report.txt",
}
client.Files.CreateDownload(
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

**snapshotID:** `*loonfs.CheckpointID` — Use the file revision captured by this snapshot
    
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

<details><summary><code>client.Files.List(NamespaceID) -> *loonfs.ListPathEntriesResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Lists a directory from the current state or a live snapshot.
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
    SnapshotID: loonfs.String(
        "pin_00000000000000000001-0000000000000002",
    ),
}
client.Files.List(
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

<dl>
<dd>

**snapshotID:** `*loonfs.CheckpointID` — Use the directory state captured by this snapshot
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Files.Retrieve(NamespaceID) -> *loonfs.PathEntry</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns path metadata from the current state or a live snapshot.
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
request := &loonfs.GetPathEntryRequest{
    NamespaceID: "namespace_id",
    Path: "path",
    SnapshotID: loonfs.String(
        "pin_00000000000000000001-0000000000000002",
    ),
}
client.Files.Retrieve(
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

<dl>
<dd>

**snapshotID:** `*loonfs.CheckpointID` — Use the path state captured by this snapshot
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Files.ListRevisions(NamespaceID) -> *loonfs.ListFileRevisionsResponse</code></summary>
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
client.Files.ListRevisions(
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

<details><summary><code>client.Files.Grep(NamespaceID) -> *loonfs.GrepResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Searches file content with a regular expression, accelerated by the namespace's grep index. Matches are verified against the real pattern and returned in ascending `(inode_id, byte_offset)` order; revisions committed after the index watermark are scanned exhaustively unless `allow_stale` skips them. Requires this deployment to serve grep and the namespace to carry a materialized active grep index.
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
client.Files.Grep(
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

**pattern:** `string` — Pattern in the Rust `regex` crate's dialect. Its UTF-8 encoding must be at most 1024 bytes.
    
</dd>
</dl>

<dl>
<dd>

**caseInsensitive:** `*bool` — Match case-insensitively (`true` or `false`). Defaults to `false`.
    
</dd>
</dl>

<dl>
<dd>

**pathPrefix:** `*string` — Complete absolute path used to restrict matches.
    
</dd>
</dl>

<dl>
<dd>

**allowScan:** `*bool` — Permit a capped exhaustive scan when the pattern has no required grams (`true` or `false`). Defaults to `false`.
    
</dd>
</dl>

<dl>
<dd>

**allowStale:** `*bool` — Return indexed-only results when the unindexed tail exceeds the scan budget (`true` or `false`). Defaults to `false`.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `*int` — Maximum matches per page
    
</dd>
</dl>

<dl>
<dd>

**cursor:** `*string` — Opaque grep page cursor
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Trash
<details><summary><code>client.Trash.List(NamespaceID) -> *loonfs.ListTrashResponse</code></summary>
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
client.Trash.List(
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
<details><summary><code>client.Inodes.Retrieve(NamespaceID, InodeID) -> *loonfs.PathEntry</code></summary>
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
request := &loonfs.GetInodeRequest{
    NamespaceID: "namespace_id",
    InodeID: "ino_123",
}
client.Inodes.Retrieve(
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

<details><summary><code>client.Inodes.ListChildren(NamespaceID, InodeID) -> *loonfs.ListInodeChildrenResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Lists one page of a directory's children addressed by parent inode ID, in canonical name-key order. Inode addressing keeps a listing and its resumption on the same directory across concurrent renames or moves of the parent.
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
request := &loonfs.ListInodeChildrenRequest{
    NamespaceID: "namespace_id",
    InodeID: "ino_123",
}
client.Inodes.ListChildren(
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

**inodeID:** `string` — Directory inode ID
    
</dd>
</dl>

<dl>
<dd>

**limit:** `*int` — Maximum page size
    
</dd>
</dl>

<dl>
<dd>

**cursor:** `*string` — Opaque directory page cursor
    
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

<details><summary><code>client.Inodes.ListRevisions(NamespaceID, InodeID) -> *loonfs.ListFileRevisionsResponse</code></summary>
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
client.Inodes.ListRevisions(
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

<details><summary><code>client.Inodes.Content(NamespaceID, InodeID, RevisionNo) -> string</code></summary>
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
client.Inodes.Content(
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

<details><summary><code>client.Inodes.CreateDownload(NamespaceID, InodeID, RevisionNo, request) -> *loonfs.BeginDownloadByInodeResponse</code></summary>
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
request := &loonfs.CreateDownloadByInodeRequest{
    NamespaceID: "namespace_id",
    InodeID: "ino_123",
    RevisionNo: int64(1000000),
    Body: map[string]any{
        "key": "value",
    },
}
client.Inodes.CreateDownload(
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

## Snapshots
<details><summary><code>client.Snapshots.List(NamespaceID) -> *loonfs.ListSnapshotsResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Lists live snapshots in snapshot-id order. Released and expired snapshots are omitted.
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
request := &loonfs.ListSnapshotsRequest{
    NamespaceID: "namespace_id",
}
client.Snapshots.List(
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

**cursor:** `*string` — Opaque snapshot-list page cursor
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Snapshots.Create(NamespaceID, request) -> *loonfs.Snapshot</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a snapshot of the current namespace state. Every call creates a new snapshot.
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
request := &loonfs.CreateSnapshotRequest{
    NamespaceID: "namespace_id",
    Name: "name",
    TTLMs: int64(1000000),
}
client.Snapshots.Create(
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

**name:** `string` — A label that does not need to be unique.
    
</dd>
</dl>

<dl>
<dd>

**ttlMs:** `int64` — Snapshot lifetime from the current server time, in milliseconds.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Snapshots.Extend(NamespaceID, SnapshotID, request) -> *loonfs.Snapshot</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Extends a live snapshot without passing its lifetime limit. Repeating the request has the same result.
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
request := &loonfs.ExtendSnapshotRequest{
    NamespaceID: "namespace_id",
    SnapshotID: "snapshot_id",
    TTLMs: int64(1000000),
}
client.Snapshots.Extend(
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

**snapshotID:** `string` — Snapshot id
    
</dd>
</dl>

<dl>
<dd>

**ttlMs:** `int64` — Requested lifetime from the server's current time, in milliseconds.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Snapshots.Release(NamespaceID, SnapshotID) -> *loonfs.ReleaseSnapshotResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Deletes a snapshot pin. A missing id returns snapshot_not_found.
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
request := &loonfs.ReleaseSnapshotRequest{
    NamespaceID: "namespace_id",
    SnapshotID: "snapshot_id",
}
client.Snapshots.Release(
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

**snapshotID:** `string` — Snapshot id
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## uploads
<details><summary><code>client.Uploads.Create(NamespaceID, request) -> *loonfs.BeginUploadResponse</code></summary>
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
request := &loonfs.CreateUploadRequest{
    NamespaceID: "namespace_id",
    Body: &loonfs.BeginUploadRequest{
        DirectMultipart: &loonfs.BeginUploadDirectMultipart{},
    },
}
client.Uploads.Create(
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

<details><summary><code>client.Uploads.Retrieve(NamespaceID, UploadID) -> *loonfs.UploadSession</code></summary>
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
request := &loonfs.GetUploadRequest{
    NamespaceID: "namespace_id",
    UploadID: "upload_id",
}
client.Uploads.Retrieve(
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

<details><summary><code>client.Uploads.Abort(NamespaceID, UploadID) -> *loonfs.UploadSession</code></summary>
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
client.Uploads.Abort(
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

<details><summary><code>client.Uploads.Complete(NamespaceID, UploadID, request) -> *loonfs.UploadSession</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Completes an upload. The request mode must match the mode used to start the session. Direct uploads include a content claim; multipart also includes completed parts.
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
request := &loonfs.CompleteUploadRequest{
    NamespaceID: "namespace_id",
    UploadID: "upload_id",
    Body: &loonfs.UploadCompletion{
        DirectMultipart: &loonfs.CompleteUploadDirectMultipart{
            Content: &loonfs.UploadContentClaim{
                Checksum: &loonfs.Checksum{
                    Algorithm: loonfs.ChecksumAlgorithmSha256,
                    Value: "value",
                },
                SizeBytes: int64(1000000),
            },
            Parts: []*loonfs.CompletedUploadPart{
                &loonfs.CompletedUploadPart{
                    Checksum: &loonfs.Checksum{
                        Algorithm: loonfs.ChecksumAlgorithmSha256,
                        Value: "value",
                    },
                    Etag: "etag",
                    PartNumber: 1,
                },
            },
        },
    },
}
client.Uploads.Complete(
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

**request:** `*loonfs.UploadCompletion` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Uploads.SignParts(NamespaceID, UploadID, request) -> *loonfs.SignUploadPartsResponse</code></summary>
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
client.Uploads.SignParts(
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

**parts:** `[]*loonfs.UploadPartChecksumClaim` — The parts to authorize; repeated part numbers replace their previous uploads.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Maintenance Checkpoints
<details><summary><code>client.Maintenance.Checkpoints.List(NamespaceID) -> *loonfs.ListCheckpointsResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Lists existing pins in checkpoint-id order. Expired pins remain visible until collection deletes them after expiry plus grace. The cursor resumes a live listing.
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
request := &maintenance.ListCheckpointsRequest{
    NamespaceID: "namespace_id",
}
client.Maintenance.Checkpoints.List(
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

<details><summary><code>client.Maintenance.Checkpoints.Create(NamespaceID, request) -> *loonfs.Checkpoint</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a named, user-owned checkpoint record pinning the current namespace view. Every call mints a new record under a new id; the name is a label, not a key. The record is a garbage-collection root until it is released, so routine maintenance should flush the WAL instead. This is a maintenance operation, not a file mutation.
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
request := &maintenance.CreateCheckpointRequest{
    NamespaceID: "namespace_id",
    Name: "name",
}
client.Maintenance.Checkpoints.Create(
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

**name:** `string` — The non-unique label recorded on the checkpoint.
    
</dd>
</dl>

<dl>
<dd>

**ttlMs:** `*int64` — The checkpoint lifetime in milliseconds, or `None` for an explicit release only.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Maintenance.Checkpoints.Release(NamespaceID, CheckpointID) -> *loonfs.ReleaseCheckpointResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Deletes a user-owned checkpoint pin. A missing id returns checkpoint_not_found. Garbage collection can reclaim its unreferenced manifest and runs.
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
request := &maintenance.ReleaseCheckpointRequest{
    NamespaceID: "namespace_id",
    CheckpointID: "checkpoint_id",
}
client.Maintenance.Checkpoints.Release(
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

## Maintenance Diagnostics
<details><summary><code>client.Maintenance.Diagnostics.Retrieve(NamespaceID) -> *loonfs.NamespaceDiagnostics</code></summary>
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
request := &maintenance.GetNamespaceDiagnosticsRequest{
    NamespaceID: "namespace_id",
}
client.Maintenance.Diagnostics.Retrieve(
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

## Maintenance GrepIndex
<details><summary><code>client.Maintenance.GrepIndex.Retrieve(NamespaceID) -> *loonfs.GrepIndex</code></summary>
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
request := &maintenance.GetGrepIndexRequest{
    NamespaceID: "namespace_id",
}
client.Maintenance.GrepIndex.Retrieve(
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

<details><summary><code>client.Maintenance.GrepIndex.Disable(NamespaceID) -> *loonfs.GrepIndex</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Disables the namespace's grep index by publishing the next manifest number with no segment references. Index maintenance stops when a step reads the disabled manifest. Explicit grep garbage collection later reclaims the segments. Idempotent. Requires this deployment to maintain the grep index.
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
request := &maintenance.DisableGrepIndexRequest{
    NamespaceID: "namespace_id",
}
client.Maintenance.GrepIndex.Disable(
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

<details><summary><code>client.Maintenance.GrepIndex.Enable(NamespaceID) -> *loonfs.GrepIndex</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Enables the namespace's grep index and asks this deployment's maintenance runner for the backfill's first step. The response reports the lifecycle and bookkeeping read after the transition: a fresh enable is `backfilling` with the sequence its checkpoint captured, while an already-enabled namespace answers with its current status. Idempotent. Requires this deployment to maintain the grep index.
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
request := &maintenance.EnableGrepIndexRequest{
    NamespaceID: "namespace_id",
}
client.Maintenance.GrepIndex.Enable(
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

<details><summary><code>client.Maintenance.GrepIndex.Gc(NamespaceID, request) -> *loonfs.GrepGcResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Runs one explicit garbage-collection pass over only this namespace's grep-owned extension keyspace. A tombstoned or absent namespace has aged extension state reaped. Every call reads durable roots and completes one pass. Unreadable or invalid roots fail before deletion. Requires this deployment to maintain the grep index.
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
request := &maintenance.GcGrepIndexRequest{
    NamespaceID: "namespace_id",
    Body: map[string]any{
        "key": "value",
    },
}
client.Maintenance.GrepIndex.Gc(
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

**request:** `loonfs.GrepGcRequest` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Maintenance Runs
<details><summary><code>client.Maintenance.Runs.Create(NamespaceID, request) -> *loonfs.RunMaintenanceResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Runs one maintenance job for the namespace. The body names the job with `kind`: `metadata`, `metadata_compaction`, `gc`, or `retention`. The response carries the same `kind` and that job's result. A deleted namespace accepts only `gc`. A `gc` call reads current roots, then sweeps every family to the end. Each listing starts at the beginning. The call keeps no continuation.
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
request := &maintenance.CreateRunsRequest{
    NamespaceID: "namespace_id",
    Body: &loonfs.RunMaintenanceRequest{
        Gc: &loonfs.RunMaintenanceRequestGc{},
    },
}
client.Maintenance.Runs.Create(
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

**request:** `*loonfs.RunMaintenanceRequest` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Maintenance Store
<details><summary><code>client.Maintenance.Store.Probe(request) -> *loonfs.StoreProbeResponse</code></summary>
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
client.Maintenance.Store.Probe(
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

