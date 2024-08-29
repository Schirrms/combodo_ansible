# itop_core

This new role is aimed to replace the different preceding roles:

* itop_core_delete
* itop_core_ispresent
* itop_core_read
* itop_core_write

As such, there will be no 'main.yaml' tasks, but specific tasks

So, instead of calling the role as:

~~~yaml
      ansible.builtin.import_role:
        name: schirrms.combodo_ansible.itop_core_read
      vars:
        itop_obj_class: "..."
        itop_key: "SELECT ..."
        itop_fields:
          "...":
          "...":
~~~

The new syntax will be:

~~~yaml
      ansible.builtin.import_role:
        name: schirrms.combodo_ansible.itop_core
        task_from: read.yaml
      vars:
        itop_core_read_obj_class: "..."
        itop_core_read_key: "SELECT ..."
        itop_core_read_fields:
          "...":
          "...":
~~~

But common var as the itop url will simply be prefixed `itop_core_`

The idea behind this change is to be able to share common vars (who will just
be prefixed `itop_core_`) while using cleaner var isolation on the task level.

## ⚙️ Role Variables

### read

Read the iTop CMDB

| Variable Name | Required | Type | Default | Elements | Description |
|---------------|----------|------|---------|----------|-------------|
| itop_core_ws_auth_user | True | str |  |  | iTop Rest Username |
| itop_core_ws_auth_pwd | True | str |  |  | iTop Rest Password |
| itop_core_root | True | str |  |  | iTop server URL |
| itop_core_ws_version | False | str | 1.3 |  | Web Service version |
| itop_core_ws_ansible_uuid | False | str | ANS_APP1 |  | UUID in iTop of the Ansible application to work with |
| itop_core_ws_ansible_inventory | False | str | ANS_ITOP |  | Name of the inventory to consider |
| itop_core_read_obj_class | True | str |  |  | Class of CI to read |
| itop_core_read_key | True | str |  |  | Set of attributes to be used for the reconciliation<br>or id of the record |
| itop_core_read_fields | True | dict |  |  | Set of attributes to be sent back |

### write

Write data to the iTop CMDB

| Variable Name | Required | Type | Default | Elements | Description |
|---------------|----------|------|---------|----------|-------------|
| itop_core_ws_auth_user | True | str |  |  | iTop Rest Username |
| itop_core_ws_auth_pwd | True | str |  |  | iTop Rest Password |
| itop_core_root | True | str |  |  | iTop server URL |
| itop_core_ws_version | False | str | 1.3 |  | Web Service version |
| itop_core_ws_ansible_uuid | False | str | ANS_APP1 |  | UUID in iTop of the Ansible application to work with |
| itop_core_ws_ansible_inventory | False | str | ANS_ITOP |  | Name of the inventory to consider |
| itop_core_write_obj_class | True | str |  |  | Class of CI to create or update |
| itop_core_write_key | True | str |  |  | Set of attributes to be used for the reconciliation<br>or id of the record |
| itop_core_write_fields | True | dict |  |  | Set of attributes to be written |
| itop_core_write_output_fields | False | dict |  |  | Set of attributes to be sent back<br>After creating or updating a CI, this task read the result and<br>Send back the new values in the dict `itop_core_write_output_values`<br>by default, the returned value list is the list defined in `itop_core_write_fields`<br>If `itop_core_write_output_fields` is set, then this define the list of values that will be sent back |

### ispresent

check if an iTop query give an answer and how many answers are given

| Variable Name | Required | Type | Default | Elements | Description |
|---------------|----------|------|---------|----------|-------------|
| itop_core_ws_auth_user | True | str |  |  | iTop Rest Username |
| itop_core_ws_auth_pwd | True | str |  |  | iTop Rest Password |
| itop_core_root | True | str |  |  | iTop server URL |
| itop_core_ws_version | False | str | 1.3 |  | Web Service version |
| itop_core_ws_ansible_uuid | False | str | ANS_APP1 |  | UUID in iTop of the Ansible application to work with |
| itop_core_ws_ansible_inventory | False | str | ANS_ITOP |  | Name of the inventory to consider |
| itop_core_ispresent_obj_class | True | str |  |  | Class of CI to create or update |
| itop_core_ispresent_key | True | str |  |  | Set of attributes to be used for the reconciliation<br>or id of the record |

### delete

Delete an item from iTop CMDB

| Variable Name | Required | Type | Default | Elements | Description |
|---------------|----------|------|---------|----------|-------------|
| itop_core_ws_auth_user | True | str |  |  | iTop Rest Username |
| itop_core_ws_auth_pwd | True | str |  |  | iTop Rest Password |
| itop_core_root | True | str |  |  | iTop server URL |
| itop_core_ws_version | False | str | 1.3 |  | Web Service version |
| itop_core_ws_ansible_uuid | False | str | ANS_APP1 |  | UUID in iTop of the Ansible application to work with |
| itop_core_ws_ansible_inventory | False | str | ANS_ITOP |  | Name of the inventory to consider |
| itop_core_delete_obj_class | True | str |  |  | Class of CI to delete |
| itop_core_delete_key | True | str |  |  | Set of attributes to be used for the reconciliation<br>or id of the record |
| itop_core_delete_simulate | False | str | true |  | Determines if the deletion is to be done<br>if set to 'false', the deletion will be done<br>if set to 'true', there will be no actual deletion |
