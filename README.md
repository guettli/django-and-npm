# Django and NPM

The documentation of Django about their [Static file handling](https://docs.djangoproject.com/en/dev/howto/static-files/) explains only the basics.

There is `settings.STATICFILES_DIRS` to configure which directories should be included.

If you are just using your own JS/CSS files this usualy works fine.

But how to handle dependencies?

Imagine you want to use bootstrap5 or htmx.org. How to integrate these files?

Let's list the options we have:

# CDN

You can use the CDN version of the library.

For example for htmx this would be at the moment: https://unpkg.com/htmx.org@1.5.0

But since this URL is a redirect, it it better to use the target of the redirect.

The CDN could get hacked, and the library could get replaced by evil code. You should
use a tool like [srihash.org](https://www.srihash.org/) to calculate the hash.

Result:

```
<script src="https://unpkg.com/htmx.org@1.5.0/dist/htmx.min.js" 
        integrity="sha384-oGA+prIp5Vchu6we2YkI51UtVzN9Jpx2Z7PnR1I78PnZlN8LkrCT4lqqqmDkyrvI"
        crossorigin="anonymous"></script>
```

PRO:
* Lightweight and easy. No need to download and manage JS/CSS files locally.

Con: 

* You can't develop offline. 
* You can't modify the code.


# Vendoring

You can download the third party files and store them in your git repo.

To make it more obvious for developers that this code was downloaded and not
created within the current project, it is a good habit to store the files
in a directory called "vendor". Usualy it is enough to download the minified version.

PRO:

* You don't need a "integrity" hash, since you most likely serve the files from the same host.
* If you need to modify the library, you can do so easily.


Con: 

* Extra work. Especialy if the library has dependencies.
* Git Repo size increases by "binary" data.


# Django-Compressor

I think [django-compressor](https://github.com/django-compressor/django-compressor) does not what I want.

> Compresses linked and inline javascript or CSS into a single cached file.

I just want an easy way to get some JS modules which are available via npm in my project.

I don't want to compress the third-party JS or my JS.

# TODO Django-Node-Assets

https://github.com/whitespy/django-node-assets

# npm by hand

You can create a `package.json` file:

example: 

```
{
  "dependencies": {
    "bootstrap": "^5.0.2",
    "htmx.org": "^1.5.0"
  }
```

After running `node install` I have the file `node_modules/htmx.org/dist/htmx.min.js.gz`



Then add directory to STATICFILES_DIRS



