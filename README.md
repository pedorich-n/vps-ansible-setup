# vps-ansible-setup
Ansible Playbook to setup a Debian-based Server

This playbook includes the following steps:
- Set a locale from vars
- Update apt preferences
- Install `aptitude`
- Upgrade the system using `safe-upgrade`
- Install sudo
- [Optionally] change the `root` password
- Create new user and add it to `sudo` and `systemd-journal` groups
- Copy public ssh key to `authorized_keys`
- Disable SSH password authentication
- Install [UFW](https://en.wikipedia.org/wiki/Uncomplicated_Firewall)
- Setup UFW to block all incoming connections but SSH
- Install custom packages defined in vars
- Reboot machine


## Configuration
### Vars
Before running the playbook you need to create `global.yml` file under `vars` directory. Use `global.example.yml` as a source.

Variables are pretty self-explanatory, except the password hashes. To generate hashes use `openssl passwd -6 -salt <some-random-salt>` or just `openssl passwd -6`. This will produce sha512 hash.

If you wish to change the `root`'s user password, fill the `root_password_hash` field. This step is optional. `user_password_hash` on the other hand should be filled.

## Running the playbook
1. Make sure your SSH public key has been deployed to the root account as the playbook is using root as the `remote_user`. If you'd rather SSH into your own account and use sudo to run the playbook, then you'll have to replace `remote_user` with `become_user` in site.yml
2. Set a host:
    - Edit the inventory (`/etc/ansible/hosts` or a custom inventory file) to add your server's IP or hostname
        - Change the hosts entry in `setup.yml` to your server's IP or hostname
    - Or use `-i` argument to pass the ad-hoc host, like `-i "xxx.xxx.xxx.xxx," setup.yml`. _Note the trailing comma._
        - In this case do not change the `setup.yml`
3. Run the playbook with `ansible-playbook setup.yml` or `ansible-playbook -i "xxx.xxx.xxx.xxx," setup.yml` if you're using ad-hoc host
