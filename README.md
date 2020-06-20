Deploying a Simple Web Application on DreamCompute
==================================================

Introduction
------------
In this article, you'll learn to spin up a compute instance on an
OpenStack public cloud ([DreamCompute](http://dreamhost.com/cloud/compute/))
using the popular automation tool, [Ansible](http://www.ansible.com).  To get
started:

* Sign up for a [DreamCompute account](https://signup.dreamhost.com/compute/)
  (DreamHost offers a free trial period)
* Log in to the [DreamCompute Dashboard](http://dashboard.dreamcompute.com) and
  [create an SSH
  key](https://dashboard.dreamcompute.com//project/access_and_security/keypairs/create/)
  (filling out the form will download a PEM keyfile you'll use to
  authenticate to your server).  Remember the name of the key that you create \-
  you'll need that value later.
* Install a recent version of [Python](https://www.python.org/downloads/) (2.7+)
* Install the `helloworld` Python package in this repository, ``shade``, and
  ``ansible``:

    ``$ git clone https://github.com/ryanpetrello/dreamcompute-hello-world.git hello-world``

    ``$ cd hello-world && pip install -e . shade ansible``

Deploying with Ansible
----------------------
We'll use Ansible to create a [repeatable "playbook" of
commands](https://github.com/ryanpetrello/dreamcompute-hello-world/blob/master/playbooks/deploy.yml)
to set up the Hello World application on a DreamCompute server.  These commands
include:

* Launching a server in DreamCompute and assigning a public IP address to it

In order for Ansible to run DreamCompute API calls on your behalf, you'll need
to download a small shell script that sets up your API credentials.  Do so by
visiting [API
Access](https://dashboard.dreamcompute.com/project/access_and_security/api_access/openrc/)
in the OpenStack Dashboard.  After the shell script downloads, open your
computer's command line and run the following (substituting the actual
location of your downloaded file).

    $ source /path/to/downloaded/file/dhc123456789-openrc.sh

You'll be prompted for a password - it's the one you use to log in to the
DreamCompute Dashboard.

At this point you should be ready to deploy your application.  Do so by running
the following commands:

    $ chmod 600 /path/to/keyname.pem
    $ ansible-playbook -vvvv -i "localhost," playbooks/deploy.yml --extra-vars "key_name=keyname private_key=/path/to/keyname.pem"

You'll need to substitute the ``keyname`` key name value for the actual name
you chose earlier, and you'll also need to replace ``/path/to/keyname.pem``
with the actual path to the PEM file you downloaded.

If all is well, you'll be greeted with an instructional message:

    Visit http://1.2.3.4/ in your browser!
