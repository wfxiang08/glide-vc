# Glide vendor cleaner

## Important Note!!!

Before using this tool be sure that cleaning and commiting vendored directories to VCS does not violate the licenses of the packages you're vendoring.

For a detailed explanation on why Glide doesn't do this see [here](http://engineeredweb.com/blog/2016/go-why-not-strip-unused-pkgs/)

## Description

This tool will help you removing from the project vendor directories all the files not needed for building your project. By default it'll keep all the files provided by packages listed in the `glide.lock` file.
If you want to keep only go (including tests) files you can provide the `--only-go` option.
If you want to remove also the go test files you can add the `--no-tests` option.

By default `glide-vc` doesn't remove:

* files that are likely to contain some type of of legal declaration or licensing information (to remove them use the `--no-legal-files` option)

* nested vendor directories.
Doing this will change compilation and runtime behavior of your project because only the top level vendored dependencies will be used for compilation. If these are at a different revision (from the one provided inside nested vendor directories) they can cause compilation problems or runtime misbehiaviours. On the other side, keeping nested vendor directories can cause compilation problems like [this one](https://github.com/mattfarina/golang-broken-vendor)

## Build

Install glide.

`glide install` or `glide install --cache` or `glide install --cache-gopath` etc... (as you are more accustomed)

`go build` or `go install`

## Run
```
glide vendor cleaner

Usage:
  glide-vc [flags]

Flags:
      --dryrun           just output what will be removed
      --no-legal-files   remove also licenses and legal files
      --no-tests         remove also go test files (requires --only-go)
      --only-go          keep only go files (including go test files)
```

You have to run `glide-vc`, or (if glide is installed) `glide vc` inside your current project directory.

To see what it'll do use the `--dryrun` option.

### Examples

Tests removal of all unneeded packages.

```
glide-vc --dryrun
```

Do it

```
glide-vc
```


Keep only go (including tests) file.

```
glide-vc --only-go
```

Keep only non test go files.

```
glide-vc --only-go --no-tests
```

Keep only non test go files and also remove all licenses and legal files.

```
glide-vc --only-go --no-tests --no-legal-files
```
