---
title: componentversions
name: add componentversions
url: /docs/cli/add/componentversions/
date: 2023-03-10T15:23:12Z
draft: false
images: []
menu:
  docs:
    parent: add
toc: true
isCommand: true
---
### Usage

```
ocm add componentversions [<options>] [--version <version>] [<ctf archive>] {<components.yaml>}
```

### Options

```
      --addenv                 access environment for templating
  -c, --create                 (re)create archive
      --dry-run                evaluate and print component specifications
  -F, --file string            target file/directory (default "transport-archive")
  -f, --force                  remove existing content
  -h, --help                   help for componentversions
  -O, --output string          output file for dry-run
  -S, --scheme string          schema version (default "v2")
  -s, --settings stringArray   settings file with variable settings (yaml)
      --templater string       templater to use (go, none, spiff, subst) (default "subst")
  -t, --type string            archive format (directory, tar, tgz) (default "directory")
  -v, --version string         default version for components
```

### Description


Add component versions specified by a description file to a Common Transport
Archive. This might be either a directory prepared to host component version
content or a tar/tgz file (see option --type).

If option <code>--create</code> is given, the archive is created first. An
additional option <code>--force</code> will recreate an empty archive if it already exists.

The source, resource and reference list can be composed according the commands
[ocm add sources](/docs/cli/add/sources), [ocm add resources](/docs/cli/add/resources), [ocm add references](/docs/cli/add/references), respectively.

The description file might contain:
- a single component as shown in the example
- a list of components under the key <code>components</code>
- a list of yaml documents with a single component or component list

The <code>--type</code> option accepts a file format for the
target archive to use. The following formats are supported:
- directory
- tar
- tgz

The default format is <code>directory</code>.

If the option <code>--scheme</code> is given, the specified component descriptor format is used/generated.
The following schema versions are supported:

  - <code>ocm.software/v3alpha1</code>: 
  - <code>v2</code> (default): 

All yaml/json defined resources can be templated.
Variables are specified as regular arguments following the syntax <code>&lt;name>=&lt;value></code>.
Additionally settings can be specified by a yaml file using the <code>--settings <file></code>
option. With the option <code>--addenv</code> environment variables are added to the binding.
Values are overwritten in the order environment, settings file, command line settings. 

Note: Variable names are case-sensitive.

Example:
<pre>
&lt;command> &lt;options> -- MY_VAL=test &lt;args>
</pre>

There are several templaters that can be selected by the <code>--templater</code> option:
- <code>go</code> go templating supports complex values.

  <pre>
    key:
      subkey: "abc {{.MY_VAL}}"
  </pre>
  
- <code>none</code> do not do any substitution.

- <code>spiff</code> [spiff templating](https://github.com/mandelsoft/spiff).

  It supports complex values. the settings are accessible using the binding <code>values</code>.
  <pre>
    key:
      subkey: "abc (( values.MY_VAL ))"
  </pre>
  
- <code>subst</code> simple value substitution with the <code>drone/envsubst</code> templater.

  It supports string values, only. Complex settings will be json encoded.
  <pre>
    key:
      subkey: "abc ${MY_VAL}"
  </pre>
  


### Examples

```

<pre>
$ ocm add componentversions --file ctf --version 1.0 components.yaml
</pre>

and a file <code>components.yaml</code>:

<pre>
name: ocm.software/demo/test
version: 1.0.0
provider:
  name: ocm.software
  labels:
    - name: city
      value: Karlsruhe
labels:
  - name: purpose
    value: test

resources:
  - name: text
    type: PlainText
    input:
      type: file
      path: testdata
  - name: data
    type: PlainText
    input:
      type: binary
      data: IXN0cmluZ2RhdGE=

</pre>

The resource <code>text</code> is taken from a file <code>testdata</code> located
next to the description file.

```

### See Also

* [ocm add](/docs/cli/add)	 &mdash; Add resources or sources to a component archive
