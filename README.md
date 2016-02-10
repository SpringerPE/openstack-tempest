# docker-tempest-openstack

Docker container to run smoke tests against an OpenStack environment. It is not
dangerous (unless you intentionally run the stress tests) by running it without
args.

* To build the Docker container `./build.sh` (name of the image creaded: tempest). 
  Type `docker images` to see it.

* To run the container, do the auto-configuration and list all the tests,
  just put a `openrc.sh` file in this folder (admin or other user/project with
  admin role) and type `./run.sh` without args. Without args it will list all the test
  available and a `tempest` folder will be created to store the test repository. 


# Configuration

You can provide your own tempest configuration by creating these files in this 
folder (apart of the `openrc.sh` file) before running `./run.sh` for first time:

* tempest.conf
* accounts.yaml
* logging.conf  

If those files are found, no autoconfiguration will be done. Otherwise, the 
container will try to find out the tempest settings for your environment, by
using the OpenStack client `openstack`.

Autoconfiguration is not easy for complicated environments, so in order to
help or for fine tuning, you can provide some variables, have a look at `run.sh`
and adapt it for your environment.  

By default it uses `ostestr` command to run the tests: http://docs.openstack.org/developer/os-testr/readme.html
but it is possible to switch to `testr` by defining  `TEMPEST_COMMAND=testr`


# Examples

Tempest repository is created in `tempest`. 

To see the list of available tests: `./run.sh`

To run help: `./run.sh --help`

To run api tests with pretty print and one test at a time (concurrency == 1) : `./run.sh -p -c 1  --regex '(^tempest\.(api))'`


# Hacking the container

Go to `docker` folder and:

* `conf` are the files will be copied to `\etc\tempest`
* `confd` is the configuration for confd and the template for tempest autoconfiguration file.
* `bin` includes the entrypoint (`init.sh`) and the `confd` utility.
* `init` includes the files for initialization and autoconfiguration which are launched from
   `bin\init.sh', otherwise they could run using the `my_init` system of `phusion/baseimage`


## Author

José Riguera López  <jose.riguera@springer.com> 
