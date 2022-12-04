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

Apply the playbook:

    ansible-playbook deployment.yaml
