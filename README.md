# Provisioning

- This is our following `Vagrantfile`:
```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.network "private_network", ip: "192.168.10.100"
  config.hostsupdater.aliases = ["development.local"]

  config.vm.synced_folder "app", "/app"
end
```

- In this context it will be about providing our VM's with instructions, variables, files and folders.

- We can essentially set up a machine in a specific state when turned on

<br>

**Core sections of provisioning**
1. Tool agnostic
2. Language agnostic

<br>

- Making files available
- Being able to run commands/scripts
- Injecting environment variables

<br>

**Managing running services in Linux**
- `sudo systemctl <action> <service>`



<br>

**Running a bash script**
- A bash script can install packages, alter files, do actions in our Linux servers when we run vagrant up

- This allows us to set up the machine to a state that is desirable for us, essentially automating the process of creating a VM with the necessary packages installed

<br>

1. Create a `provision.sh` file. Include the following commands in there:
```sh
#!/bin/bash

sudo apt-get update
sudo apt-get install nginx -y
```

<br>

2. Include it in the `Vagrantfile` -- do this by adding a line in config. The file should look like the following now:
```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.network "private_network", ip: "192.168.10.100"
  config.hostsupdater.aliases = ["development.local"]

  config.vm.synced_folder "app", "/app"

  #runs a .sh file, paths are relative to the vagrantfile
  config.vm.provision "shell", path: "environment/provision.sh"

end
```

<br>

**New projects**
- When given a new project, you want to ask a few questions to help you get going:
    - What language is it in?
    - Is there some framework that needs to be installed? (rails, flask, django, wordpress etc.)
    - Any specific packages? If so, is there some list? (gemfile, requirements.txt)
    - Any tests that need to be run?

**In this case:**
- A framework to install? No. We only need the testing framework `rspec`

- Any specific package? Yes. With the environment file, you have a `Gemfile` with dependencies

- Any tests? Yes. You have integration tests to run outside the machine - `rake spec`. There might be some unit tests in JS inside.


**Ruby**
- `gem` and `bundler` are the equivalents in Ruby to `pip` and `packages` in Python

- `bundler` is Ruby's package manager

- Ruby's testing framework is rpsec. This needs to be installed
- Gemfile keeps a list of dependencies to be installed

- To install gems from the `Gemfile`, one must run: `bundle install`

**Ruby testing**
1. Go to folder with `Gemfile`
2. Run `bundle` in terminal

THEN:

1. `rake spec` to run the tests 


---
AIMS:
- run tests
- understand what they are testing
- solve these failing tests

WHAT YOU SHOULD KNOW:
- how to provision your machone with code
- instructions