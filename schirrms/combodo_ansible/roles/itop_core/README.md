# itop_core

This new role is aimed to replace the different preceding roles:

* itop_core_delete
* itop_core_ispresent
* itop_core_read
* itop_core_write

As such, there will be no 'main.yaml' tasks, but specific tasks

So, instead of calling the role as:

~~~yaml
      ansible.builtin.set_fact:
        itop_obj_class: "..."
        itop_key: "SELECT ..."
        itop_fields:
          "...":
          "...":

      ansible.builtin.import_role:
        name: schirrms.combodo_ansible.itop_core_read
~~~

The new syntax will be:

~~~yaml
      ansible.builtin.import_role:
        name: schirrms.combodo_ansible.itop_core
        task_from: read
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

Most variables are hopefully easy to understand. One var can be a little tricky:
the `itop_core_{task}_key`

According to iTop documentation: [iTop REST/JSON Services - How to specify a key](https://www.itophub.io/wiki/page?id=latest:advancedtopics:rest_json#how_to_specify_a_key),
there are 3 ways for querying iTop CMDB:

* Specify search criteria (all criteria are combined with a AND operator)
* Specify a search query (OQL)
* Specify the id of the object (as a number)

So this var can contains the three syntax type for the reconciliation.

### Common information

The next 7 variables are common to every tasks in the role, as those vars define
the access to the itop Server.

| Input/Output | Variable Name                   | Required | Type | Default  | Description |
|--------------|---------------------------------|----------|------|----------|-------------|
| Input shared | itop_core_ws_auth_user          | True *   | str  |          | iTop Rest Username |
| Input shared | itop_core_ws_auth_pwd           | True *   | str  |          | iTop Rest Password |
| Input shared | itop_core_ws_auth_token         | True *   | str  |          | iTop Rest Token |
| Input shared | itop_core_root                  | True     | str  |          | iTop server URL |
| Input shared | itop_core_ws_version            | False    | str  | 1.3      | Web Service version |
| Input shared | itop_core_ws_ansible_uuid       | False    | str  | ANS_APP1 | UUID in iTop of the Ansible application to work with |
| Input shared | itop_core_ws_ansible_inventory  | False    | str  | ANS_ITOP | Name of the Inventory to consider |

\* *You'll need either the couple itop_core_ws_auth_user / itop_core_ws_auth_pwd,
   or the itop_core_ws_auth_token. Should you give the 3 values,
 then the token will be used.*

### read

Read the iTop CMDB

| Input/Output | Variable Name                   | Required | Type | Default  | Description |
|--------------|---------------------------------|----------|------|----------|-------------|
| Input        | itop_core_read_obj_class        | True     | str  |          | Class of CI to read |
| Input        | itop_core_read_key              | True     | str  |          | Set of attributes to be used for the reconciliation |
| Input        | itop_core_read_fields           | True     | dict |          | Set of attributes to be sent back |
| Output       | itop_core_read_code             | out      | int  |          | Return code sent by iTop's API |
| Output       | itop_core_read_message          | out      | str  |          | Error message sent by iTop's API |
| Output       | itop_core_read_obj_key          | out      | list |          | Keys (ids) of the queried CI |
| Output       | itop_core_read_output_values    | out      | dict |          | All values for the selected fields |

### write

Write data to the iTop CMDB

| Input/Output | Variable Name                   | Required | Type | Default  | Description |
|--------------|---------------------------------|----------|------|----------|-------------|
| Input        | itop_core_write_obj_class       | True     | str  |          | Class of CI to create or update |
| Input        | itop_core_write_key             | True     | str  |          | Set of attributes to be used for the reconciliation |
| Input        | itop_core_write_fields          | True     | dict |          | Set of attributes to be written |
| Input        | itop_core_write_output_fields   | False    | dict |          | Set of attributes to be sent back<br>After creating or updating a CI, this task read the result and<br>Send back the new values in the dict `itop_core_write_output_values`<br>by default, the returned value list is the list defined in `itop_core_write_fields`<br>If `itop_core_write_output_fields` is set, then this define the list of values that will be sent back |
| Input        | itop_core_write_create_comment  | False    | str  |          | A comment set in iTop while creating an object |
| Input        | itop_core_write_update_comment  | False    | str  |          | A comment set in iTop while updating an object |
| Output       | itop_core_write_code            | out      | int  |          | Return code sent by iTop's API |
| Output       | itop_core_write_message         | out      | str  |          | Error message sent by iTop's API |
| Output       | itop_core_write_obj_key         | out      | list |          | Key of the created or updated CI |
| Output       | itop_core_write_output_values   | out      | dict |          | values of the fields after creation/update |

### ispresent

check if an iTop query give an answer and how many answers are given

| Input/Output | Variable Name                   | Required | Type | Default  | Description |
|--------------|---------------------------------|----------|------|----------|-------------|
| Input        | itop_core_ispresent_obj_class   | True     | str  |          | Class of CI to create or update |
| Input        | itop_core_ispresent_key         | True     | str  |          | Set of attributes to be used for the reconciliation |
| Output       | itop_core_ispresent_code        | out      | int  |          | Return code sent by iTop's API |
| Output       | itop_core_ispresent_message     | out      | str  |          | Error message sent by iTop's API |
| Output       | itop_core_ispresent_obj_key     | out      | list |          | Key of the CI found |
| Output       | itop_core_ispresent_found_items | out      | int  |          | number of items found |

### delete

Delete an item from iTop CMDB

| Input/Output | Variable Name                   | Required | Type | Default  | Description |
|--------------|---------------------------------|----------|------|----------|-------------|
| Input        | itop_core_delete_obj_class      | True     | str  |          | Class of CI to delete |
| Input        | itop_core_delete_key            | True     | str  |          | Set of attributes to be used for the reconciliation |
| Input        | itop_core_delete_simulate       | False    | str  | true     | Determines if the deletion is to be done<br>if set to 'false', the deletion will be done<br>if set to 'true', there will be no actual deletion |
| Input        | itop_core_delete_comment        | False    | str  |          | A comment set in iTop while deleting an object |
| Output       | itop_core_delete_code           | out      | int  |          | Return code sent by iTop's API |
| Output       | itop_core_delete_message        | out      | str  |          | Error message sent by iTop's API |
| Output       | itop_core_delete_obj_key        | out      | list |          | Key of the deleted CI |
