# Earthly configuration file

Global configuration values for earth can be stored on disk in the configuration file.

By default, earth reads the configuration file `~/.earthly/config.yml`; however, it can also be
overridden with the `--config` command flag option.

## Format

The earthly config file is a [yaml](https://yaml.org/) formatted file that looks like:

```yaml
global:
  cache_size_mb: <cache_size_mb>
git:
    global:
        url_instead_of: <url_instead_of>
    <site>:
        auth: https|ssh
        user: <username>
        password: <password>
    <site2>:
        ...
```

Example:

```yaml
global:
    cache_size_mb: 20000
git:
    global:
        url_instead_of: "git@example.com:=https://localmirror.example.com/"
    github.com:
        auth: https
        user: alice
        password: itsasecret
```

## Global configuration reference

### cache_size_mb

Specifies the total size of the BuildKit cache, in MB. The BuildKit daemon uses this setting to configure automatic garbage collection of old cache. A value of 0 causes the size to be adaptive depending on how much space is available on your system. The default is 0.

### disable_analytics

When set to true, disables collecting command line analytics; otherwise, earth will report anonymized analytics for invokation of the earth command. For more information see the [data collection page](../data-collection/data-collection.md).

### no_loop_device (obsolete)

This option is obsolete and it is ignored. Earthly no longer uses a loop device for its cache.

### cache_path (obsolete)

This option is obsolete and it is ignored. Earthly cache has moved to a Docker volume. For more information see the [page on managing cache](../guides/cache.md).

## Git configuration reference

The git configuration is split up into global config options, or site-specific options.

### global options

The global git options.

#### url_instead_of

Rewrites git URLs of a certain pattern. Similar to [`git-config url.<base>.insteadOf`](https://git-scm.com/docs/git-config#Documentation/git-config.txt-urlltbasegtinsteadOf).
Multiple values can be separated by commas. Format: `<base>=<instead-of>[,...]`.

This setting allows rewriting all git URLs of the form `https://example...` into `git@example.com:...`, or vice-versa.

For example:

* `--git-url-instead-of='git@example.com:=https://example.com/'` forces use of SSH-based URLs rather than HTTPS
* `--git-url-instead-of='https://localmirror.example.com/=git@example.com:'` forces use of HTTPS-based local mirror for ssh-based example.com repositories

NOTE: if the `auth` option is configured under a site-specific configuration, then the appropriate rewriting rule will be automatically applied.

### site-specific options

#### site

The git repository hostname. For example `github.com`, or `gitlab.com`

#### auth

Either `https` or `ssh` (default). If https is specified, user and password fields are used
to authenticate over https when pulling from git for the corresponding site.

See the [Authentication guide](../guides/auth.md) for a guide on setting up authentication.

#### user

The https username to use when auth is set to `https`. This setting is ignored when auth is `ssh`.

#### password

The https password to use when auth is set to `https`. This setting is ignored when auth is `ssh`.
