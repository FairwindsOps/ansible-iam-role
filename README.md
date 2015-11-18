# ansible-iam-role

# Role Variables

* ```secrets_bucket``` is a required variable.

# Usage

```
- name:  Ensure existence of a role
  hosts: localhost
  connection: local
  gather_facts: False

  pre_tasks:
    - include_vars: "{{item}}"
      with_items:
        - ../../../account/vars.yml
        - ../env.yml
        - vars.yml
      tags: always

  roles:
    - { role: ansible-iam-role, iam_role_name: 'myrole', policy_name: 'account_secrets' }
```

```policy_name``` is one of a set of policies that are defined by files in /templates.
