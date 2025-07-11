# LibYAML2.jl

A Julia wrapper for [libyaml](https://github.com/yaml/libyaml), providing fast and minimal YAML parsing.

## Why LibYAML2.jl?

[LibYAML.jl](https://github.com/JuliaData/LibYAML.jl) was unmaintained at the time `LibYAML2.jl` was created.

## Installation

To install LibYAML2, simply use the Julia package manager:

```julia
] add LibYAML2
```

## Usage

A basic example of parsing structured YAML data in Julia, including anchors and merge keys:

```julia
using LibYAML2

yaml_config = """
defaults: &defaults
  port: !!int 443
  enable_tls: true

server:
  <<: *defaults
  bind_address: "0.0.0.0"
  tls:
    cert_file: "/etc/certs/cert_file.pem"
    key_file: "/etc/certs/key_file.pem"

metrics:
  prometheus_enabled: true
  listen: "0.0.0.0:9100"
  service_labels: {service: "secure-backend"}

role_defaults: &role_defaults
  permissions:
    - read
    - write

users:
  - username: "admin"
    <<: *role_defaults
  - username: "stanislav"
    <<: *role_defaults
    permissions:
      - read
"""

julia> parse_yaml(yaml_config)
Dict{String, Any} with 5 entries:
  "metrics" => Dict{String,Any}("listen"=>"0.0.0.0:9100", …)
  "users"   => Any[...]
  ...
```

## Comparison of [`YAML.jl`](https://github.com/JuliaData/YAML.jl) and `LibYAML2.jl`

| Feature                                | `YAML.jl`                         | `LibYAML2.jl`                      |
|----------------------------------------|-----------------------------------|-----------------------------------|
| Implicit type parsing                  | Always                            | Only when explicitly specified    |
| YAML emission (serialization) support  | ✅                                | ❌                                |
| `!include` support                     | ❌                                | ✅                                |
| Newline handling in quoted strings     | ❌ Broken                         | ✅ Correct                         |
| Multiple top-level documents (`---`)   | ❌ (`load_all_file` broken)        | ✅                                |
| YAML 1.2 compliance                    | Limited                           | Full (via `libyaml`)              |
| Pure Julia implementation              | ✅                                | ❌ (uses `libyaml`)               |
| Performance                            | Low                               | High                              |

## Useful Links

- [libyaml](https://github.com/yaml/libyaml) – Official library repository.  
- [LibYAML_jll.jl](https://github.com/JuliaBinaryWrappers/LibYAML_jll.jl) – Julia wrapper for libyaml.
