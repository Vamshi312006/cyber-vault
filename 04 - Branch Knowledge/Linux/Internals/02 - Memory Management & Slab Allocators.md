---
id: "mod-nix-memory-slab"
title: "Linux Memory Management & Slab/Slub Allocators"
domain: "Domain-01"
branch: "linux-kernel"
type: "module"
maintainer: "Cyber Act Engineering Team"
last_audited: "2026-07-29"
---

# Linux Memory Management & Slab/Slub Allocators

## 1. Overview & Purpose
Linux memory management governs how physical RAM and virtual address spaces are partitioned between user-space applications and the kernel.

This module details the Virtual Memory Area (`vm_area_struct`), Page Table layout, Buddy Allocator, Kernel Slab/SLUB/SLOB allocators (`kmalloc`), Out-Of-Memory (OOM) killer, and kernel heap exploitation primitives (Use-After-Free, Heap Spraying). Understanding kernel memory allocation is essential for kernel exploit analysis, eBPF memory safety, and system stability diagnostics.

---

## 2. Architecture & Key Components

```mermaid
graph TD
    subgraph Physical RAM Management
        BUDDY["Buddy Allocator (Page-Level Allocation - 4KB / 2MB / 1GB)"]
    end

    subgraph Kernel Heap Allocators (Sub-Page Sized Memory)
        SLUB["SLUB Allocator (Default Kernel Heap - kmalloc-64, kmalloc-512, kmalloc-2048)"]
        SLAB["Legacy SLAB Allocator (Object Caching - task_struct, mm_struct, cred)"]
    end

    subgraph User Space vs Kernel Space Virtual Address Space
        USER_VM["User Space VAS (0x0000000000000000 to 0x00007fffffffffff)<br/>Managed via vm_area_struct (VMA List/Tree)"]
        KERNEL_VM["Kernel Direct Map (page_offset_base)<br/>HighMem / vmalloc / kmalloc Memory"]
    end

    BUDDY --> SLUB
    BUDDY --> SLAB
    BUDDY --> USER_VM
    SLUB --> KERNEL_VM
```

---

## 3. Detailed Mechanics & Internal Structures

### 3.1 Virtual Memory Areas (`vm_area_struct`)
Each process `mm_struct` maintains a memory map of contiguous virtual memory regions called VMAs (`vm_area_struct`):

```c
struct vm_area_struct {
    unsigned long vm_start;     /* Start virtual address */
    unsigned long vm_end;       /* End virtual address */
    struct vm_area_struct *vm_next; /* Linked list of VMAs */
    pgprot_t vm_page_prot;      /* Access permissions (PROT_READ, PROT_WRITE, PROT_EXEC) */
    unsigned long vm_flags;     /* VM_READ, VM_WRITE, VM_EXEC, VM_MAYSHARE */
    struct file *vm_file;       /* Mapped file pointer (or NULL for anonymous) */
};
```

---

### 3.2 The SLUB Allocator (`kmalloc`)
While the Buddy Allocator allocates memory in whole pages (4KB powers of two), the kernel requires small object allocations (e.g., a 64-byte or 512-byte structure). The **SLUB Allocator** (unqueued slab allocator) manages these caches:
- **General Purpose Caches**: `kmalloc-32`, `kmalloc-64`, `kmalloc-128`, `kmalloc-256`, `kmalloc-512`, `kmalloc-1024`, `kmalloc-2048`, `kmalloc-4096`.
- **Specialized Object Caches**: `task_struct`, `cred`, `files_struct`, `sighand_struct`.

#### SLUB Page Layout & Free Lists:
Each SLUB page (`struct page`) contains a `freelist` pointer referencing the next available free object in the slab chunk. When `kfree(ptr)` is invoked, `ptr` is prepended to the head of the `freelist`.

---

## 4. Security Implications & Memory Mitigations

- **KASLR (Kernel Address Space Layout Randomization)**: Randomizes the kernel text base address (`_text`) and direct map base on boot, requiring an information leak to locate kernel function pointers.
- **`SLAB_FREELIST_HARDENING`**: Encrypts the `freelist` pointer inside SLUB objects using a random per-cache XOR key (`freelist_xor`), preventing attackers from overwriting freelist pointers during heap buffer overflows.
- **`CONFIG_INIT_ON_ALLOC_DEFAULT_ON`**: Automatically zeros out memory on allocation to mitigate Use-After-Free data leaks.

---

## 5. Attack Vectors & Exploitation Primitives

1. **Kernel Use-After-Free (UAF)**:
   Freeing a kernel object (e.g., `kfree(object)`) while retaining a dangling pointer. The attacker then sprays the SLUB cache with a target structure (e.g., `user_key_payload`) of matching size to reclaim the freed memory chunk, enabling parameter corruption.
2. **SLUB Heap Spraying**:
   Allocating thousands of objects of a specific `kmalloc` size class (e.g., via `msgnd` or `add_key` syscalls) to force predictable heap layouts and control `freelist` pointer addresses.
3. **Out-of-Bounds (OOB) Kernel Write**:
   Writing past the boundary of a `kmalloc` object to corrupt adjacent object metadata or function pointers.

---

## 6. Defense & Telemetry Verification

### Telemetry Tracing Sources:
- **eBPF Kprobes**: `kprobe:kmalloc`, `kprobe:kfree`.
- **Kernel Slab Diagnostic Log**: `/proc/slabinfo`.
- **KASAN (Kernel Address Sanitizer)**: Dynamic compiler instrumentation detecting out-of-bounds and use-after-free bugs in kernel development builds.

### Inspecting Slab Caches via `/proc/slabinfo`:
```bash
# Display top active slab caches
sudo slabtop -o

# Check specific cache allocation count (e.g., cred or kmalloc-512)
grep -E "cred|kmalloc-512" /proc/slabinfo
```

---

## 7. Engineering & Hands-On Implementation

### Inspecting SLUB Allocations via `gdb` (Kernel Debugging with QEMU/gdb):
```text
(gdb) lx-symbols
(gdb) print *(struct vm_area_struct *) 0xffff888002a4b120
$1 = {
  vm_start = 0x7ffff7ffa000,
  vm_end = 0x7ffff7ffb000,
  vm_flags = 0x8000075,
  vm_page_prot = { pgprot = 0x8000000000000025 }
}
```

---

## 8. Troubleshooting & Diagnostics

| Symptom | Root Cause | Remediation / Diagnostic Command |
| :--- | :--- | :--- |
| Kernel Panic: `Kernel panic - not syncing: Out of memory`. | OOM Killer triggered due to unconstrained memory consumption in cgroup. | Inspect `/var/log/syslog` for `oom-killer` log lines. Set `memory.max` limits in cgroups. |
| Kernel Panic: `BUG: Bad page state in process...`. | Memory corruption detected by Buddy or SLUB allocator. | Enable `slub_debug=FZPU` kernel boot parameter to pinpoint bad allocations. |

---

## 9. References
- Christoph Lameter, *SLUB: The Unqueued Slab Allocator*.
- Linux Kernel Documentation: *Memory Management APIs & SLUB Allocator*.
