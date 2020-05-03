# <img src="images/icon.png" width="20px" height="20px"> Shaarli

The personal, minimalist, super-fast, database free, bookmarking service.

Do you want to share the links you discover?
Shaarli is a minimalist bookmark manager and link sharing service that you can install on your own server.
It is designed to be personal (single-user), fast and handy.

[![](https://i.imgur.com/8wEBRSG.png)](https://i.imgur.com/WWPfSj0.png) [![](https://i.imgur.com/rrsjWYy.png)](https://i.imgur.com/TZzGHMs.png) [![](https://i.imgur.com/uICDOle.png)](https://i.imgur.com/27wYsbC.png) [![](https://i.imgur.com/KNvFGVB.png)](https://i.imgur.com/0f5faqw.png) [![](https://i.imgur.com/tVvD3gH.png)](https://i.imgur.com/zGF4d6L.jpg) [![](https://i.imgur.com/8iRzHfe.png)](https://i.imgur.com/sfJJ6NT.png) [![](https://i.imgur.com/GjZGvIh.png)](https://i.imgur.com/QsedIuJ.png) [![](https://i.imgur.com/TFZ9PEq.png)](https://i.imgur.com/KdtF8Ll.png) [![](https://i.imgur.com/IvlqXXK.png)](https://i.imgur.com/boaaibC.png) [![](https://i.imgur.com/nlETouG.png)](https://i.imgur.com/Ib9O7n3.png)

Visit the pages in the sidebar to find information on how to setup, use, configure, tweak and troubleshoot Shaarli.


* [GitHub project page](https://github.com/shaarli/Shaarli)
* [Online documentation](https://shaarli.readthedocs.io/)
* [Latest releases](https://github.com/shaarli/Shaarli/releases)
* [Changelog](https://github.com/shaarli/Shaarli/blob/master/CHANGELOG.md)


## Demo

You can use this [public demo instance of Shaarli](https://demo.shaarli.org).
It runs the latest development version of Shaarli and is updated/reset daily.

Login: `demo`; Password: `demo`


## Features

Shaarli can be used:

- to share, comment and save interesting links
- to bookmark useful/frequent links and share them between computers
- as a minimal blog/microblog/writing platform
- as a read-it-later/todo list
- as a notepad to draft and save articles/posts/ideas
- as a knowledge base to keep notes, documentation and code snippets
- as a shared clipboard/notepad/pastebin between computers
- as playlist manager for online media
- to feed other blogs, aggregators, social networks...

### Edit, view and search your links

- Editable URL, title, description, tags, private/public status for all your [Shaares](Usage.md)
- [Tags](Usage.md#tags) to organize your Shaares
- [Search](Usage.md#search) in all fields
- Unique [permalinks](Usage.md#permalinks) for easy reference
- Paginated Shaares list view (with image and video thumbnails)
- [Tag cloud/list](Usage#tag-cloud) views
- [Picture wall](Usage#picture-wall)/thumbnails view (with lazy loading)
- [ATOM and RSS feeds](Usage.md#rss-feeds) (can also be filtered using tags or text search)
- [Daily](Usage.md#daily): newspaper-like daily digest (and daily RSS feed)
- URL cleanup: automatic removal of `?utm_source=...`, `fb=...` tracking parameters
- Extensible through [plugins](Plugins.md)
- Easily extensible by any client using the [REST API](REST-API.md) exposed by Shaarli


### Easy setup

- Dead-simple [installation](Installation.md): drop the files on your server, open the page
- Shaares are stored in a file (no database required, easy [backup](Backup-and-restore.md))
- [Import and export](Usage.md#import-export) Shaares as Netscape bookmarks compatible with most Web browsers
- [Configurable](Shaarli-configuration) dialog/file
- Extensible through third-party [plugins and themes](Community-&-related-software.md)


### Accessibility

- Bookmarklet and other tools to share links in one click
- Responsive/support for mobile browsers
- Degrades gracefully with Javascript disabled


### Fast

- Small datastore file, write-once/read-many, served most of the time from OS disk caches (no disk I/O)
- Stays fast with even tens of thousands shaares!


### Self-hosted

- Shaarli is an alternative to commercial services such as StumbleUpon, Delicio.us, Diigo...
- The data is yours, [import and export](Usage#import-export) it to/from a variety of formats
- Shaarli does not send any telemetry/metrics/private information to developers
- Shaarli is Free and Open-Source software, inspect and change how the program works in the [source code](https://github.com/shaarli/Shaarli)
- Built-in [Security](Development.md#security) features to help you protect your Shaarli instance


## About

This [community fork](https://github.com/shaarli/Shaarli) of the original [Shaarli](https://github.com/sebsauvage/Shaarli/) project by [Sébastien Sauvage](http://sebsauvage.net/) (now [unmaintained](https://github.com/sebsauvage/Shaarli/issues/191)) has carried on the work to provide [many patches](https://github.com/shaarli/Shaarli/compare/sebsauvage:master...master) for [bug fixes and enhancements](https://github.com/shaarli/Shaarli/issues?q=is%3Aclosed+) in this repository, and will keep maintaining the project for the foreseeable future, while keeping Shaarli simple and efficient.

The original Shaarli instance is still available [here](https://sebsauvage.net/links/) (+25000 shaares!)


### Contributing and getting help

Feedback is very appreciated!

- If you have any questions or ideas, please join the [chat](https://gitter.im/shaarli/Shaarli) (also reachable via [IRC](https://irc.gitter.im/)), post them in our [general discussion](https://github.com/shaarli/Shaarli/issues/308) or read the current [issues](https://github.com/shaarli/Shaarli/issues).
- Have a look at the open [issues](https://github.com/shaarli/Shaarli/issues) and [pull requests](https://github.com/shaarli/Shaarli/pulls)
- If you would like a feature added to Shaarli, check the issues labeled [`feature`](https://github.com/shaarli/Shaarli/labels/feature), [`enhancement`](https://github.com/shaarli/Shaarli/labels/enhancement), and [`plugin`](https://github.com/shaarli/Shaarli/labels/plugin).
- If you've found a bug, please create a [new issue](https://github.com/shaarli/Shaarli/issues/new).
- Feel free to propose solutions to existing problems, help us improve the documentation and translations, and submit pull requests :-)


### License

Shaarli is [Free Software](http://en.wikipedia.org/wiki/Free_software). See
[COPYING](https://github.com/shaarli/Shaarli/blob/master/COPYING) for a detail
of the contributors and licenses for each individual component. A list of
contributors is available
[here](https://github.com/shaarli/Shaarli/blob/master/AUTHORS).

