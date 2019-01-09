grafana
=======

Ansible role that installs and configures Grafana.

The configuration of the role is done in such way that it should not be necessary
to change the role for any kind of configuration. All can be done either by
changing role parameters or by declaring completely new configuration as a
variable. That makes this role absolutely universal. See the examples below for
more details.

Please report any issues or send PR.


Example
-------

```yaml
---

- name: Example of how to use the role
 hosts: myhost1
  roles:
    - grafana

- name: Example of how to modify the grafana configuration
  hosts: myhost2
  vars:
    # Define Datasource for Datasource provisioning
    grafana_provisioning_datasource_datasources:
      - name: Prometheus
        type: prometheus
        access: proxy
        orgId: 1
        url: http://localhost:9090
        isDefault: yes
        editable: no
        version: 1
    # Instruct the Datasource provisioner to delete a Datasource
    grafana_provisioning_datasource__custom:
      deleteDatasources:
        - name: Graphite
          orgId: 1
    # Define Dashboard provider for Dashboard provisioning
    grafana_provisioning_dashboard_providers:
      - name: default
        orgId: 1
        folder: ""
        type: file
        disableDeletion: no
        updateIntervalSeconds: 10
        options:
          path: /var/lib/grafana/dashboards
    # Install Grafana piechart panel plugin
    grafana_plugins:
      - name: grafana-piechart-panel
  roles:
    - grafana
```


Role variables
--------------

List of variables used by the role:

```yaml
```


Dependencies
------------

- [`config_encoder_filters`](https://github.com/picotrading/ansible-config_encoder_filters)
- [`prometheus`](https://github.com/jtyr/ansible-prometheus) (optional)
- [`alertmanager`](https://github.com/jtyr/ansible-alertmanager) (optional)
- [`telegraf`](https://github.com/jtyr/ansible-telegraf) (optional)


License
-------

MIT


Author
------

Jiri Tyr
