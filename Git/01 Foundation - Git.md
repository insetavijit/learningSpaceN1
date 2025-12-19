## **[[1.1 Definitions & Keywords — Git]]**

```
Git · Distributed Version Control System (DVCS) ·
Source Control Management (SCM) ·
Repository ·
Working Directory ·
Staging Area (Index) ·
Commit · Commit Object ·
Snapshot-Based Versioning ·
Hash (SHA-1 / SHA-256) ·
Object Database ·
Blob · Tree · Commit · Tag ·
Branch · HEAD · Detached HEAD ·
Merge · Fast-Forward Merge · Three-Way Merge ·
Rebase ·
Remote Repository ·
Clone · Fetch · Pull · Push ·
Conflict · Conflict Resolution ·
Tagging · Annotated Tags · Lightweight Tags ·
History Graph · Directed Acyclic Graph (DAG) ·
Ignore Rules (.gitignore) ·
Hooks ·
Submodules ·
Bare Repository ·
Git Workflow ·
Version History ·
Collaboration Model
```

---

## **[[1.2 Core Principles — Git]]**

```
1. Distributed Architecture — Every repository contains full history
2. Snapshot-Based Tracking — Commits store complete project states
3. Content-Addressable Storage — Objects identified by cryptographic hashes
4. Immutable History Objects — Commits and trees are not modified in place
5. Branch-as-Pointer Model — Branches are lightweight movable references
6. Explicit State Transitions — Changes flow through working tree → index → repository
7. Non-Linear History Support — Parallel development via branching
8. Local-First Operations — Most commands run without network access
9. Integrity by Design — Hashing ensures data consistency
10. Tool-Agnostic Collaboration — Works independently of hosting platforms
```

---

## **[[1.3 Mental Models — Git]]**

```
1. Snapshot Timeline Model — History as a sequence of snapshots
2. Graph Model — Commits form a directed acyclic graph
3. Pointer Model — Branches and HEAD reference commits
4. Three-Zone Model — Working directory, staging area, repository
```

---

## **[[1.4 Architecture Overview — Git]]**

### **High-Level Diagram**

```
Working Directory →
Staging Area (Index) →
Local Repository →
Remote Repository →
Shared History
```

---

### **[[1.4.2 Components & Responsibilities — Git]]**

```
1. Working Directory — Holds checked-out project files
2. Staging Area (Index) — Prepares changes for commit
3. Local Repository — Stores complete project history
4. Object Database — Persists blobs, trees, commits, and tags
5. References — Branches, tags, and HEAD pointers
6. Merge Engine — Combines divergent histories
7. Remote Interfaces — Synchronize repositories across systems
8. Hook System — Executes custom scripts on lifecycle events
```

---

### **[[1.4.3 Data / Version Flow — Git]]**

```
File Modified →
Change Staged →
Commit Created →
Branch Pointer Updated →
(Optional) Merge or Rebase →
Push to Remote →
Shared History Updated
```

---

## **[[1.5 Internals & Mechanics — Git]]**

1. **Object Storage Model** — Content stored as compressed, hashed objects
    
2. **Tree Construction** — Directory structures represented as tree objects
    
3. **Commit Linking** — Commits reference parent commits forming a DAG
    
4. **Index File Management** — Tracks staged file state
    
5. **Merge Algorithms** — Three-way merge using common ancestors
    
6. **Rebase Mechanics** — Commit replay onto new base
    
7. **Garbage Collection** — Cleanup of unreachable objects
    
8. **Reference Resolution** — Symbolic and direct reference lookup
    

---

## **[[1.6 Limitations & Trade-offs — Git]]**

|Limitation|Impact / Trade-off|
|---|---|
|**Steep Learning Curve**|Non-intuitive concepts for beginners|
|**Complex History Manipulation**|Rebasing and resets risk data loss|
|**Large Binary Handling**|Inefficient without extensions (e.g., LFS)|
|**Manual Conflict Resolution**|Requires developer intervention|
|**No Built-In Access Control**|Delegated to hosting platforms|
|**Repository Size Growth**|Full history stored in every clone|
|**Tooling Dependence**|Productivity tied to command-line literacy|

---

## 🎓 **Micro-Conclusion (Inline Insight)**

> Section 1 defines Git as a **content-addressable, distributed version control system** built on **immutable snapshots and graph-based history**.  
> Its architecture emphasizes **integrity, flexibility, and parallel development**, while trading off simplicity in favor of powerful history manipulation.

---

If you want the **same academic-grade document** next for:

- GitHub / GitLab
    
- Docker
    
- Linux
    
- CI/CD
    
- System Design (Developer Toolchain)
    

State the next subject only.