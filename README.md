# condo

> A build system for \<any\> project.

## Vitals

Info          | Badges
--------------|--------------
Version       | [![Version][release-v-image]][release-url]
License       | [![License][license-image]][license]
Build Status  | [![Circle CI Build Status][circleci-image]][circleci-url] [![AppVeyor Build Status][appveyor-image]][appveyor-url]
Chat          | [![Join Chat][gitter-image]][gitter-url]
Issues        | [![Issues][issues-image]][issues-url]

## Getting Started

### What is Condo?

Condo is a cross-platform command line interface (CLI) build system for projects using NodeJS, CoreCLR, .NET Framework,
or... well, anything. It is capable of automatically detecting and executing all of the steps necessary to make <any>
project function correctly. Some of the most-used features of the build system include:

* Automatic semantic versioning
* Restoring package manager dependencies (NuGet, NPM, Bower)
* Executing default task runner commands
* Compiling projects and test projects (package.json and msbuild)
* Executing unit tests (xunit, mocha, jasmine, karma, protractor)
* Packing NuGet packages
* Pushing (Publishing) NuGet packages

### Using Condo
We are currently developing `condo-cli` to make bootstrapping your projects to use condo a snap.

But it's not ready yet...
So let's do it the old fashioned way:

1. Get the necessary files:

    Copy the four files in the `templates` folder and add them to the root folder of your project.
    ```
    condo.build
    condo.cmd
    condo.ps1
    condo.sh
    ```

2. Edit the `condo.build` config file:

    Configure some stuff, only if ya want

3. Run the build:

	OS X / Linux:

	```bash
	./condo.sh
	```

	Windows (CLI):

	```cmd
	condo
	```

	Windows (PoSH):
	```posh
	./condo.ps1
	```

### Helpful hints

If you are using any protected nuget feeds, run the following command to add your credentials:

    OS X / Linux:
	```./condo.sh --username USERNAME --password PASSWORD -- /t:Bootstrap```

    Windows:
    ```condo -SecureFeed```

You can also get the latest and greatest version of condo by running this command:

    OS X / Linux:
	```./condo.sh --reset```

    Windows:
    ```condo -Reset```

## Documentation

For more information, please refer to the [official documentation][docs-url].

## Copright and License

&copy; automotiveMastermind and contributors. Distributed under the MIT license. See [LICENSE][] and [CREDITS][credits] for details.

[license-image]: https://img.shields.io/badge/license-MIT-blue.svg
[license]: LICENSE
[credits]: CREDITS.md

[release-url]: //github.com/automotivemastermind/condo/releases/latest
[release-v-image]:https://img.shields.io/github/release/automotivemastermind/condo.svg?style=flat-square&label=github

[travis-url]: //travis-ci.org/automotivemastermind/condo
[travis-image]: https://img.shields.io/travis/automotivemastermind/condo.svg?label=travis

[appveyor-url]: //ci.appveyor.com/project/automotivemastermind/condo
[appveyor-image]: https://img.shields.io/appveyor/ci/automotivemastermind/condo.svg?label=appveyor

[yo-url]: //www.npmjs.com/package/generator-condo

[docs-url]: //automotivemastermind.github.io/condo

[gitter-url]: //gitter.im/automotivemastermind/condo
[gitter-image]:https://img.shields.io/badge/⊪%20gitter-join%20chat%20→-1dce73.svg

[issues-url]: //waffle.io/automotivemastermind/condo
[issues-image]: https://badge.waffle.io/automotivemastermind/condo.svg?columns=backlog,ready,in%20progress,needs%20review

[circleci-url]: //circleci.com/gh/automotiveMastermind/condo
[circleci-image]: https://img.shields.io/circleci/project/github/automotiveMastermind/condo.svg?label=circle-ci
