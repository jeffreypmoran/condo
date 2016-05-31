---
layout: docs
title: nuget
group: shades
---

@{/*

nuget
    Executes the nuget package manager command.

nuget_args=''
    Required. The arguments to pass to the nuget package manager command line tool.

nuget_options='' (Environment Variable: NUGET_OPTIONS)
    Additional options to include when executuing the nuget package manager command line tool.

nuget_path='$(working_path)'
    The path in which to execute the nuget package manager command line tool.

base_path='$(CurrentDirectory)'
    The base path in which to execute the nuget package manager command line tool.

working_path='$(base_path)'
    The working path in which to execute the nuget package manager command line tool.

nuget_download_path='$(base_path)/.nuget'
    The path in which to install the nuget executable if it is not already available.

nuget_wait='true'
    A value indicating whether or not to wait for exit.

nuget_quiet='$(Build.Log.Quiet)'
    A value indicating whether or not to avoid printing output.

nuget_secure='false'
    A value indicating whether or not to avoid printing output for public builds used to hide secure information.

*/}

use namespace = 'System'
use namespace = 'System.IO'

use import = 'Condo.Build'

default base_path           = '${ Directory.GetCurrentDirectory() }'
default working_path        = '${ base_path }'

default nuget_path          = '${ working_path }'
default nuget_download_path = '${ Path.Combine(base_path, ".nuget") }'
default nuget_args          = 'restore'
default nuget_options       = '${ Build.Get("NUGET_OPTIONS") }'
default nuget_wait          = '${ true }'
default nuget_quiet         = '${ Build.Log.Quiet }'
default nuget_secure        = '${ false }' type='bool'
default nuget_retries       = '${ 1 }' type='int'

nuget-download once='nuget-download'

@{
    Build.Log.Header("nuget");

    var nuget_cmd           = Path.Combine(nuget_download_path, "nuget.exe");

    if (string.IsNullOrEmpty(nuget_args))
    {
        throw new ArgumentException("nuget: arguments must be specified when executing the nuget command line tool.", "nuget_args");
    }

    nuget_args = nuget_args.Trim();

    if (!string.IsNullOrEmpty(nuget_options))
    {
        nuget_options = nuget_options.Trim();
    }

    Build.Log.Argument("path", nuget_path);
    Build.Log.Argument("arguments", nuget_args, nuget_secure);
    Build.Log.Argument("options", nuget_options, nuget_secure);
    Build.Log.Argument("wait", nuget_wait);
    Build.Log.Argument("quiet", nuget_quiet);
    Build.Log.Argument("retries", nuget_retries);
    Build.Log.Header();
}

run run_args='"${ nuget_cmd }"' run_options = '${ nuget_args } ${ nuget_options }' run_path='${ nuget_path }' run_wait='${ nuget_wait }' run_quiet='${ nuget_quiet }' run_secure='${ nuget_secure }' run_retries='${ nuget_retries }'