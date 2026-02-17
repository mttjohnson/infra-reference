# Test System Container

## Update Variables

To create this implementation the `tofu/provider_incus.tf` and `tofu/variables.auto.tfvars` files will need to be updated.

For `tofu/provider_incus.tf` you will need to specify which Incus remote host you want to target to create the instance on. To see a list of the remtoes your CLI client has authenticated with run:
```bash
incus remote list
```

For `tofu/variables.auto.tfvars` you will want to make sure that the `cloud_init_ssh_key_pub_path` points to the path for the public key you want to authorized on the new instances. 
You should also update the static IP assignment with addresses that will work on your local network
```
static_ipv4_address = "10.10.30.10"
static_net_gateway  = "10.10.30.1"
static_net_dns      = "10.10.30.3"
```

You might also consider modifying the `tofu/.gitignore` file if you don't want the tfstate file contents to be maintained in the git repo. Some resources will store secrets or sensitive information in the state file, so it is important to be aware of what is maintained there or prevent it form being exposed.

## Initialize Instance

Setup dependencies
```bash
gitman install
direnv allow ansible
pushd ansible
    ./ansible_requirements.sh
popd
```

Create resources with OpenTofu
```bash
pushd tofu
tofu --version
tofu init
tofu plan
tofu apply
popd
```
