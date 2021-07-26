
Role Name
=========

Docker Engine Overlay Ansible role for bionic (Ubuntu 18.04)......

Description
-----------
This role will install docker-ce or docker-ee. This role also includes CIS hardening. 

The CIS Bencmark used for this release is covered in the [version.txt](version.txt)


Requirements
------------

This build requires an Ubuntu 18.04 build with access to Canonical's apt repositories.  
Future releases will also include Focal (20.04).  
This may extend to RedHat derived builds.   

Role Variables
--------------

This section cover the variables required for use within this role:

| Variable | Example | Description |
| -------- | ------- | ----------- |
| docker_user | user1 | The name of the user that you will be using. |  
| ubuntu_release | bionic | Default to bionic (18.04). |
| env_http_proxy | proxy.com:8080  | Set if a proxy is required. |
| env_https_proxy | proxy.com:8080 | set if a https proxy is required. |
| docker_registry | registry.com | Set if not using hub.docker.com |

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles...

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

Andy Seed (pipseed@gmail.com)
