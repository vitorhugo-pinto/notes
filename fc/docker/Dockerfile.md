`Dockerfile` is **a text document that contains all the commands a user could call on the command line to assemble an image**.

`docker build -t <name> .`
`docker run <name>`

## [WORKDIR](https://docs.docker.com/reference/dockerfile/#workdir)

```Dockerfile
WORKDIR /path/to/workdir
```

The `WORKDIR` instruction sets the working directory for any `RUN`, `CMD`, `ENTRYPOINT`, `COPY` and `ADD` instructions that follow it in the Dockerfile. If the `WORKDIR` doesn't exist, it will be created even if it's not used in any subsequent Dockerfile instruction.

The `WORKDIR` instruction can be used multiple times in a Dockerfile. If a relative path is provided, it will be relative to the path of the previous `WORKDIR` instruction. For example:

```Dockerfile
WORKDIR /a
WORKDIR b
WORKDIR c
RUN pwd
```

The output of the final `pwd` command in this Dockerfile would be `/a/b/c`.

The `WORKDIR` instruction can resolve environment variables previously set using `ENV`. You can only use environment variables explicitly set in the Dockerfile. For example:

```Dockerfile
ENV DIRPATH=/path
WORKDIR $DIRPATH/$DIRNAME
RUN pwd
```

The output of the final `pwd` command in this Dockerfile would be `/path/$DIRNAME`

If not specified, the default working directory is `/`. In practice, if you aren't building a Dockerfile from scratch (`FROM scratch`), the `WORKDIR` may likely be set by the base image you're using.

Therefore, to avoid unintended operations in unknown directories, it's best practice to set your `WORKDIR` explicitly.

## [RUN](https://docs.docker.com/reference/dockerfile/#run)

The `RUN` instruction will execute any commands to create a new layer on top of the current image. The added layer is used in the next step in the Dockerfile. `RUN` has two forms:

```dockerfile
# Shell form:
RUN [OPTIONS] <command> ...
# Exec form:
RUN [OPTIONS] [ "<command>", ... ]
```

