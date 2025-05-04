+++
title = "gcloud CLI - pro tips"
date = 2024-07-30

[taxonomies]
tags=["devops"]
+++

## Pro Tips

- Check for `machine-type` availability:
  - [gcloud topic filters  |  Google Cloud CLI Documentation](https://cloud.google.com/sdk/gcloud/reference/topic/filters)

```sh
gcloud compute machine-types list --filter="name:g2-standard-32 AND zone:us-west1-*"
```

- Set project:

```sh
gcloud config set project project_id
```
