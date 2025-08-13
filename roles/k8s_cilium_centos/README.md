Role Name
=========

A brief description of the role goes here.

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).

# Kubernetes & Cilium Bootstrap Role for CentOS Stream 10

This role **installs and configures a single-node Kubernetes v{{ k8s_version }} cluster** with CRI-O and Cilium on a fresh CentOS Stream 10 VM.

## Prerequisites

- CentOS Stream 10 (fresh VM recommended)
- Must run as root (use `become: true` in playbook)
- Outbound internet access required
- `firewalld` must not conflict with other security tooling

## Variables

All main software versions are parameterized. To override:
k8s_version: "1.33"
crio_version: "1.33"
cilium_cli_version: "v0.18.5"
cilium_version: "1.17.5"



## Usage

1. Add this role to your roles path.
2. Create a playbook similar to:


hosts: all
become: true
roles:

    role: k8s_cilium_centos
    vars:
    k8s_version: "1.33"
    crio_version: "1.33"
    cilium_cli_version: "v0.18.5"
    cilium_version: "1.17.5"


3. Apply to a fresh CentOS Stream 10 box:

ansible-playbook -i <inventory> site.yml


## Post-Install Validation

- `kubectl get nodes` – control-plane Ready
- `cilium status` – all systems online
- `cilium connectivity test` – success

## Manual/Molecule Testing

If not using Molecule, **manual test is:**

1. Deploy on a clean VM.
2. Confirm successful Ansible run (no errors).
3. Run `kubectl get nodes` as root and as your regular user.
4. Run `cilium status` and `cilium connectivity test`.

If you want `molecule` scenarios, add under `molecule/`.

## Idempotency

This role is written so you can **safely re-run** it on a node; changes only process if required.

## Notes

- Currently single-node only.
- Do not apply to production or multi-node clusters without review.

---

**Attach or distribute as a `.tar.gz` as needed.**

---

> This answer gives you a clean, idempotent role matching your script, well-factored, fully parameterized, and ready for production bootstrap standardization.

