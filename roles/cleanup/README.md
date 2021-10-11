# Cleanup

Ansible role to cleanup environments.

For more information see [collection's README](../../README.md).

## Index

- [Variables](#variables)
- [License](#license)
- [Author](#author)

## Variables

Supported variables are displayed along with
their default values (*./defaults/main.yml*):

```
cleanup_crontab_names: []
cleanup_custom_command: []
cleanup_db_names: []
cleanup_db_password: ''
cleanup_db_user: 'root'
cleanup_file_path: []
cleanup_line_delete: []
cleanup_package_uninstall: []
cleanup_package_snap_uninstall: []
cleanup_reboot_os: false
cleanup_services_reload: []
cleanup_services_restart: []
```

## License

MIT.

## Autor

![Runitcr](../../img/author.png)

[Runitcr](https://runitcr.com).