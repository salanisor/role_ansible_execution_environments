Role Name
=========

You can build Execution Environment container images to separate the control plane from the execution plane and execute your playbooks and roles directly from Tower on to your Kubernetes cluster.

This role can be used in two ways:

**1**.) You can build an Execution Environment directly via Podman. 

**2**.) For testing and local development, you can build a virtual environment on a local machine and push to a local or remote registry with either method.

Requirements
------------


**1**.) Single Ansible Tower instance or Ansible Tower cluster (tested on a 3 node cluster proxied via HAproxy).

**2**.) Administrative access to an existing Kubernetes cluster to create a namespace to deploy the Execution Environment to via a [Container Group](https://docs.ansible.com/ansible-tower/latest/html/administration/external_execution_envs.html#ag-container-groups) in Ansible Tower.

  - [Kubernetes](https://github.com/salanisor/role_ansible_execution_environments/blob/master/files/execution-environments-k8s-template.yaml) namespace creation template for project `ansible-tower`.

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
    # The remote registry you want to publish to.
    remote_registry: 'quay.io/canit0/ee-custom-container'



Dependencies
------------

Before running this role update the following configuration files with the proper authentication credentials.

**A**.) [Configure](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/1.0/html/getting_started_with_red_hat_ansible_automation_hub/proc-configure-automation-hub-server) `ansible.cfg` with your `ansible_hub` [authentication token](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/1.0/html/getting_started_with_red_hat_ansible_automation_hub/proc-create-api-token).

**B**.) [Configure the registry credentials](https://access.redhat.com/terms-based-registry/) to pull from `registry.redhat.io` when building your Execution Environments and recommend updating this with your internal / external registry credentials you'll be pushing the Execution Environment container image to.

**C**.) Alternatively, drop in the [Tower cli configuration](https://tower-cli.readthedocs.io/en/latest/quickstart.html). First, you'll need to install the [`ansible-tower-cli`](https://tower-cli.readthedocs.io/en/latest/install.html) via pip.


To manually verify the requirements in a collection run:

    ansible-builder introspect --sanitize ~/.ansible/collections/

Example output:

    ---
    python:
    - 'jsonschema  # from collection ansible.utils'
    - 'textfsm  # from collection ansible.utils'
    - 'ttp  # from collection ansible.utils'
    - 'xmltodict  # from collection ansible.utils'
    - 'pytz  # from collection awx.awx'
    - 'python-dateutil>=2.7.0  # from collection awx.awx'
    - 'awxkit  # from collection awx.awx'
    - 'dnacentersdk>=2.0.2  # from collection cisco.dnac'
    system:
    - 'gcc-c++ [doc test platform:rpm]  # from collection ansible.utils'
    - 'python3-devel [test platform:rpm]  # from collection ansible.utils'
    - 'python3 [test platform:rpm]  # from collection ansible.utils'
    - 'python38-pytz [platform:centos-8 platform:rhel-8]  # from collection awx.awx'
    - 'python38-requests [platform:centos-8 platform:rhel-8]  # from collection awx.awx'
    - 'python38-pyyaml [platform:centos-8 platform:rhel-8]  # from collection awx.awx'

To manually verify the requirements in your current `venv` run:

    pip freeze > requirements.txt

Output example: `cat requirements.txt`


    ansible==4.3.0
    ansible-builder==1.0.1
    ansible-core==2.11.3
    ansible-navigator==1.0.0
    ansible-runner==2.0.1
    ansible-test==1.0.1
    bindep==2.9.0
    cffi==1.14.6
    cryptography==3.4.7
    dataclasses==0.8
    distro==1.5.0
    docutils==0.17.1
    importlib-resources==5.2.0
    Jinja2==3.0.1
    lockfile==0.12.2
    MarkupSafe==2.0.1
    onigurumacffi==1.1.0
    packaging==21.0
    Parsley==1.3
    pbr==5.6.0
    pexpect==4.8.0
    ptyprocess==0.7.0
    pycparser==2.20
    pyparsing==2.4.7
    python-daemon==2.3.0
    PyYAML==5.4.1
    requirements-parser==0.2.0
    resolvelib==0.5.4
    six==1.16.0
    zipp==3.5.0


Example Playbook
----------------

Once you update the [role varilables](https://github.com/salanisor/role_ansible_execution_environments#role-variables) execute the following playbook to build your Execution Environment.

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
