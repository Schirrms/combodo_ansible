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

The idea behind this change is to be able to share common vars (who will simply boe prefixed `itop_core_`)
while using cleaner var isolation on the task level.
