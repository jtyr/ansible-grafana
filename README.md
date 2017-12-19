grafana
=======

Role to install and configure Grafana - the metrics dashboard and graph editor.

It supports both [RPM package](http://docs.grafana.org/installation/rpm/)
and [DEB pacakage](http://docs.grafana.org/installation/debian/)
available in the official [repos](https://packagecloud.io/grafana/stable).

The configuration of the role is done in such way that it should not be necessary
to change the role for any kind of configuration. All can be done either by
changing role parameters or by declaring completely new configuration as a
variable. That makes this role absolutely universal. See the examples below for
more details.

Please report any issues or send PR.

(See *Dependancies* below for details on config_encoder_filters role)

Example
-------

```
---

- name: Example of how to use the role
  hosts: myhost1
  roles:
    - config_encoder_filters
    - grafana

- name: Example of how to modify the grafana configuration
  hosts: myhost2
  vars:
    # Allow grafana to bind to TCP port <1024
    grafana_allow_low_port: true
    # Changing port nunmber from the default 3000 to 80
    grafana_config_server:
      http_port: 80
  roles:
    - config_encoder_filters
    - grafana
```


Role variables
--------------

List of variables used by the role:

```
# GPG key used to sign both rpm and deb packages
grafana_repo_key: https://packagecloud.io/gpg.key

# APT repo URL for grafana
grafana_debian_apt_repository: deb https://packagecloud.io/grafana/stable/debian/ jessie main

# YUM repo URL
grafana_yumrepo_url: https://packagecloud.io/grafana/stable/el/$releasever/$basearch

# Custom YUM repo params
grafana_yumrepo_params: {}

# Package to be installed (you can force a specific version here)
grafana_pkg: grafana

# Allow grafana to bind to TCP port <1024
grafana_allow_low_port: no

# Name of the Grafana service
grafana_service: grafana-server


# Defautl values of the Grafana sysconfig options
grafana_sysconfig_grafana_user: grafana
grafana_sysconfig_grafana_group: grafana
grafana_sysconfig_grafana_home: /usr/share/grafana
grafana_sysconfig_log_dir: /var/log/grafana
grafana_sysconfig_data_dir: /var/lib/grafana
grafana_sysconfig_max_open_files: 10000
grafana_sysconfig_conf_dir: /etc/grafana
grafana_sysconfig_conf_file: "{{ grafana_sysconfig_conf_dir ~ '/grafana.ini' }}"
grafana_sysconfig_restart_on_upgrade: "false"
grafana_sysconfig_plugins_dir: /var/lib/grafana/plugins

# Defautl options of the Grafana sysconfig
grafana_sysconfig__default:
  grafana_user: "{{ grafana_sysconfig_grafana_user }}"
  grafana_group: "{{ grafana_sysconfig_grafana_group }}"
  grafana_home: "{{ grafana_sysconfig_grafana_home }}"
  log_dir: "{{ grafana_sysconfig_log_dir }}"
  data_dir: "{{ grafana_sysconfig_data_dir }}"
  max_open_files: "{{ grafana_sysconfig_max_open_files }}"
  conf_dir: "{{ grafana_sysconfig_conf_dir }}"
  conf_file: "{{ grafana_sysconfig_conf_file }}"
  restart_on_upgrade: "{{ grafana_sysconfig_restart_on_upgrade }}"
  plugins_dir: "{{ grafana_sysconfig_plugins_dir }}"

# Custom options of the Grafana sysconfig
grafana_sysconfig__custom: {}

# Final Grafana sysconfig
grafana_sysconfig: "{{
  grafana_sysconfig__default.update(grafana_sysconfig__custom) }}{{
  grafana_sysconfig__default }}"


# Logging verbosity of the LDAP config
grafana_ldap_config_verbose_logging: false

# Values of the default Servers section of the LDAP config
grafana_ldap_config_servers_host: 127.0.0.1
grafana_ldap_config_servers_port: 389
grafana_ldap_config_servers_use_ssl: false
grafana_ldap_config_servers_ssl_skip_verify: false
grafana_ldap_config_servers_bind_dn: cn=admin,dc=grafana,dc=org
grafana_ldap_config_servers_bind_password: grafana
grafana_ldap_config_servers_search_filter: "(cn=%s)"
grafana_ldap_config_servers_search_base_dns:
  - dc=grafana,dc=org

# Default Servers section of the LDAP config
grafana_ldap_config_servers__default:
  - host: "{{ grafana_ldap_config_servers_host }}"
    port: "{{ grafana_ldap_config_servers_port }}"
    use_ssl: "{{ grafana_ldap_config_servers_use_ssl }}"
    ssl_skip_verify: "{{ grafana_ldap_config_servers_ssl_skip_verify }}"
    bind_dn: "{{ grafana_ldap_config_servers_bind_dn }}"
    bind_password: "{{ grafana_ldap_config_servers_bind_password }}"
    search_filter: "{{ grafana_ldap_config_servers_search_filter }}"
    search_base_dns: "{{ grafana_ldap_config_servers_search_base_dns }}"

# Custom Servers section of the LDAP config
grafana_ldap_config_servers__custom: []

# Final Servers section of the LDAP config
grafana_ldap_config_servers: "{{
  grafana_ldap_config_servers__default +
  grafana_ldap_config_servers__custom }}"

# Values of the default Servers Atributtes section of the LDAP config
grafana_ldap_config_servers_attributes_name: givenName
grafana_ldap_config_servers_attributes_surname: sn
grafana_ldap_config_servers_attributes_username: cn
grafana_ldap_config_servers_attributes_member_of: memberOf
grafana_ldap_config_servers_attributes_email: email

# Default Servers Atributtes section of the LDAP config
grafana_ldap_config_servers_attributes__default:
  name: "{{ grafana_ldap_config_servers_attributes_name }}"
  surname: "{{ grafana_ldap_config_servers_attributes_surname }}"
  username: "{{ grafana_ldap_config_servers_attributes_username }}"
  member_of: "{{ grafana_ldap_config_servers_attributes_member_of }}"
  email: "{{ grafana_ldap_config_servers_attributes_email }}"

# Custom Servers Atributtes section of the LDAP config
grafana_ldap_config_servers_attributes__custom: {}

# Final Servers Atributtes section of the LDAP config
grafana_ldap_config_servers_attributes: "{{
  grafana_ldap_config_servers_attributes__default.update(grafana_ldap_config_servers_attributes__custom) }}{{
  grafana_ldap_config_servers_attributes__default }}"

# Values of the default Servers Group Mappings section of the LDAP config
grafana_ldap_config_servers_group_mappings_admin_group_dn: cn=admins,dc=grafana,dc=org
grafana_ldap_config_servers_group_mappings_editor_group_dn: cn=users,dc=grafana,dc=org
grafana_ldap_config_servers_group_mappings_viewer_group_dn: "*"

# Default Servers Group Mappings section of the LDAP config
grafana_ldap_config_servers_group_mappings__default:
  - group_dn: "{{ grafana_ldap_config_servers_group_mappings_admin_group_dn }}"
    org_role: Admin
  - group_dn: "{{ grafana_ldap_config_servers_group_mappings_editor_group_dn }}"
    org_role: Editor
  - group_dn: "{{ grafana_ldap_config_servers_group_mappings_viewer_group_dn }}"
    org_role: Viewer

# Custom Servers Group Mappings section of the LDAP config
grafana_ldap_config_servers_group_mappings__custom: []

# Final Servers Group Mappings section of the LDAP config
grafana_ldap_config_servers_group_mappings: "{{
  grafana_ldap_config_servers_group_mappings__default +
  grafana_ldap_config_servers_group_mappings__custom }}"

# Default LDAP config
grafana_ldap_config__default:
  verbose_logging: "{{ grafana_ldap_config_verbose_logging }}"
  servers: "{{ grafana_ldap_config_servers }}"
  servers.attributes: "{{ grafana_ldap_config_servers_attributes }}"
  servers.group_mappings: "{{ grafana_ldap_config_servers_group_mappings }}"

# Custom LDAP config
grafana_ldap_config__custom: {}

# Final LDAP config
grafana_ldap_config: "{{
  grafana_ldap_config__default.update(grafana_ldap_config__custom) }}{{
  grafana_ldap_config__default }}"


# Values of the default options of the Analytics section of the Grafana config
grafana_config_analytics_check_for_updates: "true"

# Default options of the Analytics section of the Grafana config
grafana_config_analytics__default:
  check_for_updates: "{{ grafana_config_analytics_check_for_updates }}"

# Custom options of the Analytics section of the Grafana config
grafana_config_analytics__custom: {}

# Contents of the default sections of the Grafana config
grafana_config_paths: {}
grafana_config_server: {}
grafana_config_database: {}
grafana_config_session: {}
grafana_config_analytics: "{{
  grafana_config_analytics__default.update(grafana_config_analytics__custom) }}{{
  grafana_config_analytics__default }}"
grafana_config_security: {}
grafana_config_snapshots: {}
grafana_config_users: {}
grafana_config_auth_anonymous: {}
grafana_config_auth_github: {}
grafana_config_auth_google: {}
grafana_config_auth_proxy: {}
grafana_config_auth_basic: {}
grafana_config_auth_ldap: {}
grafana_config_smtp: {}
grafana_config_emails: {}
grafana_config_log: {}
grafana_config_lob_console: {}
grafana_config_log_file: {}
grafana_config_event_publisher: {}
grafana_config_dashboard_json: {}

# Default options of the Grafana config
grafana_config__default:
  paths: "{{ grafana_config_paths }}"
  server: "{{ grafana_config_server }}"
  database: "{{ grafana_config_database }}"
  session: "{{ grafana_config_session }}"
  analytics: "{{ grafana_config_analytics }}"
  security: "{{ grafana_config_security }}"
  snapshots: "{{ grafana_config_snapshots }}"
  users: "{{ grafana_config_users }}"
  auth.anonymous: "{{ grafana_config_auth_anonymous }}"
  auth.github: "{{ grafana_config_auth_github }}"
  auth.google: "{{ grafana_config_auth_google }}"
  auth.proxy: "{{ grafana_config_auth_proxy }}"
  auth.basic: "{{ grafana_config_auth_basic }}"
  auth.ldap: "{{ grafana_config_auth_ldap }}"
  smtp: "{{ grafana_config_smtp }}"
  emails: "{{ grafana_config_emails }}"
  log: "{{ grafana_config_log }}"
  log.console: "{{ grafana_config_lob_console }}"
  log.file: "{{ grafana_config_log_file }}"
  event_publisher: "{{ grafana_config_event_publisher }}"
  dashboards.json: "{{ grafana_config_dashboard_json }}"

# Custom options of the Grafana config
grafana_config__custom: {}

# Final Grafana config
grafana_config: "{{
  grafana_config__default.update(grafana_config__custom) }}{{
  grafana_config__default }}"
```


Dependencies
------------

* [`config_encoder_filters`](https://github.com/jtyr/ansible-config_encoder_filters)


License
-------

MIT


Author
------

Jiri Tyr
