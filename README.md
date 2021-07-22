Role Name
=========

This role can be used in two ways:

**1**.) You can build an Execution Environment directly via Podman. 

**2**.) Alternatively, you can build a virtual environment on a local machine and push to a local or remote registry with either method.

Requirements
------------


**1**.) Single Ansible Tower instance or Ansible Tower cluster (tested on a 3 node cluster proxied via HAproxy).

**2**.) Administrative access to an existing Kubernetes cluster to create a namespace to deploy the Execution Environment to via a [Container Group](https://docs.ansible.com/ansible-tower/latest/html/administration/external_execution_envs.html#ag-container-groups) in Ansible Tower.

  - [Kubernetes](https://github.com/salanisor/role_ansible_execution_environments/blob/master/files/execution-environments-k8s-template.yaml) namespace creation template.

**3**.) Docker or Podman installed on the Tower nodes - (configured with non-root privileges tested via Podman).

  - Install [podman / buildah](https://github.com/salanisor/role_ansible_execution_environments/blob/master/files/pb_buildah.yaml) to run `podman` as the `awx` user on each of the Tower hosts in your cluster.


Role Variables
--------------

    # Path to your build artifacts directory.
    builder_path: '/tmp/ansible-builder'
    # Name you want to give to your virtual environment directory.
    venv: 'test_venv'
    # Container image tag version.
    tag_version: '0.0.1'


Dependencies
------------

Before running this role update the following configuration files with the proper authentication credentials.

**A**.) [Configure](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/1.0/html/getting_started_with_red_hat_ansible_automation_hub/proc-configure-automation-hub-server) `ansible.cfg` with your `ansible_hub` [authentication token](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/1.0/html/getting_started_with_red_hat_ansible_automation_hub/proc-create-api-token).

**B**.) [Configure the registry credentials](https://access.redhat.com/terms-based-registry/) to pull from registry.redhat.io when building your Execution Environments and recommend updating this with your internal / external registry credentials you'll be pushing the Execution Environment container image to.

**C**.) Alternatively, drop in the [Tower cli configuration](https://tower-cli.readthedocs.io/en/latest/quickstart.html). First, you'll need to install the [`ansible-tower-cli`](https://tower-cli.readthedocs.io/en/latest/install.html) via pip.


Example Playbook
----------------

Once you update the [role varilables](https://github.com/salanisor/role_ansible_execution_environments#role-variables) execute the following playbook to build your Execution Environment varilable.

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
      - { role: role_ansible_execution_environments }

License
-------

BSD

Author Information
------------------

@salanisor
