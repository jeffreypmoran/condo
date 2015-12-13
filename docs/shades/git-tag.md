---
layout: docs
title: git-tag
group: shades
---

@{/*

git-tag
    Tags the specified commit ID with the specified tag.

git_tag_commit='HEAD'
    The commit that should be tagged.

git_tag_name='$(AssemblyInfo.InformationalVersion)'
    The tag for the specified commit.

git_tag_message='$(AssemblyInfo.InformationalVersion)'
    The message for the specified tag.

*/}

use namespace = 'System'

use import = 'Condo.Build'
use import = 'Condo.AssemblyInfo'

default git_tag_commit  = 'HEAD'
default git_tag_name    = '${ "v" + AssemblyInfo.InformationalVersion }'
default git_tag_message = '${ "v" + AssemblyInfo.InformationalVersion }'

@{
    Build.Log.Header("git-tag");

    if (string.IsNullOrEmpty(git_tag_commit))
    {
        throw new ArgumentException("git-tag: a commit id must be specified.", "git_tag_commit");
    }

    if (string.IsNullOrEmpty(git_tag_name))
    {
        throw new ArgumentException("git-tag: a tag name must be specified.", "git_tag_name");
    }

    if (string.IsNullOrEmpty(git_tag_message))
    {
        throw new ArgumentException("git-tag-message: a tag message must be specified.", "git_tag_message");
    }

    git_tag_commit = git_tag_commit.Trim();
    git_tag_name = git_tag_name.Trim();
    git_tag_message = git_tag_message.Trim();
}

git git_args='tag -a ${ git_tag_name } -m "${ git_tag_message }" ${ git_tag_commit }'