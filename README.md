# Spiped role for ansible

Create `./library` directory in your ansible project:

```
mkdir ./library
```

And configure `ansible.cfg`:

```
[defaults]
roles_path = ./library
```

Add submodule:

```
git submodule add git@github.com:kressh/ansible-spiped.git library/spiped
```

Add keys to `library/spiped/keys` and define `spiped_containers` variable. For example:

```
spiped_containers:
  - name: postgresql-db01
    source: '[0.0.0.0]:5432'
    target: postgresql:5432
    mode: '-d'
    key: postgresql.key
    links:
      - postgresql
    published_ports:
      - '{{ ip }}:5432:5432'
```

Use role:

```yaml
---
- hosts: db01.yourserver.io
  remote_user: ansible
  become: true
  roles:
    - spiped
```
