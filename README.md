# setting up pleroma on FreeBSD

This repository consists of an ansible playbook that basically follows along the [official FreeBSD installation guide](https://docs-develop.pleroma.social/backend/installation/freebsd_en/) for [pleroma](https://pleroma.social/).

To be useful you will need to adjust the variables in  `inventory.yaml` and `deployment.yaml`.

YMMV.

## local setup

install ansible locally:

    python3 -m pip install --user ansible

## bootstrap host

Install sudo and python:

    pkg install sudo python3

Run `visudo` so that this line exists:

    %wheel  ALL=(ALL)       NOPASSWD: ALL

Back on your local machine, prepare the inventory:

    cp inventory_sample.yaml inventory.yaml

Edit the variables to your needs, then apply the playbook:

    ansible-playbook deployment.yaml

This will setup everything except for the final configuration of the pleroma application itself, since that relies on interactive input.

To complete that step ssh into the host, become the plerome user and run the configuration script like so:

    sudo su - pleroma
    $ cd pleroma
    $ MIX_ENV=prod mix pleroma.instance gen

Answer the questions, then:

    $ mv config/generated_config.exs config/prod.secret.exs

Then:

    sudo su - postgres
    $ psql -f /home/pleroma/pleroma/config/setup_db.psql

Then again as pleroma:

    sudo su - pleroma
    $ cd pleroma
    $ MIX_ENV=prod mix ecto.migrate
    
You can then start the application once manually to prime everything:

    $ MIX_ENV=prod mix phx.server

Interrupt it and start it as service:

    $ sudo service pleroma start

Finally, create an admin user:

    $ sudo su - pleroma
    $ cd pleroma
    $ MIX_ENV=prod mix pleroma.user new <username> <your@emailaddress> --admin

You now should be able to visit the URL printed and start using the instance.