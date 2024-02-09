# combodo-ansible

This is just a role conversion of Combodo's iTop Ansible playbooks.

If this is your first encounter with Ansible fot iTop, I created also some (1, at this time 😉) test sets in this other repo:
[Combodo_Ansible_Demo](https://github.com/Schirrms/Combodo_Ansible_Demo)

## Goal, origins

[Combodo](https://www.combodo.com/), the company who created an maintains our favorite ITSM software [iTop](https://www.combodo.com/itop) created in 2023 at set of Ansible Playbooks to interact with iTop from Ansible.

## Quote from the source package

### Extension Ansible playbooks for iTop

#### About

This extension provides a set of Ansible playbooks that enable communication between Ansible and iTop applications.
Together with the [Data model for Ansible](https://store.itophub.io/en_US/products/combodo-ansible-datamodel) extension,
it allows Ansible playbooks, tasks or modules to directly fetch in iTop CMDB related information like inventory files or
list of hosts to work on. It allows as well the creation and the modification of CIs in iTop.

For more information about this module have a look at the
corresponding [extension documentation](https://store.itophub.io/en_US/products/combodo-ansible-playbooks).

#### Download

Release packages can be found on the [iTop Hub Store](https://store.itophub.io/en_US/taxons/all-extensions). This is the best way to get a
running package as those contains all the needed modules and stable code.

When downloading directly from GitHub (by cloning or downloading as zip) you will get potentially unstable code, and you will miss
additional modules.

#### About Us

This iTop module development is sponsored, led and supported by [Combodo](https://www.combodo.com).

## End of quote

The playbooks work fine, but are a little bit hard to integrate in an existing ansible functionality. They are more built as a standalone function.

As I need to integrate those functionalities in other workflow, I cic a crude conversion of the playbook into a role.

Basically, I cut and pasted the playbook. I also prefixed all var input and output by 'itop_' as there is no 'scope' for vars in Ansible.

## Current state

At for now, only the playbook itop.core.write has been ported.

## Usage

to use this role, you have to import it.

direct way:

~~~bash
ansible-galaxy collection install git+https://github.com/Schirrms/combodo_ansible.git#/schirrms
~~~

But it is probably better to have a requirements.yml file in your environment :

~~~yaml
collections:
  - name: git+https://github.com/Schirrms/combodo_ansible.git#/schirrms
    type: git
    version: master
~~~

then, the collection is installed/updated by:

~~~bash
ansible-galaxy collection install -r requirements.yml
~~~

Once te requirement in place, you can  use it in your playbook like this:

~~~yaml
- name: Create a Virtual Machine in iTop
  hosts: localhost
  gather_facts: false

  tasks:
    - name: Set variables for the CI to Update
      ansible.builtin.set_fact:
        itop_obj_class: "Name of the iTop Class"
        itop_key:
          "name": "....."
          ".....": "....."
        itop_fields:
          "name": "....."
          ".....":
            ".....": "....."

- name: Call iTop role to create or update a CI
  gather_facts: false
  hosts: localhost
  roles:
    - schirrms.combodo_ansible.itop_core_write
~~~
