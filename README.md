# Sonatype Nexus Repository Manager configuration (MVP)

[![Galaxy Quality](https://img.shields.io/ansible/quality/57180?style=flat&logo=ansible)](https://galaxy.ansible.com/monolithprojects/nexus_config)
[![Role version](https://img.shields.io/github/v/release/MonolithProjects/ansible-nexus_config)](https://galaxy.ansible.com/monolithprojects/nexus_config)
[![Role downloads](https://img.shields.io/ansible/role/d/57180)](https://galaxy.ansible.com/monolithprojects/nexus_config)
<!-- [![GitHub Actions](https://github.com/MonolithProjects/ansible-nexus_config/workflows/molecule%20test/badge.svg?branch=master)](https://github.com/MonolithProjects/ansible-nexus_config/actions) -->
[![License](https://img.shields.io/github/license/MonolithProjects/ansible-nexus_config)](https://github.com/MonolithProjects/ansible-nexus_config/blob/main/LICENSE)

This Ansible role will configure Sonatype Nexus Repository Manager using the Rest API.
Currently this role is just an MVP. It supports:

- [x] Initial admin password setup
- [x] Users creation
- [x] Users update
- [x] Users deletion
- [x] Blob storage (file) creation
- [x] Blob storage (file) update
- [x] Blob storage (file) deletion
- [x] Blob storage (AWS S3) creation
- [x] Blob storage (AWS S3) update
- [x] Blob storage (AWS S3) deletion
- [ ] Blob storage (Azure) creation
- [ ] Blob storage (Azure) update
- [ ] Blob storage (Azure) deletion
- [x] Repositories (Maven) creation
- [x] Repositories (Maven) update
- [x] Repositories (Maven) deletion
- [ ] Roles creation
- [ ] Roles update
- [ ] Roles detetion
- TBD ...

## Requirements

Ansible >= 2.10

## Tested on:

- Nexus repository Manager 3.37.0-01
- Fedora 35

## Role Variables

This is a copy of `defaults/main.yml`

```yaml
---

# Administrator user name
admin_username: admin

# Initial Nexus admin password
initial_admin_password: admin123

# Admin password which will be set during the initial setup.
admin_password: "{{ lookup('env', 'ADMIN_PASSWORD') }}"

# Nexus API port
api_port: 8081

# Nexus endpoint protocol
api_protocol: http

# Hide sensitive Ansible error logs (may contain passwords)
hide_sensitive_logs: true

# Anonymous access
anonymous_access: true

users: []
  # - id: joan                    # User ID
  #   first_name: Joan            # User's first name
  #   last_name: Doe              # User's last name
  #   email: joan@example.org     # Email
  #   password: nbusr123          # Password ( do not push it to git :) )
  #   status: active              # Status of the user. You can set active/disabled or deleted to delete the user.
  #   source: default             # Source
  #   roles:                      # List of the assigned roles
  #     - nx-admin
  # - id: joe
  #   first_name: Joe
  #   last_name: Doe
  #   email: joe@example.org
  #   password: "{{ lookup('env', 'JOE_PASSWORD') }}"
  #   status: disabled
  #   source: default
  #   roles:
  #     - nx-anonymous

stores: []
  # - name: file_blob             # Blob Store name
  #   type: file                  # Blob Store type (file, s3)
  #   soft_quota: 0               # Blob Store quota
  #   path: /tmp/blobs
  #   status: active              # Blob Store status (active, deleted)
  # - name: s3_blog
  #   type: s3
  #   soft_quota: 0
  #   prefix: ""
  #   region: default
  #   expiration_days: -1
  #   status: active

repositories: []
  # - name: maven_repo_hosted
  #   online: true                                  # Repository state (true, false, deleted)
  #   type: maven                                   # Repository type (Currently supported: maven)
  #   kind: hosted                                  # Repository kind (hosted, proxy)
  #   blob_store: default                           # Blob storeage
  #   strict_content_type_validation: false         # Strict Content Type Validation
  #   version_policy: MIXED                         # Version Policy (MIXED, RELEASE, SNAPSHOT)
  #   layout_policy: STRICT                         # Layout Policy (STRICT, PERMISSIVE)
  #   content: INLINE                               # Content Disposition (INLINE)

  # - name: maven_repo_proxy
  #   online: true
  #   type: maven
  #   kind: proxy
  #   blob_store: default
  #   strict_content_type_validation: false
  #   remote_url: https://maven.example.org/repo    # Remote repository url
  #   maximum_artifacts_age: -1                     # Maximum component age
  #   maximum_metadata_age: 1440                    # Maximum metadata age
  #   negative_cache: true                          # Not found cache
  #   not_found_cache_ttl: 1440                     # Not found cache TTL
  #   http_client:
  #     blocked: false
  #     auto_block: true
  #     connection:
  #       retries: 0
  #       user_agent_suffix: ""
  #       timeout: 60
  #       enable_circular_redirects: false
  #       enable_cookies: false
  #       user_trust_store: false
  #     authentication:                               # Remote repo authentication
  #       type: username                              # Authetication type (username, ntlm)
  #       username: joe
  #       password: nbusr123
  #       ntlm_host:
  #       ntlm_domain:
  #       preemptive: false
  #   routing_rule: null
  #   version_policy: MIXED
  #   layout_policy: STRICT
  #   content: INLINE

  # - name: maven_repo_group
  #   online: true
  #   type: maven
  #   kind: group
  #   blob_store: default
  #   strict_content_type_validation: false
  #   group:
  #     - maven-releases
  #     - maven-snapshots

```

## Example Playbook

In this example the playbook will create two additional Nexus users and one additional Blob Storage.

```yaml
---
- name: Configure Nexus
  hosts: all
  user: ansible
  become: yes
  vars:
    config:
      users:
        - id: joan
          first_name: Joan
          last_name: Doe
          email: joan@example.org
          password: "{{ lookup('env', 'JOAN_PASSWORD') }}"
          status: active
          source: default
          roles:
            - nx-admin
        - id: joe
          first_name: Joe
          last_name: Doe
          email: joe@example.org
          password: nbusr123
          status: disabled
          source: default
          roles:
            - nx-anonymous
      stores:
        - name: file_blob
          type: file
          soft_quota: 0
          path: /mydata/blobs
          status: active
  roles:
    - role: monolithprojects.nexus_config
```

## License

MIT

## Author Information

Created in 2021 by Michal Muransky
