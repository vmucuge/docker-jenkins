# docker-compose
Docker compose

Use Vagrant, docker-compose for testing and integrate with Jenkins

A Vagrant provisioner for [Docker Compose](https://docs.docker.com/compose/). Installs Docker Compose and can also bring up the containers defined by a [docker-compose.yml](https://docs.docker.com/compose/yml/).

## Install

```bash
vagrant plugin install vagrant-docker-compose
```

## Usage

### To install docker and docker-compose

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.provision :docker
  config.vm.provision :docker_compose
end
```

### To install and run docker-compose on `vagrant up`

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.provision :docker
  config.vm.provision :docker_compose, yml: "/vagrant/composes/docker-compose.yml", run: "always"
end
```

Equivalent to running:

```bash
docker-compose -f [yml] up -d
```

### To install and run docker-compose, with multiple files, on `vagrant up`

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.provision :docker
  config.vm.provision :docker_compose,
    yml: [
      "/vagrant/composes/docker-compose-base.yml",
      "/vagrant/composes/docker-compose.yml",
      ...
    ],
    run: "always"
end
```

Equivalent to running:

```bash
docker-compose -f [yml-0] -f [yml-1] ... up -d
```

### To install, rebuild and run docker-compose on `vagrant up`

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.provision :docker
  config.vm.provision :docker_compose, yml: "/vagrant/composes/docker-compose.yml", rebuild: true, run: "always"
end
```

Equivalent to running:

```bash
docker-compose -f [yml] rm --force
docker-compose -f [yml] build
docker-compose -f [yml] up -d
```

### To install, rebuild and run docker-compose with options on `vagrant up`

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.provision :docker
  config.vm.provision :docker_compose, yml: "/vagrant/composes/docker-compose.yml", rebuild: true,
    options: "--x-networking", command_options: { rm: "", up: "-d --timeout 20"}, run: "always"
end
```

Equivalent to running:

```bash
docker-compose --x-networking -f [yml] rm
docker-compose --x-networking -f [yml] build
docker-compose --x-networking -f [yml] up -d --timeout 20
```


### Other configs

* `yml` – one or more [Compose files](https://docs.docker.com/compose/compose-file/) (YAML), may be a `String` for a single file, or `Array` for multiple.
* `compose_version` – defaults to `1.5.0`.
* `project_name` – compose will default to naming the project `vagrant`.
* `executable` – the location the executable will be stored, defaults to `/usr/local/bin/docker-compose`.
* `options` - a `String` that's included as the first arguments when calling the docker-compose executable, you can use this to pass arbitrary options/flags to docker-compose, default to `nil`.
* `command_options` - a `Hash` of docker-compose commands to options, you can use this to pass arbitrary options/flags to the docker-compose commands, defaults to: `{ rm: "--force", up: "-d" }`.

## Example

See `example` in the repository for a full working example.
