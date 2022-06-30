## Stop and clear Windows 10 search and history

Open an elevated command prompt (no powershell) and type the following:

To enable the search indexing service:

```
sc config "wsearch" start=delayed-auto && sc start "wsearch"
```

To disable the search indexing service:

```
sc stop "wsearch" && sc config "wsearch" start=disabled
```

You can check the service status by going to windows settings and the "Windows search indexing status".

Alternatively: Check the services with: 

```
Win + R
services.msc
Check Windows Search service
```

