### Before we begin

This template is very opinionated and makes sense only in a very specific case.

Disadvantages of this template:
- you're ok with some downtime, ~1 minute for every release. I think it can be mitigated if you handle staticfiles differently, but I'd rather prefer to KISS. Otherwise, use django-storages with fly's release command to collect staticfiles. Another option is to use `strategy` flag in the `flyctl deploy` command, but it might require an additional app slot.
- **your DB is read only on prod** ( see [`tomwojcik/django-fly-postgres-template`](https://github.com/tomwojcik/django-fly-postgres-template) for the alternative )


Advantages of this template:
- it's free to run on fly and requires only a single "app" slot
- the DB is read only on prod, meaning that you deploy sqlite with the project, it's in your VCS, which means you don't need to worry about a lot of things
- the entire site is cached indefinitely, both app-side (`django.middleware.cache.{Update,FetchFrom}CacheMiddleware`, remove if needed) and client side (`Whitenoise`, staticfiles indefinitely, Django views for 60 seconds)
- all assets are compressed (`Whitenoise` with `brotli`)
- all statics are integrated out of the box, including Bootstrap 4, jQuery and HTMX
- once you need read and write on prod DB, it's relatively easy to migrate


### How to use

Last I checked, Jinja2 didn't work well with Python3.11. Use any lower Python version.

```shell
$ pip install -U cookiecutter jinja2
$ cookiecutter gh:tomwojcik/django-fly-sqlite-template
```

Give it a proper name

```shell
project_name [Fly App]: My Blog
project_slug [my_blog]:  # just hit enter if it looks fine, the slug is generated automatically
```

Post generation hook already creates a git repository and adds everything that's been created. Just add remote and push

```shell
$ git remote add origin git@github.com:<username>/<reponame>.git
$ git push -u -f origin main
```

For the next steps follow the `README.md` from the generated app.
