python-pip-update-scanner
=========================

Author: Steve Maher

This Ansible content downloads the OSV scanner plus its offline vulnerability database, captures Python package inventories from RPM-based hosts, and audits those inventories locally using the offline data.

Structure
- `scanner.yml`: top-level playbook orchestrating the roles.
- `roles/osv_download`: installs `osv-scanner` and refreshes the offline DB cache.
- `roles/osv_scan`: gathers per-interpreter `pip list` output, stores snapshots on the controller, runs `osv-scanner` offline against them, and optionally fails if vulnerabilities are found.
- `inventory`: sample inventory defining target hosts.

Usage
1. Adjust inventory/variables as needed (paths default to `/usr/local/bin/osv-scanner`, `/srv/osv-db`, `/srv/python-reqs`).
2. Run the playbook: `ansible-playbook scanner.yml -i inventory`.
3. Optional tags:
   - `--tags update_db` to force only the offline DB refresh.
   - `--tags enforce` to fail the run if any vulnerabilities are detected.
