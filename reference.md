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
<details><summary><code>client.Namespaces.List() -> *loonfs.ListNamespacesResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Lists the store's namespaces in object-key order (namespace ID order, except that an ID sorts after the longer IDs that extend it with `-` or `.`), and returns for each one the object that `GET /v0/namespaces/{namespace_id}` returns. The list leaves out deleted namespaces and namespaces whose creation hasn't finished, so a page can have fewer namespaces than `limit`, even none, and still include `next_cursor`. Without `limit`, a page lists up to 100 namespaces.
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
request := &loonfs.ListNamespacesRequest{}
client.Namespaces.List(
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

**limit:** `*int` — The maximum number of namespace IDs the page lists from the store.
    
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

<details><summary><code>client.Namespaces.Create(request) -> *loonfs.NamespaceMetadata</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a new empty namespace. Without `namespace_id` in the request, LoonFS generates an ID. The response's `namespace_id` is the new namespace's ID.
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
request := &loonfs.CreateNamespaceRequest{}
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

**namespaceID:** `*loonfs.NamespaceID` 

The durable ID of the new namespace. Without this field, LoonFS generates an ID. The
response's `namespace_id` is the new namespace's ID.
    
</dd>
</dl>

<dl>
<dd>

**retentionWindowMs:** `*int64` 

The namespace's retention window, in milliseconds, which must be at least the grace window
(86400000, which is 24 hours). The reference server's maintenance sweep uses this window
for the namespace. Without this field, the namespace's window is the server's default
window, if the server has one, which the capability document reports as
`namespace.default_retention_window_ms`. A `retention_window` maintenance job can change
or clear the window later.
    
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
    NamespaceID: "demo",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**consistency:** `*loonfs.ReadConsistency` 

How recently the read confirms the namespace head. `eventual` (the default, unless the
server's `consistency` setting is `strong`) reads at the head that the server confirmed last
while that confirmation is younger than the maximum staleness, and `strong` confirms the
head before the read.
    
</dd>
</dl>

<dl>
<dd>

**maxStalenessMs:** `*int64` 

With `consistency=eventual`, how old the server's last confirmation of the head can be, in
milliseconds. The default is the server's `max_staleness_ms`.
    
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
    NamespaceID: "demo",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**expectedHeadSeq:** `*loonfs.CommitSeq` — The head sequence that the namespace must still have for the delete to succeed.
    
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

Creates a new namespace from the source namespace's current head or from an unexpired snapshot. Without `new_namespace_id` in the request, LoonFS generates an ID. The response's `namespace_id` is the new namespace's ID.
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
    NamespaceID: "demo",
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

**namespaceID:** `loonfs.NamespaceID` — The source namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**newNamespaceID:** `*loonfs.NamespaceID` 

The durable ID of the new namespace that the fork creates. Without this field, LoonFS
generates an ID. The response's `namespace_id` is the new namespace's ID.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `*loonfs.SnapshotID` — An unexpired snapshot to fork from. Without it, the fork starts from the current head.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Namespaces.NextMergeRange(NamespaceID) -> *loonfs.NextMergeRangeResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns where the fork's next merge into the namespace starts, without planning the merge: the fork's newest merge into the namespace, the fork sequence that the next merge starts after, the fork point, and the fork's head. When the namespace is a rebase's new fork for the fork, the range continues the new fork's replay commits. When a retention floor would refuse the merge with `below_retention_floor`, the response's `outcome` says which floor instead of failing.
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
request := &loonfs.GetNextMergeRangeRequest{
    NamespaceID: "demo",
    SourceNamespaceID: "demo",
}
client.Namespaces.NextMergeRange(
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

**namespaceID:** `loonfs.NamespaceID` — The namespace that the fork merges into.
    
</dd>
</dl>

<dl>
<dd>

**sourceNamespaceID:** `loonfs.NamespaceID` 

The fork whose next merge into the namespace the query reads. It has to be a fork of the
namespace, or the fork that a rebase replays into the namespace.
    
</dd>
</dl>

<dl>
<dd>

**consistency:** `*loonfs.ReadConsistency` 

How recently the read confirms the heads of the namespace and the fork. `eventual` (the
default, unless the server's `consistency` setting is `strong`) reads at the heads that the
server confirmed last while those confirmations are younger than the maximum staleness, and
`strong` confirms both heads before the read.
    
</dd>
</dl>

<dl>
<dd>

**maxStalenessMs:** `*int64` 

With `consistency=eventual`, how old the server's last confirmation of a head can be, in
milliseconds. The default is the server's `max_staleness_ms`.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Namespaces.Merge(NamespaceID, request) -> *loonfs.Commit</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Merges a fork's commits into the namespace as one commit. The merge applies the fork's net change since its previous merge, or since the fork point, through `through_seq` or the fork's head. A change that conflicts with the namespace returns `merge_conflict`, and nothing is committed. The commit ID makes retries idempotent. The merge's WAL object counts toward the namespace's fold triggers like any commit.
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
request := &loonfs.MergeForkRequest{
    NamespaceID: "demo",
    CommitID: "cmt_7kn71d9dq885x91hejz3gvf9p0",
    SourceNamespaceID: "demo",
}
client.Namespaces.Merge(
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

**namespaceID:** `loonfs.NamespaceID` — The namespace that the merge commits into.
    
</dd>
</dl>

<dl>
<dd>

**commitID:** `loonfs.CommitID` — The caller's idempotency key for the merge.
    
</dd>
</dl>

<dl>
<dd>

**message:** `*string` — An annotation from the caller. The message is part of the merge's identity.
    
</dd>
</dl>

<dl>
<dd>

**sourceNamespaceID:** `loonfs.NamespaceID` 

The fork whose commits the merge applies. It has to be a fork of the namespace that the
request merges into.
    
</dd>
</dl>

<dl>
<dd>

**throughSeq:** `*loonfs.CommitSeq` 

The fork sequence that the merge applies the fork's commits through. Without it, the merge
applies them through the fork's head.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Namespaces.ForkForRebase(NamespaceID, request) -> *loonfs.NamespaceMetadata</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a rebase's new fork from the namespace's head, for a fork of the namespace whose unmerged commits the replay route then replays into the new fork. Without `new_namespace_id` in the request, LoonFS generates an ID. The response's `namespace_id` is the new fork's ID, and its `fork_basis.rebased_from` identifies the fork. A request whose new fork already exists returns `namespace_exists`.
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
request := &loonfs.ForkNamespaceForRebaseRequest{
    NamespaceID: "demo",
    SourceNamespaceID: "demo",
}
client.Namespaces.ForkForRebase(
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

**namespaceID:** `loonfs.NamespaceID` — The namespace that the fork was forked from.
    
</dd>
</dl>

<dl>
<dd>

**newNamespaceID:** `*loonfs.NamespaceID` 

The durable ID of the new fork. Without this field, LoonFS generates an ID. The response's
`namespace_id` is the new fork's ID.
    
</dd>
</dl>

<dl>
<dd>

**sourceNamespaceID:** `loonfs.NamespaceID` 

The fork whose unmerged commits the new fork's replay commits replay. It has to be a fork
of the namespace that the request identifies in its path.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.Namespaces.Replay(NamespaceID, request) -> *loonfs.ReplayForkResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Replays a fork's unmerged commits into the namespace, a rebase's new fork for the fork, as one replay commit, which applies the changes that don't conflict with the new fork and reports the ones that do. Each request continues after the new fork's previous replay commit. The commit ID makes retries idempotent, and a retry of a committed replay commit returns the new fork with the same conflict report. The report counts toward the replay commit's size, so a report that doesn't fit returns `merge_too_large` with a split hint.
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
request := &loonfs.ReplayForkRequest{
    NamespaceID: "demo",
    CommitID: "cmt_7kn71d9dq885x91hejz3gvf9p0",
    SourceNamespaceID: "demo",
}
client.Namespaces.Replay(
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

**namespaceID:** `loonfs.NamespaceID` — The rebase's new fork, which the replay commit commits into.
    
</dd>
</dl>

<dl>
<dd>

**commitID:** `loonfs.CommitID` — The caller's idempotency key for the replay commit.
    
</dd>
</dl>

<dl>
<dd>

**message:** `*string` — An annotation from the caller. The message is part of the replay commit's identity.
    
</dd>
</dl>

<dl>
<dd>

**sourceNamespaceID:** `loonfs.NamespaceID` 

The fork whose unmerged commits the replay commit replays. The namespace that the request
identifies in its path has to be a rebase's new fork for this fork.
    
</dd>
</dl>

<dl>
<dd>

**throughSeq:** `*loonfs.CommitSeq` 

The fork sequence that the replay commit replays the fork's commits through. Without it,
the replay commit replays them through the fork's head.
    
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
    NamespaceID: "demo",
    AfterSeq: int64(1000000),
    SnapshotID: loonfs.String(
        "snp_0000000000000tfy68ekkbj6bx",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**afterSeq:** `loonfs.CommitSeq` 

The sequence after which to return committed changes. With `event_index`, the commit at
this sequence that the page resumes.
    
</dd>
</dl>

<dl>
<dd>

**splitCommits:** `*bool` 

Whether a page can end inside a commit that alone exceeds the page's budget. The default is
`false`, which returns every commit whole.
    
</dd>
</dl>

<dl>
<dd>

**eventIndex:** `*int` 

The `next_event_index` that the previous page returned, which resumes the commit at
`after_seq` at that event. It requires `split_commits=true`. The default is 0, which starts
after the commit at `after_seq`.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `*int` — The maximum page size.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `*loonfs.SnapshotID` — A snapshot to read the feed from. The feed ends at the sequence that the snapshot captured.
    
</dd>
</dl>

<dl>
<dd>

**consistency:** `*loonfs.ReadConsistency` 

How recently the read confirms the namespace head. `eventual` (the default, unless the
server's `consistency` setting is `strong`) reads at the head that the server confirmed last
while that confirmation is younger than the maximum staleness, and `strong` confirms the
head before the read. It can't be combined with `snapshot_id`.
    
</dd>
</dl>

<dl>
<dd>

**maxStalenessMs:** `*int64` 

With `consistency=eventual`, how old the server's last confirmation of the head can be, in
milliseconds. The default is the server's `max_staleness_ms`.
    
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
    NamespaceID: "demo",
    CommitID: "cmt_7kn71d9dq885x91hejz3gvf9p0",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
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

## Diff
<details><summary><code>client.Diff.List(NamespaceID) -> *loonfs.ListDiffResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a page of the inodes whose visible state at `to_seq` differs from their state at `from_seq`, read from the commits between the two sequences.
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
request := &loonfs.ListDiffRequest{
    NamespaceID: "demo",
    FromSeq: int64(1000000),
}
client.Diff.List(
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**fromSeq:** `loonfs.CommitSeq` 

The sequence that the diff starts after. Each entry compares an inode's state at this
sequence with its state at `to_seq`. It can't be below the namespace's retention floor.
    
</dd>
</dl>

<dl>
<dd>

**toSeq:** `*loonfs.CommitSeq` 

The sequence that the diff ends at, inclusive. The default is the head that the request
reads. A later page passes the `to_seq` that the first page returned.
    
</dd>
</dl>

<dl>
<dd>

**path:** `*string` — The absolute path of the directory whose subtree the diff reports. The default is the root.
    
</dd>
</dl>

<dl>
<dd>

**afterSeq:** `*loonfs.CommitSeq` — The `next_after_seq` that the previous page returned. The default is `from_seq`.
    
</dd>
</dl>

<dl>
<dd>

**splitCommits:** `*bool` 

Whether a page can end inside a commit that alone touches more inodes than one page
examines. The default is `false`, which reads every commit whole.
    
</dd>
</dl>

<dl>
<dd>

**eventIndex:** `*int` 

The `next_event_index` that the previous page returned, which resumes the commit at
`after_seq` at that event. It requires `split_commits=true`. The default is 0.
    
</dd>
</dl>

<dl>
<dd>

**limit:** `*int` — The maximum number of commits that the page reads.
    
</dd>
</dl>

<dl>
<dd>

**consistency:** `*loonfs.ReadConsistency` 

How recently the read confirms the namespace head. `eventual` (the default, unless the
server's `consistency` setting is `strong`) reads at the head that the server confirmed last
while that confirmation is younger than the maximum staleness, and `strong` confirms the
head before the read.
    
</dd>
</dl>

<dl>
<dd>

**maxStalenessMs:** `*int64` 

With `consistency=eventual`, how old the server's last confirmation of the head can be, in
milliseconds. The default is the server's `max_staleness_ms`.
    
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

Returns the current file bytes, a retained revision, or the revision captured by an unexpired snapshot. A `Range` header asks for one byte range of the file instead.
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
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

**snapshotID:** `*loonfs.SnapshotID` — The snapshot whose captured file revision to read.
    
</dd>
</dl>

<dl>
<dd>

**consistency:** `*loonfs.ReadConsistency` 

How recently the read confirms the namespace head. `eventual` (the default, unless the
server's `consistency` setting is `strong`) reads at the head that the server confirmed last
while that confirmation is younger than the maximum staleness, and `strong` confirms the
head before the read. It can't be combined with `snapshot_id`.
    
</dd>
</dl>

<dl>
<dd>

**maxStalenessMs:** `*int64` 

With `consistency=eventual`, how old the server's last confirmation of the head can be, in
milliseconds. The default is the server's `max_staleness_ms`.
    
</dd>
</dl>

<dl>
<dd>

**range_:** `*string` — One byte range of the file to read instead of the whole file, as `bytes=start-end`, where `end` is the offset of the last byte, or as `bytes=start-`. An `end` past the last byte reads to the end of the file. Any other value, including a suffix range or several ranges, returns 400 `invalid_request`.
    
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

Returns ordered, signed object ranges that cover exactly `[start_offset, size_bytes)` of the selected revision. Each range includes its signed headers in `transfer.headers`, and the client sends them unchanged. The client checks each range's length and the complete revision's checksum. To resume a download, a client asks for a new download from its current offset. A deployment that can't presign URLs returns 501 `not_supported`. The proxied `GET /filesystem/content` route is still available, up to the `download.service_proxied.max_content_bytes` limit.
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
    NamespaceID: "demo",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**consistency:** `*loonfs.ReadConsistency` 

How recently the read confirms the namespace head. `eventual` (the default, unless the
server's `consistency` setting is `strong`) reads at the head that the server confirmed last
while that confirmation is younger than the maximum staleness, and `strong` confirms the
head before the read. It can't be combined with `snapshot_id`.
    
</dd>
</dl>

<dl>
<dd>

**maxStalenessMs:** `*int64` 

With `consistency` set to `eventual`, how old the server's last confirmation of the head can
be, in milliseconds. The default is the server's `max_staleness_ms`.
    
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

**snapshotID:** `*loonfs.SnapshotID` 

A snapshot to read the file from. The download reads the revision that the snapshot
captured. It can't be combined with `revision_no`.
    
</dd>
</dl>

<dl>
<dd>

**startOffset:** `*int64` 

The first byte that the download reads. The download covers the revision's bytes from
`start_offset` up to `size_bytes`, so a client that resumes a download asks for a new download
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

Lists a directory from the current state or an unexpired snapshot.
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
    NamespaceID: "demo",
    Path: "path",
    SnapshotID: loonfs.String(
        "snp_0000000000000tfy68ekkbj6bx",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
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

**includeAttributes:** `*bool` 

Whether the response includes each entry's attribute map and attribute revision. The default
is `false`, because a page can have many entries and each map can be up to 64 KiB.
    
</dd>
</dl>

<dl>
<dd>

**recursive:** `*bool` 

Whether the listing includes the directory's whole subtree, depth first, with each
directory's entry before its descendants. The default is `false`, which lists the
directory's children only.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `*loonfs.SnapshotID` — The snapshot whose captured directory state to read.
    
</dd>
</dl>

<dl>
<dd>

**consistency:** `*loonfs.ReadConsistency` 

How recently the read confirms the namespace head. `eventual` (the default, unless the
server's `consistency` setting is `strong`) reads at the head that the server confirmed last
while that confirmation is younger than the maximum staleness, and `strong` confirms the
head before the read. It can't be combined with `snapshot_id`.
    
</dd>
</dl>

<dl>
<dd>

**maxStalenessMs:** `*int64` 

With `consistency=eventual`, how old the server's last confirmation of the head can be, in
milliseconds. The default is the server's `max_staleness_ms`.
    
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

Returns path metadata from the current state or an unexpired snapshot.
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
    NamespaceID: "demo",
    Path: "path",
    SnapshotID: loonfs.String(
        "snp_0000000000000tfy68ekkbj6bx",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**path:** `string` — The absolute path to read.
    
</dd>
</dl>

<dl>
<dd>

**includeAttributes:** `*bool` 

Whether the response includes the inode's attribute map and attribute revision. The default
is `false`, as on every route that takes this parameter.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `*loonfs.SnapshotID` — The snapshot whose captured path state to read.
    
</dd>
</dl>

<dl>
<dd>

**consistency:** `*loonfs.ReadConsistency` 

How recently the read confirms the namespace head. `eventual` (the default, unless the
server's `consistency` setting is `strong`) reads at the head that the server confirmed last
while that confirmation is younger than the maximum staleness, and `strong` confirms the
head before the read. It can't be combined with `snapshot_id`.
    
</dd>
</dl>

<dl>
<dd>

**maxStalenessMs:** `*int64` 

With `consistency=eventual`, how old the server's last confirmation of the head can be, in
milliseconds. The default is the server's `max_staleness_ms`.
    
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
    NamespaceID: "demo",
    Path: "path",
    SnapshotID: loonfs.String(
        "snp_0000000000000tfy68ekkbj6bx",
    ),
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
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

<dl>
<dd>

**snapshotID:** `*loonfs.SnapshotID` 

The snapshot whose state to list the file's revisions in. The listing starts at the
revision that the snapshot captured.
    
</dd>
</dl>

<dl>
<dd>

**consistency:** `*loonfs.ReadConsistency` 

How recently the read confirms the namespace head. `eventual` (the default, unless the
server's `consistency` setting is `strong`) reads at the head that the server confirmed last
while that confirmation is younger than the maximum staleness, and `strong` confirms the
head before the read. It can't be combined with `snapshot_id`.
    
</dd>
</dl>

<dl>
<dd>

**maxStalenessMs:** `*int64` 

With `consistency=eventual`, how old the server's last confirmation of the head can be, in
milliseconds. The default is the server's `max_staleness_ms`.
    
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

Searches file content with a regular expression, using the namespace's grep index to narrow the candidates. The server checks each candidate against the full pattern and returns matches ordered by the content value of each file's revision, then by `inode_id`, and then by `byte_offset`. It scans every revision committed after the index watermark, unless `allow_stale` skips them. A first page reads the namespace at its head, and a later page reads at the first page's head. The deployment must serve grep, and the namespace must have a materialized, active grep index.
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
    NamespaceID: "demo",
    Pattern: "pattern",
    InodeID: loonfs.String(
        "ino_000000r0000013kje0d47gc8kw",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**pattern:** `string` 

The search pattern, in the Rust `regex` crate's syntax. Its UTF-8 encoding must be at most
1024 bytes.
    
</dd>
</dl>

<dl>
<dd>

**caseInsensitive:** `*bool` — Whether matching ignores case. The default is `false`.
    
</dd>
</dl>

<dl>
<dd>

**path:** `*string` 

A complete absolute path that limits matches to its descendants. It can't be combined with
`inode_id`.
    
</dd>
</dl>

<dl>
<dd>

**inodeID:** `*string` — An inode that limits matches to its descendants. It can't be combined with `path`.
    
</dd>
</dl>

<dl>
<dd>

**allowScan:** `*bool` 

Whether a pattern with no required literal bytes can use a capped exhaustive scan. The
default is `false`.
    
</dd>
</dl>

<dl>
<dd>

**allowStale:** `*bool` 

Whether the server returns results from the index alone when the unindexed tail exceeds the
scan budget. The default is `false`.
    
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

<dl>
<dd>

**consistency:** `*loonfs.ReadConsistency` 

How recently a first page confirms the namespace head that it reads. `eventual` (the
default, unless the server's `consistency` setting is `strong`) reads at the head that the
server confirmed last while that confirmation is younger than the maximum staleness, and
`strong` confirms the head before the read. A later page reads at the first page's head.
    
</dd>
</dl>

<dl>
<dd>

**maxStalenessMs:** `*int64` 

With `consistency=eventual`, how old the server's last confirmation of the head can be, in
milliseconds. The default is the server's `max_staleness_ms`.
    
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

Returns the namespace's trash entries, oldest deletion first. An entry leaves the trash when it's restored, or when the namespace's retention floor reaches its `deletion_seq`. Each entry includes the inode ID and deletion sequence that a restore needs, and the directory binding that the delete removed.
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
    NamespaceID: "demo",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
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

**consistency:** `*loonfs.ReadConsistency` 

How recently the read confirms the namespace head. `eventual` (the default, unless the
server's `consistency` setting is `strong`) reads at the head that the server confirmed last
while that confirmation is younger than the maximum staleness, and `strong` confirms the
head before the read.
    
</dd>
</dl>

<dl>
<dd>

**maxStalenessMs:** `*int64` 

With `consistency=eventual`, how old the server's last confirmation of the head can be, in
milliseconds. The default is the server's `max_staleness_ms`.
    
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

Returns the path entry for a visible inode, from the current state or an unexpired snapshot. An unknown or hidden inode returns `inode_not_found`.
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
    NamespaceID: "demo",
    InodeID: "ino_000000r0000013kje0d47gc8kw",
    SnapshotID: loonfs.String(
        "snp_0000000000000tfy68ekkbj6bx",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**inodeID:** `loonfs.InodeID` — The inode ID.
    
</dd>
</dl>

<dl>
<dd>

**includeAttributes:** `*bool` 

Whether the response includes the inode's attribute map and attribute revision. The default
is `false`, as on every route that takes this parameter.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `*loonfs.SnapshotID` — The snapshot whose captured path state to read.
    
</dd>
</dl>

<dl>
<dd>

**consistency:** `*loonfs.ReadConsistency` 

How recently the read confirms the namespace head. `eventual` (the default, unless the
server's `consistency` setting is `strong`) reads at the head that the server confirmed last
while that confirmation is younger than the maximum staleness, and `strong` confirms the
head before the read. It can't be combined with `snapshot_id`.
    
</dd>
</dl>

<dl>
<dd>

**maxStalenessMs:** `*int64` 

With `consistency=eventual`, how old the server's last confirmation of the head can be, in
milliseconds. The default is the server's `max_staleness_ms`.
    
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

Lists one page of a directory's children, in canonical name key order, from the current state or an unexpired snapshot. The request identifies the directory by its inode ID, so a listing and its later pages stay on the same directory even if the directory is renamed or moved at the same time.
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
    NamespaceID: "demo",
    InodeID: "ino_000000r0000013kje0d47gc8kw",
    SnapshotID: loonfs.String(
        "snp_0000000000000tfy68ekkbj6bx",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**inodeID:** `loonfs.InodeID` — The directory's inode ID.
    
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

**includeAttributes:** `*bool` 

Whether the response includes each entry's attribute map and attribute revision. The default
is `false`, because a page can have many entries and each map can be up to 64 KiB.
    
</dd>
</dl>

<dl>
<dd>

**recursive:** `*bool` 

Whether the listing includes the directory's whole subtree, depth first, with each
directory's entry before its descendants. The default is `false`, which lists the
directory's children only.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `*loonfs.SnapshotID` — The snapshot whose captured directory state to read.
    
</dd>
</dl>

<dl>
<dd>

**consistency:** `*loonfs.ReadConsistency` 

How recently the read confirms the namespace head. `eventual` (the default, unless the
server's `consistency` setting is `strong`) reads at the head that the server confirmed last
while that confirmation is younger than the maximum staleness, and `strong` confirms the
head before the read. It can't be combined with `snapshot_id`.
    
</dd>
</dl>

<dl>
<dd>

**maxStalenessMs:** `*int64` 

With `consistency=eventual`, how old the server's last confirmation of the head can be, in
milliseconds. The default is the server's `max_staleness_ms`.
    
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

Reads and verifies the current revision of a visible file inode, wherever the inode is bound, or the revision that an unexpired snapshot captured. A `Range` header asks for one byte range of the file instead. An unknown or hidden inode returns `inode_not_found`.
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**inodeID:** `loonfs.InodeID` — The file's inode ID.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `*loonfs.SnapshotID` — The snapshot whose captured file revision to read.
    
</dd>
</dl>

<dl>
<dd>

**consistency:** `*loonfs.ReadConsistency` 

How recently the read confirms the namespace head. `eventual` (the default, unless the
server's `consistency` setting is `strong`) reads at the head that the server confirmed last
while that confirmation is younger than the maximum staleness, and `strong` confirms the
head before the read. It can't be combined with `snapshot_id`.
    
</dd>
</dl>

<dl>
<dd>

**maxStalenessMs:** `*int64` 

With `consistency=eventual`, how old the server's last confirmation of the head can be, in
milliseconds. The default is the server's `max_staleness_ms`.
    
</dd>
</dl>

<dl>
<dd>

**range_:** `*string` — One byte range of the file to read instead of the whole file, as `bytes=start-end`, where `end` is the offset of the last byte, or as `bytes=start-`. An `end` past the last byte reads to the end of the file. Any other value, including a suffix range or several ranges, returns 400 `invalid_request`.
    
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

Authorizes a direct read of the current revision of a visible file inode, wherever the inode is bound, or of the revision that an unexpired snapshot captured. The presigned URLs read exactly `[start_offset, size_bytes)`, as on the path route. The response doesn't include a path.
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
    NamespaceID: "demo",
    InodeID: "ino_000000r0000013kje0d47gc8kw",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**inodeID:** `loonfs.InodeID` — The file's inode ID.
    
</dd>
</dl>

<dl>
<dd>

**consistency:** `*loonfs.ReadConsistency` 

How recently the read confirms the namespace head. `eventual` (the default, unless the
server's `consistency` setting is `strong`) reads at the head that the server confirmed last
while that confirmation is younger than the maximum staleness, and `strong` confirms the
head before the read. It can't be combined with `snapshot_id`.
    
</dd>
</dl>

<dl>
<dd>

**maxStalenessMs:** `*int64` 

With `consistency` set to `eventual`, how old the server's last confirmation of the head can
be, in milliseconds. The default is the server's `max_staleness_ms`.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `*loonfs.SnapshotID` 

A snapshot to read the file from. The download reads the revision that the snapshot
captured.
    
</dd>
</dl>

<dl>
<dd>

**startOffset:** `*int64` 

The first byte that the download reads. It defaults to 0 and must be less than the file's size.
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
    NamespaceID: "demo",
    InodeID: "ino_000000r0000013kje0d47gc8kw",
    SnapshotID: loonfs.String(
        "snp_0000000000000tfy68ekkbj6bx",
    ),
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**inodeID:** `loonfs.InodeID` — The file's inode ID.
    
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

**snapshotID:** `*loonfs.SnapshotID` 

The snapshot whose state to list the file's revisions in. The listing starts at the
revision that the snapshot captured.
    
</dd>
</dl>

<dl>
<dd>

**consistency:** `*loonfs.ReadConsistency` 

How recently the read confirms the namespace head. `eventual` (the default, unless the
server's `consistency` setting is `strong`) reads at the head that the server confirmed last
while that confirmation is younger than the maximum staleness, and `strong` confirms the
head before the read. It can't be combined with `snapshot_id`.
    
</dd>
</dl>

<dl>
<dd>

**maxStalenessMs:** `*int64` 

With `consistency=eventual`, how old the server's last confirmation of the head can be, in
milliseconds. The default is the server's `max_staleness_ms`.
    
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

Reads and verifies one retained file revision by inode ID and revision number, at the live head or in the state that a snapshot or a fork snapshot keeps. A `Range` header asks for one byte range of the revision instead.
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**inodeID:** `loonfs.InodeID` — The file's inode ID.
    
</dd>
</dl>

<dl>
<dd>

**revisionNo:** `loonfs.RevisionNo` — The revision number.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `*loonfs.SnapshotID` 

A snapshot, or one of the namespace's fork snapshots, whose state to find the revision
in. The snapshot's manifest keeps the revision after the namespace's retention floor passes it.
    
</dd>
</dl>

<dl>
<dd>

**consistency:** `*loonfs.ReadConsistency` 

How recently the read confirms the namespace head that it finds the revision in. `eventual`
(the default, unless the server's `consistency` setting is `strong`) reads at the head that
the server confirmed last while that confirmation is younger than the maximum staleness, and
`strong` confirms the head before the read. It can't be combined with `snapshot_id`.
    
</dd>
</dl>

<dl>
<dd>

**maxStalenessMs:** `*int64` 

With `consistency=eventual`, how old the server's last confirmation of the head can be, in
milliseconds. The default is the server's `max_staleness_ms`.
    
</dd>
</dl>

<dl>
<dd>

**range_:** `*string` — One byte range of the file to read instead of the whole file, as `bytes=start-end`, where `end` is the offset of the last byte, or as `bytes=start-`. An `end` past the last byte reads to the end of the file. Any other value, including a suffix range or several ranges, returns 400 `invalid_request`.
    
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

Authorizes a direct read of one retained inode revision. With `snapshot_id`, the server finds the revision in the snapshot instead of at the live head. The presigned URLs read exactly `[start_offset, size_bytes)`, as on the path route. The response doesn't include a path.
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
    NamespaceID: "demo",
    InodeID: "ino_000000r0000013kje0d47gc8kw",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**inodeID:** `loonfs.InodeID` — The file's inode ID.
    
</dd>
</dl>

<dl>
<dd>

**revisionNo:** `loonfs.RevisionNo` — The revision number.
    
</dd>
</dl>

<dl>
<dd>

**consistency:** `*loonfs.ReadConsistency` 

How recently the read confirms the namespace head that it finds the revision in. `eventual`
(the default, unless the server's `consistency` setting is `strong`) reads at the head that
the server confirmed last while that confirmation is younger than the maximum staleness,
and `strong` confirms the head before the read. It can't be combined with `snapshot_id`.
    
</dd>
</dl>

<dl>
<dd>

**maxStalenessMs:** `*int64` 

With `consistency` set to `eventual`, how old the server's last confirmation of the head can
be, in milliseconds. The default is the server's `max_staleness_ms`.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `*loonfs.SnapshotID` 

A snapshot to find the revision in instead of the live head. The snapshot keeps the
revision readable after the namespace's retention floor passes it, and a revision committed
after the snapshot returns `revision_not_found`.
    
</dd>
</dl>

<dl>
<dd>

**startOffset:** `*int64` 

The first byte that the download reads. It defaults to 0 and must be less than the revision's
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

Lists snapshots in snapshot ID order. The list leaves out expired snapshots, deleted snapshots, and fork snapshots, which the maintenance snapshot inventory lists. A deleted namespace returns `namespace_deleted`.
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
    NamespaceID: "demo",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
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

Creates a snapshot of the current namespace state. The server first folds any WAL tail after the current manifest. Every accepted call creates a new snapshot with a new ID, and the name is a label, not a key. A snapshot with `ttl_ms` expires that many milliseconds from the server's current time, and a snapshot without it doesn't expire. The snapshot keeps its manifest until garbage collection deletes it, after it expires or is deleted and its grace window passes.
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
    NamespaceID: "demo",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**name:** `*string` 

The snapshot's label, which doesn't need to be unique. Omit the field for a snapshot with no
label.
    
</dd>
</dl>

<dl>
<dd>

**ttlMs:** `*int64` 

The snapshot's lifetime in milliseconds, counted from the server's current time. Omit it to
keep the snapshot until it's deleted.
    
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

Returns one snapshot, as the snapshot listing returns it. A missing ID, the ID of an expired or deleted snapshot, or the ID of a fork snapshot returns `snapshot_not_found`. A deleted namespace returns `namespace_deleted`.
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
    NamespaceID: "demo",
    SnapshotID: "snp_0000000000000tfy68ekkbj6bx",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `loonfs.SnapshotID` — The snapshot ID.
    
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

Deletes a snapshot by recording the server's current time as its delete time. From then on, the snapshot can't be read, forked, extended, or retrieved, and the snapshot listing leaves it out. Garbage collection can then reclaim its unreferenced manifest and runs after the grace window. Deleting a missing or deleted snapshot succeeds without a write, so repeating the request has the same result. The ID of a fork snapshot returns `snapshot_not_found`. Deletion stays available after the namespace is deleted, except to a request with principals, which returns `namespace_deleted`.
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
    NamespaceID: "demo",
    SnapshotID: "snp_0000000000000tfy68ekkbj6bx",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `loonfs.SnapshotID` — The snapshot ID.
    
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

Moves an unexpired snapshot's expiry to `ttl_ms` from the server's current time. The expiry never moves earlier, so a snapshot without an expiry is returned unchanged. A missing, deleted, or expired snapshot returns `snapshot_not_found`. Repeating the request has the same result.
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
    NamespaceID: "demo",
    SnapshotID: "snp_0000000000000tfy68ekkbj6bx",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `loonfs.SnapshotID` — The snapshot ID.
    
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

Starts an upload session for content that a later commit can reference. A service-proxied upload sends the bytes through the server, and a direct PUT upload returns a presigned URL for the upload.
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
    NamespaceID: "demo",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
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

Returns an upload session. An open `direct_put` session includes a new presigned URL for the upload. A completed session includes a new content token, so the client can retry the commit without uploading the content again.
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
    NamespaceID: "demo",
    UploadID: "upl_2dhwp939xk9r7srv8t5cz5yw4c",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**uploadID:** `loonfs.UploadID` — The upload session ID.
    
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
    NamespaceID: "demo",
    UploadID: "upl_2dhwp939xk9r7srv8t5cz5yw4c",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**uploadID:** `loonfs.UploadID` — The upload session ID.
    
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
    NamespaceID: "demo",
    UploadID: "upl_2dhwp939xk9r7srv8t5cz5yw4c",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**uploadID:** `loonfs.UploadID` — The upload session ID.
    
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

Returns one short-lived presigned URL for each requested part of an open `direct_multipart` session. Each presigned URL is bound to the part's checksum. To retry a part, a client asks for its presigned URL again.
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
    NamespaceID: "demo",
    UploadID: "upl_2dhwp939xk9r7srv8t5cz5yw4c",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**uploadID:** `loonfs.UploadID` — The upload session ID.
    
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
    NamespaceID: "demo",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
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
    NamespaceID: "demo",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
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

Disables the namespace's grep index by publishing a namespace manifest whose grep entry lists no segments. Index maintenance stops when a step reads the disabled entry, and garbage collection reclaims the segments later. Repeating the request has the same result. If the grep entry is corrupt, the request replaces it with a disabled entry, so the index can be enabled again. If the entry has another version of the grep format, the request returns `index_corrupt` and leaves the entry unchanged. A deployment that doesn't maintain the grep index returns `not_supported`.
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
    NamespaceID: "demo",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
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

Enables the namespace's grep index. A deployment that runs maintenance starts the backfill at the next maintenance interval and continues until the index is active. The response reports the index state read after the change. A new enable reports `backfilling`, with the sequence whose files the backfill reads, and an index that was already enabled reports its current status. Repeating the request has the same result. A deployment that doesn't maintain the grep index returns `not_supported`.
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
    NamespaceID: "demo",
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Maintenance Jobs
<details><summary><code>client.Maintenance.Jobs.Create(NamespaceID, request) -> *loonfs.RunMaintenanceJobResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Runs one maintenance job for the namespace. The body's `kind` selects the job, which is one of `metadata`, `gc`, `retention`, or `retention_window`. The response includes the same `kind` and the job's result. A deleted namespace accepts only `gc`. A `metadata` job folds the WAL tail and compacts metadata until nothing is due, or until it publishes 16 compaction units. A compaction unit can take minutes, so the call can too. When the result's `outcome` is `due`, another `metadata` run continues the work. A `gc` job reads the current manifest, lists the snapshots, and then sweeps every object family under the namespace's prefix to the end, including the segments of extensions such as the grep index. The collection of the namespace's pool, which deletes content objects, upload records, and temporary objects, is a job of the pool. Each listing starts at the beginning, and the job doesn't keep a position to continue from. A `retention` job advances the retention floor to the folded manifest head, or to the one target that `to_seq` or `cutoff_at_ms` sets. A `retention_window` job sets the namespace's retention window to `retention_window_ms`, or clears it when the body doesn't include that field.
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
request := &maintenance.CreateJobsRequest{
    NamespaceID: "demo",
    Body: &loonfs.RunMaintenanceJobRequest{
        Gc: &loonfs.RunMaintenanceJobRequestGc{},
    },
}
client.Maintenance.Jobs.Create(
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**request:** `*loonfs.RunMaintenanceJobRequest` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Maintenance Snapshots
<details><summary><code>client.Maintenance.Snapshots.List(NamespaceID) -> *loonfs.ListSnapshotRecordsResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Lists every snapshot, user snapshots and fork snapshots, in snapshot ID order. This is the inventory of what keeps manifests from garbage collection. An expired snapshot stays in the list until garbage collection deletes it, after its expiry plus the grace window, and a deleted snapshot stays, with its `deleted_at_ms`, until its delete time plus the grace window. A cursor continues the listing against the current set of snapshots.
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
request := &maintenance.ListSnapshotRecordsRequest{
    NamespaceID: "demo",
}
client.Maintenance.Snapshots.List(
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
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

<details><summary><code>client.Maintenance.Snapshots.Retrieve(NamespaceID, SnapshotID) -> *loonfs.SnapshotRecord</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns one existing snapshot of any owner, as the snapshot inventory lists it, including an expired or deleted snapshot that garbage collection hasn't deleted. A missing ID returns `snapshot_not_found`.
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
request := &maintenance.GetSnapshotRecordRequest{
    NamespaceID: "demo",
    SnapshotID: "snp_0000000000000tfy68ekkbj6bx",
}
client.Maintenance.Snapshots.Retrieve(
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

**namespaceID:** `loonfs.NamespaceID` — The namespace ID.
    
</dd>
</dl>

<dl>
<dd>

**snapshotID:** `loonfs.SnapshotID` — The snapshot ID.
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Maintenance Store
<details><summary><code>client.Maintenance.Store.Probe(request) -> *loonfs.ProbeStoreResponse</code></summary>
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

**request:** `loonfs.ProbeStoreRequest` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Maintenance Pools Jobs
<details><summary><code>client.Maintenance.Pools.Jobs.Create(PoolID, request) -> *loonfs.RunPoolMaintenanceJobResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Runs one maintenance job for the pool. The body's `kind` selects the job, which is `gc`. The response includes the same `kind` and the job's result. A `gc` job lists the pool's upload sessions and member entries, reads the roots of every member of the pool, and then sweeps the pool's content objects, upload records, and temporary objects to the end. Each listing starts at the beginning, and the job doesn't keep a position to continue from.
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
request := &pools.CreateJobsRequest{
    PoolID: "pool_3s0drmj8em84yb4mnetqkz2j0x",
    Body: &loonfs.RunPoolMaintenanceJobRequest{
        Gc: &loonfs.RunPoolMaintenanceJobRequestGc{},
    },
}
client.Maintenance.Pools.Jobs.Create(
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

**poolID:** `loonfs.PoolID` — The pool ID.
    
</dd>
</dl>

<dl>
<dd>

**request:** `*loonfs.RunPoolMaintenanceJobRequest` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

