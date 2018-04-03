libvmod-queryfilter
===================

Travis
-------

[![Build Status](https://travis-ci.org/gsdevme/libvmod-queryfilter.svg?branch=master)](https://travis-ci.org/gsdevme/libvmod-queryfilter)


Overview
--------
This is a simple VMOD for [Varnish Cache](https://www.varnish-cache.org/) which
provides query string filtering. It accepts a whitelist of parameter names and
returns the request URL with only the provided parameters and their values.

### Sort and Filter Functionality
The order of query string parameters is typically not considered, so an
effective caching strategy involves sorting the querystring parameters so that
identical name/value pairs that are in a different order for a given URL do not
create multiple cache records.

Libvmod-queryfilter does not require separate sort and filter calls because it
works by traversing the provided parameters and filtering as it goes.
The output contains parameter names in the same order that they were provided
to the VMOD, so by passing in parameter names in a consistent manner (e.g.,
alphabetical order), a resulting filtered request URL will be unique for its
combination of parameter names and values.

LICENSE
-------
Copyright © 2014-2018 The New York Times Company.
Licensed under the Apache 2.0 License. See [LICENSE](./LICENSE) for more information.

See the [NOTICE](./NOTICE) file for a list of contributors.

(To list individual developers, try `git shortlog -s` or [this page](https://github.com/NYTimes/libvmod-queryfilter/graphs/contributors)).

Usage
-----
Rewrite the request URL so that the query string only contains parameter
name/value pairs for the "id" and "q" parameters:

    import queryfilter;
    set req.url = queryfilter.filterparams(req.url, "id,q");

Building
--------
Libvmod-queryfilter attempts to be 100% C99 conformant. You should, therefore,
be able to build it without issue on most major compilers.

### Setup
Before anything else is done, your source directory has to be initialized:

```Shell
./autogen.sh
```

### Configuration
This vmod must be compiled against a pre-built Varnish Cache 3.x/4.x/5.x source
tree. The path to the Varnish Cache source tree is specified via the
*VARNISHSRC* variable at configure time, e.g.:

```Shell
./configure VARNISHSRC=path/to/varnish-M.m.p && make && make check
```

Additional configuration variables and options can be found by invoking
`configure --help`.

#### Query Arrays
By default, libvmod-queryfilter assumes query parameters are individual
name/value pairs (e.g. `a=1&b=2...`). Optional support for arrays in query
parameters (e.g. `a[]=1&a[]=2...` - see [this Stackoverflow Question](http://stackoverflow.com/questions/6243051/how-to-pass-an-array-within-a-query-string) or [Issue #2](https://github.com/NYTimes/libvmod-queryfilter/issues/2))
can be enabled by passing the `--enable-query-arrays` at configure time. With
this option enabled, array parameters will be preserved - in order - in the
output URI.

**NOTE**: _At present, query arrays are a compile-time option. There are plans
to make this behavior run-time configurable in the next major-level release._

### Check Targets
Libvmod-queryfilter provides a set of simple unit tests driven by
**varnishtest**. They can be executed as part of the build process by
invoking `make check`.


