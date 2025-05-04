+++
title = "Terraform HandsOn"
date = 2024-07-01

[taxonomies]
tags=["devops"]
+++

## Terraform

Terraform is used to manage infrastructure. Keeping the state of the project and applying just the changes.

## Hands On

Starting with the proposed tutorial, after having `terraform` installed, let's create a new file:

```hcl
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.1"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "tutorial"

  ports {
    internal = 80
    external = 8000
  }
}
```

The first block sets the provider, which handles the needed resources/infrastrucre, that will be used, in this case `"kreuzwerker/docker"`. There are many providers: [Browse Providers | Terraform Registry](https://registry.terraform.io/browse/providers).

Then, a `resource` is declared, which is an `infrastructure` component, it may be a physical server or a logical service such as a `dns service` or even a `storage` component.

In this example, there are two resources, an image and a container, the image is used to create the container. The two `strings` of the resource block are the resource's `type` and `name`.

A clearer way to declare a resource, since we may have multiple resources of the same type could be naming them as follows: `instanceN`: `resource "docker_container" "container1"`.


## .tfvars and  variables.tf

The `variables.tf` is used to define variables such as:

```hcl
variable "secret_name" {
  description = "Nginx Container Name"
  type = string
  sensitive = true
}
```

> Since it's sensitive, it will not be compromised in any way.

And then we can set the variable with the next descending order of prescedence:

- `terraform apply -var secret_name=secret` 
- using a `*.auto.tfvars`
  - in the file: `secret_name=secret`
- using `terraform.tfvars`
  - in the file: `secret_name=secret`
- using an ENV variable: `TF_VAR_<name>`
  - this case: `TF_VAR_secret_name`
- `default = value`
- if the above options are not present, the command `terraform apply` will show a prompt to provide the variable

## Modules

If there is a file we want to reuse we can import it into another file, for example with this project structure:

```
.
├── modules
│   └── api
│      ├── main.tf
│      └── variables.tf
├── main.tf
└── terraform.tfvars
```

inside `./main.tf` the `./modules/api/main.tf` can be imported as follows:

```hcl
module "app" {
  source      = "./modules/api"
  // set the needed variables
}
```

## Project Structure for different deployments

We can use `Workspaces` but may be confusing down the road, a better setup is to divide the infrastructure needed depending on the purpose of it. 
For example, a good project structure may be:

```
.
├── modules
│   ├── api
│   │   ├── main.tf
│   │   └── variables.tf
│   ├── app
│   │   ├── main.tf
│   │   └── variables.tf
│   └── db
│       ├── main.tf
│       └── variables.tf
└── workspaces
    ├── dev
    │   ├── main.tf
    │   └── terraform.tfvars
    ├── global
    │   └── main.tf
    └── prod
        ├── main.tf
        └── terraform.tfvars
```

If inside of `workspaces/dev` we do:

```sh
terraform init
terraform apply
```

it only initializes the infrastructure defined in `workspaces/dev/main.tf`

The idea of having a `global` directory is to have infrastructure that is common for both `dev` and `prod`.

> [!NOTE]
> The `provider` is not declared inside the `main.tf` file. A file called `provider.tf` file is commonly used.

## Github Workflows

Docs: [Triggering a workflow - GitHub Docs](https://docs.github.com/en/actions/using-workflows/triggering-a-workflow)

`GITHUB Secrets` are used to manage our Terraform's sensitive variables.

```yml
env:
  CLOUD_PROVIDER_API: "${{ secrets.CLOUD_PROVIDER_API }}"

# [...]

run: |
    terraform init
    terraform apply -var db_pwd=${{ secrets.CLOUD_PROVIDER_API }} -auto-approve
```

## Google Cloud as Provider

- [Build infrastructure | Terraform | HashiCorp Developer](https://developer.hashicorp.com/terraform/tutorials/gcp-get-started/google-cloud-platform-build)
- [Install the gcloud CLI  |  Google Cloud CLI Documentation](https://cloud.google.com/sdk/docs/install)
- [Getting Started with the Google Cloud provider | Guides | hashicorp/google | Terraform | Terraform Registry](https://registry.terraform.io/providers/hashicorp/google/latest/docs/guides/getting_started)
- [Pricing per product | Google Cloud](https://cloud.google.com/pricing/list?hl=en)
- [Episode 4 - Use Terraform to Provision a Google Compute instance | Head in the Clouds](https://headintheclouds.site/episodes/episode4)
- [tasdikrahman/terraform-gcp-examples: Terraform Google cloud platform examples](https://github.com/tasdikrahman/terraform-gcp-examples/tree/master)

Authenticate:

```sh
gcloud auth application-default login
```

And lets start with the `Terraform's webpage`  tutorial:

`main.tf`:
```hcl
terraform {
  required_providers {
    google = {
      source  = "hashicorp/google"
      version = "4.51.0"
    }
  }
}

variable "project_id" {
  description = "GCP project ID"
  type        = string
  default     = "value"
}

provider "google" {
  project = var.project_id
  region  = "us-central1"
  zone    = "us-central1-c"
}

resource "google_compute_network" "net_instance1" {
  name                    = "tf-network-instance"
  auto_create_subnetworks = true
}
```

Now if we run:

```sh
terraform init
terraform apply
```

Terraform will request your Google Cloud project ID. After providing it, Google Cloud will create the specified resource defined in main.tf, such as `google_compute_network` named `net_instance1`. You can access the name attribute of this resource using google_compute_network.net_instance1.name. Remember, you can define multiple instances of any resource by defining them as follows:
- `resource x instance1`
- `resource x instance2`

> [VPC networks  |  Google Cloud](https://cloud.google.com/vpc/docs/vpc)

### Compute Instance

Having the network set, we can connect a computer right adding it as resource.

```hcl
resource "google_compute_instance" "vm_instance1" {
  name         = "tf-vm-instance"
  machine_type = "f1-micro"
  tags         = ["test", "dev"]

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-11"
    }
  }

  network_interface {
    network = google_compute_network.net_instance1.name
  }

  depends_on = [google_compute_network.net_instance1]
}
```

The compute_instance is making use of the `net_instance1` previously declared.

Now if we run `terraform apply`, the compute instance will be created, but we'll not have access to it.

### Firewall Instance

The missing piece is the `Firewall`, the port `22` has to be exposed. The `Firewall` has to be applied to our `network`:

```hcl
resource "google_compute_firewall" "fw_instance1" {
  name      = "tf-firewall-instance"
  network   = google_compute_network.net_instance1.name
  direction = "INGRESS"
  allow {
    protocol = "tcp"
    ports    = ["22"]
  }

  source_ranges = ["0.0.0.0/0"]
  target_tags   = ["ssh-server"]
}
```

Now with the Firewall enabled, we have to `target` the `compute_instance` in use. The firewall makes use of the `target_tags`, we've just need to add `"ssh-server"` inside the `tags` array of the compute_instance:

```hcl
resource "google_compute_instance" "vm_instance1" {
  //[...]
  tags         = ["test", "dev", "ssh-server"]
  //[...]
}
```

We just need one more piece, a `static public IP`. It can be declared with a resource and then use the IP for the `compute_instance`:

```hcl
resource "google_compute_address" "static" {
  name = "static-ipv4"
}
//[...]
resource "google_compute_instance" "vm_instance1" {
  //[...]
  network_interface {
    network = google_compute_network.net_instance1.name
    access_config {
      nat_ip = google_compute_address.static.address
    }
  }
  //[...]
}
```

In fact, it is not strictly necessary to declare the `compute_address` resource, if the `access_config` field is left without the `nat_ip`, terraform will create a `static public` IP automatically.

> [!WARNING]
> Anyone can reach the compute_instance since the firewall allows connections from any address.
> Access is granted only with the correct keys. See below for details.

### Connect with SSH

Finally, to access via ssh to the vm:
```sh
gcloud compute ssh --zone "us-central1-c" "tf-vm-instance" --project "<project_id>"
```

It also can be done without gcloud, the proposed steps are:
>[!TIP] 
> - [google cloud platform - How to add an ssh key to an GCP instance using terraform? - Stack Overflow](https://stackoverflow.com/questions/38645002/how-to-add-an-ssh-key-to-an-gcp-instance-using-terraform/78388162#78388162)

1. Generate an ssh key &rarr; `ssh-keygen`
2. Load the variable as `metadata` of the `compute_instance`:

```hcl
variable "ssh_key_file" {
  description = "ssh key"
  type        = string
  default = "./ssh_keys/gcp.pub"
}
//[...]
resource "google_compute_instance" "vm_instance1" {
  //[...]
  metadata = {
    ssh-keys = "dev:${file(var.ssh_key_file)}"
  }
  //[...]
}
```

3. Output the public ip:
  
```hcl
//[...]
output "compute_instance_public_ip" {
  value = google_compute_instance.vm_instance1.network_interface.0.access_config.0.nat_ip
}
```

4. SSH to that public ip with the private key that matches the public key sent as metadata.
   - `ssh -i <priv key> <usr>@<public_ip>`

## terraform-docs

- [terraform-docs/terraform-docs: Generate documentation from Terraform modules in various output formats](https://github.com/terraform-docs/terraform-docs/)

```sh
terraform-docs markdown path/to/module --output-file README.md
```

Output:

```markdown
## Requirements

| Name                                                             | Version |
| ---------------------------------------------------------------- | ------- |
| <a name="requirement_google"></a> [google](#requirement\_google) | 4.51.0  |

## Providers

| Name                                                       | Version |
| ---------------------------------------------------------- | ------- |
| <a name="provider_google"></a> [google](#provider\_google) | 4.51.0  |

## Modules

No modules.

## Resources

| Name                                                                                                                                    | Type     |
| --------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| [google_compute_instance.vm_instance1](https://registry.terraform.io/providers/hashicorp/google/4.51.0/docs/resources/compute_instance) | resource |
| [google_compute_network.net_instance1](https://registry.terraform.io/providers/hashicorp/google/4.51.0/docs/resources/compute_network)  | resource |

## Inputs

| Name                                                               | Description    | Type     | Default | Required |
| ------------------------------------------------------------------ | -------------- | -------- | ------- | :------: |
| <a name="input_project_id"></a> [project\_id](#input\_project\_id) | GCP project ID | `string` | n/a     |   yes    |

## Outputs

No outputs.
```


## Resources

- [Automate Terraform with GitHub Actions | Terraform | HashiCorp Developer](https://developer.hashicorp.com/terraform/tutorials/automation/github-actions)
- [What is Infrastructure as Code with Terraform? | Terraform | HashiCorp Developer](https://developer.hashicorp.com/terraform/tutorials/docker-get-started/infrastructure-as-code)
