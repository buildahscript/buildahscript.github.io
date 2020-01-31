Buildahscript is a new container definition language built to be imperative and flexible.

It allows you to do more with buildargs, create actually re-usable modules, and in general create more flexible containers.

## Why Buildahscript?

Traditional Dockerfiles have limitations that can make it hard to express some things succintly, especially around buildargs. If you would like to do these things, you should look at using buildahscript instead:

* Have buildargs affect configuration data, like LABEL, VOLUME, EXPOSE, etc
* Avoid spinning up entire containers just to download or transform some data
* Avoid extra scripts to manipulate the context before building the image
* Avoid generating Dockerfiles from scratch
* Not lose track of buildargs

## What does it look like?

Here's a port of [the xonsh Dockerfile](https://github.com/xonsh/container/blob/9faa8ade1977b144526099758bd543ca1cc7a36e/templates/Dockerfile.xonsh)

```python
#| arg: variant
#| arg: version

dashvariant = f"-{variant}" if variant else ""
specifier = f"=={version}" if version else ""

with container(f"python:3{dashvariant}") as cnt:
    cnt.run([
        'pip', 'install', '--no-cache-dir',
        '--disable-pip-version-check',
        f'xonsh[linux]{specifier}',
    ])
    cnt.run('ln -s $(which xonsh) /usr/bin/xonsh', shell=True)
    
    cnt.command = ['/usr/bin/xonsh']
    cnt.labels.update({
        "repository": "http://github.com/xonsh/container",
        "homepage": "https://xon.sh/",
        "maintainer": "Jamie Bliss <jamie@ivyleav.es>",
    })
    
    return cnt.commit()
```

## License

Buildahscript is made available publically under the [Prosperity License](https://prosperitylicense.com/). Commercial Licenses are available via [licensezero](https://licensezero.com/offers/6aeb69c8-088b-41c2-b6ef-e7327ded1b7b)

## Projects

{% for repository in site.github.public_repositories %}
  {% if repository.name != ".github" %}
  * [{{ repository.name }}]({{ repository.html_url }}): {{ repository.description }}
  {% endif %}
{% endfor %}
