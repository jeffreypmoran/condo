---
layout: docs
title: cover-report
group: shades
---

@{ /*

cover-report
    Creates a human-readable report from code coverage results.

cover_report_target='$(target_test_path)/*-coverage.xml'
    The path containing the code coverage results that should be parsed.

cover_report_output_path='$(target_test_path)'
    The path where the report will be emitted.

cover_report_types=''
    The types of the reports to generate.

cover_report_src_path=''
    The path(s) containing source files for reference.

cover_report_history_path=''
    The path where historical coverage reports should be retained.

cover_report_assembly_filter=''
    The filter(s) used to remove specific assemblies from the report.

cover_report_class_filter=''
    The filter(s) used to remove specific classes from the report.

base_path='$(CurrentDirectory)'
    The base path in which to execute the code coverage tool.

working_path='$(base_path)'
    The working path in which to execute the code coverage tool.

target_path='$(base_path)/artifacts'
    The path where build artifacts and results should be stored.

target_test_path='$(target_path)/test'
    The path where test results should be stored.

cover_report_wait='true'
    A value indicating whether or not to wait for exit.

cover_report_quiet='$(Build.Log.Quiet)'
    A value indicating whether or not to avoid printing output.

*/ }

default base_path                     = '${ Directory.GetCurrentDirectory() }'
default working_path                  = '${ base_path }'
default target_path                   = '${ Build.Get("BUILD_BINARIESDIRECTORY", Path.Combine(base_path, "artifacts")) }'
default target_test_path              = '${ Build.Get("COMMON_TESTRESULTSDIRECTORY", Path.Combine(target_path, "test")) }'

default cover_report_target           = '${ Path.Combine(target_test_path, "coverage", "*.xml") }'
default cover_report_output_path      = '${ Path.Combine(target_test_path, "coverage") }'
default cover_report_type             = ''
default cover_report_src_path         = ''
default cover_report_history_path     = ''
default cover_report_assembly_filter  = ''
default cover_report_class_filter     = ''
default cover_report_wait             = '${ true }'
default cover_report_quiet            = '${ Build.Log.Quiet }'
default cover_report_install_path     = '${ Path.Combine(base_path, "packages") }'

var cover_report_id                   = 'ReportGenerator'
var cover_report_exe                  = 'ReportGenerator.exe'
var cover_report_install              = '${ Path.Combine(cover_report_install_path, cover_report_id, "tools", cover_report_exe) }'

nuget-install nuget_install_id='${ cover_report_id }' nuget_install_exclude_version='${ true }' if='!File.Exists(cover_report_install) && !Build.Unix' nuget_install_prerelease='${ true }' once='cover-report-install'

@{
    Build.Log.Header("cover-report");

    if (string.IsNullOrEmpty(cover_report_target))
    {
        throw new ArgumentException("cover-report: a report target must be specified.", "cover_report_target");
    }

    Build.Log.Argument("target", cover_report_target);
    Build.Log.Argument("type", cover_report_type);
    Build.Log.Argument("source path", cover_report_src_path);
    Build.Log.Argument("history path", cover_report_history_path);
    Build.Log.Argument("assembly filter", cover_report_assembly_filter);
    Build.Log.Argument("class filter", cover_report_class_filter);
    Build.Log.Argument("wait", cover_report_wait);
    Build.Log.Argument("quiet", cover_report_quiet);
    Build.Log.Header();

    if (Build.Unix)
    {
        Build.Log.Warn("cover-report: code coverage analysis is only available on the Windows platform at the present time.");
    }

    var cover_report_args = string.Format(@"""-reports:{0}"" ""-targetdir:{1}""", cover_report_target, cover_report_output_path);

    if (!string.IsNullOrEmpty(cover_report_type))
    {
        cover_report_args += string.Format(@" ""-reporttypes:{0}""", cover_report_type);
    }

    if (!string.IsNullOrEmpty(cover_report_src_path))
    {
        cover_report_args += string.Format(@" ""-sourcedirs:{0}""", cover_report_src_path);
    }

    if (!string.IsNullOrEmpty(cover_report_history_path))
    {
        cover_report_args += string.Format(@" ""-historydir:{0}""", cover_report_history_path);
    }

    if (!string.IsNullOrEmpty(cover_report_assembly_filter))
    {
        cover_report_args += string.Format(@" ""-assemblyfilters:""", cover_report_assembly_filter);
    }

    if (!string.IsNullOrEmpty(cover_report_class_filter))
    {
        cover_report_args += string.Format(@" ""-classfilters:""", cover_report_class_filter);
    }

    cover_report_args = cover_report_args.Trim();
}

exec exec_cmd='${ cover_report_install }' exec_args='${ cover_report_args }' exec_wait='${ cover_report_wait }' exec_quiet='${ cover_report_quiet }' if='!Build.Unix'