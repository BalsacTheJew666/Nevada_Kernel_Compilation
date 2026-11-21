# Gemini review guidance — XT2615-1 / NEVADA kernel

Review this repository as a Linux kernel and GitHub Actions project for the Motorola moto g play (2026), XT2615-1 (nevada), MediaTek MT6835, Linux 5.15.180.

## Review priorities

1. Kernel correctness: locking, interrupt context, workqueues, timers, RCU, refcounts, memory lifetime, error unwinding, DMA, and power-management interactions.
2. Kernel ABI and build compatibility: exported symbols, symbol CRCs, KMI, modversions, compiler/linker assumptions, Kconfig dependencies, defconfig fragments, Kbuild and module build dependencies.
3. Device integration: MT6835 / Nevada-specific assumptions, device-tree expectations, vendor module compatibility, and boot-image layout assumptions.
4. Security: privilege boundaries, SELinux interactions, unsafe user-controlled inputs, secret exposure, workflow permissions, untrusted pull-request execution, dependency pinning, and artifact provenance.
5. CI reliability: immutable source/toolchain pins, deterministic configuration, correct output paths, timeouts, runner memory/storage constraints, secret handling, and meaningful failure diagnostics.

## Review rules

- Report only actionable defects or material risks introduced by the pull request.
- Categorize each finding as Critical, High, Medium, or Low and explain the concrete failure mechanism.
- Reference the exact changed file and line(s); include a focused remediation where possible.
- Distinguish verified defects from hypotheses. Do not invent runtime behavior or claim a compile or boot test passed unless the workflow evidence proves it.
- Flag any accidental mixing of stock baseline work with NetHunter, KernelSU, SUSFS, KMI experiments, or performance tuning. Those must remain separate changes and branches.
- Treat a compiled Image.gz as a compile artifact only. Do not call it a flashable boot image or recommend flashing without independent packaging, ABI, and device boot validation.
- Do not make changes, push commits, approve the pull request, or merge it. This job is review-only.
- Do not repeat secrets, tokens, private keys, or sensitive values if they appear in a diff or log. Report the location and recommend rotation/removal instead.
