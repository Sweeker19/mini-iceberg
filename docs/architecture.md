\# Mini Iceberg Architecture



\## Scope



Mini Iceberg is an educational local table format implemented in Python. It models the central metadata and snapshot ideas behind Apache Iceberg without claiming compatibility with the Apache Iceberg specification or ecosystem.



\## Intended metadata graph



```text

Current table pointer

&#x20;       |

&#x20;       v

Table metadata file

&#x20;       |

&#x20;       v

Current snapshot

&#x20;       |

&#x20;       v

Manifest list

&#x20;       |

&#x20;       v

Manifest files

&#x20;       |

&#x20;       v

Immutable data files

```



\## Core rule



A physical data file belongs to a table only when the selected committed metadata graph references it.



\## Commit rule



A writer must create new data and metadata files first. It must update the current table pointer only after the complete new state exists.



\## First invariant



The current table pointer must always point to an existing, valid metadata file.

