# Devops Lesson

> Marouani Ceif-Edine

## TP1

### Question 1.1 Document your database container essentials: commands and Dockerfile

Comments done on the DockerFile inside /database

- `docker build -t kingkongbilly/postgres .` is used to create a Docker image named kingkongbilly/postgres from the Dockerfile in the /database directory.
- `docker network create app-network` is used to create a custom network, allowing us later to bridge communication between differents containers.
- `docker run -p 5432:5432 --name database --network app-network kingkingbilly/postgres` the -p option is for port binding, so that any localhost connection on the 5432 port is redirected to the docker container 5432 port. The -network is for selecting a network and building the image throught that network.
- `docker run  -p "8090:8080"  --net=app-network --name=adminer  -d  adminer` --net is the same as --netowrk, -d is for detach, so we wont attach the logs to current terminal. Adminer is a tool to visualise and access the database on the web (usefull in case you want to check the data inside your database).

### Question 1.2 : Why do we need a multistage build? And explain each step of this dockerfile

In the case of a spring boot application, we may need to build the application before we run it, so to keep things lightweight (as the build stage image, which includes the JDK, Maven, and source code, can be significantly larger than the runtime image), we can multistage our build. Maintenance can be esaier, as if one of the containers don't run you we will target faster where the problem comes from. Also for a security reason, the final containers doesn't contain sensible tools that can have security problems.

#### Question 1.3 Document docker-compose most important commands

Docker-compose is a community tool now officially integrated in the docker environnement because it permit to run mutli-container docker applications.

- `docker-compose up` This command is used to create and start all the containers defined in the docker-compose.yml file. If the containers do not already exist, docker-compose creates them following the specifications in the file
- `docker-compose down` This command stops and removes all the containers that were created with docker-compose up. It also removes the default network, but not the volumes, unless you specify the -v option. This is useful for cleaning up all the resources that were created.
- `docker-compose build` This command is used to build or rebuild containers specified in the docker-compose.yml file.

#### Question 1.4: Document your docker-compose file

Done inside the file

#### Question 1.5: Document your publication commands and published images in dockerhub

> In the case of the httpd server

- `docker build -t kingkongbilly/httpd . ` build the container with the current directory as context.

* `docker tag docker-compose_httpd kingkongbilly/httpd:1.3` tag is a docker command used to create a new tag for an image. the first part is the source image (have to already exist), the second part is the following github conventions, USERNAME/REPOSITORY_NAME:VERSION.
* `docker push kingkongbilly/httpd:1.3` For push the current version of the docker image to DockerHub.

## TP2

#### Question 2.1: What are testcontainers?

They simply are java libraries that allow you to run a bunch of docker containers while testing.

#### Question 2.2: Document your Github Actions configurations?

Done inside the /.github/worflows directory

#### Question 2.3: Document your quality gate configuration.

SonarCloud analyzes the codebase for bugs, vulnerabilities, and code smells. We integrated SonarCloud with our GitHub Actions workflow, we added a step that runs the SonarCloud scanner during the build process and used github secrets (similar for Docker secrets environnement that we needed).

> Bonus respect : the second workflow is only triggered if the first successfully completed

## TP3

#### Question 3.1: Document your inventory and base commands.

To connect to a distant server with ansible, a setup have to be done in the inventory, it's done the /inventories folder. Once the setup done, we use the following command to connect :
` ansible all -i inventories/setup.yml -m setup -a "filter=ansible_distribution*"` all stand for all the machines in the setup file, -i inventories/setup.yml is the location of the inventory,"filter=ansible_distribution\*" apply a filter that limits the returned facts so that "logs" that are displayed are only about ansible distribution (ex: version), -m setup is the module to use.

another variant : ` ansible all -i inventories/setup.yml -m yum -a "name=httpd state=absent" --become` -a "name=httpd state=absent" tells yum (RedHat package manager) that the package named httpd (Apache server) is absent, meaning it should be uninstalled if it is currently installed, become permit ansible to be root.

#### Question 3.2: Document your inventory and base commands.

Comment done on the file

> NB roles are created by the following commande : `ansible-galaxy init roles/[NAME OF THE ROLE]`, each role contain a list of steps to do once they are called, similarly to a docker compose file or a github workflow job

#### Question 3.3: Document container tasks

Each container task is inside a role, where are defined the different settings specific to each container (such a environnement variables or more specific behaviours like a command to run following the services tag)

#### Ansible Vault

Ansible Vault is a tool included with Ansible that allows us to encrypt files containing sensitive or confidential information, such as passwords or secret keys, to secure them. This enables us to store this information securely in our code repositories or version control systems without exposing sensitive data in plain text.

Before we can use Ansible Vault, we first need to encrypt our files containing sensitive data. To do this, use the following command:
`ansible-vault encrypt secrets.yaml`

When we running a playbook that references vault, we need to provide the password used to encrypt the file, the following command show how to use the playbook with vault :
`ansible-playbook -i inventories/setup.yml -e @secrets.yaml --ask-vault-pass playbook.yml`

#### Front

Bonus done, added the frontend to the different dockerized parts and inside the httpd.conf of the proxy.

#### Finally

I've thoroughly enjoyed discovering DevOps and the CI/CD practices. Learning to use Docker, GitHub Actions, and Ansible has made the whole field of DevOps seem much more accessible to me, even presenting itself as a viable career option.
