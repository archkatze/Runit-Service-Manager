# `rsm` - Runit Service Manager

- This is the CLI Runit Service Manager {rsm}, forked from https://gitea.artixlinux.org/linuxer/Runit-Service-Manager, which is a fork of Void Service Manager {vsv} https://github.com/bahamas10/vsv/blob/master/vsv

- Terminal Commands are pretty much the same as sv with a beautified layout.

![](https://imgur.com/S9zdEIU.png)

## Manage and view runit services.

Usage
-----

Quick Examples:

- `rsm` - show all services
- `rsm status` - same as above
- `rsm stop <svc>` - stop a service
- `rsm start <svc>` - start a service
- `rsm restart <svc>` - restart a service, or start service right after `rsm enable <svc>`
- `rsm enable <svc>` - enable a service (autostart at boot)
- `rsm disable <svc>` - disable a service (no autostart at boot)
- `rsm hup <svc>` - refresh a service (`SIGHUP`)
- `rsm logs <svc>` or `rsm alllogs <svc>` - lists all logs for a service (access and error)
- `rsm errorlogs <svc>` - lists all error logs for a service
- `rsm edit <svc>` - edit the run file of a service with the EDITOR environment variable

Status:

The `status` subcommand has the following fields:

- `SERVICE` - the service (directory) name.
- `STATE` - the service state: output from `.../$service/supervise/stat`.
- `ENABLED` - if the service is enabled (lacks the `.../$service/down` file).
- `PID` - the pid of the process being monitored.
- `COMMAND` - arg0 from the pid being monitored (first field of `/proc/$pid/cmdline`.
- `TIME` - time the service has been in whatever state it is in.

Command Usage:

    [rsm]    Manage and view runit services
    [rsm]    Made specifically for Void Linux but should work anywhere
    [rsm]    Author: Dave Eddy <dave@daveeddy.com> (bahamas10)

    USAGE:
    rsm [OPTIONS] [SUBCOMMAND] [<ARGS>]
    rsm [-u] [-d <dir>] [-h] [-t] [SUBCOMMAND] [...]

    OPTIONS:
    -c <yes|no|auto>          Enable/disable color output, defaults to auto
    -d <dir>                  Directory to look into, defaults to env SVDIR or /var/service if unset
    -h                        Print this message and exit
    -l                        Show log processes, this is a shortcut for 'status -l'
    -t                        Tree view, this is a shortcut for 'status -t'
    -u                        User mode, this is a shortcut for '-d ~/runit/service'
    -v                        Increase verbosity
    -V                        Print the version number and exit

    ENV:
    SVDIR                     The directory to use, passed to the 'sv' command, can
                              be overridden with '-d <dir>'

    SUBCOMMANDS:
    status [-lt] [filter]     Default subcommand, show process status
                              '-t' enables tree mode (process tree)
                              '-l' enables log mode (show log processes)
                              'filter' is an optional string to match service names against

    enable <svc> [...]        Enable the service(s) (remove the "down" file, does not start service)

    disable <svc> [...]       Disable the service(s) (create the "down" file, does not stop service)

    Any other subcommand gets passed directly to the 'sv' command, see sv(1) for the
    full list of subcommands and information about what each does specifically.
    Common subcommands:

    start <service>           Start the service
    stop <service>            Stop the service
    restart <service>         Restart the service
    reload <service>          Reload the service (send SIGHUP)

    EXAMPLES:
    rsm                       Show service status in /var/service
    rsm status                Same as above
    rsm -t                    Show service status + pstree output
    rsm status -t             Same as above
    rsm status tty            Show service status for any service that matches tty*
    rsm check uuidd           Check the uuidd svc, wrapper for 'sv check uuidd'
    rsm restart sshd          Restart sshd, wrapper for 'sv restart sshd'
    rsm -u                    Show service status in ~/runit/service
    rsm -u restart ssh-agent  Restart ssh-agent in ~/runit/service/ssh-agent

Syntax
------

This project uses:

- Bash Style Guide: https://www.daveeddy.com/bash/
- `shellcheck`: https://github.com/koalaman/shellcheck

```
$ make check
shellcheck rsm
```

License
-------

MIT License
