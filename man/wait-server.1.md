% wait-server(1)
% Nguyễn Hồng Quân <ng.hong.quan@gmail.com>
% March 2019


# NAME

wait-server - Test and wait for server availability

# SYNOPSIS

**wait-server** \[[_HOST_] _PORT_] [**-t** _\<timeout\>_][**-q**][**-h**]

# DESCRIPTION

**wait-server** tries connecting to port **PORT** at **HOST** (a domain name or IP address) in 5 seconds. Before that time is exhausted, if the port is connected successfully, **wait-server** will exit with success status (_0_), otherwise, it will exit with error status (_1_). You can change the timeout with option **-t**.

# ARGUMENTS

**PORT**
: TCP port where the server is supposed to listen. Default to _8000_.

**HOST**
: Hostname or IP address of the server. Default to _localhost_. Note that when this argument is given, you must provide **PORT**.

# OPTIONS

**-t**
: Duration in second to test connectivity, before exiting. **wait-server** will retry to connect every second in this time range. Default is _5_ second.

**-q**
: Quiet. If this option is absent, the tool will show a sequence of dots to indicate its progress.

**-h**
: Show help and exit.

# EXAMPLES

```
$ wait-server 5000
$ wait-server agriconnect.vn 80
$ wait-server 8086 -t 10
```

Main purpose of this tool, is to be used in a _systemd_ service, to delay the service until some server is online. For example, you have _service-a_ and _service-b_, in which _service-a_ is a server software that takes long time to initialize (often Java application), and _service-b_ should be run only after _service-a_ is ready. You can write the _systemd_ service file like this:


```ini
[Unit]
After=service-a.service

[Service]
ExecStartPre=/usr/bin/wait-server
ExecStart=/usr/bin/application-b
```
