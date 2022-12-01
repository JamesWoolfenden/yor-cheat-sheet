## Tagging a repository

With resources in the root.

```bash
Get a IAC repo e.g.:

git clone https://github.com/JamesWoolfenden/terraform-gcp-googlecomputeinstance/

yor tag -d .
  __    __
  \ \  / /
   \ \/ /___  _  ____
    \  /  _ \| |/  __|
    | |  |_| |   /
    |_|\____/|__|v0.1.155
 Yor Findings Summary
 Scanned Resources:	  15
 New Resources Traced: 	  1
 Updated Resources:	  0

New Resources Traced (1):
+----------------------------------------+-------------------------------------+----------------------+------------------------------------------+--------------------------------------+
|                  FILE                  |              RESOURCE               |       TAG KEY        |                TAG VALUE                 |                YOR ID                |
+----------------------------------------+-------------------------------------+----------------------+------------------------------------------+--------------------------------------+
| google_compute_instance.vm_instance.tf | google_compute_instance.vm_instance | yor_trace            | 656f9b49-4249-4da0-b6ab-62684d99af78     | 656f9b49-4249-4da0-b6ab-62684d99af78 |
+                                        +                                     +----------------------+------------------------------------------+                                      +
|                                        |                                     | git_repo             | terraform-gcp-googlecomputeinstance      |                                      |
+                                        +                                     +----------------------+------------------------------------------+                                      +
|                                        |                                     | git_org              | JamesWoolfenden                          |                                      |
+                                        +                                     +----------------------+------------------------------------------+                                      +
|                                        |                                     | git_modifiers        | amesoolfenden                            |                                      |
+                                        +                                     +----------------------+------------------------------------------+                                      +
|                                        |                                     | git_last_modified_by | amesoolfenden                            |                                      |
+                                        +                                     +----------------------+------------------------------------------+                                      +
|                                        |                                     | git_last_modified_at | 2021-03-25-09-54-05                      |                                      |
+                                        +                                     +----------------------+------------------------------------------+                                      +
|                                        |                                     | git_file             | google_compute_instance_vm_instance_tf   |                                      |
+                                        +                                     +----------------------+------------------------------------------+                                      +
|                                        |                                     | git_commit           | f45763bb5b3707565573cf5afb0d3bbcc8bee8a2 |                                      |
+----------------------------------------+-------------------------------------+----------------------+------------------------------------------+--------------------------------------+
```

The compute resource tags are updated, for GCP these are called labels and not tags.

If using a local module in a template:

```bash
yor tag -d example/examplea
  __    __
  \ \  / /
   \ \/ /___  _  ____
    \  /  _ \| |/  __|
    | |  |_| |   /
    |_|\____/|__|v0.1.155
 Yor Findings Summary
 Scanned Resources:	  6
 New Resources Traced: 	  0
 Updated Resources:	  0
```

This is because in this case the module referenced is a local module and the default behaviour for local modules is not to add tags.

If you really want tags/labels and your using a local module:

```bash
yor tag -d example/examplea -tag-local-modules
  __    __
  \ \  / /
   \ \/ /___  _  ____
    \  /  _ \| |/  __|
    | |  |_| |   /
    |_|\____/|__|v0.1.155
 Yor Findings Summary
 Scanned Resources:	  6
 New Resources Traced: 	  1
 Updated Resources:	  0

New Resources Traced (1):
+------------------------------------+----------+----------------------+------------------------------------------+--------------------------------------+
|                FILE                | RESOURCE |       TAG KEY        |                TAG VALUE                 |                YOR ID                |
+------------------------------------+----------+----------------------+------------------------------------------+--------------------------------------+
| example/examplea/module.compute.tf | compute  | yor_trace            | ec753e9c-acaa-4ad6-a555-aaebe08f59f9     | ec753e9c-acaa-4ad6-a555-aaebe08f59f9 |
+                                    +          +----------------------+------------------------------------------+                                      +
|                                    |          | git_repo             | terraform-gcp-googlecomputeinstance      |                                      |
+                                    +          +----------------------+------------------------------------------+                                      +
|                                    |          | git_org              | JamesWoolfenden                          |                                      |
+                                    +          +----------------------+------------------------------------------+                                      +
|                                    |          | git_modifiers        | james.woolfenden                         |                                      |
+                                    +          +----------------------+------------------------------------------+                                      +
|                                    |          | git_last_modified_by | james.woolfenden@gmail.com               |                                      |
+                                    +          +----------------------+------------------------------------------+                                      +
|                                    |          | git_last_modified_at | 2020-02-14 21:24:06                      |                                      |
+                                    +          +----------------------+------------------------------------------+                                      +
|                                    |          | git_file             | example/examplea/module.compute.tf       |                                      |
+                                    +          +----------------------+------------------------------------------+                                      +
|                                    |          | git_commit           | f359c7bebcc5a93406cc8dfc58826529727b5e1d |                                      |
+------------------------------------+----------+----------------------+------------------------------------------+--------------------------------------+
```

This module gets its tags passed by attributes and so the module file has been updated:

```golang
module "compute" {
  source = "../../"
  common_tags = merge(var.common_tags, {
    git_commit           = "f359c7bebcc5a93406cc8dfc58826529727b5e1d"
    git_file             = "example/examplea/module.compute.tf"
    git_last_modified_at = "2020-02-14 21:24:06"
    git_last_modified_by = "james.woolfenden@gmail.com"
    git_modifiers        = "james.woolfenden"
    git_org              = "JamesWoolfenden"
    git_repo             = "terraform-gcp-googlecomputeinstance"
    yor_trace            = "ec753e9c-acaa-4ad6-a555-aaebe08f59f9"
  })
  region     = var.region
  zone       = var.zone
  project_id = var.project_id
  username   = var.username
}
```

Certain attributes tags, common_tags can be detected as passing tags/labels to a module and updated.
