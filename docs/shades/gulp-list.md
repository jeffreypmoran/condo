---
layout: docs
title: gulp-list
group: shades
---

@{/*

gulp-list
    Gets a list of the available tasks from a gulpfile.

gulp_list_path='$(base_path)'
    The path containing a gulpfile for which to list tasks.

gulp_list_name='$(gulp_list_path)'
    The name of the key used to cache the list of tasks.

base_path='$(CurrentDirectory)'
    The base path containing a gulpfile for which to list tasks.

working_path='$(base_path)'
    The working path containing a gulpfile for which to list tasks.

gulp_list_quiet='$(Build.Log.Quiet)'
    A value indicating whether or not to avoid printing output.

*/}

use namespace = 'System'
use namespace = 'System.IO'

use import = 'Condo.Build'

default base_path           = '${ Directory.GetCurrentDirectory() }'
default working_path        = '${ base_path }'

default gulp_list_path      = '${ working_path }'
default gulp_list_quiet     = '${ Build.Log.Quiet }'
default gulp_list_name      = '${ gulp_list_path }'

gulp-download once='gulp-download'
node-download once='node-download'

@{
    var gulp_list_cmd = Build.GetPath("gulp");

    Build.Log.Header("gulp-list");
    Build.Log.Argument("path", gulp_list_path);
    Build.Log.Argument("quiet", gulp_list_quiet);
    Build.Log.Header();

    var gulp_list_args = "--tasks-simple";
    var gulp_list_tasks = default(string);
    var gulp_list_success = false;

    if (gulp_list_cmd.Global)
    {
        gulp_list_success = Build.TryExecute(gulp_list_cmd.Path, out gulp_list_tasks, gulp_list_args, gulp_list_path);
    }
    else
    {
        var gulp_list_node = Build.GetPath("node").Path;
        gulp_list_args = "\"" + gulp_list_cmd.Path + "\" " + gulp_list_args;

        gulp_list_success = Build.TryExecute(gulp_list_node, out gulp_list_tasks, gulp_list_args, gulp_list_path);
    }

    if (!gulp_list_success)
    {
        Build.Log.Warn("gulp-list: could not get a list of tasks for the specified gulpfile -- is it parsing successfully?");
    }
    else
    {
        foreach(var gulp_list_task in gulp_list_tasks.Split(new char[0], StringSplitOptions.RemoveEmptyEntries))
        {
            Build.Log.Argument("task", gulp_list_task);
        }

        Build.Log.Header();
    }

    Build.SetPath(gulp_list_name, gulp_list_tasks, gulp_list_success);
}