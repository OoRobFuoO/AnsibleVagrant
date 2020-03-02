# AnsibleVagrant

## Introduction

This is a local vagrant script to create 6 local vm on centos/7.

* VM Master - Used as the Ansible controller
* VM 1-3 - Used for any distributed system like Kafka as most need 3 nodes to form a quorum
* VM 4-5 - Used for any other client app
