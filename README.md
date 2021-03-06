# Automate Liveness Agent

Automate liveness agent sends keepalive messages to Chef Automate, which
prevents nodes that are up but not frequently running Chef Client from
appearing as "missing" in the Automate UI.

For more info about Chef Automate, see [https://www.chef.io/automate/](https://www.chef.io/automate/).

## Installation

Automate liveness agent is designed to be installed via Chef's "Required
Recipe" feature, which is in development. Check back later for full
instructions.

## Usage

Usage instructions will be made available once this software is
released.

### Configuration

The config file is a JSON formatted file like this:

```json
{
  "chef_server_fqdn": "api.chef.io",
  "client_key_path": "/etc/chef/client.pem",
  "client_name": "example-node",
  "daemon_mode": true,
  "data_collector_url": "https://api.chef.io/organizations/chef-oss-dev/data-collector",
  "entity_uuid": "3D0F27ED-AA32-4C73-9A18-F342AAABAA62",
  "install_check_file": "/opt/chef/embedded/bin/ruby",
  "log_file": "/var/log/chef/automate-liveness-agent/automate-liveness-agent.log",
  "org_name": "chef-oss-dev",
  "scheduled_task_mode": false
  "ssl_ca_file": null,
  "ssl_ca_path": null,
  "ssl_verify_mode": "verify_peer",
  "trusted_certs_dir": null,
  "unprivileged_uid": 100,
  "unprivileged_gid": 200,
}

```

Settings:

* `chef_server_fqdn`: what it says
* `client_key_path`: Private key used by Chef Client to authenticate API
  requests
* `client_name`: identity used by Chef Client to authenticate API
  requests
* `daemon_mode`: Whether or not the process should daemonize
* `data_collector_url`: Org-specific data collector endpoint on the Chef
  Server
* `entity_uuid`: UUID generated by Chef Client to identify itself to the
  data collector
* `install_check_file`: Agent will check for this file to detect if chef
  client has been uninstalled. Setting it to Chef's `Gem.ruby` should
  work. Setting to `null` disables the check
* `log_file`: Where the agent should output log entries
* `org_name`: Chef Server organization name
* `scheduled_task_mode`: Run in single shot mode, eg: update once and exit
* `ssl_ca_file`: Override the default CA bundle with a custom file
* `ssl_ca_path`: Override the default CA bundle with a directory containing
  a CA bundle
* `ssl_verify_mode`: The SSL verification mode. eg: `verify_peer` or `verify_none`
* `trusted_certs_dir`: A directory with trusted SSL certificates
* `unprivileged_uid`: Agent will change it's uid to this after start up.
  Setting to `null` disables that
* `unprivileged_gid`: Agent will change it's gid to this after start up.
  Setting to `null` disables that
* `interval`: The number of seconds between agent checkins, defaults to 1800

### Environment Variables

#### `DEBUG`

Setting `DEBUG=1` will enable logging of HTTP request data.

#### `INTERVAL`

By default, the liveness agent will send an update every 30 minutes.
Setting the `INTERVAL` variable will configure the agent to send an
update to the specified value (in seconds).

#### `RUBYOPT`

This application is designed to run with rubygems disabled. This can be
done by setting `RUBYOPT` like so:

```
RUBYOPT="--disable-gems"
```

#### `RUBY_GC_HEAP_GROWTH_MAX_SLOTS`

There are a lot of ways to tune the ruby GC, but setting
`RUBY_GC_HEAP_GROWTH_MAX_SLOTS` is an easy way to prevent ruby from
allocating too much extra memory we don't want it to have.

```
RUBY_GC_HEAP_GROWTH_MAX_SLOTS=500
```

## Development

This project is developed as a typical ruby app with bundler and rspec.

To install deps:

```
bundle install
```

To run tests:

```
bundle exec rspec
```

To test the compiled recipe on a target platform:

```
rake compile_recipe vendor_stable_recipe
kitchen converge required-faux-chef-automate
kitchen test $target-platform
```

To see supported platforms:

```
kitchen list
```

## Contributing

Bug reports should be filed with your support representitive.

