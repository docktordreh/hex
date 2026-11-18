# runit service

Hex is a per-user desktop process. Do not enable `packaging/runit/hex` in the
system service directory: a root-owned service cannot access the user's audio,
desktop, or D-Bus session.

Linux packages install the service template at
`/usr/share/hex/runit/hex`. On Void Linux with turnstile, link it into the
user's service directory:

```sh
mkdir -p ~/.config/service
ln -s /usr/share/hex/runit/hex ~/.config/service/hex
sv status ~/.config/service/hex
```

The service is also compatible with a regular user `runsvdir` using
`~/service`; in that case link `/usr/share/hex/runit/hex` there instead.

The run script reads Hex's saved `session.env` and, when present, imports
turnstile's `TURNSTILE_ENV_DIR`. This keeps Wayland, X11, and D-Bus session
variables aligned with the graphical login that started Hex.
