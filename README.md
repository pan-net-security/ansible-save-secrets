ansible-save-secrets
====================

As a complimentary role to `ansible-load-secrets`, this role is able to save secrets to Hashicorp Vault and to local filesystem.

Requirements
------------

This role requires 
* `pip install ansible-hashivault-modules`, and a functional Vault connection for Vault saving OR
* writable directory for local filesystem storage

Role Variables
--------------

* `ass_secret_store` - set either to `vault` or `fs`
* `ass_vault_mount`/`ass_vault_path` for vault path
* `ass_fs_path` - for fs path
* `vars_stored` - list of dicts (see below)

Example Playbook
----------------

Don't forget to set `VAULT_ADDR` and `VAULT_TOKEN` as you are used to.

```
- hosts: all
  vars:
    ass_secret_store: "vault"
    ass_vault_mount: "secret"
    ass_vault_path: "mytest"
    vars_stored:
      - var: "mysecret"
        key: "secret"
  - role: ansible-save-secrets
    become: false
```

`var` is the var name which has to be saved. `key` is the Vault key. In fact, variable will be saved to `{{ ass_vault_mount }}/{{ ass_vault_path }}/{{ var }}, {{ key }} = {{ mysecret }}`.

If you specify `group` for a secret (`var: "mysecret", key: "secret", group: "government"`), the item will be saved to `{{ ass_vault_mount }}/{{ ass_vault_path }}/{{ group }}/{{ var }}, {{ key }} = {{ mysecret }}`.

You can specify `host` for a secret, in that case the secret will be saved to `{{ ass_vault_mount }}/{{ ass_vault_path }}/hostvars/{{ host }}/{{ var }}, {{ key }} = {{ mysecret }}`

You can specify `ignore_undefined` per item; if it's ok that you might try saving undefined variable (you may find that useful).

You can specify `token` per item; otherwise `VAULT_TOKEN` env var is used.

License
-------

GPL

Author Information
------------------

Michal Medvecky
