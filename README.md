# Ansible Role: microceph

Role installs a ceph cluster using [microceph](https://canonical-microceph.readthedocs-hosted.com/en/latest/)

## Requirements

* Ansible >= 2.7
* Linux Distribution
    * Rocky Linux 9
    <!-- * Debian Family
        * Ubuntu
            * Xenial (16.04)
            * Bionic (18.04)
            * Focal (20.04) (untested)
            * Jammy (22.04) (untested)
    * Arch Linux (untested) -->

## License

MIT

## Usage

### Role Variables

Some variables available in this role are listed here.  The full set is
defined in `[defaults/main.yml](defaults/main.yml)`.

* `microceph_version`: Version to utilize, defaults value is `latest/edge`.
* `microceph_cluster_nodes`: Hostgroup whose members will form ceph cluster
* `microceph_seed_node`: Node name that will be used to start cluster formation
* `microceph_encrypt_data`: Encrypt all the data in the microceph drive at rest see : [Full disk encryption](https://canonical-microceph.readthedocs-hosted.com/en/latest/explanation/fde-osd/) 
* `ceph_release`: Used when building the ceph.repo file to identify the main ceph release train.
* `el9`: Used when building the ceph.repo file to identify the distro.

### Playbook example

```yaml
- hosts: ceph_nodes
  become: yes
  become_method: su
  roles:
    - role: "./roles/ansible_role_microceph"
      vars:
        microceph_cluster_nodes: ceph_nodes
        microceph_seed_node: rocky9-lab-node1 # Optional
        microceph_encrypt_data: False
        microceph_disk_devices:
          - /dev/vdb
        ceph_release: squid
        distro: el9

```


### Increasing data nodes

Additional nodes to the cluster can be added at any time. All nodes in the `microceph_cluster_nodes` hostgroup 
will run `microceph cluster join <join token>`, more info on this can be found here: [microceph join non primary node](https://canonical-microceph.readthedocs-hosted.com/en/latest/tutorial/multi-node/#join-the-non-primary-nodes-to-the-cluster).


### Using Python virtual environment

* Set up virtual environment
    ```
    $ python3 -m venv venv
    ```
* Activate the environment
    ```
    $ . venv/bin/activate
    ```
* Install Requirements
    ```
    $ pip install -r requirements.txt
    ```