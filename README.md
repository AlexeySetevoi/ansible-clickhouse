ansible-clickhouse [![Build Status](https://travis-ci.org/AlexeySetevoi/ansible-clickhouse.svg?branch=develop)](https://travis-ci.org/AlexeySetevoi/ansible-clickhouse)
=========

Simple clickhouse-server deploy and management role.
Any issues and pr are welcome.

Role Variables
--------------

F: You can create custom profiles
```yaml
clickhouse_profiles_custom:
 my_custom_profile:
   max_memory_usage: 10000000000
   use_uncompressed_cache: 0
   load_balancing: random
   my_super_param: 9000
```

Allow any plain k-v. Transform to xml
```xml
<profiles>
    <!-- Profiles of settings. -->
    <!-- Default profiles. -->
        <default>
            <max_memory_usage>10000000000</max_memory_usage>
            <load_balancing>random</load_balancing>
            <use_uncompressed_cache>0</use_uncompressed_cache>
        </default>
        <readonly>
            <readonly>1</readonly>
        </readonly>
        <!-- Default profiles end. -->
        <!-- Custom profiles. -->
        <my_custom_profile>
            <max_memory_usage>10000000000</max_memory_usage>
            <load_balancing>random</load_balancing>
            <use_uncompressed_cache>0</use_uncompressed_cache>
            <my_super_param>0</my_super_param>
        </my_custom_profile>
        <!-- Custom profiles end. -->
</profiles>
```

F: You can create custom users:
```yaml
clickhouse_users_custom:
      - { name: "testuser",
          password_sha256_hex: "f2ca1bb6c7e907d06dafe4687e579fce76b37e4e93b7605022da52e6ccc26fd2",
          networks: "{{ clickhouse_networks_default }}",
          profile: "default",
          quota: "default",
          dbs: {testu1} ,
          comment: "classic user with plain password"}
      - { name: "testuser2",
          password: "testplpassword",
          networks: "{{ clickhouse_networks_default }}",
          profile: "default",
          quota: "default",
          dbs: {testu2} ,
          comment: "classic user with hex password"}
      - { name: "testuser3",
          password: "testplpassword",
          networks: { 192.168.0.0/24, 10.0.0.0/8 },
          profile: "default",
          quota: "default",
          dbs: {testu1,testu2,testu3} ,
          comment: "classic user with multi dbs and multi-custom network allow password"}
```

F: You can manage own quotas:
```yaml
clickhouse_quotas_custom:
 - { name: "my_custom_quota", intervals: "{{ clickhouse_quotas_intervals_default }}",comment: "Default quota - count only" }
```
Quote object is simple dict:
```yaml
 - { duration: 3600, queries: 0, errors: 0,result_rows: 0,read_rows: 0,execution_time: 0 }
```

F: You can create any databases:
```yaml
clickhouse_dbs_custom:
      - testu1
      - testu2
      - testu3
```

F: Flag for remove clickhouse from host(disabled by default)
```yaml
clickhouse_remove: no
```

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:
```yaml
  - hosts: localhost
    remote_user: root
    vars:
      clickhouse_users_custom:
          - { name: "testuser",
              password_sha256_hex: "f2ca1bb6c7e907d06dafe4687e579fce76b37e4e93b7605022da52e6ccc26fd2",
              networks: "{{ clickhouse_networks_default }}",
              profile: "default",
              quota: "default",
              dbs: {testu1} ,
              comment: "classic user with plain password"}
          - { name: "testuser2",
              password: "testplpassword",
              networks: "{{ clickhouse_networks_default }}",
              profile: "default",
              quota: "default",
              dbs: {testu2} ,
              comment: "classic user with hex password"}
          - { name: "testuser3",
              password: "testplpassword",
              networks: { 192.168.0.0/24, 10.0.0.0/8 },
              profile: "default",
              quota: "default",
              dbs: {testu1,testu2,testu3} ,
              comment: "classic user with multi dbs and multi-custom network allow password"}
      clickhouse_dbs_custom:
         - testu1
         - testu2
         - testu3
    roles:
      - ansible-clickhouse
```

License
-------

BSD

Author Information
------------------

[ClickHouse](https://clickhouse.yandex/docs/en/index.html) by [Yandex LLC](https://yandex.ru/company/).

Role by [AlexeySetevoi](https://github.com/AlexeySetevoi).
