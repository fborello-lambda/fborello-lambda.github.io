+++
title = "Terraform GCP CloudSQL"
date = 2024-07-02

[taxonomies]
tags=["devops"]
+++


# Terraform GCP CloudSQL

Main Resource:
- [Configure private IP  |  Cloud SQL for MySQL  |  Google Cloud](https://cloud.google.com/sql/docs/mysql/configure-private-ip#console_1)

Taking as foundation the [Terraform Handson](./terraform_handson.md) note. Where a VM is declared.

## Declaring the Database


```hcl
// First we need a Postgres Server
resource "google_sql_database_instance" "pg_instance1" {
  name             = "db-instance1"
  region           = "us-central1"
  database_version = "POSTGRES_14"

  settings {
    tier = "db-f1-micro"
    ip_configuration {
      ipv4_enabled = true
    }
  }
}

// We specify the name of the DB that will be created
resource "google_sql_database" "db_policy1" {
  name            = "db_policy1"
  instance        = google_sql_database_instance.pg_instance1.name
  deletion_policy = "DELETE"
}
```

A user must be created; a `host` value can be set to limit where the user can connect from:

```hcl
resource "google_sql_user" "db_user1" {
  name     = "dev"
  instance = google_sql_database_instance.pg_instance1.name
  password = "password"
}
```

With this configuration, we are setting up a DB with `ipv4_enabled` set to true, which will create a public IP automatically. We should set up a firewall and extra protection methods. An alternative could be blocking access from outside and using a private network instead of a public IP.

## Set a Service Network

First, we need to enable the `Service Networking API` within the gcloud project's gui. Then, we can declare the following resources:

```hcl
// Private Network
resource "google_compute_global_address" "private_ip_address" {
  name          = "private-ip-address"
  purpose       = "VPC_PEERING"
  address_type  = "INTERNAL"
  prefix_length = 16
  network       = google_compute_network.net_instance1.id
}

resource "google_service_networking_connection" "tf_network" {
  network                 = google_compute_network.net_instance1.id
  service                 = "servicenetworking.googleapis.com"
  reserved_peering_ranges = [google_compute_global_address.private_ip_address.name]
}
//
```

Now this network has to be used in conjunction with the Database, and `ipv4_enabled` has to be set to false to avoid creating a `private_ip`:

```hcl
resource "google_sql_database_instance" "pg_instance1" {
  name             = "db-instance1"
  region           = "us-central1"
  database_version = "POSTGRES_14"
  depends_on       = [google_service_networking_connection.tf_network]

  settings {
    tier = "db-f1-micro"
    ip_configuration {
      ipv4_enabled    = false
      private_network = google_compute_network.net_instance1.id
    }
  }
}
```

At this stage, we are creating a connection between the VPC `net_instance1` and the PostgreSQL server network. Our VPC will `auto_create_subnetworks` automatically, so this setting has to be set to false to avoid overlapping. Then, we will need to create a custom subnetwork:

```hcl
resource "google_compute_network" "net_instance1" {
  name                    = "tf-network-instance"
  auto_create_subnetworks = false
}

resource "google_compute_subnetwork" "subnet_instance1" {
  name          = "subnet"
  ip_cidr_range = "10.0.0.0/16"
  region        = "us-central1"
  network       = google_compute_network.net_instance1.self_link
}
```

With all these changes, we should have a VM whose access is open to the world but restricted (needing the SSH key), and a PostgreSQL server whose access is only allowed through the VM, both resources inside the same VPC subnetwork.

The full code and instructions can be found in the GitHub repo: [fborello-lambda/terraform_handson: Terraform HandsOn using GCP](https://github.com/fborello-lambda/terraform_handson)

## Resources
- [google_sql_database_instance | Resources | hashicorp/google | Terraform | Terraform Registry](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/sql_database_instance#ipv4_enabled)
- [Learn about using private IP  |  Cloud SQL for MySQL  |  Google Cloud](https://cloud.google.com/sql/docs/mysql/private-ip#network_requirements)
- [Create Google Cloud Sql MySql instance using Terraform - YouTube](https://www.youtube.com/watch?v=4H8_eu9l5cE&list=PLL220wRvDvTm_MyPtW0W3kc1_Htb3cJev&index=8)
- [Learn Terraform with Google Cloud Platform – Infrastructure as Code Course - YouTube](https://www.youtube.com/watch?v=VCayKl82Lt8)
