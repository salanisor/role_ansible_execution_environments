Role Name
=========

This role can be used in two ways:

**1**.) You can build an Execution Environment directly via Podman. 

**2**.) Alternatively, you can build a virtual environment on a local machine and push to a local or remote registry with either method.

Requirements
------------


**1**.) Single Ansible Tower or Ansible Tower cluster (tested on a 3 node cluster proxied via HAproxy).

**2**.) Administrative access to an existing Kubernetes cluster to create a namespace to deploy the Execution Environments to.

**3**.) Docker or Podman installed on the Tower nodes - (configured with non-root privileges tested via Podman).

  - You can configure [podman / buildah](https://github.com/salanisor/ansible/blob/master/role_ansible_execution_environments/files/pb_buildah.yaml) to run `podman` as the `awx` user on each of the Tower hosts in the cluster.


Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

Before running this role update the following configuration files with the proper authentication credentials.

**A**.) [Configure](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/1.0/html/getting_started_with_red_hat_ansible_automation_hub/proc-configure-automation-hub-server) `ansible.cfg` with your `ansible_hub` [authentication token](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/1.0/html/getting_started_with_red_hat_ansible_automation_hub/proc-create-api-token)

**B**.) [Configure the registry credentials](https://access.redhat.com/terms-based-registry/) to pull from registry.redhat.io and recommend updating this file with your internal / external registry credentials you'll be pushing the Execution Environment container image to.

**C**.) Alternatively, drop in the [Tower cli configuration](https://tower-cli.readthedocs.io/en/latest/quickstart.html). First, you'll need to install the [`ansible-tower-cli`](https://tower-cli.readthedocs.io/en/latest/install.html) via pip.


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    ---
    #
    # Simple Ansible playbook to set up the environment to build an Execution Environment image.
    #
    # How-to: ansible-playbook -v execution_environments.yaml
    #
    - hosts: 127.0.0.1
      connection: local
      gather_facts: false
      roles:
      - { role: ../role_ansible_execution_environments }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
