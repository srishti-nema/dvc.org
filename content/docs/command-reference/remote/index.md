# remote

A set of commands to set up and manage data remotes:
[add](/doc/command-reference/remote/add),
[default](/doc/command-reference/remote/default),
[list](/doc/command-reference/remote/list),
[modify](/doc/command-reference/remote/modify), and
[remove](/doc/command-reference/remote/remove).

## Synopsis

```usage
usage: dvc remote [-h] [-q | -v] {add,default,remove,modify,list} ...

positional arguments:
  COMMAND
    add                 Add remote.
    default             Set/unset default remote.
    remove              Remove remote.
    modify              Modify remote.
    list                List available remotes.
```

## Description

What is data remote?

The same way as GitHub provides storage hosting for Git repositories, DVC
remotes provide a central place to keep and share data and model files. With
this remote storage, you can pull models and data files created by colleagues
without spending time and resources to build or process them locally. It also
saves space on your local environment – DVC can
[fetch](/doc/command-reference/fetch) into the <abbr>cache directory</abbr> only
the data you need for a specific branch/commit.

DVC supports several types of remote storage: local file system, SSH, Amazon S3,
Google Cloud Storage, HTTP, HDFS, among others. Refer to `dvc remote add` for
more details.

> If you installed DVC via `pip` and plan to use cloud services as remote
> storage, you might need to install these optional dependencies: `[s3]`,
> `[azure]`, `[gdrive]`, `[gs]`, `[oss]`, `[ssh]`. Alternatively, use `[all]` to
> include them all. The command should look like this: `pip install "dvc[s3]"`.
> (This example installs `boto3` library along with DVC to support S3 storage.)

Using DVC with remote storage is optional. Most DVC commands use the local cache
(usually in dir `.dvc/cache`) as data storage only. This enables basic DVC usage
scenarios out of the box.

[Add](/doc/command-reference/remote/add),
[default](/doc/command-reference/remote/default),
[list](/doc/command-reference/remote/list),
[modify](/doc/command-reference/remote/modify), and
[remove](/doc/command-reference/remote/remove) commands read or modify DVC
[config files](/doc/command-reference/config). Alternatively, `dvc config` can
be used or these files could be edited manually.

For the typical process to share the <abbr>project</abbr> via remote, see
[Sharing Data And Model Files](/doc/use-cases/sharing-data-and-model-files).

## Options

- `-h`, `--help` - prints the usage/help message, and exit.

- `-q`, `--quiet` - do not write anything to standard output. Exit with 0 if no
  problems arise, otherwise 1.

- `-v`, `--verbose` - displays detailed tracing information.

## Example: Add a default local remote

<details>

### What is a "local remote" ?

While the term may seem contradictory, it doesn't have to be. The "local" part
refers to the machine where the project is stored, so it can be any directory
accessible to the same system. The "remote" part refers specifically to the
project/repository itself. Read "local, but external" storage.

</details>

We use the `-d` (`--default`) option of `dvc remote add` for this:

```dvc
$ dvc remote add -d myremote /path/to/remote
```

The <abbr>project</abbr>'s config file should now look like this:

```ini
['remote "myremote"']
url = /path/to/remote
[core]
remote = myremote
```

## Example: Customize an additional S3 remote

> 💡 Before adding an S3 remote, be sure to
> [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html).

```dvc
$ dvc remote add newremote s3://mybucket/myproject
$ dvc remote modify newremote endpointurl https://object-storage.example.com
```

The project's config file should now look something like this:

```ini
['remote "myremote"']
url = /path/to/remote
[core]
remote = myremote
['remote "newremote"']
url = s3://mybucket/myproject
endpointurl = https://object-storage.example.com
```

## Example: List all remotes in the project

```dvc
$ dvc remote list
myremote	/path/to/remote
newremote	s3://mybucket/myproject
```

## Example: Remove a remote

```dvc
$ dvc remote remove newremote
```
