*This project has been created as part of the 42 curriculum by apinho-a.*

# Born2beRoot

## Description

Born2beRoot is an introduction to Linux system administration. I installed Debian 13 as a command-line server in VirtualBox.

### Setup choices

- **Disk layout.** I used Debian's guided encrypted LVM setup with a separate `/home` volume, which gives user files their own space. `/`, `/home`,  and unlock with one passphrase. `/boot` sits outside it so Debian can load the kernel through GRUB.
- **Users and passwords.** The hostname is `apinho-a42` and my login is `apinho-a`. It belongs to `user42` and `sudo`. I chose `libpam-pwquality` for password strength checks. The password aging policy uses a 30-day maximum, two days between changes, and a seven-day warning.
- **SSH and firewall.** OpenSSH listens on TCP port `4242`. The server policy denies root login over SSH. UFW is the firewall for this setup and should start at boot with `4242/tcp` as the only incoming port allowed. AppArmor loads application security profiles at boot.
- **Sudo and monitoring.** The sudo policy requires a terminal, allows three password attempts, sets a custom failure message, restricts the command search path, and logs commands and their input and output under `/var/log/sudo/`. I chose `cron` to schedule `monitoring.sh`, a Bash script that broadcasts system statistics with `wall`. The subject calls for a run at startup and every ten minutes.

### Tools compared

- **Debian and Rocky Linux.** I chose Debian because the subject recommends it for a first server and its installer let me set up the encrypted LVM layout I needed. Debian Stable favors predictable updates, so newer software features can take longer to arrive. Rocky Linux is designed to be compatible with Red Hat Enterprise Linux  and is much more Enterprise focused, being more complex to setup. Debian uses AppArmor and UFW, while Rocky uses SELinux and firewalld.
- **AppArmor and SELinux**. AppArmor gives a program a profile that specifies which file paths it can access. SELinux gives processes and files security labels, then uses rules to decide which labels can access which.
- **UFW and firewalld.** Both manage firewall rules. UFW uses simple commands to allow or block ports. firewalld uses zones so different networks can have different rules. I used UFW on Debian to allow incoming SSH on port `4242`.
- **VirtualBox and UTM.** Both run virtual machines. I used VirtualBox because it was available on campus and is the subject’s first choice. I set up port forwarding so I could connect to my VM over SSH from the campus terminal. UTM is the allowed fallback and can emulate an x86 machine on an Apple Silicon Mac.

## Instructions

1. Open the `Born2beRoot` VM in VirtualBox on a campus workstation and start it.
2. Enter the disk passphrase at boot, then log in as `apinho-a` at the Debian console.
3. To connect from the same campus workstation, use the VirtualBox NAT rule that forwards `127.0.0.1:4241` to port `4242` in the guest.

   ```sh
   ssh -p 4241 apinho-a@127.0.0.1 or @localhost
   ```

From inside Debian, `lsblk` shows the encrypted LVM layout, `id` shows group membership, and `sudo ufw status` and `sudo aa-status` show the firewall and AppArmor status. Use `sudo crontab -l` to check root's monitoring schedule.

For submission, shut down the VM and run `sha1sum Born2beRoot.vdi` from the directory containing the virtual disk on the campus machine. Put the hash in `signature.txt` at the root of this repository. Keep the VDI in campus storage and start each evaluation with zero snapshots.

## Resources

- [Born2BeRoot guide I followed](https://noreply.gitbook.io/born2beroot)
- [Debian installation guide](https://www.debian.org/releases/trixie/amd64/)
- [Debian Administrator's Handbook on AppArmor](https://www.debian.org/doc/manuals/debian-handbook/sect.apparmor.en.html) and [SELinux](https://www.debian.org/doc/manuals/debian-handbook/sect.selinux.en.html)
- [UFW manual](https://manpages.debian.org/trixie/ufw/ufw.8.en.html) and [firewalld documentation](https://firewalld.org/documentation/concepts.html)
- [Debian sudoers manual](https://manpages.debian.org/trixie/sudo/sudoers.5.en.html) and [password quality settings](https://manpages.debian.org/trixie/libpwquality-common/pwquality.conf.5.en.html)
- [VirtualBox NAT and port forwarding](https://docs.oracle.com/en/virtualization/virtualbox/7.2/user/networkingdetails.html)

>[!info] **AI Usage**
>I used ChatGPT to help interpret the subject, understand encrypted LVM, sudo settings, check package choices, troubleshoot APT and SSH port forwarding.
