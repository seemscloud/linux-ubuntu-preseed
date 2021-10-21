```bash
d-i debian-installer/language string en
d-i debian-installer/country string US
d-i debian-installer/locale string en_US.UTF-8

d-i netcfg/choose_interface select auto
d-i netcfg/link_wait_timeout string 15
d-i netcfg/dhcpv6_timeout string 15
d-i netcfg/dhcp_timeout string 15
d-i netcfg/hostname string hostname
d-i netcfg/get_hostname string hostname
d-i netcfg/get_domain string hostdomain

d-i mirror/country string manual
d-i mirror/http/hostname string pl.archive.ubuntu.com
d-i mirror/http/directory string /ubuntu
d-i mirror/suite string focal

d-i user-setup/allow-password-weak boolean true

# mkpasswd -m sha-512 -S $(pwgen -ns 16 1) mypassword
#d-i passwd/root-login boolean true
#d-i passwd/root-password-crypted password $6$rdp2Pq9okmQm$mrIjThChupd4A9zbT4CIk9YXbhmFPWhobNsUk7bKApMtdWxaWJDhRcbB0cXUBvxbDZGxz2uOrRa1ga/Z1a29H1
#d-i passwd/make-user boolean true
#d-i passwd/user-fullname string User
#d-i passwd/username string user
#d-i passwd/user-uid string 1001
#d-i passwd/user-default-groups string sudo
#d-i passwd/user-password-crypted password $6$rdp2Pq9okmQm$mrIjThChupd4A9zbT4CIk9YXbhmFPWhobNsUk7bKApMtdWxaWJDhRcbB0cXUBvxbDZGxz2uOrRa1ga/Z1a29H1

d-i clock-setup/utc boolean true
d-i time/zone string EU/Warsaw
d-i clock-setup/ntp boolean true
d-i clock-setup/ntp-server string 0.pl.pool.ntp.org

d-i partman-auto/init_automatically_partition select biggest_free
d-i partman/unmount_active boolean true
d-i partman-auto/disk string /dev/nvme0n1
# d-i partman-auto/disk string /dev/sda
d-i partman-auto/method string crypto
d-i partman-efi/non_efi_system boolean true
d-i partman-lvm/device_remove_lvm boolean true
d-i partman-md/device_remove_md boolean true
d-i partman-lvm/confirm boolean true
d-i partman-lvm/confirm_nooverwrite boolean true
d-i partman-auto-lvm/guided_size string max
d-i partman-auto-lvm/new_vg_name string crypt
d-i partman-auto/choose_recipe select root-encrypted
d-i partman-auto/expert_recipe string                         \
      root-encrypted ::                                       \
              1 1 1 free                                      \
                      $bios_boot{ }                           \
                      method{ biosgrub }                      \
              .                                               \
              1024 150 1024 fat32                             \
                      $iflabel{ gpt }                         \
                      $reusemethod{ }                         \
                      $gptonly{ }                             \
                      $primary{ }                             \
                      method{ efi } format{ }                 \
                      use_filesystem{ } filesystem{ fat32 }   \
              .                                               \
              1024 100 1024 ext4                              \
                      $gptonly{ }                             \
                      $primary{ } $bootable{ }                \
                      method{ format } format{ }              \
                      use_filesystem{ } filesystem{ ext4 }    \
                      mountpoint{ /boot }                     \
              .                                               \
              8192 200 -1 ext4                                \
                      $lvmok{ } lv_name{ root }               \
                      in_vg { crypt }                         \
                      $gptonly{ }                             \
                      $primary{ }                             \
                      method{ format } format{ }              \
                      use_filesystem{ } filesystem{ ext4 }    \
                      mountpoint{ / }                         \
              .
d-i partman/default_filesystem string ext4
d-i partman-partitioning/no_bootable_gpt_biosgrub boolean false
d-i partman-partitioning/no_bootable_gpt_efi boolean false
d-i partman-basicfilesystems/no_mount_point boolean false
d-i partman-basicfilesystems/choose_label string gpt
d-i partman-basicfilesystems/default_label string gpt
d-i partman-partitioning/choose_label string gpt
d-i partman-partitioning/default_label string gpt
d-i partman/choose_label string gpt
d-i partman/default_label string gpt
d-i partman-partitioning/confirm_write_new_label boolean true
d-i partman/choose_partition select finish
d-i partman/confirm boolean true
d-i partman/confirm_nooverwrite boolean true

d-i pkgsel/updatedb boolean true
d-i pkgsel/upgrade select full-upgrade
d-i pkgsel/include string kde-plasma-desktop ubuntu-gnome-desktop
d-i pkgsel/language-packs multiselect en, pl
d-i pkgsel/update-policy select unattended-upgrades

d-i cdrom-detect/eject boolean true
d-i finish-install/reboot_in_progress note

d-i preseed/late_command string in-target apt-get update ; \
                                in-target apt-get upgrade -y ; \
                                in-target apt-get dist-upgrade -y ; \
                                in-target apt-get install -y openssh-server build-essential lvm2 cryptsetup ; \
                                in-target apt-get install -y sudo gdebi wget perl gawk sed awk python3 ; \
                                in-target wget https://zoom.us/client/latest/zoom_amd64.deb -O /tmp/zoom_amd64.deb ; \
                                in-target wget https://downloads.slack-edge.com/linux_releases/slack-desktop-4.4.3-amd64.deb -O /tmp/slack-desktop-4.4.3-amd64.deb ; \
                                in-target wget https://download.teamviewer.com/download/linux/teamviewer_amd64.deb -O /tmp/teamviewer_amd64.deb ; \
                                in-target wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb -O /tmp/google-chrome-stable_current_amd64.deb ; \
                                in-target dpkg -i /tmp/zoom_amd64.deb ; \
                                in-target dpkg -i /tmp/slack-desktop-4.4.3-amd64.deb ; \
                                in-target dpkg -i /tmp/teamviewer_amd64.deb ; \
                                in-target dpkg -i /tmp/google-chrome-stable_current_amd64.deb ; \
                                in-target apt-get --fix-broken install -y ;
```
