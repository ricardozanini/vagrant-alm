# Vagrant ALM - Application Lifecicle Management for labs and tests

This project aims to build a [ALM](https://en.wikipedia.org/wiki/Application_lifecycle_management) infrastructure within a virtual environment using [Vagrant](https://www.vagrantup.com/) and provisioning with [Ansible](https://www.ansible.com/).

The picture bellow illustrates the infrastructure defined in the Vagrant file:

![Vagrant ALM infrastructure](https://raw.githubusercontent.com/ricardozanini/vagrant-alm/master/infrastructure_view.png)

The process could be summarized as follows:

1. The main point of interest in this scenario is Jenkins doing his job which is orchestrating a CI/CD process.

2. Next we send the code to be analised by Sonar Source for find some buggy code and to check how good our tests are.

3. For artifact archiving we use Nexus v.3 so our binaries could reside there for later deploy.

4. And finally, a simple CentOS 7 machine waiting to receive the binaries and be provisioned by Jenkins using the [Ansible Plugin](https://wiki.jenkins.io/display/JENKINS/Ansible+Plugin).

I've managed to build a simple lab using this sample workflow that could be found [in here](https://github.com/ricardozanini/soccer-stats). Also, if you are interested, [there's a presentation](https://docs.google.com/presentation/d/1sXz5b_PX6wuhpXJqanXD3Sk-AMdL4SV_J628pr7Ccwc/edit?usp=sharing) that I did on Ansible Meetup in São Paulo describing this scenario. Enjoy!

## How to use

Just install Vagrant on your unix machine and you are ready to go. I've tried to keep the `Vagrantfile` as much agnostic as possible to be simple to reproduce on any Vagrant environment. Even if you couldn't manage to work, I hope this file could help you at least as a reference. Also install the Vagrant Networkmanager plugin to have your `hosts` file configured accordingly: `vagrant plugin install vagrant-hostmanager`.

Remember that the Ansible playbooks on this repo are the real project rock star. It's responsible to give the environment everything you need to build your ALM.

Don't forget to install the Ansible Roles from the repository by executing the following command from the project's root dir:

`ansible-galaxy install -r requirements.yml`

### Jenkins configuration post install

After your Jenkins server got provisioned, you will need to set:

1. Ansible Tower Global Credentials:
   a. Username: your username
   b. Password: your pass
   c. ID: tower

2. Ansible Tower Installation:
   a. Name: tower
   b. URL: https://tower.local
   c. Credenctials: the same that was set in the last step
   d. Force Trust Cert: true

3. Maven in Global Tool Configuration:
   a. Name: M3
   b. MAVEN_HOME: `/opt/apache-maven-3.5.4`

### Ansible Tower

To provision Ansible Tower you have to first install VirtualBox and then execute: `vagrat up tower --provider virtualbox`. A brand new machine with Ansible Tower will be provisioned for you. After installation, go to  http://tower.local and set your trial license and you're a set!

## Notes

The original ansible role [`geerlingguy.sonar`](https://github.com/geerlingguy/ansible-role-sonar) doesn't work with Postgres. I've already sent a [pull request](https://github.com/geerlingguy/ansible-role-sonar/pull/32) to the original author but until now, he haven't have the time to merge. In the future I have plans to aniquilate the `zanini.sonar` role from the local dir and rely only on the Geerling Guy's role.

There's [an issue on Vagrant 1.9.1](https://github.com/mitchellh/vagrant/issues/8166) that is very annoying: the `eth01` needs a restart after bring the machine up. Do the following after sshing at the machine:

`sudo /etc/init.d/network restart`

## Credits

- [Geerling Guy](https://github.com/geerlingguy) for the awesome roles
