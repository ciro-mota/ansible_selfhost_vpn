<h2> Ansible playbooks to setup a self-hosted WireGuard VPN server </h2>

<p align="center">
    <img alt="Ansible" src="https://img.shields.io/badge/Ansible-000000?style=for-the-badge&logo=ansible&logoColor=white" />
    <img alt="Docker" src="https://img.shields.io/badge/Docker-2CA5E0?style=for-the-badge&logo=docker&logoColor=white">
    <img alt="GitHub Workflow Status" src="https://img.shields.io/github/actions/workflow/status/ciro-mota/ansible_selfhost_vpn/ansible-lint.yml?style=for-the-badge&logo=github&label=Lint">
</p>

![Screenshot](https://user-images.githubusercontent.com/101431112/209468911-88c70c8d-c686-4dac-b4c7-bc3b1fb67568.png)

This repo is forked from [rishavnandi/ansible_selfhost_vpn](https://github.com/rishavnandi/ansible_selfhost_vpn) and contains Ansible playbooks to setup a self-hosted WireGuard VPN server with my modifications. Originally based on [wg-easy](https://github.com/WeeJeWel/wg-easy) which provides a nice web interface to add and remove clients.

## Support

Only the OS below support running this script.

|     OS     |   Support   |
| ---------- | ----------- |
| Debian     |     Yes     |
| Ubuntu     |     Yes     |
| RHEL       |     Yes     |
| AlmaLinux  |     Yes     |
| RockyLinux |     Yes     |


## Usage

- Clone the repo:

```bash
git clone https://github.com/ciro-mota/ansible_selfhost_vpn
```
- The password to access `wg-easy` needs to be encrypted in `Bcrypt` format. Open a terminal at the root of the `ansible_selfhost_vpn` directory and run the command below to generate it and save it with Ansible Vault.

```bash
ansible-vault encrypt_string $(htpasswd -nbBC 12 "" your-password-here | cut -d ':' -f2) --name 'wg_password' >> wireguard/vars/main.yml
```

- Create a file `vault-pass` with the password you set in the previous step in Ansible Vault in your `/home` directory.

- Enter the server's IP address where WireGuard will be provisioned in the `hosts` file.

- You will need to generate SSH passwords and configure them with your cloud provider where you will provision the WireGuard.

- Set your `private key` on the `host` file.

    - If using with Oracle, pass the parameter `ansible_user=ubuntu` on the `host` file.

- It is necessary to install the `community.docker` module for it to work, run the command below to install it on your system.

```bash
ansible-galaxy collection install community.docker
```

- Then simply run the Ansible playbook.

```bash
ansible-playbook -i hosts run.yml --vault-password-file ~/vault-pass
```
- Finally you can visit the `wg-easy` at your server's IP address on port `51821` to configure your WireGuard devices.

You can also configure the [Nginx Proxy Manager](https://nginxproxymanager.com/guide/) so that you can access your services over the internet using a domain name. You should access it through your server's IP address on port `81`.

On your first access you must access it with the credentials below:

```
Email:    admin@example.com
Password: changeme
```

You can obtain a free domain name from [DuckDNS](https://www.duckdns.org/) to use with Nginx Proxy Manager, also you can use a domain name if you already own.
Make sure the domain name is pointing to your server's public IP address. [See how](https://www.youtube.com/watch?v=qlcVx-k-02E).

## Tested on

<img alt="DigitalOcean" src="https://img.shields.io/badge/DigitalOcean-0080FF?logo=digitalocean&logoColor=fff&style=for-the-badge" />

<img alt="Linode" src="https://img.shields.io/badge/Linode-00A95C?style=for-the-badge&logo=Linode&logoColor=white" />

<img alt="Oracle" src="https://img.shields.io/badge/Oracle-F80000?logo=oracle&logoColor=fff&style=for-the-badge" />