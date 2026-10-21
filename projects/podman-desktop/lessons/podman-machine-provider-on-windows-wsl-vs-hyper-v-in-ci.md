---
topic: Podman machine provider on Windows (WSL vs Hyper-V) in CI
description: "Testing Podman machines on Windows with GitHub windows-2025 runners: WSL and Hyper-V machines, switching providers, 99-podman-machine-provider.conf drop-in, containers.conf.d overriding containers.conf, machine provider default, only one machine running at a time."
sources:
  - https://github.com/axel7083/podman-desktop/issues/40#issuecomment-6317895043
last_verified: 2026-10-21
confidence: medium
---
- On GitHub `windows-2025` runners, Hyper-V Podman machines work next to WSL ones, but only one machine runs at a time. Stop the WSL machine before starting a Hyper-V one.
- The Podman installer records the provider it was installed for in `%APPDATA%\containers\containers.conf.d\99-podman-machine-provider.conf` (`provider="wsl"` or `"hyperv"`). This drop-in overrides `containers.conf`, so editing `containers.conf` alone does not change the provider the CLI uses.
- To switch the CLI provider in a test, edit the drop-in, then run `podman machine init` and check VM TYPE in `podman machine list`.
- Podman Desktop's form default (`podman.factory.machine.win.provider`) is a static `wsl` and does not read the drop-in (as of c18209f).
- Source: maintainer statement plus the loop 3 reproduction in issue #40, where the CLI created a `hyperv` machine after the drop-in changed. Re-check with the steps above.
