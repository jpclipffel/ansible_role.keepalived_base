# AWXLab - Common roles - Base Keepalived

Install and configure a generic `keepalived` setup.

## Usage

* Include role using `include_role`
* Do **not** import another task than `main.yml`

Example:

```yaml
include_role:
  name: common/keepalived_base
```

## Run

|Tags|Description|
|----|-----------|
|`setup`|Install and (re)configure `keepalived`|
|`teardown`|Disable `keepalived`, remove its scripts and service|
|`remove`|Remove `keepalived` binary<br>Note: next deployment will takes more time to rebuild it from source|

## Variables

This role configure `keepalived` using a generic configuration profile (`templates/keepalived.conf.j2`) and optionnaly a set of check scripts (`templates/scripts/*.j2`).
The profile is configured using one or more `instances` objects (see bellow).

### Generic

|Variable|Type|Required|Description|
|--------|----|--------|-----------|
|`keepalived_base_profile`|`string`|No|Profile template|

### Profile

|Variable|Default|
|--------|-------|
|`keepalived_base_defaults_script_interval`|`2`|
|`keepalived_base_defaults_interface`|`{{ ansible_default_ipv4.interface }}`|
|`keepalived_base_defaults_state`|`BACKUP`|
|`keepalived_base_defaults_priority`|`150`|
|`keepalived_base_defaults_router_id`|`20`|
|`keepalived_base_defaults_advert_int`|`1`|
|`keepalived_base_defaults_auth_pass`|`1234`|

```yaml
# Keepalived check script
keepalived_base_scripts:                  # Optional ONLY if no check script is referenced in any `keepalived_base_instances`
  - name: "script_name"                   # Required; Script name
    path: "/path/to/script"               # Optional; Full path to script
    interval: 1                           # Optional; Run interval in seconds

# Keepalived VRRP instances
keepalived_base_instances:                # Optional; Keepalived does nothing if there is no instance defined)
  - name: "instance_name"                 # Required; VRRP instance name
    interface: -                          # Optional; Interface name or IP
    state: -                              # Optional; Initial state
    priority: -                           # Optional; Node priority
    router_id: -                          # Optional; Router ID (auto generated and auto incremented)
    advert_int: -                         # Optional; Advertisement interval in seconds
    auth:                                 # Optional; Object
      - pass: -                           # Optional; VRRP password
    peers: []                             # Optional; List of peers (default to Ansible hosts targeted in the play)
    vips: []                              # Optional; List of Virtual IPs
    script: "script_name"                 # Optional; Check script name (as defined in `keepalived_base_scripts{}.name`)
```

## Tags

|`setup`|Install and (re)configure `keepalived`|
|`teardown`|Disable `keepalived`, remove its scripts and service|
|`remove`|Remove `keepalived` binary<br>Note: next deployment will takes more time to rebuild it from source|