# consul-template-plugin-skip

DEPRECATED. In consul-template 0.18 will be "scratch" keyword, using to store information in templating process. Same as this plugin. So there is no need this plugin anymore.

[consul-template](https://github.com/hashicorp/consul-template) plugin to query all [Consul](https://www.consul.io) services in every DC

How to query all available services in all Consul datacenters (multi-datacenter environment)? Standart "services" function will query all services only in current (or named datacenter).

With this plugin you can do this easily.

Plugin is lightweight and support correct service renewal.

## Installation

Install script into any folder in system PATH environment variable

```
sudo cp skip.sh /usr/local/bin
```

## Usage

Usage from consul-template

```
# Init plugin (once per every template, always required)
{{plugin "skip.sh"}}

{{range $dc := datacenters}}{{range $services := services (printf "@%s" $dc)}}{{range $service := service (printf "%s@%s" $services.Name $dc)}}{{if plugin "skip.sh" $service.Name }}
{{$service.Name}}
{{end}}{{end}}{{end}}{{end}}
```

Filter by tag

```
# Init plugin (once per every template, always required)
{{plugin "skip.sh"}}

{{range $dc := datacenters}}{{range $services := services (printf "@%s" $dc)}}{{range $service := service (printf "%s@%s" $services.Name $dc)}}{{if $service.Tags | contains "some-tag-1"}}{{if plugin "skip.sh" $service.Name }}
{{$service.Name}}
{{end}}{{end}}{{end}}{{end}}{{end}}
```
