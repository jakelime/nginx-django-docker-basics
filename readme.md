# Docker setup for nginx + django

This is a basics tutorial on how to set up
nginx reverse proxy to django using docker-compose
orchestrations.

The setup can be very confusing because of nginx's
special niches in dealing with trialing slashes `/`,
and considerations with mixing in with prefixes
`FORCE_SCRIPT_NAME` from django.

In this example, we will use nginx as the front-facing
static files server, media files server, reverse proxying
to Django as a backend server.

We will also reverse-proxy to multiple Django server.
I have spent time to troubleshoot to fix this because
the internal routing and caching on nginx side plus
chrome's internal cache can create a lot of problems when
learning to deal with how to route.

## Quickstart

1. Create the `.env` files using `.env.example`.
1. Simply use `docker compose up` to fire up the whole stack.

## Validation

To confirm things are all working, you should be able to

1. View landing page `http://localhost/`
1. Click to redirect to next Django app, `http://localhost/app1/`
1. When you access `http://localhost/app1/admin` or `http://localhost/app1/admin/`,
   chrome should route you correctly to the correct admin page.
1. Test it with `http://localhost/app2/admin` and `http://localhost/app1/admin`,
   chrome or nginx should be directing/redirecting the routes correctly (not
   from `/app2/admin` to `/admin`)

## Helper commands

```bash
docker compose down nginx && docker compose up nginx --build -d
docker compose down app0 && docker compose up app0 --build -d
docker compose down app1 && docker compose up app1 --build -d
docker compose down app2 && docker compose up app2 --build -d

```
