# tunnel-plugin-khulnasoft

Tunnel plugin for integration with Khulnasoft Security SaaS platform

## Usage

Tunnel's options need to be passed after `--`. Tunnel receives a target directory containing IaC files

Set Khulnasoft plugin as Tunnel's current default plugin by exporting an environment variable

```
  export TUNNEL_RUN_AS_PLUGIN=khulnasoft
```

### Scan an IaC target

```
  tunnel  <target>
```

### Scan an IaC target and tag the scan

```
  tunnel  <target>
```

### Scan an IaC target and report only specific severities

```
  tunnel --severities CRITICAL,HIGH <target>
```

## Command Line Arguments

| Argument         | Purpose                                    | Example Usage                                 |
| ---------------- | ------------------------------------------ | --------------------------------------------- |
| `--debug`        | Get more detailed output as Tunnel runs.    |                                               |
| `--severities`   | The Severities that you are interested in. | `--severities CRITICAL,HIGH,UNKNOWN`          |
| `--tags`         | Arbitrary tags to be stored with the scan. | `--tags 'BUILD_HOST=$HOSTNAME,foo=bar'`       |
| `--pipelines`    | Scan repository pipeline files.            | `--pipelines` / `PIPELINES=1 tunnel ...`       |
| `--package-json` | Scan package.json files without lock files | `--package-json` / `PACKAGE_JSON=1 tunnel ...` |

## Environment Variables

### Required

The only explicitly required environment variables are

| Variable    | Purpose                                                       |
|:------------|:--------------------------------------------------------------|
| KHULNASOFT_KEY    | Generated through CSPM UI                                     |
| KHULNASOFT_SECRET | Generated through CSPM UI                                     |


### Optional

| Variable    | Purpose                                                       |
|:------------|:--------------------------------------------------------------|
| CSPM_URL    | URL to generate Khulnasoft Platform token (default: us-east-1 CSPM) |
| KHULNASOFT_URL    | Khulnasoft platform URL (default: us-east-1 Khulnasoft platform)          |



Tunnel will attempt to resolve the following details from the available environment variables;

- repository name
- branch name
- commit id
- committing user
- build system

There are some special case env vars;

| Variable             | Purpose                                                                                |
| :------------------- | :------------------------------------------------------------------------------------- |
| OVERRIDE_REPOSITORY  | Use this environment variable to explicitly specify the repository used by Tunnel       |
| FALLBACK_REPOSITORY  | Use this environment variable as a backup if no other repository env vars can be found |
| OVERRIDE_BRANCH      | Use this environment variable to explicitly specify the branch used by Tunnel           |
| FALLBACK_BRANCH      | Use this environment variable as a backup if no other branch env vars can be found     |
| OVERRIDE_BUILDSYSTEM | Use this environment variable to explicitly specify the build system                   |
| OVERRIDE_SCMID       | Use this environment variable to explicitly specify the scm id                         |
| IGNORE_PANIC         | Use this environment variable to return exit code 0 on cli panic                       |
| OVERRIDE_REPOSITORY_URL  | Use this environment variable to explicitly specify the repository link used by Tunnel (For result's web link)       |
| OVERRIDE_REPOSITORY_SOURCE  | Use this environment variable to explicitly specify the repository source used by Tunnel       |

# Scanners

Certain scanners have additional behaviors

### Pipelines

The pipelines scanner is enabled, to skip fetch and scan pipelines add `--skip-pipelines` flag.
pipelines scanning uses [Pipeline Parser](https://github.com/argonsecurity/pipeline-parser) to parse the pipelines, and therefore, supports only the platforms that are supported by the package.

The results of the scanner are:

- parsed version of the pipeline files
- pipeline misconfigurations
