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
<details><summary><code>client.Namespaces.Create(request) -> *loonfs.NamespaceMetadata</code></summary>
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

**access:** `*loonfs.NamespaceAccess` 

The namespace's access mode, which can't be changed after the namespace is created. The
default is unrestricted.
    
</dd>
</dl>

<dl>
<dd>

**namespaceID:** `loonfs.NamespaceID` — The durable ID of the new namespace.
    
</dd>
</dl>

<dl>
<dd>

**naming:** `*loonfs.NamespaceNaming` 

How the namespace compares sibling names. The mode can't be changed after the namespace is
created, and it defaults to `case_insensitive`.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Namespaces.Retrieve(NamespaceID) -> *loonfs.NamespaceMetadata</code></summary>
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

**namespaceID:** `string` — The namespace ID.
    
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

Marks a namespace as deleted. The namespace ID can never be used again to create or fork a namespace.
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**expectedHeadSeq:** `*loonfs.ChangeSeq` — The head sequence that the namespace must still have for the delete to succeed.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Namespaces.Fork(NamespaceID, request) -> *loonfs.NamespaceMetadata</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a new namespace from the source namespace's current head or from a live snapshot.
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

**namespaceID:** `string` — The source namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**newNamespaceID:** `loonfs.NamespaceID` — The durable ID of the new namespace that the fork creates.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `*loonfs.PinID` — A live snapshot to fork from. Without it, the fork starts from the current head.
    
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**afterSeq:** `loonfs.ChangeSeq` — The sequence after which to return committed changes.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `*int` — The maximum page size.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `*loonfs.PinID` — A snapshot to read the feed from. The feed ends at the sequence that the snapshot captured.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Commits
<details><summary><code>client.Commits.Create(NamespaceID, request) -> *loonfs.Commit</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Applies one commit, which is an ordered, non-empty list of path operations that are committed together. The commit ID makes retries idempotent. The server checks the request's preconditions against the state before the commit, after it resolves any commit receipt and before it runs the operations. A failed precondition's position is reported in `details.precondition_index`. A request can contain a single operation. If an operation fails, the whole request fails, and for a request with more than one operation, `details.operation_index` reports the failed operation's position.
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
    CommitID: "c_f3a9c2d4b6e8417a90c5d2f8e1b7a6c0",
    Operations: []*loonfs.FilesystemOperation{
        &loonfs.FilesystemOperation{
            AppendFile: &loonfs.FilesystemOperationAppendFile{
                InlineContent: "inline_content",
                Path: "/docs/report.txt",
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**commitID:** `loonfs.CommitID` — The caller's idempotency key for the whole request.
    
</dd>
</dl>

<dl>
<dd>

**contentTokens:** `[]*loonfs.ContentToken` — One content token for each uploaded `content_ref` that the request uses.
    
</dd>
</dl>

<dl>
<dd>

**message:** `*string` — An annotation from the caller. The message is part of the commit's identity.
    
</dd>
</dl>

<dl>
<dd>

**operations:** `[]*loonfs.FilesystemOperation` — The operations to commit atomically, in order. The list can't be empty.
    
</dd>
</dl>

<dl>
<dd>

**preconditions:** `[]*loonfs.CommitPrecondition` — The conditions that the server checks, in order, before it runs any operation.
    
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
request := &loonfs.GetFileContentRequest{
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**path:** `string` — The absolute path of the file to read.
    
</dd>
</dl>

<dl>
<dd>

**revisionNo:** `*loonfs.RevisionNo` — An earlier revision to read. It can't be combined with `snapshot_id`.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `*loonfs.PinID` — The snapshot whose captured file revision to read.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Files.CreateDownload(NamespaceID, request) -> *loonfs.CreateDownloadResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns ordered, signed object ranges that cover exactly `[start_offset, size_bytes)` of the selected revision. Each range includes its signed headers in `access.headers`, and the client sends them unchanged. The client checks each range's length and the complete revision's checksum. To resume a download, a client asks for a new grant from its current offset. A deployment that can't presign URLs returns 501 `not_supported`. The proxied `GET /filesystem/content` route is still available, up to the `download.service_proxied.max_content_bytes` limit.
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
request := &loonfs.CreateDownloadRequest{
    NamespaceID: "namespace_id",
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**path:** `loonfs.AbsolutePath` — The absolute path of the file to read.
    
</dd>
</dl>

<dl>
<dd>

**revisionNo:** `*loonfs.RevisionNo` 

The revision to read. Omit it to read the path's current revision. It can't be combined with
`snapshot_id`.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `*loonfs.PinID` 

A snapshot to read the file from. The download reads the revision that the snapshot
captured. It can't be combined with `revision_no`.
    
</dd>
</dl>

<dl>
<dd>

**startOffset:** `*int64` 

The first byte that the grant reads. The grant covers the revision's bytes from
`start_offset` up to `size_bytes`, so a client that resumes a download asks for a new grant
that starts after the bytes it already has. The offset must be less than the revision's
size. For a revision of zero bytes, it must be 0.
    
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**path:** `string` — The absolute path of the directory to list.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `*int` — The maximum page size.
    
</dd>
</dl>

<dl>
<dd>

**cursor:** `*string` — The opaque cursor that the previous page returned.
    
</dd>
</dl>

<dl>
<dd>

**includeAttributes:** `*bool` — Whether the response includes each entry's attribute map and attribute revision. The default is `false`, because a page can have many entries and each map can be up to 64 KiB.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `*loonfs.PinID` — The snapshot whose captured directory state to read.
    
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**path:** `string` — The absolute path to read.
    
</dd>
</dl>

<dl>
<dd>

**includeAttributes:** `*bool` — Whether the response includes the inode's attribute map and attribute revision. The default is `true`, because a stat reads one path and a map is at most 64 KiB.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `*loonfs.PinID` — The snapshot whose captured path state to read.
    
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

Resolves the current path to a file inode and returns that file's revisions. If the file might be renamed, use the inode revisions route, which identifies the file by a stable ID.
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**path:** `string` — The absolute path of the file.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `*int` — The maximum page size.
    
</dd>
</dl>

<dl>
<dd>

**cursor:** `*string` — The opaque cursor that the previous page returned.
    
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

Searches file content with a regular expression, using the namespace's grep index to narrow the candidates. The server checks each candidate against the full pattern and returns matches in ascending `(inode_id, byte_offset)` order. It scans every revision committed after the index watermark, unless `allow_stale` skips them. The deployment must serve grep, and the namespace must have a materialized, active grep index.
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
    InodeID: loonfs.String(
        "ino_123",
    ),
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**pattern:** `string` — The search pattern, in the Rust `regex` crate's syntax. Its UTF-8 encoding must be at most 1024 bytes.
    
</dd>
</dl>

<dl>
<dd>

**caseInsensitive:** `*bool` — Whether matching ignores case. The default is `false`.
    
</dd>
</dl>

<dl>
<dd>

**pathPrefix:** `*string` — A complete absolute path that limits matches to its descendants. It can't be combined with `inode_id`.
    
</dd>
</dl>

<dl>
<dd>

**inodeID:** `*string` — An inode that limits matches to its descendants. It can't be combined with `path_prefix`.
    
</dd>
</dl>

<dl>
<dd>

**allowScan:** `*bool` — Whether a pattern with no required literal bytes can use a capped exhaustive scan. The default is `false`.
    
</dd>
</dl>

<dl>
<dd>

**allowStale:** `*bool` — Whether the server returns results from the index alone when the unindexed tail exceeds the scan budget. The default is `false`.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `*int` — The maximum number of matches per page.
    
</dd>
</dl>

<dl>
<dd>

**cursor:** `*string` — The opaque cursor that the previous page returned.
    
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

Returns the namespace's recoverable deletions, oldest first. Advancing the retention floor doesn't remove entries. Each entry includes the inode ID and deletion sequence that an undelete needs, and the deleted name when the delete recorded one.
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `*int` — The maximum page size.
    
</dd>
</dl>

<dl>
<dd>

**cursor:** `*string` — The opaque cursor that the previous page returned.
    
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

Returns the path entry for a visible inode, from the current state or a live snapshot. An unknown or hidden inode returns `inode_not_found`.
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
    SnapshotID: loonfs.String(
        "pin_00000000000000000001-0000000000000002",
    ),
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**inodeID:** `string` — The inode ID.
    
</dd>
</dl>

<dl>
<dd>

**includeAttributes:** `*bool` — Whether the response includes the inode's attribute map and attribute revision. The default is `true`, because a stat reads one path and a map is at most 64 KiB.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `*loonfs.PinID` — The snapshot whose captured path state to read.
    
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

Lists one page of a directory's children, in canonical name key order, from the current state or a live snapshot. The request identifies the directory by its inode ID, so a listing and its later pages stay on the same directory even if the directory is renamed or moved at the same time.
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
    SnapshotID: loonfs.String(
        "pin_00000000000000000001-0000000000000002",
    ),
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**inodeID:** `string` — The directory's inode ID.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `*int` — The maximum page size.
    
</dd>
</dl>

<dl>
<dd>

**cursor:** `*string` — The opaque cursor that the previous page returned.
    
</dd>
</dl>

<dl>
<dd>

**includeAttributes:** `*bool` — Whether the response includes each entry's attribute map and attribute revision. The default is `false`, because a page can have many entries and each map can be up to 64 KiB.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `*loonfs.PinID` — The snapshot whose captured directory state to read.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Inodes.Content(NamespaceID, InodeID) -> string</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Reads and verifies the current revision of a visible file inode, wherever the inode is bound, or the revision that a live snapshot captured. An unknown or hidden inode returns `inode_not_found`.
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
request := &loonfs.GetFileContentByInodeRequest{
    NamespaceID: "namespace_id",
    InodeID: "inode_id",
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**inodeID:** `string` — The file's inode ID.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `*loonfs.PinID` — The snapshot whose captured file revision to read.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Inodes.CreateDownload(NamespaceID, InodeID, request) -> *loonfs.CreateDownloadByInodeResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Authorizes a direct read of the current revision of a visible file inode, wherever the inode is bound, or of the revision that a live snapshot captured. The capability reads exactly `[start_offset, size_bytes)`, as on the path route. The response doesn't include a path.
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**inodeID:** `string` — The file's inode ID.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `*loonfs.PinID` 

A snapshot to read the file from. The download reads the revision that the snapshot
captured.
    
</dd>
</dl>

<dl>
<dd>

**startOffset:** `*int64` 

The first byte that the grant reads. It defaults to 0 and must be less than the file's size.
For a file of zero bytes, it must be 0.
    
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

Returns the retained revisions of a file inode. The file doesn't need a current path.
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**inodeID:** `string` — The file's inode ID.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `*int` — The maximum page size.
    
</dd>
</dl>

<dl>
<dd>

**cursor:** `*string` — The opaque cursor that the previous page returned.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Inodes.RevisionContent(NamespaceID, InodeID, RevisionNo) -> string</code></summary>
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
request := &loonfs.GetFileRevisionContentByInodeRequest{
    NamespaceID: "namespace_id",
    InodeID: "inode_id",
    RevisionNo: int64(1000000),
}
client.Inodes.RevisionContent(
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**inodeID:** `string` — The file's inode ID.
    
</dd>
</dl>

<dl>
<dd>

**revisionNo:** `loonfs.RevisionNo` — The revision number.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Inodes.CreateRevisionDownload(NamespaceID, InodeID, RevisionNo, request) -> *loonfs.CreateDownloadByInodeResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Authorizes a direct read of one retained inode revision. The capability reads exactly `[start_offset, size_bytes)`, as on the path route. The response doesn't include a path.
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
request := &loonfs.CreateRevisionDownloadByInodeRequest{
    NamespaceID: "namespace_id",
    InodeID: "ino_123",
    RevisionNo: int64(1000000),
}
client.Inodes.CreateRevisionDownload(
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**inodeID:** `string` — The file's inode ID.
    
</dd>
</dl>

<dl>
<dd>

**revisionNo:** `loonfs.RevisionNo` — The revision number.
    
</dd>
</dl>

<dl>
<dd>

**startOffset:** `*int64` 

The first byte that the grant reads. It defaults to 0 and must be less than the revision's
size. For a revision of zero bytes, it must be 0.
    
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

Lists live snapshots in snapshot ID order. The list leaves out deleted and expired snapshots.
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `*int` — The maximum page size.
    
</dd>
</dl>

<dl>
<dd>

**cursor:** `*string` — The opaque cursor that the previous page returned.
    
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

Creates a snapshot of the current namespace state. Every accepted call creates a new snapshot.
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**name:** `string` — The snapshot's label, which doesn't need to be unique.
    
</dd>
</dl>

<dl>
<dd>

**ttlMs:** `int64` — The snapshot's lifetime in milliseconds, counted from the server's current time.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Snapshots.Retrieve(NamespaceID, SnapshotID) -> *loonfs.Snapshot</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns one live snapshot, as the snapshot listing returns it. A missing ID, or the ID of a pin that isn't a snapshot, returns `snapshot_not_found`, and an expired snapshot returns `snapshot_gone`.
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
request := &loonfs.GetSnapshotRequest{
    NamespaceID: "namespace_id",
    SnapshotID: "pin_00000000000000000001-0000000000000002",
}
client.Snapshots.Retrieve(
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `loonfs.PinID` — The snapshot ID.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Snapshots.Delete(NamespaceID, SnapshotID) -> *loonfs.DeleteSnapshotResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Deletes a snapshot pin. A missing ID returns `snapshot_not_found`.
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
request := &loonfs.DeleteSnapshotRequest{
    NamespaceID: "namespace_id",
    SnapshotID: "pin_00000000000000000001-0000000000000002",
}
client.Snapshots.Delete(
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `loonfs.PinID` — The snapshot ID.
    
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
    SnapshotID: "pin_00000000000000000001-0000000000000002",
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `loonfs.PinID` — The snapshot ID.
    
</dd>
</dl>

<dl>
<dd>

**ttlMs:** `int64` — The new lifetime in milliseconds, counted from the server's current time.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## uploads
<details><summary><code>client.Uploads.Create(NamespaceID, request) -> *loonfs.UploadSession</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Starts an upload session for content that a later commit can reference. A service-proxied upload sends the bytes through the server, and a direct PUT upload returns a presigned upload capability.
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
    Body: &loonfs.CreateUploadBody{
        DirectMultipart: &loonfs.CreateUploadBodyDirectMultipart{},
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**request:** `*loonfs.CreateUploadBody` 
    
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

Returns an upload session. An open `direct_put` session includes a newly signed upload capability. A completed session includes a new content token, so the client can retry the commit without uploading the content again.
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**uploadID:** `string` — The upload session ID.
    
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

Aborts an open upload session and deletes the object that the session was writing. Repeating the request succeeds, but a completed session can't be aborted.
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**uploadID:** `string` — The upload session ID.
    
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

Completes an upload. The request mode must match the mode that started the session. A direct upload includes a content claim, and a multipart upload also includes the completed parts.
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
    Body: &loonfs.CompleteUploadBody{
        DirectMultipart: &loonfs.CompleteUploadBodyDirectMultipart{
            Content: &loonfs.UploadContentClaim{
                Checksum: &loonfs.Checksum{
                    Algorithm: loonfs.ChecksumAlgorithmCrc64Nvme,
                    Value: "value",
                },
                SizeBytes: int64(1000000),
            },
            Parts: []*loonfs.CompletedUploadPart{
                &loonfs.CompletedUploadPart{
                    Checksum: &loonfs.Checksum{
                        Algorithm: loonfs.ChecksumAlgorithmCrc64Nvme,
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**uploadID:** `string` — The upload session ID.
    
</dd>
</dl>

<dl>
<dd>

**request:** `*loonfs.CompleteUploadBody` 
    
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

Returns one short-lived upload capability for each requested part of an open `direct_multipart` session. Each capability is bound to the part's checksum. To retry a part, a client asks for its capability again.
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
                Algorithm: loonfs.ChecksumAlgorithmCrc64Nvme,
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**uploadID:** `string` — The upload session ID.
    
</dd>
</dl>

<dl>
<dd>

**parts:** `[]*loonfs.UploadPartChecksumClaim` — The parts to authorize. A repeated part number replaces the earlier upload of that part.
    
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

Lists existing pins in checkpoint ID order. An expired pin stays in the list until garbage collection deletes it, after its expiry plus the grace window. A cursor continues the listing against the current set of pins.
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `*int` — The maximum page size.
    
</dd>
</dl>

<dl>
<dd>

**cursor:** `*string` — The opaque cursor that the previous page returned.
    
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

Creates a user checkpoint record that pins the current namespace view. The server first folds any WAL tail after the current manifest. Every call creates a new record with a new ID, and the name is a label, not a key. The record keeps its manifest until the record is deleted, either explicitly or by garbage collection after the record expires and its grace window passes. Because of that, routine maintenance should fold the WAL instead of creating checkpoints. This is a maintenance operation, not a file change.
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**name:** `string` — A label to record on the checkpoint. Labels don't need to be unique.
    
</dd>
</dl>

<dl>
<dd>

**ttlMs:** `*int64` 

The checkpoint's lifetime in milliseconds. Omit it to keep the checkpoint until it's
deleted.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Maintenance.Checkpoints.Retrieve(NamespaceID, CheckpointID) -> *loonfs.Checkpoint</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns one existing pin, as the checkpoint listing returns it, including an expired pin that garbage collection hasn't deleted. A missing ID returns `checkpoint_not_found`.
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
request := &maintenance.GetCheckpointRequest{
    NamespaceID: "namespace_id",
    CheckpointID: "checkpoint_id",
}
client.Maintenance.Checkpoints.Retrieve(
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**checkpointID:** `string` — The checkpoint ID.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Maintenance.Checkpoints.Delete(NamespaceID, CheckpointID) -> *loonfs.DeleteCheckpointResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Deletes a user checkpoint pin. A missing ID returns `checkpoint_not_found`. Garbage collection can then reclaim the pin's unreferenced manifest and runs.
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
request := &maintenance.DeleteCheckpointRequest{
    NamespaceID: "namespace_id",
    CheckpointID: "checkpoint_id",
}
client.Maintenance.Checkpoints.Delete(
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

**namespaceID:** `string` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**checkpointID:** `string` — The checkpoint ID.
    
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

**namespaceID:** `string` — The namespace ID.
    
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

Reports whether the namespace's grep index is `disabled`, `backfilling`, or `active`, with build progress when it's available. A namespace that has never enabled the index is `disabled`. A deployment that doesn't maintain grep indexes returns `not_supported`, and this operation doesn't change the index.
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

**namespaceID:** `string` — The namespace ID.
    
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

Disables the namespace's grep index by publishing a manifest with the next number and no segment references. Index maintenance stops when a step reads the disabled manifest, and an explicit grep garbage collection run reclaims the segments later. Repeating the request has the same result. A deployment that doesn't maintain the grep index returns `not_supported`.
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

**namespaceID:** `string` — The namespace ID.
    
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

Enables the namespace's grep index. A deployment that runs maintenance starts the backfill on its next session tick and continues until the index is active. The response reports the index state read after the change. A new enable reports `backfilling`, with the sequence that its checkpoint captured, and an index that was already enabled reports its current status. Repeating the request has the same result. A deployment that doesn't maintain the grep index returns `not_supported`.
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

**namespaceID:** `string` — The namespace ID.
    
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

Runs one maintenance job for the namespace. The body's `kind` selects the job, which is one of `metadata`, `metadata_compaction`, `gc`, `grep_gc`, `retention`, or `recover_administrator`. The response includes the same `kind` and the job's result. A deleted namespace accepts only `gc` and `grep_gc`. A `grep_gc` job deletes aged, unreferenced grep index objects, and it requires `maintenance.grep.index`. A `gc` job reads the current manifest, lists the pins, and then sweeps every object family to the end. Each listing starts at the beginning, and the job doesn't keep a position to continue from. A `retention` job advances the retention floor to the folded manifest head, or to the one target that `to_seq` or `cutoff_at_ms` sets.
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

**namespaceID:** `string` — The namespace ID.
    
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

Checks that the configured object store supports the create-if-absent, compare-and-swap, visibility, listing, and ranged-read behavior that LoonFS depends on, and reports the result of each check. The probe runs only when a client calls this route, because it writes and deletes objects. It writes them all under a scratch prefix that isn't a durable object family, and its last check deletes them and confirms that the prefix is empty. If the store fails a check, the response is still 200, and it reports that check as `failed`. If the store declares that it doesn't support an optional capability, the response reports that check as `unsupported`, which isn't a failure. The probe doesn't control whether the deployment can serve presigned direct uploads. The endpoint allowlist controls that, because the probe tests only the server's own requests and never a presigned capability issued to a client.
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

