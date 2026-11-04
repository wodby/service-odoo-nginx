# Nginx for Odoo on Wodby

What this service adds to the Nginx Proxy service it is based on. It is the public entry point of an Odoo app: Odoo's own
ports are private.

## Rules

Odoo's rules replace the image's `http-proxy` preset. They are the `preset` config file of this service and can be
overridden there. The preset keeps the name `http-proxy` (`NGINX_VHOST_PRESET`), which also makes the image point the
`app_server` upstream at the linked Odoo service's HTTP port.

- `/websocket` (live updates: chat, notifications) goes to the Odoo service's gevent port, 8072
  (`NGINX_ODOO_GEVENT_PORT`), with the WebSocket upgrade headers and a one-hour timeout.
- Everything else goes to Odoo's HTTP port.
- The scheme and client address that Wodby's gateway received are passed on in `X-Forwarded-Proto` and
  `X-Forwarded-For`, with the original `Host`; Odoo runs with `proxy_mode`.
- Proxy timeouts are 720 s (`NGINX_ODOO_PROXY_TIMEOUT`); Odoo stops a request after its own limit first.
- `/.healthz` returns 204.
- The image's default rules are off (`NGINX_VHOST_NO_DEFAULTS`): they would serve `/favicon.ico` and video files from
  disk and forbid framing, which Odoo's website editor needs.

## Image

The stock Nginx image; nothing is built for this service.

## Uploads and timeouts

`NGINX_CLIENT_MAX_BODY_SIZE` is `128m`. Routes allow requests and live updates up to one hour.

## Check the result

- `nginx -T` shows the rules in effect.
- In the browser's developer tools, the request to `/websocket` returns 101.
