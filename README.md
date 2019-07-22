# playground-tf-plugin
TF custom plugin example

# Pre-requirements

- [Terraform](https://www.terraform.io/downloads.html)
- [Golang](https://golang.org/dl/)
- [Vagrant](https://www.vagrantup.com/downloads.html)

## create new repository (github) & clone it

```bash
git clone git@github.com:andrewpopa/playground-tf-plugin.git
cd playground-tf-plugin
```

## use Vagrant file from existing [repo](https://github.com/kikitux/golang-110)

copy `Vagrantfile` from repo above

```bash
vagrant up
vagrant ssh
```

## compile custom plugin (example from [here](https://github.com/petems/terraform-provider-extip))

```bash
go get github.com/petems/terraform-provider-extip
cd ~/go/src/github.com/petems/terraform-provider-extip
make build
ls -al ~/go/bin/
```

## copy custom plugin to the required path
```bash
mkdir -p /vagrant/terraform.d/plugins/linux_amd64
cp ~/go/bin/terraform-provider-extip /vagrant/terraform.d/plugins/linux_amd64/
```

## add to main.tf example from plugin [repo](https://github.com/petems/terraform-provider-extip/blob/master/examples/main.tf)

```
cd /vagrant
touch main.tf
```

```bash
# main.tf
data "extip" "external_ip" {
}

data "extip" "external_ip_from_aws" {
  resolver = "https://checkip.amazonaws.com/"
}

output "external_ip" {
  value = "${data.extip.external_ip.ipaddress}"
}

output "external_ip_from_aws" {
  value = "${data.extip.external_ip_from_aws.ipaddress}"
}
```

## test it
```bash
terraform init
terraform apply
```

## install rbenv
```bash
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
```

## install ruby-build (gives - rbenv install)
```bash
git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
```

## configure ruby 2.4.0
```bash
rbenv install 2.4.0
rbenv local 2.4.0
rbenv versions
sudo gem install bundler
bundle install
```

## execute kitchen tests
```bash
bundle exec kitchen converge
bundle exec kitchen verify
bundle exec kitchen destroy
```

## cleanup
```
vagrant destroy -f
```

## Enjoy! 
