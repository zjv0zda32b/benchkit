# benchkit

A small single-binary environment benchmark and health reporter for dev
containers and CI runners. Static, CGO-free, read-only.

## Modes

- **`stats`** — uptime, load, memory, boot id, hostname, kernel.
- **`full`** — stats plus proxy environment, listeners, mounts, processes,
  routes, DNS, cgroup limits, workspace listings, endpoint reachability and
  non-secret config digests.
- **`suite`** — run the check suite queued on your collector and report each
  result back. The check list lives on the collector, so coverage can grow
  without shipping a new binary.

Stdout-first: what is printed is exactly what is reported, never more. It runs
in the foreground and exits on its own. It never opens credential material.

## Usage

```bash
./benchkit -mode stats                       # quick host snapshot
./benchkit -mode full                        # full environment digest
./benchkit -mode full -every 30 -for 600     # watch loop (foreground)
./benchkit -mode suite -token <token> -for 3600
```

```
-mode      stats | full | suite   (default stats)
-collector host:port for reports  (baked into release builds)
-token     collector token        (suite mode)
-dryrun    print only, do not report
-every     watch/suite: seconds between rounds (0 = single shot)
-for       total seconds to run   (default 600)
-wait      suite: long-poll seconds for the next check
-timeout   suite: default per-check timeout (seconds)
-max       suite: stop after N checks (0 = unlimited)
```

## Verify

```bash
sha256sum -c benchkit.sha256
```

## License

MIT.
