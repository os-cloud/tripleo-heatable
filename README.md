# Project Tripleo-Heatable

This project aims to provide an interface that will consume heat templates and
produce Ansible playbooks.

## Interface

CLI interface which uses the `heat` template arguments to read templates and
parse them. The `heatable` command is the user interface for interacting with
the parser.

## Installation

``` console
python -m venv heatable
heatable/bin/pip install tripleo-heatable
```

Once installed, the `heatable` command will be available within the virtual
environment.

## Usage

The `heatable` command will parse heat templates and generate a set of Ansible
playbooks. These playbooks will map all items discovered in the heat templates
to appropriate modules within the Ansible paradigm. In the event that an
interaction does not have a valid mapping, an error will be raised.

``` console
heatable --stack overcloud \
         --templates $THT/ \
         --environment-file $THT/environments/disable-telemetry.yaml \
         --environment-file $THT/environments/enable-swap.yaml \
         --environment-file $THT/environments/ssl/inject-trust-anchor.yaml \
         --environment-file /home/cloud-user/local_images.yaml \
         --environment-file /tmp/templates/environments/docker.yaml \
         --environment-file /home/cloud-user/parameters.yaml \
         -role $THT/roles_data.yaml \
         --disable-validations \
         --validation-errors-nonfatal
         --output-directory ~/generated-playbooks
```

This invocation will generate a set of Ansible playbooks based on the provided
environment and heat template options. The output playbooks will be monolythic
in nature but will remain separated based on the original template context. To
use these playbooks, the entry point is the `site.yml` however, all generated
playbook should be executable on their own.
