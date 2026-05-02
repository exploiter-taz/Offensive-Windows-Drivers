# CallbackRemoval
> built by [@taz](https://github.com/exploiter-taz)

A Windows kernel-mode driver for **enumerating and neutralizing** kernel notify callbacks registered via `PsSetCreateProcessNotifyRoutine`, `PsSetCreateThreadNotifyRoutine`, and `PsSetLoadImageNotifyRoutine`.

Designed to be **HVCI-compatible** => operates exclusively on read/write kernel data structures, never touches executable code.

---

## Overview

Windows exposes a set of kernel APIs that allow drivers to register callbacks for process creation, thread creation, and image load events. Security products (EDRs, AVs, monitoring tools) rely heavily on these callbacks to observe system activity.

This driver provides:

- **Enumeration** of all registered callbacks for each notify type
- **Neutralization** by replacing the registered function pointer with a harmless stub
- **Restoration** of original pointers on demand or automatically on unload

All operations target the `EX_CALLBACK_ROUTINE_BLOCK` structure at offset `+0x08` (the `Function` field) => no executable pages are modified.

---

## Architecture

```
User-mode client
      │
      │  DeviceIoControl
      ▼
\\.\CallbackDriver
      │
      ├── IOCTL_LIST_CALLBACKS    → enumerate registered callbacks
      ├── IOCTL_ZERO_CALLBACK     → replace fn ptr with dummy stub
      └── IOCTL_RESTORE_CALLBACK  → restore original fn ptr
```

### Callback resolution — fully dynamic

**No hardcoded offsets. No symbol files. No version tables.**

The driver resolves everything at runtime on every load => no hardcoded offsets, no PDB symbols, no version tables.

---

## Compatibility

| Feature | Details |
|---|---|
| OS | Windows 10 / 11 (all builds) |
| Architecture | x64 only |
| HVCI | Compatible => no executable page writes |
| Offset resolution | Runtime `no hardcoded build offsets` |
| Driver signing | Requires test signing or a valid EV certificate |

---

## IOCTLs

| IOCTL | Input | Output |
|---|---|---|
| `IOCTL_LIST_CALLBACKS` | `CALLBACK_TYPE` | Array of `CALLBACK_INFORMATION` |
| `IOCTL_ZERO_CALLBACK` | `ZERO_CALLBACK_INPUT` (type + index) | — |
| `IOCTL_RESTORE_CALLBACK` | `RESTORE_CALLBACK_INPUT` (type + index) | — |

```c
typedef enum _CALLBACK_TYPE {
    CallbackTypeProcess = 0,
    CallbackTypeThread  = 1,
    CallbackTypeImage   = 2,
} CALLBACK_TYPE;
```

---

## Safety

- All patched entries are saved in an internal table before modification
- On `DriverUnload`, every patched entry is **automatically restored**
- This is critical: the dummy stubs live inside the driver image — if the driver unloads while stubs are still active, the kernel will call into freed memory

---

## Disclaimer

This project is intended for **security research and educational purposes only**.  
Use only on systems you own or have explicit authorization to test.  
Loading unsigned drivers requires **test signing mode** or a bypass — do not use on production systems.

---

## References

- [Microsoft — Kernel Callback Functions](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/callback-objects)
