# RockyLinux Apptainer Environment

This repository contains a portable container image based on RockyLinux 9.3, set up to reproduce the workflow of the ESP platform on the mamuthones server, for the PULP-ESP project.

---

## Download the Container Image

Download the pre-built `.sif` image from the [GitHub Releases page](https://github.com/eugeniomuscinelli/rockylinux_apptainer/releases), or directly from the terminal:

```bash
curl -L -o rockylinux_image.sif https://github.com/eugeniomuscinelli/rockylinux_apptainer/releases/download/v1.0/rockylinux_image.sif
```

---

## Running the Container

### Option 1: Run Read-Only

This runs the container without modifying it:

```bash
apptainer shell --fakeroot --pwd /home/ \
  --bind /sw/CAD/ \
  --bind /sw/tool/ \
  --bind /usr/share/i18n/ \
  rockylinux_image.sif
```

---

### Option 2: Run as Writable (Sandbox)

If you want to modify the container (e.g., install software), first convert it into a sandbox:

```bash
apptainer build --sandbox rockylinux_dev/ rockylinux_image.sif
```

Then launch it:

```bash
apptainer shell --fakeroot --writable --pwd /home/ \
  --bind /sw/CAD/ \
  --bind /sw/tool/ \
  --bind /usr/share/i18n/ \
  rockylinux_dev/
```

Changes will be saved in the `rockylinux_dev/` folder, not the `.sif` image.

---

## What's Inside

- Based on `rockylinux:9.3`
- Pre-installed tools and dependencies required for the project workflow
- No user-specific data included (e.g., `/home`, `/root/.ssh` were cleaned before image creation)

---

## Notes

- Image size: ~1.4 GB
- Built using Apptainer with `--fakeroot` from a sandbox
- Verified to run reproducibly on compatible Linux systems