For more information about the differences between these two forms, see [shell or exec forms](https://docs.docker.com/reference/dockerfile/#shell-and-exec-form).

The shell form is most commonly used, and lets you break up longer instructions into multiple lines, either using newline [escapes](https://docs.docker.com/reference/dockerfile/#escape), or with [heredocs](https://docs.docker.com/reference/dockerfile/#here-documents):

```dockerfile
RUN <<EOF
apt-get update
apt-get install -y curl
EOF
```

The available `[OPTIONS]` for the `RUN` instruction are:

- [`--mount`](https://docs.docker.com/reference/dockerfile/#run---mount)
- [`--network`](https://docs.docker.com/reference/dockerfile/#run---network)
- [`--security`](https://docs.docker.com/reference/dockerfile/#run---security)

### [Cache invalidation for RUN instructions](https://docs.docker.com/reference/dockerfile/#cache-invalidation-for-run-instructions)

The cache for `RUN` instructions isn't invalidated automatically during the next build. The cache for an instruction like `RUN apt-get dist-upgrade -y` will be reused during the next build. The cache for `RUN` instructions can be invalidated by using the `--no-cache` flag, for example `docker build --no-cache`.

See the [Dockerfile Best Practices guide](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/) for more information.

The cache for `RUN` instructions can be invalidated by [`ADD`](https://docs.docker.com/reference/dockerfile/#add) and [`COPY`](https://docs.docker.com/reference/dockerfile/#copy) instructions.

## [CMD](https://docs.docker.com/reference/dockerfile/#cmd)

The `CMD` instruction sets the command to be executed when running a container from an image.

You can specify `CMD` instructions using [shell or exec forms](https://docs.docker.com/reference/dockerfile/#shell-and-exec-form):

- `CMD ["executable","param1","param2"]` (exec form)
- `CMD ["param1","param2"]` (exec form, as default parameters to `ENTRYPOINT`)
- `CMD command param1 param2` (shell form)

There can only be one `CMD` instruction in a Dockerfile. If you list more than one `CMD`, only the last one takes effect.

The purpose of a `CMD` is to provide defaults for an executing container. These defaults can include an executable, or they can omit the executable, in which case you must specify an `ENTRYPOINT` instruction as well.

If you would like your container to run the same executable every time, then you should consider using `ENTRYPOINT` in combination with `CMD`. See [`ENTRYPOINT`](https://docs.docker.com/reference/dockerfile/#entrypoint). If the user specifies arguments to `docker run` then they will override the default specified in `CMD`, but still use the default `ENTRYPOINT`.

If `CMD` is used to provide default arguments for the `ENTRYPOINT` instruction, both the `CMD` and `ENTRYPOINT` instructions should be specified in the [exec form](https://docs.docker.com/reference/dockerfile/#exec-form).

> **Note**
> 
> Don't confuse `RUN` with `CMD`. `RUN` actually runs a command and commits the result; `CMD` doesn't execute anything at build time, but specifies the intended command for the image.

## [ENTRYPOINT](https://docs.docker.com/reference/dockerfile/#entrypoint)
An `ENTRYPOINT` allows you to configure a container that will run as an executable.

`ENTRYPOINT` has two possible forms:

- The exec form, which is the preferred form:
```dockerfile
ENTRYPOINT ["executable", "param1", "param2"]
```
- The shell form:
```dockerfile
ENTRYPOINT command param1 param2
```

For more information about the different forms, see [Shell and exec form](https://docs.docker.com/reference/dockerfile/#shell-and-exec-form).

The following command starts a container from the `nginx` with its default content, listening on port 80:

```console
$ docker run -i -t --rm -p 80:80 nginx
```

Command line arguments to `docker run <image>` will be appended after all elements in an exec form `ENTRYPOINT`, and will override all elements specified using `CMD`.

This allows arguments to be passed to the entry point, i.e., `docker run <image> -d` will pass the `-d` argument to the entry point. You can override the `ENTRYPOINT` instruction using the `docker run --entrypoint` flag.

The shell form of `ENTRYPOINT` prevents any `CMD` command line arguments from being used. It also starts your `ENTRYPOINT` as a subcommand of `/bin/sh -c`, which does not pass signals. This means that the executable will not be the container's `PID 1`, and will not receive Unix signals. In this case, your executable doesn't receive a `SIGTERM` from `docker stop <container>`.

Only the last `ENTRYPOINT` instruction in the Dockerfile will have an effect.

### [Exec form ENTRYPOINT example](https://docs.docker.com/reference/dockerfile/#exec-form-entrypoint-example)

You can use the exec form of `ENTRYPOINT` to set fairly stable default commands and arguments and then use either form of `CMD` to set additional defaults that are more likely to be changed.

```dockerfile
FROM ubuntu
ENTRYPOINT ["top", "-b"]
CMD ["-c"]
```

When you run the container, you can see that `top` is the only process:

```console
$ docker run -it --rm --name test  top -H

top - 08:25:00 up  7:27,  0 users,  load average: 0.00, 0.01, 0.05
Threads:   1 total,   1 running,   0 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.1 us,  0.1 sy,  0.0 ni, 99.7 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem:   2056668 total,  1616832 used,   439836 free,    99352 buffers
KiB Swap:  1441840 total,        0 used,  1441840 free.  1324440 cached Mem

  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
    1 root      20   0   19744   2336   2080 R  0.0  0.1   0:00.04 top
```

To examine the result further, you can use `docker exec`:

```console
$ docker exec -it test ps aux

USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  2.6  0.1  19752  2352 ?        Ss+  08:24   0:00 top -b -H
root         7  0.0  0.1  15572  2164 ?        R+   08:25   0:00 ps aux
```

And you can gracefully request `top` to shut down using `docker stop test`.

The following Dockerfile shows using the `ENTRYPOINT` to run Apache in the foreground (i.e., as `PID 1`):

```dockerfile
FROM debian:stable
RUN apt-get update && apt-get install -y --force-yes apache2
EXPOSE 80 443
VOLUME ["/var/www", "/var/log/apache2", "/etc/apache2"]
ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
```

If you need to write a starter script for a single executable, you can ensure that the final executable receives the Unix signals by using `exec` and `gosu` commands:

```bash
#!/usr/bin/env bash
set -e

if [ "$1" = 'postgres' ]; then
    chown -R postgres "$PGDATA"

    if [ -z "$(ls -A "$PGDATA")" ]; then
        gosu postgres initdb
    fi

    exec gosu postgres "$@"
fi

exec "$@"
```

Lastly, if you need to do some extra cleanup (or communicate with other containers) on shutdown, or are co-ordinating more than one executable, you may need to ensure that the `ENTRYPOINT` script receives the Unix signals, passes them on, and then does some more work:

```bash
#!/bin/sh
# Note: I've written this using sh so it works in the busybox container too

# USE the trap if you need to also do manual cleanup after the service is stopped,
#     or need to start multiple services in the one container
trap "echo TRAPed signal" HUP INT QUIT TERM

# start service in background here
/usr/sbin/apachectl start

echo "[hit enter key to exit] or run 'docker stop <container>'"
read

# stop service and clean up here
echo "stopping apache"
/usr/sbin/apachectl stop

echo "exited $0"
```

If you run this image with `docker run -it --rm -p 80:80 --name test apache`, you can then examine the container's processes with `docker exec`, or `docker top`, and then ask the script to stop Apache:

```console
$ docker exec -it test ps aux

USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.1  0.0   4448   692 ?        Ss+  00:42   0:00 /bin/sh /run.sh 123 cmd cmd2
root        19  0.0  0.2  71304  4440 ?        Ss   00:42   0:00 /usr/sbin/apache2 -k start
www-data    20  0.2  0.2 360468  6004 ?        Sl   00:42   0:00 /usr/sbin/apache2 -k start
www-data    21  0.2  0.2 360468  6000 ?        Sl   00:42   0:00 /usr/sbin/apache2 -k start
root        81  0.0  0.1  15572  2140 ?        R+   00:44   0:00 ps aux

$ docker top test

PID                 USER                COMMAND
10035               root                {run.sh} /bin/sh /run.sh 123 cmd cmd2
10054               root                /usr/sbin/apache2 -k start
10055               33                  /usr/sbin/apache2 -k start
10056               33                  /usr/sbin/apache2 -k start

$ /usr/bin/time docker stop test

test
real	0m 0.27s
user	0m 0.03s
sys	0m 0.03s
```

> **Note**
> 
> You can override the `ENTRYPOINT` setting using `--entrypoint`, but this can only set the binary to exec (no `sh -c` will be used).

### [Shell form ENTRYPOINT example](https://docs.docker.com/reference/dockerfile/#shell-form-entrypoint-example)

You can specify a plain string for the `ENTRYPOINT` and it will execute in `/bin/sh -c`. This form will use shell processing to substitute shell environment variables, and will ignore any `CMD` or `docker run` command line arguments. To ensure that `docker stop` will signal any long running `ENTRYPOINT` executable correctly, you need to remember to start it with `exec`:

```dockerfile
FROM ubuntu
ENTRYPOINT exec top -b
```

When you run this image, you'll see the single `PID 1` process:

```console
$ docker run -it --rm --name test top

Mem: 1704520K used, 352148K free, 0K shrd, 0K buff, 140368121167873K cached
CPU:   5% usr   0% sys   0% nic  94% idle   0% io   0% irq   0% sirq
Load average: 0.08 0.03 0.05 2/98 6
  PID  PPID USER     STAT   VSZ %VSZ %CPU COMMAND
    1     0 root     R     3164   0%   0% top -b
```

Which exits cleanly on `docker stop`:

```console
$ /usr/bin/time docker stop test

test
real	0m 0.20s
user	0m 0.02s
sys	0m 0.04s
```

If you forget to add `exec` to the beginning of your `ENTRYPOINT`:

```dockerfile
FROM ubuntu
ENTRYPOINT top -b
CMD -- --ignored-param1
```

You can then run it (giving it a name for the next step):

```console
$ docker run -it --name test top --ignored-param2

top - 13:58:24 up 17 min,  0 users,  load average: 0.00, 0.00, 0.00
Tasks:   2 total,   1 running,   1 sleeping,   0 stopped,   0 zombie
%Cpu(s): 16.7 us, 33.3 sy,  0.0 ni, 50.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   1990.8 total,   1354.6 free,    231.4 used,    404.7 buff/cache
MiB Swap:   1024.0 total,   1024.0 free,      0.0 used.   1639.8 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
    1 root      20   0    2612    604    536 S   0.0   0.0   0:00.02 sh
    6 root      20   0    5956   3188   2768 R   0.0   0.2   0:00.00 top
```

You can see from the output of `top` that the specified `ENTRYPOINT` is not `PID 1`.

If you then run `docker stop test`, the container will not exit cleanly - the `stop` command will be forced to send a `SIGKILL` after the timeout:

```console
$ docker exec -it test ps waux

USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.4  0.0   2612   604 pts/0    Ss+  13:58   0:00 /bin/sh -c top -b --ignored-param2
root         6  0.0  0.1   5956  3188 pts/0    S+   13:58   0:00 top -b
root         7  0.0  0.1   5884  2816 pts/1    Rs+  13:58   0:00 ps waux

$ /usr/bin/time docker stop test

test
real	0m 10.19s
user	0m 0.04s
sys	0m 0.03s
```

### [Understand how CMD and ENTRYPOINT interact](https://docs.docker.com/reference/dockerfile/#understand-how-cmd-and-entrypoint-interact)

Both `CMD` and `ENTRYPOINT` instructions define what command gets executed when running a container. There are few rules that describe their co-operation.

1. Dockerfile should specify at least one of `CMD` or `ENTRYPOINT` commands.
2. `ENTRYPOINT` should be defined when using the container as an executable.
3. `CMD` should be used as a way of defining default arguments for an `ENTRYPOINT` command or for executing an ad-hoc command in a container.
4. `CMD` will be overridden when running the container with alternative arguments.

The table below shows what command is executed for different `ENTRYPOINT` / `CMD` combinations:

||No ENTRYPOINT|ENTRYPOINT exec_entry p1_entry|ENTRYPOINT ["exec_entry", "p1_entry"]|
|---|---|---|---|
|**No CMD**|error, not allowed|/bin/sh -c exec_entry p1_entry|exec_entry p1_entry|
|**CMD ["exec_cmd", "p1_cmd"]**|exec_cmd p1_cmd|/bin/sh -c exec_entry p1_entry|exec_entry p1_entry exec_cmd p1_cmd|
|**CMD exec_cmd p1_cmd**|/bin/sh -c exec_cmd p1_cmd|/bin/sh -c exec_entry p1_entry|exec_entry p1_entry /bin/sh -c exec_cmd p1_cmd|

> **Note**
> 
> If `CMD` is defined from the base image, setting `ENTRYPOINT` will reset `CMD` to an empty value. In this scenario, `CMD` must be defined in the current image to have a value.