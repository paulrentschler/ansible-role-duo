paulrentschler.duo
==================

[![MIT licensed][mit-badge]][mit-link]

Installs and configures Duo Security's Duo Unix with Pluggable Authentication Modules (PAM) support via the `pam_duo` module on Ubuntu Linux.

It does **not** use the `login_duo` module although an [Ansible role to do that does exist](https://github.com/jlafon/ansible-duo-security).


Requirements
------------

You will need a Duo account and the ability to get integration and secret keys from the Duo Admin Panel. The [documentation for Duo Unix](https://duo.com/docs/duounix) explains how to do this.


Role Variables
--------------

The following variables are available with defaults defined in `defaults/main.yml`:

### Required configuration settings

The integration and secret key should be stored in Ansible Vault and passed in when the role is invoked.

Integration key obtained from the Duo Admin Panel.

    duo_integration_key: "obtained-from-Duo"

Secret key (like a password) obtained from the Duo Admin Panel.

    duo_secret_key: "obtained-from-Duo"

The fully-qualified host name of the host.

    duo_host: "{{ inventory_hostname }}"


### Optional configuration settings

Additional information on the options below can be found [in the Duo documentation](https://duo.com/docs/duounix#duo-configuration-options).

Automatically send a push login request to the user's phone.

    duo_autopush: "no"

Service or configuration errors will not prevent Duo authentication.

    duo_failmode: "safe"

When "yes", if Duo Unix cannot detect the IP address of the client, Duo Unix will send the IP address of the server it is running on.

    duo_fallback_local_ip: "no"

Number of seconds to wait for HTTPS responses from Duo Security (0 disables the timeout).

    duo_https_timeout: "0"

When "yes", prints /etc/motd to the screen after successful login.

    duo_motd: "no"

Maximum number of times Duo Unix will prompt the user to authenticate when the second factor fails.

    duo_prompts: "3"

When "yes", includes information such as the command being executed in the Duo Push message.

    duo_pushinfo: "no"

If specified, Duo authentication is required only for the users in the groups specified (comma-delimited list, no spaces), unless a group is negated (!)

    # duo_groups: "users,!wheel,!*admin"

HTTP proxy to use, including authentication if needed.

    # duo_http_proxy: http://username:password@proxy.example.com:8080


Dependencies
------------

None.


Example Playbook
----------------

Simple example:

    - hosts: all
      roles:
        - role: paulrentschler.duo
          vars:
            duo_integration_key: "{{ duo_integration_key_vaulted }}"
            duo_secret_key: "{{ duo_secret_key_vaulted }}"

More complicated example that specifies more parameters including specifying the host, turning on auto-push, specifying a https timeout, and excluding users in the `root` group from two-factor authentication via Duo.

    - hosts: all
      roles:
        - role: paulrentschler.duo
          vars:
             duo_integration_key: "{{ duo_integration_key_vaulted }}"
             duo_secret_key: "{{ duo_secret_key_vaulted }}"
             duo_host: "secure.example.com"
             duo_autopush: yes
             duo_https_timeout: 60
             duo_groups: "!root"


License
-------

[MIT][mit-link]


Author Information
------------------

Created by Paul Rentschler in 2021.


[mit-badge]: https://img.shields.io/badge/license-MIT-blue.svg
[mit-link]: https://github.com/paulrentschler/ansible-role-duo/blob/master/LICENSE
