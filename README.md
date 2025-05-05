ansible-role-nfs-ganesha-krb5
=========

A role to set up [NFS-Ganesha](https://github.com/nfs-ganesha/nfs-ganesha/) on
RHEL9 with Kerberos authentication.

Requirements
------------

No external requirements.

Role Variables
--------------

### Generic Variables

- **`ganesha_add_repo`**: (Default: `false`) Whether to add the Ganesha
  repository.
- **`ganesha_repo_baseurl`**: (Default: `http://127.0.0.1/ganesha`) Base URL
  for the Ganesha repository.
- **`ganesha_install_packages`**: (Default: `false`) Whether to install
  required Ganesha packages.
- **`ganesha_packages`**: (Default: List of packages) List of packages to
  install for Ganesha.

### Config Values

The role supports a subset of Ganesha config keys directly by variables using
the format `ganesha_BLOCK_NAME__CONFIG_NAME`. For example,
`ganesha_nfs_krb5__principalname: nfs` becomes the following config:

```conf
NFS_KRB5 {
  principalname: nfs
}
```

Each supported config block has a corresponding `_extra` variable (such as
`ganesha_nfs_krb5_extra`) that can be used to extend the configuration beyond
what's directly supported by the role variables.

#### General Configuration

- **`ganesha_config_extra`**: (Default: `""`) Free-form data appended at the
  end of the config file.

#### NFS_CORE_PARAM

- **`ganesha_nfs_core__enable_rquota`**: (Default: `"false"`) Enable or disable
  RQUOTA.
- **`ganesha_nfs_core__nfs_port`**: (Default: `2049`) Port for NFS.
- **`ganesha_nfs_core__nfs_protocols`**: (Default: `4`) NFS protocol version.
- **`ganesha_nfs_core__enable_fullv4_stats`**: (Default: `"true"`) Enable or
  disable full NFSv4 statistics.
- **`ganesha_nfs_core_extra`**: (Default: `""`) Additional configuration for
  NFS_CORE_PARAM.

#### NFS_KRB5

- **`ganesha_nfs_krb5__principalname`**: (Default: `nfs`) Kerberos principal
  name.
- **`ganesha_nfs_krb5__active_krb5`**: (Default: `"true"`) Enable or disable
  Kerberos authentication.
- **`ganesha_nfs_krb5_extra`**: (Default: `""`) Additional configuration for
  NFS_KRB5.

#### EXPORTS

- **`ganesha_exports`**: (Default: List of exports) List of export
  configurations. Each export can define:
  - `export_id`: Unique ID for the export.
  - `path`: Path to export.
  - `pseudo`: Pseudo path for the export.
  - Additional optional parameters like `access_type`, `squash`, `disable_acl`,
    `transports`, `sectype`, `manage_gids`, and `extra`.
    - The `extra` parameter can be used to append arbitrary config to the
      entry.
  - **`client`**: List of client configurations. Each client can define:
    - `clients`: Client IP or subnet.
    - Additional optional parameters like `sectype`.

- **`ganesha_export_default__access_type`**: (Default: `RW`) Default access
  type for all exports.
- **`ganesha_export_default__squash`**: (Default: `root_squash`) Default squash
  setting for all exports.
- **`ganesha_export_default__disable_acl`**: (Default: `"true"`) Default ACL
  setting for all exports.
- **`ganesha_export_default__transports`**: (Default: `tcp`) Default transport
  protocol for all exports.
- **`ganesha_export_default__sectype`**: (Default: `krb5`) Default security
  type for all exports.
- **`ganesha_export_default__manage_gids`**: (Default: `"true"`) Default GID
  management setting for all exports.
- **`ganesha_export_default_extra`**: (Default: `""`) Additional default
  configuration for all exports.

#### NFSV4

- **`ganesha_nfsv4__allow_numeric_owners`**: (Default: `"true"`) Allow numeric
  owners in NFSv4.
- **`ganesha_nfsv4__only_numeric_owners`**: (Default: `"true"`) Restrict to
  numeric owners in NFSv4.
- **`ganesha_nfsv4__usegetpwnam`**: (Default: `"true"`) Use `getpwnam` for user
  lookups in NFSv4.
- **`ganesha_nfsv4_extra`**: (Default: `""`) Additional configuration for
  NFSV4.

#### DIRECTORY_SERVICES

- **`ganesha_directory_services__idmapped_user_time_validity`**:
  (Default: `600`) Validity time for ID-mapped users.
- **`ganesha_directory_services__idmapped_group_time_validity`**:
  (Default: `600`) Validity time for ID-mapped groups.
- **`ganesha_directory_services__domainname`**: (Default: `EXAMPLE.COM`) Domain
  name for directory services.
- **`ganesha_directory_services__idmapping_active`**: (Default: `"true"`)
  Enable or disable ID mapping.
- **`ganesha_directory_services_extra`**: (Default: `""`) Additional
  configuration for directory services.

#### LOG

- **`ganesha_log__default_log_level`**: (Default: `INFO`) Default log level.
- **`ganesha_log__components`**: (Default: `[]`) List of per-component log
  levels. Example:

  ```yaml
  - IDMAPPER: FULL_DEBUG
  - FSAL: WARN
  ```

- **`ganesha_log_extra`**: (Default: `""`) Additional configuration for
  logging.

Dependencies
------------

No dependencies.

Example Playbook
----------------

```yaml
- hosts: servers
  roles:
  - role: ansible-role-nfs-ganesha-krb5
    vars:
      ganesha_exports:
        - export_id: 1
          path: /export/ganesha
          pseudo: /ganesha
          client:
          - clients: 192.168.0.0/16
            sectype: sys
```

License
-------

MIT

Author Information
------------------

Sami Laine, Aalto Scientific Computing

<https://scicomp.aalto.fi/triton/help/#triton-support-team>
