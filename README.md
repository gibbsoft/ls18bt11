# ls18bt11

![These aren't the droids you're looking for.](https://i.pinimg.com/originals/78/26/04/7826044ab1e045267d1019ce1ebbae35.jpg)

> Notes for OSX, FIXME: other platforms to follow.

Install `homebrew`:

```bash
$ xcode-select --install
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
Clone this repo:


```bash
$ git clone git remote add origin https://github.com/gibbsoft/ls18bt11.git
$ cd ls18bt11
```

Install a few useful packages:

```bash
$ brew install python
$ pip install virtualenv
```

Next you need to install the right version of ansible etc, which is best
carried out using a python virtualenv:

```bash
$ virtualenv venv -p`which python`
$ . venv/bin/activate
$ pip install -r requirements.txt

# tip: to deactivate, just type `deactivate`
```

Assemble the ansible roles:

```bash
$ ansible-playbook -clocal -i0, get_roles.yml
```

#### Vagrant

The `Vagrantfile` in the root of the repo is used to configure VMs for local development and testing.

```bash
$ vagrant up
$ vagrant ssh
$ vagrant destroy
```

#### Running against infrastructure

Running the playbook:

1. Firstly update the inventory file `hosts` with the required nodes

2. Ensure you have the private key in `~/.ssh/id_rsa_ls18bt11`

3. Run ansible:

```bash
$ ansible-playbook remediate_docker.yml
```

Example adhoc command:

```bash
$ ansible dockers -a "docker-compose --version" >backups/txt/docker_compose_version.txt
```
