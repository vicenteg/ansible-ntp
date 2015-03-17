ntp
===

Role which helps to install and configure NTP.

The configuraton of the role is done in such way that it should not be necessary
to change the role for any kind of configuration. All can be done either by
changing role parameters or by declaring completely new configuration as a
variable. That makes this role absolutely universal. See the examples below for
more details.

Please report any issues or send PR.


Example
-------

```
---

# Default installation
- hosts: myhost1
  roles:
    - ntp

# Example of how to set custon NTP server list
- hosts: myhost1
  vars:
    # Override the default variable
    ntp_server:
      - 0.uk.pool.ntp.org
      - 1.uk.pool.ntp.org
      - 2.uk.pool.ntp.org
      - 3.uk.pool.ntp.org
  roles:
    - ntp

# Example of how to extend the default configuration
- hosts: myhost2
  vars:
    # Adding a custom log file on the top of the default configuration
    ntp_config_custom:
      logfile: /var/log/ntp.log
  roles:
    - ntp
```


Role variables
--------------

List of variables used by the role:

```
# Package to be installed (you can force a specific version here)
ntp_pkg: ntp

# Default service name
ntp_service: ntpd

# Default location of the ntp.conf file
ntp_conf_file: /etc/ntp.conf


# Default drift file location
ntp_config_driftfile: /var/lib/ntp/drift

# Default list of NTP servers
ntp_config_server:
  - 0.centos.pool.ntp.org
  - 1.centos.pool.ntp.org
  - 2.centos.pool.ntp.org
  - 3.centos.pool.ntp.org

# Default restrictions
ntp_config_restrict_default:
  - "-4 default kod notrap nomodify nopeer noquery"
  - "-6 default kod notrap nomodify nopeer noquery"
  - 127.0.0.1

# Custom restrictions
ntp_config_restrict_custom: []

# Default configuration
ntp_config_default:
  driftfile: "{{ ntp_config_driftfile }}"
  server: "{{ ntp_config_server }}"
  restrict: "{{
    ntp_config_restrict_default +
    ntp_config_restrict_custom
  }}"

# Custom configuration
ntp_config_custom: {}

# Default ntp configuration
ntp_config: "{{
  ntp_config_default.update(
  ntp_config_custom)}}{{ ntp_config_default }}"
```


Dependencies
------------

* [Jinja2 Encoder Macros](https://github.com/picotrading/jinja2-encoder-macros)


License
-------

MIT


Author
------

Jiri Tyr
