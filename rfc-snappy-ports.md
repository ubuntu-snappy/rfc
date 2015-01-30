# The current situation

The current (spec)[https://developer.ubuntu.com/en/snappy/guides/packaging-format-apps/] states that ports are a top level entry in the `package.yaml` as

    name: go-example-webserver
    vendor: Alexander Sack <asac@canonical.com>
    architecture: amd64
    icon: meta/go.svg
    version: 1.0.1
    services:
     - name: webserver
       description: "snappy example: golang mini webserver"
       start: ./go-example-webserver
    ports:
       required: 80

## Critique

The ports declaration in it's current form gives only an indication that somewhere in this package the port is used. It is not known if it used by a service, a web front or a short lived binary.

# Proposal

The proposal, aligned to certain security features that are bound to happen, is to make ports part of the service or binary that wants to use them. Additionally, the description of their intended purpose to be more clear.

What this means is that ports are:

- bound to a service (or binary).
- have an associated protocol.
- *tagged* in such a way that other components can use them.

With these requirements in mind, the `package.yaml` would look like:

    name: foo
    description: description for foo
    version: 1.0.1
    services:
        - name: webserver
          description:  description of webserver
          start: bin/webserver
          ports:
               internal: 
                   localcomm1:
                       port: 3306/tcp
                       negotiable: yes
               external:
                   ui:
                       port: 8080/tcp 
                       negotiable: no

In this description, the port tags are:

- `localcomm1`: which can be used for local communication within the application.
- `ui`: this is a special key which will also be picked up by the *webdm*.

Tags are free form, but some can gain special use, such as an `external/ui` one which would be picked up by the `webdm` and used to open the *application* through the web.

There's also a new key, `negotiable`, which means that if the intended port is in conflict as it has been used by another package it will allow for it to be bound to another port (there is no implementation for this yet).