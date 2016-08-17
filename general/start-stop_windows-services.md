# Start/Stop Windows services

![Assembléon PLM service](http://i.imgur.com/XgjaHYv.png)
 
1. Select: the target service (e.g. `Windows Firewall`) from the list.
2. Click: `Start` or `Stop` / `Restart` in the top left corner of the `Services (Local)` pane.

> Availability of the Windows service start/stop options is context sensitive (i.e.  `Start` is only available when the service is *not* yet running, while `Stop` / `Restart` are available when the service *is* running).

> *Gracefully* shutting down Windows also results in shutting down all processes properly, as Windows sends its 'stop signal' to all of the running services as part of its shutdown procedure.
