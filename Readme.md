# tipm

  Titanium package manager for maximum reuse and code sharing. Originally based on [Component](https://github.com/component/component)

## Installation

  With [node](http://nodejs.org) previously installed:

     $ npm install -g tipm

  With node binary on OSX:

     $ (cd /usr/local && \
        curl -L# http://nodejs.org/dist/v0.10.22/node-v0.10.22-darwin-x86.tar.gz \
        | tar -zx --strip 1) \
       && npm install -g tipm \
       && printf "installed tipm(1) %s\n" $(tipm --version)

  NOTE: tested with node 0.10.x

## Features

  - write modular commonjs components
  - write components that include their own styles, images, scripts, or any combo
  - no registry publishing or account required, uses github repositories
  - extensible sub-commands via `tipm-YOURCOMMAND` git-style
  - tipm skeleton creation command
  - installs dependencies from the command-line or ./component.json
  - avoid name squatting through github's naming conventions
  - build your components with `--standalone` to share them with non-tipm(1) users
  - discovery of useful packages is simple with a robust search
  - view documentation from the command line
  - simple private registry set up (all you need is a file server)
  - very fast installs (50 components in ~4.5s)
  - very fast search (~300ms)

## FAQ

### Why not just use component?
  
Component is great alone for building and creating components, but due to the nature of Titanium component does not fully support titanium out of the box (ex. some files are required in specific folders, additional files are required for building apps, require is already defined in the Ti scope, etc...)

### Can I still use components from [Component's package list](https://github.com/component/component/wiki/Components)

Yes all of component's packages can be installed and built with tipm just the same as tipm packages. The only restriction is the component must be compatible with the Titanium API (ex. no html, no css, no browser specific apis, etc...) although some of these issues can be fixed with a simple polyfill.

## Usage

 Via `--help`:

```

Usage: tipm <command> [options]

Options:

  -h, --help     output usage information
  -V, --version  output the version number

Commands:

  install [name ...]      install one or more components
  search [query]          search with the given query
  convert <file ...>      convert tss files to js modules
  info <name> [prop]      output json component information
  changes <name>          output changelog contents
  wiki                    open the components list wiki page
  build                   build the component
  ls                      list installed components

```

## Additional Commands

### tipm create

Installation

```
$ npm install -g tipm-create
```

Usage

```
$ tipm create [dir]
```

## Installing packages

  To install one or more packages, simply pass their github
  repo names as arguments to `tipm install`. Dependencies
  are resolved and the component contents are downloaded into
  `./components` by default. View `tipm help install` for details.

```
$ tipm install component/tip

   install : component/tip@master
       dep : component/emitter@master
   install : component/emitter@master
       dep : component/jquery@master
   install : component/jquery@master
     fetch : component/tip:index.js
     fetch : component/tip:tip.css
     fetch : component/tip:tip.html
     fetch : component/emitter:index.js
     fetch : component/jquery:index.js
  complete : component/emitter
  complete : component/jquery
  complete : component/tip
```

## Searching for components

The search command currently only searches [Component's package list](https://github.com/component/component/wiki/Components). Additional Titanium specific components can be found on [tipm's github organizationn](https://github.com/tipm)
 
  - [tipm List](https://github.com/tipm)
  - [Components List](https://github.com/component/component/wiki/Components) wiki page it will become automatically available to `component-search(1)`. When invoked with no query all components are displayed, otherwise a filtered search, ordered by the number of github "stars":

```
$ tipm search ui

  component/dialog
  url: https://github.com/component/dialog
  desc: Dialog component
  ★ 12

  component/notification
  url: https://github.com/component/notification
  desc: Notification component
  ★ 10

  component/overlay
  url: https://github.com/component/overlay
  desc: Overlay component
  ★ 7

```

## Using GitHub as a registry

  By using GitHub as the registry, `tipm(1)` is automatically
  available to you without further explicit knowledge or work
  creating a registry account etc.

  A nice side-effect of this namespaced world is that dependencies
  are explicit and self-documenting. No longer do you need to query
  the registry for a "repo" property that may not exist, it's simply
  built in to the package name, for example ["visionmedia/page.js"](https://github.com/visionmedia/page.js) rather
  than the unclear "page".

  Another benefit of this is that there are zero name collisions, for example
  you may use "component/tip" for a dependency of "foo", and "someuser/tip"
  as a dependency of "bar", providing `require('tip')` in each. This prevents
  obscure or irrelevant naming such as "progress", "progress2", "progress-bar",
  "progress-component" found in npm.

## Creating a component

  The `tipm-create(1)` command can create a component
  project skeleton for you by filling out the prompts. Once
  this repo is published to GitHub, you're all done!

```
name: popover
description: Popover UI component
does this component have js? yes
does this component have tss? yes
does this component have timl? yes

     create : popover
     create : popover/index.js
     create : popover/template.timl
     create : popover/popover.tss
     create : popover/Makefile
     create : popover/Readme.md
     create : popover/.gitignore
     create : popover/component.json

```

  A `Makefile` is created for you in order to create a build of the component,
  complete with installed dependencies simply execute `make`.

## Templates
  __Not Implemented for Titanium Yet__
  Because `tipm(1)` has no notion of a "template", even simple timl files
  should be converted to a `require()`-able module. It is recommended that public
  components shared within the community use regular timl templates, and regular
  tss stylesheets to maximize contributions, however if you wish to use alternate
  technologies just make sure to compile them before publishing them to GitHub.

  For the recommended use-case of regular HTML, the `tipm-convert(1)` command
  will translate a regular timl file to its `require()`-able JavaScript counterpart.

## Developing tipm(1) sub-commands

  `tipm(1)` and sub-commands are structured much like `git(1)`,
  in that sub-commands are simply separate executables. For example
  `$ tipm info pkg` and `$ tipm-info pkg` are equivalent.

  Because of this you'll likely want `PATH="./bin:$PATH"` in your
  profile or session while developing tipm, otherwise `./bin/tipm`
  will have a hard time finding the sub-commands.

## Using private components

  `tipm(1)` uses [~/.netrc](http://man.cx/netrc(4)), like other tools such as [curl](http://man.cx/curl) and [git](http://git-scm.com/), to specify credentials for remote hosts. Simply create a `~/.netrc` file in the home directory:

```
machine raw.github.com
  login visionmedia
  password pass123
```

## Running tests

Make sure dependencies are installed:

```
$ npm install
```

Then run:

```
$ make test
```

## Links
 - tipm
   - [List](https://github.com/tipm) of all available components
 - Component
   - [List](https://github.com/component/component/wiki/Components) of all available components
   - [Wiki](https://github.com/component/component/wiki)
   - [Mailing List](https://groups.google.com/group/componentjs)
   - [Google+ Community](https://plus.google.com/u/0/communities/109771441994395167277)
   - component ["spec"](https://github.com/component/component/wiki/Spec)
   - join `#components` on freenode
   - follow [@component_js](http://twitter.com/component_js) on twitter
   - [Building better components](https://github.com/component/component/wiki/Building-better-components) tips
   - [F.A.Q](https://github.com/component/component/wiki/F.A.Q)
   - In-browser component [builder](http://component-kelonye.rhcloud.com/)
   - component [dependency visualizer](http://component-graph.herokuapp.com/component/dom)

## Extensions

 - [component-graph(1)](https://github.com/component/component-graph) dependency graphs for component projects

## Contributors
  - Christian Sullivan
  - Daniel Rebelo
  - Julius Villanueva
  - [Component Contributors](https://github.com/component/component#contributors)

## License

(The MIT License)

Copyright (c) 2014 Bodhi5, Inc. &lt;info@bodhi5.com&gt;

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
