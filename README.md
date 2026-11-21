# Motorola moto g play (2026) — XT2615-1 / NEVADA stock kernel

This repository contains a reproducible GitHub Actions build definition for the **stock Motorola kernel** used by the XT2615-1 (nevada, MT6835). It does not mirror or redistribute Motorola's kernel source; Actions fetches the source from Motorola's public repository at an immutable commit.

## Device and source identity

| Item | Pinned value |
|---|---|
| Device | Motorola moto g play (2026), XT2615-1 |
| Codename / product | nevada / nevada_g_sys |
| SoC | MediaTek MT6835 |
| Kernel source | MotorolaMobilityLLC/kernel-mtk |
| Source branch | android-16-release-w1wn36.18-114 |
| Source commit | 0fa176ca7f56c349b51acd83684284c02ced5854 |
| Kernel series | Linux 5.15.180 |
| Compiler | Android Clang r450784e / Clang 14.0.7 |
| Linker | LLD 14 |

The build uses the device's captured stock .config snapshot from the private nevada-nethunter-baseline-xt2615 repository. The workflow downloads that file by its immutable Git blob SHA and verifies both the Git blob SHA and the recorded decompressed config SHA256 before compiling.

## Build scope and safety

The workflow currently compiles **Image.gz only**. It does not build or package a complete Motorola boot image, vendor boot image, DTBO image, recovery image, installer ZIP, or flashable release. It does not apply NetHunter, KernelSU, SUSFS, KMI-supplement, or performance patches.

The output artifact is named **NEVADA-STOCK-IMAGE-DO-NOT-FLASH**. A successful compiler exit proves only that this image target compiled under the CI environment. It is not proof of bootability, device compatibility, ABI compatibility, or safe flashing. Keep the stock boot image available for recovery and do not flash this artifact.

## Required repository secret

Add a fine-grained GitHub token as an Actions secret named **PRIVATE_REPO_TOKEN**:

1. Create a fine-grained personal access token limited to the private repository BalsacTheJew666/nevada-nethunter-baseline-xt2615.
2. Grant **Contents: Read-only**. No write access is needed.
3. In this repository, open **Settings → Secrets and variables → Actions → New repository secret**.
4. Name it PRIVATE_REPO_TOKEN and paste the token value.

The token is used only to read the pinned private stock-config blob. It is never printed in Actions logs. The current private-config blob SHA is 21996d53d1dc7ee0c7f4e037b8fdb3276108db83.

## Free AI pull-request review

The workflow includes an optional Gemini CLI code-review job. It runs for non-draft pull requests opened from branches in this same repository; fork pull requests are excluded so secrets are never passed to untrusted fork code. It is configured to publish a GitHub review, not to push commits or edit repository files.

To enable it:

1. Create a Gemini API key in Google AI Studio and use its free tier.
2. In this repository, open **Settings → Secrets and variables → Actions → New repository secret**.
3. Name the secret GEMINI_API_KEY.

The free tier is quota-limited and subject to Google's current model and API limits. If the secret is absent, the job records a notice and skips the AI review without preventing the kernel build. Pull-request source and review context are sent to Google's Gemini service for analysis; do not enable it for code that must not be shared with that service.

The reviewer instructions are in GEMINI.md. Review findings should be independently verified, especially for kernel concurrency, memory management, Kbuild, and ABI issues.

## Runner selection

The workflow uses ubuntu-24.04, the largest standard GitHub-hosted runner class available to this personal-account/public-repository setup. The current standard public x64 runner provides 4 vCPUs, 16 GB RAM, and 14 GB SSD. The build is deliberately limited to two compiler jobs and a single-threaded LLD link to reduce memory pressure.

GitHub larger hosted runners (including 64-core and 96-core classes) require an organization or enterprise on GitHub Team or Enterprise Cloud. They cannot be assigned directly to a personal user account. If this project is later moved under an eligible organization, the runs-on label can be changed to the selected larger-runner label after that runner is provisioned.

## Running the build

After the workflow is merged into main and PRIVATE_REPO_TOKEN is configured:

1. Open the repository's **Actions** tab.
2. Select **Nevada stock kernel build and Gemini review**.
3. Choose **Run workflow** on main.

The job uploads the image and provenance/build reports as the NEVADA-STOCK-IMAGE-DO-NOT-FLASH artifact for 14 days. The initial pull request that introduces this workflow cannot execute the new workflow from the feature branch until the workflow is merged into the default branch.
