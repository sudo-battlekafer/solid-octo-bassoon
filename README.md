# solid-octo-bassoon
Homelab automation playbook
===========================
Be gentle, this is a Work In Progress

Requirements:
- ansible 2.4+ installed
- terraform
- ssh keys created (need the .pub file)

Parts:
- ansible playbook for orchestration
- hosts.ini file to define inventory
- group_vars/main.yml file for non-secret variables
- group_vars/vars/vault.yml file for secret variables such as password, API keys, etc

To start:
1. copy group_vars/example into a copy and rename with YOUR preference (ex: homelab)
2. edit hosts.ini with the applicable information instructions [hosts info](#hosts)
3. edit group_vars/main.yml with the applicable information [group_vars info](#group_vars)
4. create ansible vault (DON'T USE THE EXAMPLE!) more information [here](#ansible_vault)

# hosts
- rename EXAMPLE to your project (try and match the folder name, makes life easier)
- update IPs/subnet
- save

# group_vars
everything is customizable, but at the minimum, update these:
- admin_ssh_user
- admin_ssh_key (full ssh-rsa dfjklsdfjds user@machine)
- proxmox_storage
- proxmox_vlan_gateway
- proxmox_host_name
- dns_domain
- gitlab_domain (if you use gitlab in this playbook)
- metallb_ip_range
- acme_email
- cloudflare_email
- vault_traefik_auth_base64 (can even keep this in the vault if you like)


# ansible_vault
To create and ansible vault, the command is:
`ansible-vault create <vaultpath/filename>`
EXAMPLE: `ansible-vault create group_vars/vars/main.yml`

If you'd like to use a password file, first create the password file
EXAMPLE: `echo supersecretpassword >> ~/vaultpass.txt`

then create the vault with: 
`ansible-vault create group_vars/vars/main.yml`
type your supersecretpassword in twice and then call the ansible playbook with:
`ansible-playbook -i hosts.ini site.yml --vault-password-file ~/vaultpass.txt`

For this playbook, you need a minimum of FOUR variables in your vault:
- vault_proxmox_host_password: <thepass>
- su_password: <thepass>
- vault_cloudflare_email: <thecloudflareemailaddress>
- vault_cloudflare_api_key: <cloudflare_API_key>

Good link on [digital ocean](https://www.digitalocean.com/community/tutorials/how-to-use-vault-to-protect-sensitive-ansible-data)


