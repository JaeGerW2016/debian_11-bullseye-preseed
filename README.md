# Debian fully automatic install — preseed ISO remastering

Remaster a Debian netinst ISO for 100% unattended install. No boot menu prompt. No hands.

## Quickstart

```bash
# Download debian-11.x.x-amd64-netinst.iso from https://www.debian.org/CD/netinst/
# Adapt preseed.cfg: SSH key, passwords, locale, timezone
./make-preseed-iso.sh debian-11.0.0-amd64-netinst.iso
```

Output: `preseed-debian-11.0.0-amd64-netinst.iso` — boots and installs on the first non-USB disk.

## Preseed defaults

- **Hostname** — random 10-char `debian-xxxx` (from `/dev/urandom`)
- **Users** — `root` + `ops`, password `YourPassword`, SSH key via authorized_keys
- **Partitioning** — EFI + ext4 root + swap (atomic recipe)
- **Packages** — minimal: standard + ssh-server + vim + sudo
- **Kernel params** — `cgroup_enable=memory swapaccount=1`
- **IPv4 preference** — `/etc/gai.conf` gets `precedence ::ffff:0:0/96 100` so dual-stack DNS resolves IPv4 first. IPv6 kernel support stays *on*. Removal of `ipv6.disable=1` (now handled automatically).

## ⚠ Warning

**This erases the first disk** (`list-devices disk`, excluding USB) — no confirmation. Test on a throwaway machine or VM first.

## Notes

- initrd path hardcoded to `install.amd` — requires amd64 netinst ISO
- GRUB boot entry selection by position — bullseye-specific for UEFI
- Edit preseed.cfg, re-run the script. That's it.

## References

- [Debian Preseed docs](https://wiki.debian.org/DebianInstaller/Preseed)
- [Edit ISO howto](https://wiki.debian.org/DebianInstaller/Preseed/EditIso)
- [ISO repacking](https://wiki.debian.org/RepackBootableISO)