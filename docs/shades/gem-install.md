---
layout: docs
title: gem-install
group: shades
---

@{/*

gem-install
    Installs a package using the gem command line utility for ruby.

gem_install_name=''
    Required. The name of the gem to install using the gem command line utility.

gem_install_options='' (Environment Variable: GEM_INSTALL_OPTIONS)
    Additional options to use when executing the gem command.

gem_install_path='$(working_path)'
    The path in which to execute the gem install command.

base_path='$(CurrentDirectory)'
    The base path in which to execute the gem install command.

working_path='$(base_path)'
    The working path in which to execute the gem install command.

*/}

use namespace = 'System'
use namespace = 'System.IO'

use import = 'Condo.Build'

default base_path               = '${ Directory.GetCurrentDirectory() }'
default working_path            = '${ base_path }'

default gem_install_name        = ''
default gem_install_options     = '${ Build.Get("GEM_INSTALL_OPTIONS") }'
default gem_install_path        = '${ working_path }'

@{
    Build.Log.Header("gem-install");

    if (string.IsNullOrEmpty(gem_install_name))
    {
        throw new ArgumentException("gem-install: the name of the gem to install is required.", "gem_install_name");
    }

    // trim the arguments
    gem_install_name = gem_install_name.Trim();
    gem_install_options = gem_install_options.Trim();

    Build.Log.Argument("gem name", gem_install_name);
    Build.Log.Argument("options", gem_install_options);
    Build.Log.Argument("path", gem_install_path);
    Build.Log.Header();
}

gem gem_args='install ${ gem_install_name }' gem_options='${ gem_install_options }' gem_path='${ gem_install_path }'