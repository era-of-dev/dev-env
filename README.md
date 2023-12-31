# dev-env
Ansible scripts for setting up a development environment

## Target systems
* Tested with:
  * Fedora 38/39
  * Rocky 9
  * Ubuntu 22.04

## Requirements
* Internet
* Run an initial system update and restart
  ```
  sudo dnf upgrade -y
  sudo apt upgrade -y
  ```
* ansible installed via package manager
  ```
  sudo dnf install ansible -y
  sudo apt install ansible -y
  ```
* git (if using ansible pull) 
  ```
  sudo dnf install git -y
  sudo apt install git -y
  ```

## Run
* via ansible-playbook
  ```
  sudo ansible-playbook site.yml -i hosts.yml
  ```
* via ansible-pull
  ```
  sudo ansible-pull -U https://github.com/era-of-dev/dev-env.git site.yml -i hosts.yml
  ```

### Run with NVIDIA driver install (after initial run)
* via ansible-playbook
  ```
  sudo ansible-playbook site.yml -i hosts.yml -t nvidia
  ```
* via ansible-pull
  ```
  sudo ansible-pull -U https://github.com/era-of-dev/dev-env.git site.yml -i hosts.yml -t nvidia
  ```

## Testing
* Using vagrant and VirtualBox
* Run: `vagrant up`
* the default operating system is Rocky 9 via the `bento/rockylinux-9` vagrant box
  * this can be changed via the `--os` flag using a valid vagrant box (https://app.vagrantup.com/boxes/search), example:
    * `vagrant --os=generic/ubuntu2204 up`
    * `vagrant --os=bento/fedora-38 up`

## Troubleshooting
* Network issue with Fedora vagrant boxes
  * Enter VM as root:
    * `vagrant ssh dev`
    * `sudo -i`
  * Figure out which connection `NAME` is yellow `nmcli con show`
  * `nmcli con mod "Wired connection 2" ipv4.addresses 192.168.56.111/24`
    * replace `Wired connection 2` with the `NAME` in yellow
  * Exit VM and continue building:
    * `vagrant up --provision`