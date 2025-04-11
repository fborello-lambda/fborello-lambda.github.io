+++
title = "Phoenix Extra"
date = 2024-05-01

[taxonomies]
tags=["programming", "phx"]
+++

# Phoenix 

- **[FLAME](https://fly.io/blog/rethinking-serverless-with-flame/)**
- [Dokku - PaaS](https://dokku.com/)
- [Phoenix Real Case](https://www.sleepeasy.app/2024/01/21/elixir-best-language-for-bootstrapped-b2b-saas/)
- [Phoenix 1.7: A Major Step for the Phoenix Framework – Nimble](https://nimblehq.co/blog/whats-new-in-phoenix-1-7)

## Phoenix LiveView

- [Elixir Streams |> Phoenix forms without changesets!](https://www.elixirstreams.com/tips/phoenix-forms-without-changesets)
- Tailwind
    - [Tailwind Tutorial](https://github.com/dwyl/learn-tailwind)
    - [Heroicons](https://heroicons.com/)
- A good way to use "Views" with liveview is to put them in `app_web/live/view1.ex`. The view1 could be the homepage for example and inside it goes the html using `~H` . Then `router.ex` calls the defined module in `view1.ex`  → [Phoenix LiveView Counter](https://github.com/dwyl/phoenix-liveview-counter-tutorial).
- [Starknet Explorer](https://www.starkcompass.com/) → The following file of the repo shows the default layout of every view [Repo](https://github.com/lambdaclass/stark_compass_explorer/blob/main/lib/starknet_explorer_web/components/layouts/root.html.heex)

## OpenApi → 

- [Open API Specifications for Elixir Plug applications](https://github.com/open-api-spex/open_api_spex)
- [How to design better APIs. 15 language-agnostic, actionable tips on REST API design](https://r.bluethl.net/how-to-design-better-apis)
- [Swagger integration to Phoenix framework](https://github.com/xerions/phoenix_swagger)

## Counter App Error →

Solution for the following error:

```bash
17:08:15.791 [error] Could not check origin for Phoenix.Socket transport.

Origin of the request: http://<public_ip>:4000

This happens when you are attempting a socket connection to
a different host than the one configured in your config/
files. For example, in development the host is configured
to "localhost" but you may be trying to access it from
"127.0.0.1". To fix this issue, you may either:
```

In `config\runtime.exs` , the `port:` parameter isn’t needed, the app uses `4000`. And the important part, the `host` parameter needs brackets → `[host: host]` to work and the `PHX_HOST` env variable doesn’t have to be set, so that “0.0.0.0” is truthy and used as host:

```elixir
#[...]
host = System.get_env("PHX_HOST") || "0.0.0.0"
port = String.to_integer(System.get_env("PORT") || "4000")

config :counter, :dns_cluster_query, System.get_env("DNS_CLUSTER_QUERY")

config :counter, CounterWeb.Endpoint,
  url: [[host: host], scheme: "http"],
  http: [
    # Enable IPv6 and bind on all interfaces.
    # Set it to  {0, 0, 0, 0, 0, 0, 0, 1} for local network only access.
    # See the documentation on https://hexdocs.pm/plug_cowboy/Plug.Cowboy.html
    # for details about using IPv6 vs IPv4 and loopback vs public addresses.
    ip: {0, 0, 0, 0, 0, 0, 0, 0},
    port: port
  ],
  secret_key_base: secret_key_base
#[...]
```

Now the app can be accessed with `localhost:4000` and `<public_ip>:4000`
