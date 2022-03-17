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
### Ad-hoc host
To run the playbook against single host without modifying the playbook's `hosts` section you'll need to pass the host with `-i` argument. 
Like this: `ansible-playbook -i "xxx.xxx.xxx.xxx," setup.yml`. Note the trailing comma