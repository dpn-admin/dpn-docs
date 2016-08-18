# DPN Documentation

## Setting Up a DPN Node

A DPN node may provide one or more functions:
- a metadata registry of DPN content and transactions
- ingest of content into DPN bags
- replication of DPN bags
- auditing of DPN bags
- restoration of DPN bags

The specifications for these and other functions are in two areas:
- [dpn-admin/dpn-docs](https://github.com/dpn-admin/dpn-docs)
  - various documents about DPN designs and functions
- [dpn-admin/dpn-rest-spec](https://github.com/dpn-admin/dpn-rest-spec)
  - an authoritative specification of a REST API for DPN
  - branches contain different versions, e.g.
    - [api-v1](https://github.com/dpn-admin/dpn-rest-spec/tree/api-v1)
    - [api-v2](https://github.com/dpn-admin/dpn-rest-spec/tree/api-v2)

There are several software projects in the [dpn-admin](https://github.com/dpn-admin)
github organization to provide implementations of these functions:
- [dpn-admin/dpn-server](https://github.com/dpn-admin/dpn-server)
  - a ruby-on-rails application
  - implements [dpn-admin/dpn-rest-spec](https://github.com/dpn-admin/dpn-rest-spec)
  - implementations for DPN bag transfers using delayed jobs
- [dpn-admin/dpn-client](https://github.com/dpn-admin/dpn-client)
  - a ruby client for the DPN REST API
  - https://rubygems.org/gems/dpn-client
- [dpn-admin/dpn-sync](https://github.com/dpn-admin/dpn-sync)
  - a ruby project to sync registry data and DPN bags
  - based on the Sidekiq jobs, with a Sinatra dashboard
  - relies on:
    - [dpn-admin/dpn-client](https://github.com/dpn-admin/dpn-client)
    - [dpn-admin/dpn-bagit](https://github.com/dpn-admin/dpn-bagit)
- [dpn-admin/dpn-bagit](https://github.com/dpn-admin/dpn-bagit)
  - implements the [DPN BagIt specification](https://wiki.duraspace.org/display/DPNC/BagIt+Specification)
  - https://rubygems.org/gems/dpn-bagit/
