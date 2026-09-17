\# Learning Journal



This journal records the reasoning behind each implementation stage of Mini Iceberg.



The objective is not merely to reproduce Apache Iceberg terminology. The objective is to understand how immutable files, layered metadata, snapshots, and atomic pointer updates create reliable table state over ordinary file storage.



\---



\## Stage 0 — Repository and project foundation



\### What did I build?



I created the initial repository structure for a Python implementation of an Apache Iceberg-inspired table format.



\### Why is Git part of this project?



Git records the evolution of the source code and documentation. The Mini Iceberg table format will independently record the evolution of table data through metadata and snapshots.



Both systems use the idea that historical states should remain addressable rather than overwritten without trace.



\### Initial repository layout



\- `src/mini\_iceberg/`: source code for the table-format engine

\- `tests/`: automated correctness tests

\- `examples/`: executable demonstrations

\- `docs/`: architecture notes and this learning journal



\### Initial engineering rules



1\. I will keep generated table directories out of Git unless they are intentionally small test fixtures.

2\. I will not commit secrets, API keys, `.env` files, virtual environments, or caches.

3\. I will make small, focused commits with meaningful messages.

4\. I will inspect `git status` and `git diff --staged` before committing.

5\. I will write tests as the system gains behavior.



\### Questions I want this project to answer



1\. How can a data file exist on disk but not yet be part of a table?

2\. How can a table change without rewriting old data files?

3\. How can a reader see a stable table state while a writer commits a new state?

4\. How does metadata enable time travel?

5\. How can old files eventually be cleaned up safely?

