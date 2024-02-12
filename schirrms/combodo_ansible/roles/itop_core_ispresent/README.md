# itop_core_read

Heavily based on the itop_core_write role, so basically on the Combodo's itop.core.write.yml playbook, this role just query the iTop API based on the reconciliation fields and returns the number of elements found.

So, basically :
itop_code > 0 : Ansible was unable to properly quey the iTop API
itop_found_items = 0 : no items found with those reconciliation params
itop_found_items = 1 : Just one item, this is an exact match
itop_found_items > 1 : More than one item, this is not an exact match, update will not be possible
