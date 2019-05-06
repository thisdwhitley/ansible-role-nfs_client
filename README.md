# Ansible Role: nfs_client

This role will install the packages necessary to mount an NFS share and
optionally configure some remote mounts.

I prefer to pass the variables "into" the role from the playbook versus by
including variable files.  This is because I hope to make the role usable by
other projects/roles.  I don't know if this logic makes sense or not, but I am
essentially attempting to remove the variables from the role itself.

## Overview

At a very high level, this role will:

* install the necessary packages
* [*optional*] mount some specified locations
* [*optional*] create links in a user's home directory if specified

I have had a hard time testing this for various operating systems and I am
currently using it on Fedora.

## Important Notes

* Mounts are system-wide so this is not done per user.  However, the links to
  home directories is per user

## Requirements

Any package or additional repository requirements will be addressed in the role.

## Role Variables

All of these variables should be considered **optional** however, be aware that
sanity checking is minimal (if at all):

* `mounts`
  * this is a ***list*** made up of:
    * `src` the remote location (including IP/resolvable name)
    * `dest` this is the mount point on the local machine ***if this does not
      exist, it will be created***
* `home_link`
  * this is poorly named, but this is the *USER* that should have links
    created in their home directory.  **NOTE: this role will not create the
    user!** *this currently takes a single user*


## Example Playbook

Playbook with configuration options specified:

```yaml
- hosts: localhost
  connection: local
  roles:
    - role: nfs_client
      mounts:
        - { src: "192.168.1.100:/documents", dest: "/remote/docs" }
      home_link:
        - "{{ new_user | default( ansible_user_id ) }}"
```

## Inclusion

I envision this role being included in a larger project through the use of a
`requirements.yml` file.  So here is an example of what you would need in your
file:

```yaml
# get the nfs_client role from github
- src: https://github.com/thisdwhitley/ansible-role-nfs_client.git
  scm: git
  name: nfs_client
```

Have the above in a `requirements.yml` file for your project would then allow
you to "install" it (prior to use in some sort of setup script?) with:

```bash
ansible-galaxy install -p ./roles -r requirements.yml
```

## Testing

Because this really relies on OS level items, testing has proven difficult.  I
am successfully running it on Fedora.

## To-do

* verify that I'm testing what I think I'm testing

## References

* N/A

## License

All parts of this project are made available under the terms of the [MIT
License](LICENSE).