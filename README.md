#!/bin/bash

# 1. Synchronize system time with NTP
echo "Synchronizing system time with NTP..."
sudo timedatectl set-ntp true

# 2. Update /etc/apt/sources.list to prioritize Debian 12 repository
echo "Updating APT sources to prioritize Debian 12..."
echo "deb http://deb.debian.org/debian/ stable main contrib non-free" | sudo tee /etc/apt/sources.list

# 3. Update the GRUB configuration to prioritize Debian 12
echo "Updating GRUB configuration to prioritize Debian 12..."
GRUB_DEFAULT=0
GRUB_TIMEOUT=5
GRUB_CMDLINE_LINUX_DEFAULT="quiet"
GRUB_CMDLINE_LINUX=""

# Ensure /etc/default/grub is updated
sudo sed -i 's/GRUB_DEFAULT=.*/GRUB_DEFAULT=0/' /etc/default/grub
sudo sed -i 's/GRUB_TIMEOUT=.*/GRUB_TIMEOUT=5/' /etc/default/grub
sudo sed -i 's/GRUB_CMDLINE_LINUX_DEFAULT=.*/GRUB_CMDLINE_LINUX_DEFAULT="quiet"/' /etc/default/grub
sudo sed -i 's/GRUB_CMDLINE_LINUX=.*/GRUB_CMDLINE_LINUX=""/' /etc/default/grub

# 4. Update GRUB to apply changes
echo "Updating GRUB..."
sudo update-grub

# 5. Remove Kali Linux from GRUB boot menu
echo "Removing Kali Linux from GRUB boot menu..."
sudo sed -i '/Kali Linux/d' /etc/grub.d/40_custom

# 6. Rebuild the GRUB configuration
echo "Rebuilding GRUB configuration..."
sudo update-grub

# 7. Prompt for reboot (not automatic)
echo "Changes applied. Please reboot your system to finalize."
