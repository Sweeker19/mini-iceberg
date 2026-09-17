# Mini Iceberg

An educational, from-scratch implementation of the core ideas behind an Apache Iceberg-style table format, built in Python.

This project is intentionally small and inspectable. Its purpose is to understand how modern data-lake tables provide database-like features such as atomic commits, consistent snapshots, metadata-driven reads, and time travel—while storing data as files.

> This is a learning project and is **not** compatible with Apache Iceberg, Spark, Trino, Flink, or any production Iceberg catalog.

## Why am I building this?

A directory of data files is not automatically a reliable database table.

Real data-lake systems need to answer questions such as:

- Which data files belong to the table right now?
- How can a reader avoid seeing a half-finished write?
- How can the table preserve older states for auditing or time travel?
- How can new files be added without rewriting existing data and metadata?
- How can metadata help a query skip irrelevant files?

Mini Iceberg explores these problems by building the underlying storage and metadata mechanisms directly.

## My learning goals

By implementing this project, I aim to understand:

- Immutable data-file design
- Metadata-driven table state
- Manifest files and manifest lists
- Snapshots and snapshot lineage
- Atomic metadata commits
- Snapshot isolation and consistent reads
- Time-travel queries
- Logical deletes versus physical file deletion
- File statistics and metadata-based pruning
- Compaction and garbage collection
- Optimistic concurrency control
- Git and GitHub workflows for well-documented engineering projects

## Planned architecture

```text
Current table pointer
        |
        v
Table metadata file
        |
        v
Current snapshot
        |
        v
Manifest list
        |
        v
Manifest files
        |
        v
Immutable data files
```

A local table created by the project will eventually look similar to this:

```text
warehouse/
└── students/
    ├── data/
    │   ├── data-0001.jsonl
    │   └── data-0002.jsonl
    └── metadata/
        ├── v0.metadata.json
        ├── v1.metadata.json
        ├── v2.metadata.json
        ├── manifest-0001.json
        ├── manifest-0002.json
        ├── snapshot-0001.manifest-list.json
        ├── snapshot-0002.manifest-list.json
        └── version-hint.text
```

## Project status

**Stage 0 — Repository setup**

The project is currently being developed incrementally. Each feature will be implemented with tests, documentation, and small focused Git commits.

## Roadmap

- [ ] Initialize a local table and create version-0 metadata
- [ ] Write immutable JSON Lines data files
- [ ] Create manifests that describe data files
- [ ] Create snapshots and manifest lists
- [ ] Commit new metadata by atomically updating a table pointer
- [ ] Read the current snapshot
- [ ] Read historical snapshots through time travel
- [ ] Add logical delete support
- [ ] Add file statistics and basic predicate pruning
- [ ] Add compaction
- [ ] Add snapshot expiration and orphan-file cleanup
- [ ] Simulate optimistic-concurrency conflicts and retries

## Design principles

1. **Files are not table state.** A data file is part of a table only when committed metadata references it.
2. **Committed data is immutable.** Existing data files are not edited in place.
3. **Metadata commits publish complete states.** New files are written before the current metadata pointer changes.
4. **Old snapshots remain readable.** A snapshot is metadata that identifies a stable set of data files.
5. **Every feature must be inspectable.** The early versions use JSON and JSON Lines so the table state can be read manually.

## Technology

- Python
- JSON and JSON Lines for the initial educational storage format
- `pytest` for tests
- Git and GitHub for version control, documentation, and project history

## Repository structure

```text
mini-iceberg/
├── src/                # Table-format implementation
├── tests/              # Automated correctness tests
├── examples/           # Runnable demonstrations
├── docs/               # Architecture notes and learning journal
├── README.md
├── pyproject.toml
└── .gitignore
```

## Development philosophy

This repository is built as a learning artifact rather than a one-shot code submission.

For every stage, I will:

1. Define the system invariant.
2. Implement the smallest correct version.
3. Add automated tests.
4. Inspect the generated files manually.
5. Document the design decision.
6. Make a focused Git commit.

## References

- [Apache Iceberg documentation](https://iceberg.apache.org/)
- [Apache Iceberg specification](https://iceberg.apache.org/spec/)

## License

This project is licensed under the MIT License.
