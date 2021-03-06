# gNXI Test Client

A CLI tool for orchestrating tests against a gNXI target.

## CLI

### Installing

```
go get github.com/google/gnxi/gnxi_tester
go install github.com/google/gnxi/gnxi_tester
```

### Running

Tests are provided in `~/.gnxi.yml`. By default, the following processes & clients will be run: 
- `provision`
- `gnoi_os`
- `gnoi_cert`
- `gnoi_reset`

For example, the auto generated `~/.gnxi.yml` will look like:
```yml
docker:
  build: golang:1.14-alpine
  runtime: alpine:latest
order:
- gnoi_os
- gnoi_cert
- gnoi_reset
tests:
  gnoi_cert:
  - args:
      op: get
    doesntwant: ""
    mustfail: false
    name: Get certs
    prompt: []
    wait: 10
    wants: GetCertificates:\n{\w*
    [...]
  gnoi_os:
  - args:
      op: install
      os: '&<os_path>'
      version: '&<os_version>' # escaped variables the user will be prompted for
    doesntwant: ""
    mustfail: false
    name: Compatible OS with Good Hash Install
    prompt:
    - os_version
    - os_path
    wait: 0
    wants: ^$
    [...]
  gnoi_reset:
  - args: {}
    doesntwant: ""
    mustfail: false
    name: Resetting a Target Successfully
    prompt: []
    wait: 0
    wants: ^$
  provision:
  - args:
      cert_id: '&<cert_id>'
      op: provision
    doesntwant: ""
    mustfail: false
    name: Provision Bootstrapping Target
    prompt:
    - cert_id
    wait: 0
    wants: Install success
```

If `[test_names]` are provided, only those tests are ran.
```
gnxi_tester run [test_names] \ 
--cert certs/ca.crt \
--key certs/ca.key \
--target_name target.com \
--target_address localhost:9339 \
--files "os_path:/path/to/image other_file:/path_to_file"
```

### Files required by service
Files to be passed in the `--files` flag.
#### `gnoi_os`
- `os_path`: Path to OS file used in `install` operation.

