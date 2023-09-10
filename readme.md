# nixos-ansible

This project presents a proof-of-concept for deploying declarative nixos configuration with ansible.  We simply land `/etc/nixos/configuration.nix` and run `nixos-rebuild switch` in a handler.

Here we're enabling serial console output on select nodes using inventory var `serial: true`:

```
git/nixos-ansible$ ansible-playbook main.yml --diff

PLAY [nixos] *******************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************
ok: [172.30.220.14]
ok: [172.30.193.173]

TASK [nixos : land configuration.nix] ******************************************************************************
--- before: /etc/nixos/configuration.nix
+++ after: /home/nhensel/.ansible/tmp/ansible-local-1254294jzltqvkq/tmpj0i0l1en/configuration.nix
@@ -9,6 +9,9 @@
   system.stateVersion = "23.11";
   boot.loader.grub.devices = ["/dev/sda"];
 
+  boot.kernelParams = [
+    "console=ttyS0,115200"
+  ];
 
   nixpkgs.config.allowUnfree = true;
   security.sudo.wheelNeedsPassword = false;

changed: [172.30.220.14]
--- before: /etc/nixos/configuration.nix
+++ after: /home/nhensel/.ansible/tmp/ansible-local-1254294jzltqvkq/tmp5bj3lyiw/configuration.nix
@@ -9,6 +9,9 @@
   system.stateVersion = "23.11";
   boot.loader.grub.devices = ["/dev/sda"];
 
+  boot.kernelParams = [
+    "console=ttyS0,115200"
+  ];
 
   nixpkgs.config.allowUnfree = true;
   security.sudo.wheelNeedsPassword = false;

changed: [172.30.193.173]

RUNNING HANDLER [nixos : nixos-rebuild switch] *********************************************************************
changed: [172.30.193.173]
changed: [172.30.220.14]

PLAY RECAP *********************************************************************************************************
172.30.193.173             : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
172.30.220.14              : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
