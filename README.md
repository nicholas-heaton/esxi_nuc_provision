# Steps

## Create the ISO

1. Download ISO from VMware

2. Extract ISO contents into a Folder

3. Modify two files
   - boot.cfg
   - efi/boot/boot.cfg

  Edit each file so that the kernerlopt line is exactly 'kernelopt=ks=usb:/KS.CFG'
  This tells the ESXi installer to look for a KS.CFG kickstart file on every USB drive mounted to the system until a valid file is found.

4. Repackage the ISO

    mkisofs -relaxed-filenames -J -R -o usb_ks_esxi.iso -b ISOLINUX.BIN -c BOOT.CAT -no-emul-boot -boot-load-size 4 -boot-info-table -eltorito-alt-boot -eltorito-platform efi -b EFIBOOT.IMG -no-emul-boot /esxi-iso-extracted


## Create the Kickstart File

A ks.cfg file has been provided. Edit this file as needed, and copy it to a Flash drive formatted as FAT32. (ExFAT is not supported by the ESXi installer)

Note: if you are using Secure Boot, you cannot run the %firstboot script as noted here:
https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.esxi.upgrade.doc/GUID-61A14EBB-5CF3-43EE-87EF-DB8EC6D83698.html


## Config ESXi

Once the ESXi installation is complete, configure the host by running the playbook against host.

## Setup
    python3 -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt

## Run
    ansible-playbook apply_esxi_config.yml -i inventory.yml --limit esxi-01