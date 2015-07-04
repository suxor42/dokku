# Process/Container management

Dokku supports rudimentary process (really container) management via the `ps` plugin.

```
ps <app>                                        List processes running in app container(s)
ps:rebuildall                                   Rebuild all apps
ps:rebuild <app>                                Rebuild an app
ps:restartall                                   Restart all deployed app containers
ps:restart <app>                                Restart app container(s)
ps:scale <app> <proc>=<count> [<proc>=<count>]  Set how many processes of a given process to run
ps:start <app>                                  Start app container(s)
ps:stop <app>                                   Stop app container(s)
```

*NOTE*: As of v0.3.14, `dokku deploy:all` in now deprecated by `ps:restartall` and will be removed in a future version.


## Scaling

Dokku allows you to run multiple process types at different container counts. For example, if you had an app that contained 1 web app listener and 1 background job processor, dokku can, spin up 1 container for each process type defined in the Procfile. By default we will only start the web process. However, if you wanted 2 job processors running simultaneously, you can modify this behavior in a few ways.

## Include a DOKKU_SCALE file in your repo

Dokku expects this file to contain one line for every process defined in your Procfile. Example:
```
web=1
worker=2
```


## Use the ps:scale command. Example:
```
dokku ps:scale app_name web=1 worker=2
```
*NOTE*: Dokku will always use the DOKKU_SCALE file that ships with the repo to override any local settings.


## The web proctype

Like Heroku, we handle the `web` proctype differently from others. The `web` proctype is the only proctype that will invoke custom checks as defined by a CHECKS file. It is also the only proctype that will be launched in a container that is either proxied via nginx or bound to an external port.
