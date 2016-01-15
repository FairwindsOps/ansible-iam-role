# ansible-iam-role

Associates a single IAM policy with a single IAM role. The role creates the given IAM role if it does not already exist. Policies are generated from templates included within the role unless a full `iam_role_policy_template_path` file path is provided.

# Role Variables

Variable | Details
------------- | -------------
iam_role_policy_template_path | Path to policy template files. Default empty string value assumes role's templates directory.
iam_role_name | IAM role name. If the role does not exist it will be created.
policy_name | Policy name to associate with the IAM role. Also the name of the template file minus file extension used to generate the policy. Template file should have `.json.j2` file extension.
secrets_bucket  | Required by `*_account_secrets.json` policy templates.

# Usage

```yml
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
    # Template included with this role
    - role: ansible-iam-role
      iam_role_name: 'myrole'
      policy_name: 'ro_account_secrets'
      secrets_bucket: 'client-infrastructure'

    # Template in the client repo
    - role: ansible-iam-role
      iam_role_name: 'myrole'
      policy_name: 'ro_application_s3'
      iam_role_policy_template_path: "{{ lookup('env', 'INFRASTRUCTURE_REPO') }}/roles/client-iam/templates"
```
