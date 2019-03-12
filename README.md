# wait-server

Tool (as Bash script) to wait for a server to be online (connectable), in a certain duration.

## Usage

```
wait-server 8000
```

is to wait for a server at port 8000, on localhost to be online. After 5 seconds, the program will exit with error.

Full options:

```
wait-server [[HOST] PORT] [-t <timeout>][-q][-h]
```

where:

- HOST: Host name or IP address of server. Default is localhost.
- PORT: Port where the server is listening. Default is 8000.
- t: Timeout. Default is 5s.
- q: Quiet, don't show progress dots.


Main use of this tool, is to used in a _systemd_ service, to delay the service until some server is online. For example, you have `service-a` and `service-b`, in which `service-a` is a server software that takes long time to initialize (often Java application), and `service-b` should be run only after `service-a` is ready. You can write the _systemd_ service file like this:


```ini
[Unit]
After=service-a.service

[Service]
ExecStartPre=/usr/bin/wait-server
ExecStart=/usr/bin/application-b
```
