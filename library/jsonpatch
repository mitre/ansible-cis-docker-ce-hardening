#!/usr/bin/env python

DOCUMENTATION = '''
---
module: jsonpatch
short_description: Manage bits and pieces of JSON files
description:
  - A CRUD-like interface to managing bits of JSON files.
version_added: "1.0"
options:
  file:
    description:
      - Path to the file to operate on. File must exist ahead of time.
    required: true
    default: null
    choices: []
  key:
    description:
      - Dot separated path to key. For example. `first.second`.
    required: true
    default: none
    choices: []
  value:
    description:
      - Desired state of the selected key. Either a string, a number or a list.
    required: false
    default: Elements default to an empty string.
    choices: []
  state:
    description:
      - Either 'present' or 'absent'.
    required: false
    default: present
    choices: []
requirements:
    - Optional Python C(commentjson) module
author: Alik Kurdyukov
'''

__author__ = 'akurdyukov'

import json
import sys
import shlex

try:
    import commentjson
    json_load = commentjson.load
except ImportError:
    json_load = json.load

def normalize_value(value):
    if isinstance(value, basestring):
        if value.isdigit():
            return int(value)
    return value

def main():

    module = AnsibleModule(
        argument_spec=dict(
            file=dict(required=True),
            key=dict(required=True),
            value=dict(required=False, default=None),
            state=dict(required=False, default='present', choices=['present', 'absent']),
            indent=dict(required=False, default=2),
        ),
        supports_check_mode=True
    )

    if 'raw' in module._CHECK_ARGUMENT_TYPES_DISPATCHER:
        module = AnsibleModule(
            argument_spec=dict(
                file=dict(required=True),
                key=dict(required=True),
                value=dict(required=False, default=None, type='raw'),
                state=dict(required=False, default='present', choices=['present', 'absent']),
                indent=dict(required=False, default=2),
            ),
            supports_check_mode=True
        )

    json_file = os.path.expanduser(module.params['file'])
    key = module.params['key']
    value = module.params['value']
    state = module.params['state']
    indent = module.params['indent']

    # convert indent if required
    if isinstance(indent, basestring):
        if indent.isdigit():
            indent = int(indent)
        else:
            module.fail_json(
                msg="Indent should be integer: %s" %
                indent)

    if state == 'absent':
        value = None

    ##################################################################
    # Check if the file exists
    # No: abort
    if os.path.isfile(json_file):
        infile = file(json_file, 'r')
    else:
        module.fail_json(
            msg="The target JSON source does not exist: %s" %
            json_file)

    # Try to parse in the target XML file
    try:
        data = json_load(infile)
    except ValueError, e:
        module.fail_json(
            msg="Error while parsing file: %s" %
            str(e))

    changed = False

    parts = key.split('.')
    ref = data
    for part in parts[:-1]:
        if part not in ref:
            if state == 'absent' or value is None:
                # stop work if no key should be created
                module.exit_json(changed=False)
            ref[part] = {}
        ref = ref[part]


    last_part = parts[-1]
    if state == 'absent' or value is None:
        if last_part in ref:
            del ref[last_part]
            changed = True
        else:
            module.exit_json(changed=False)
    else:
        value = normalize_value(value)
        if last_part not in ref or ref[last_part] != value:
            ref[last_part] = value
            changed = True

    if changed:
        with open(json_file, 'w') as f:
            json.dump(data, f, indent=indent, separators=(',', ': '), sort_keys=True)

    module.exit_json(changed=changed)

from ansible.module_utils.basic import *
main()
