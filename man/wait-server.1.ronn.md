wait-server(1) - Test and wait for server availability
======================================================

## SYNOPSIS

`wait-server` [[HOST] PORT] [-t <timeout>][-q][-h]

## DESCRIPTION

**wait-server** tries connecting to port `PORT` at `HOST` (a domain name or IP address) in 5 seconds. Before that time is exhausted, if the port is connected successfully, `wait-server` will exit with success status (`0`), otherwise, it will exit with error status (`1`). You can change the timeout with option `-t`.

## ARGUMENTS

  * `PORT`:
   TCP port where the server is supposed to listen. Default to 8000.

  * `HOST`:
  Hostname or IP address of the server. Default to `localhost`. Note that when this argument is given, you must provide `PORT`.

## OPTIONS

  * `-t`:
    Duration in second to test connectivity, before exiting. `wait-server` will retry to connect every second in this time range.

  * `-q`:
    Quiet. If this option is absent, the tool will show a sequence of dots to indicate its progress.

  * `-h`:
    Show help and exit.

## EXAMPLES

    $ wait-server 5000
    $ wait-server agriconnect.vn 80
    $ wait-server 8086 -t 10

Main purpose of this tool, is to be used in a _systemd_ service, to delay the service until some server is online. For example, you have `service-a` and `service-b`, in which `service-a` is a server software that takes long time to initialize (often Java application), and `service-b` should be run only after `service-a` is ready. You can write the _systemd_ service file like this:


```ini
[Unit]
After=service-a.service

[Service]
ExecStartPre=/usr/bin/wait-server
ExecStart=/usr/bin/application-b
```
