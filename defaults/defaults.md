# Role variables

The role variables are divided in two, first there's variables that controll the install of thelounge, like version and install method.

The second part is all the settings for the config. These are arranged in dicts, grouped together by functionality. 

If you want to change a single key in the dicts, then fill these out in the override dicts, otherwise you'll have to copy the full dicts, as you'd otherwise override them entirely.

for example, to just change the port and debug settings:

```yaml
thelounge_server_override:
  port: 9001
  debug:
    framework: true
```

The role will replace just the specified keys in the default dict.  
This isn't necesarry if you have _hash_behaviour = merge_ in your ansible config.

## Control variables:

```yaml
# This can be latest, or a release version from github. Omit the preceeding v 
# thelounge_version: '3.0.1'
# thelounge_version: '3.1.0-pre.2
#
# If pre is present in the version number, the role will install thelounge via yarn
# WARNING: It will not check for or remove any previous installs 
thelounge_version: 'latest'
```

## Config variables:

```yaml
# The config variables are documented in the config.js template
# Variables that deviate from thelounge defaults are explained

# Use the override in your playbook to override individual keys
thelounge_server_override: {}
thelounge_server:
  public: false
  port: 9000
  host: 'undefined'
  bind: 'undefined'
  reverse_proxy: false
  max_history: 10000
  https:
    enabled: false
    key: ''
    cert: ''
    ca: ''
  debug:
    framework: false
    rawa: false

# Use the override in your playbook to override individual keys
thelounge_client_override: {}
thelounge_client:
  leave_msg: 'The Lounge - https://thelounge.chat'
  theme: 'default'
  transports:
    - 'polling'
    - 'websocket'
  prefetch:
    enabled: false

    # prefetch storage has been set to true to prioritise client privacy
    # over vps storage space. 
    # Worst case, assuming only links to prefeched content is posted, 
    # this will consume 20 GiB per channel with 10000 max history
    storage: true
    max_size: 2048
  upload:
    enable: false
    max_size: 10240

# Use the override in your playbook to override individual keys
thelounge_network_override: {}
thelounge_network:
  default:
    name: 'Freenode'
    host: 'chat.freenode.net'
    port: 6697
    password: ''
    tls: true
    reject_unauthorized: true
    nick: 'thelounge%%'
    username: 'thelounge'
    realname: 'The Lounge User'
    join: '#thelounge'
  display: true
  lock: false

# Use the override in your playbook to override individual keys
thelounge_user_mgmt_override: {}
thelounge_user_mgmt:
  # To ensure that messages will be reloaded on restarts, only use sqlite
  message_storage:
    - sqlite
  use_hex_ip: false
  webirc: 'null'
  identd:
    enable: false
    port: 113
  oidentd: 'null'
  
# Use the override in your playbook to override individual keys
thelounge_ldap_override: {}
thelounge_ldap:
  enable: false
  url: 'ldaps://example.com'
  tls_options: {}
  # tls_option:
  #   - "host: 'my::ip::v6'"
  #   - "servername: 'example.com'"
  primary_key: 'uid'
  search_dn:
    root_dn:
      - 'cn=thelounge'
      - 'ou=system-users'
      - 'dc=example,dc=com'
    root_pass: "1234"
    filter: "(objectClass=person)(memberOf=ou=accounts,dc=example,dc=com)"
    base: 
      - 'dc=example'
      - 'dc=com'
    scope: "sub"
```