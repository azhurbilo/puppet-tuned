# Puppet Tuned module

This module installs, configures and starts Tuned.

### Requirements

Module has been tested on:

* Puppet 3.7
* RHEL/CentOS 6, 7

Required modules:

* stdlib (https://github.com/puppetlabs/puppetlabs-stdlib)

# Quick Start

Setup

```puppet
include tuned
```

Full configuration options:

```puppet
class { 'opensm':
  enabled             => true|false,  # enable state
  profile             => '...',       # active profile
  dynamic_tuning      => true|false,  # enable dynamic tuning
  sleep_interval      => ...,         # check interval for new commands
  update_interval     => ...,         # update interval for dynamic tuning
  majversion          => ...,         # override detected tuned major version
  main_conf           => '...',       # override absolute path to tuned-main.conf
  profiles_path       => '...',       # override absolute path to custom profiles
  active_profile_conf => '...',       # override filename with active profile
  packages            => [...],       # override list of packages to install
  services            => [...],       # override list of services to start
}
```

Custom profile:

tuned::profile { 'name':
  ensure  => present|absent, # ensure state
  data    => {...},          # hash of hashes for tuned.conf
  scripts => {...},          # hash of scripts content into profile dir.
}

Example:

```puppet
class { 'tuned':
  profile        => 'virtual-host-cfg',
  dynamic_tuning => false,
}

tuned::profile { 'virtual-host-cfq':
  ensure  => present,
  data    => {
    'main'   => { 'include'  => 'virtual-host', },
    'disk'   => { 'elevator' => 'cfq', }
    'script' => { 'script'   => 'dummy.sh', }
  },
  scripts => {
    'dummy.sh' => '#!/bin/bash\n/bin/true',
  }
}
```

# Facts

### $::tuned\_version

Tries to detect installed Tuned version. E.g.:

```json
"2.4.1"
```

or for versions 0.x

```json
"unknown"
```

### $::tuned\_profile

Returns current active profile. E.g.:

```json
"virtual-host-cfq"
```

### $::tuned\_profiles

Returns list of available profiles. E.g.:

```json
["balanced", "desktop", "latency-performance", "network-latency",
 "network-throughput", "powersave", "throughput-performance",
 "virtual-guest", "virtual-host"]
```

***

CERIT Scientific Cloud, <support@cerit-sc.cz>