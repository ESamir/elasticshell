# ElasticShell

`elasticshell` provides a Kibana console-like interface to interact with ElasticSearch

# Features

* Cli interface, this is a one-off execution
* Interactive console-like execution
* REPL autocompletion
* Persistent history
* User/password support

# Installation

To install `elasticshell` you can either grab the latest binaries in the [release page](https://github.com/marclop/elasticshell/releases)
or install the latest and most recent commit from the source code

## Latest build

`elasticshell` will be placed in /usr/local/bin/elasticshell

```sh
git clone https://github.com/marclop/elasticshell
cd elasticshell
make install
```

# Default configuration

There are a few configuration flags you can pass to `elasticshell`:

```sh
$ elasticshell -help
Usage of elasticshell:
  -host string
    	Set the ElasticSearch host url (default "http://localhost")
  -pass string
    	Password for HTTP basic auth
  -port int
    	Set the Elasticsearch Port (default 9200)
  -timeout int
    	Set the HTTP client timeout (default 10)
  -user string
    	Username for HTTP basic auth
  -verbose
    	Verbose request/response information
```

# Usage

`elasticshell`'s usage is very intuitive, the execution is split between non-interactive and interactive, which are composed by 3 request arguments:

1. Method
2. URL
3. Body of the request

Non-interactive example:

```sh
$ elasticshell GET /
{
  "name": "GNBXbv5",
  "cluster_name": "elasticsearch",
  "cluster_uuid": "g5swow-2SHaCA6zPVvf1dQ",
  "version": {
    "number": "5.2.1",
    "build_hash": "db0d481",
    "build_date": "2017-02-09T22:05:32.386Z",
    "build_snapshot": false,
    "lucene_version": "6.4.1"
  },
  "tagline": "You Know, for Search"
}
$ elasticshell -verbose GET
Method:       GET
URL:          /
Response:     200 OK
Content-Type: application/json

{
  "name": "GNBXbv5",
  "cluster_name": "elasticsearch",
  "cluster_uuid": "g5swow-2SHaCA6zPVvf1dQ",
  "version": {
    "number": "5.2.1",
    "build_hash": "db0d481",
    "build_date": "2017-02-09T22:05:32.386Z",
    "build_snapshot": false,
    "lucene_version": "6.4.1"
  },
  "tagline": "You Know, for Search"
}
$ elasticshell GET _cat
=^.^=
/_cat/pending_tasks
/_cat/repositories
/_cat/segments
/_cat/segments/{index}
/_cat/health
/_cat/nodes
/_cat/allocation
/_cat/indices
/_cat/indices/{index}
/_cat/aliases
/_cat/aliases/{alias}
/_cat/templates
/_cat/plugins
/_cat/count
/_cat/count/{index}
/_cat/tasks
/_cat/nodeattrs
/_cat/thread_pool
/_cat/thread_pool/{thread_pools}/_cat/master
/_cat/fielddata
/_cat/fielddata/{fields}
/_cat/snapshots/{repository}
/_cat/recovery
/_cat/recovery/{index}
/_cat/shards
/_cat/shards/{index}
```

## Interactive mode

```sh
$ elasticshell -verbose
ElasticShell> GET
Method:       GET
URL:          /
Response:     200 OK
Content-Type: application/json

{
  "name": "GNBXbv5",
  "cluster_name": "elasticsearch",
  "cluster_uuid": "g5swow-2SHaCA6zPVvf1dQ",
  "version": {
    "number": "5.2.1",
    "build_hash": "db0d481",
    "build_date": "2017-02-09T22:05:32.386Z",
    "build_snapshot": false,
    "lucene_version": "6.4.1"
  },
  "tagline": "You Know, for Search"
}
ElasticShell> exit
$ elasticshell
ElasticShell> GET
Method:       GET
URL:          /

{
  "name": "GNBXbv5",
  "cluster_name": "elasticsearch",
  "cluster_uuid": "g5swow-2SHaCA6zPVvf1dQ",
  "version": {
    "number": "5.2.1",
    "build_hash": "db0d481",
    "build_date": "2017-02-09T22:05:32.386Z",
    "build_snapshot": false,
    "lucene_version": "6.4.1"
  },
  "tagline": "You Know, for Search"
}
ElasticShell> exit
```

### Change configuration

While in interactive mode you an choose to change the application's configuration at any time:

```sh
$ elasticshell
ElasticShell> get
Method:       GET
URL:          /

{
  "name": "GNBXbv5",
  "cluster_name": "elasticsearch",
  "cluster_uuid": "g5swow-2SHaCA6zPVvf1dQ",
  "version": {
    "number": "5.2.1",
    "build_hash": "db0d481",
    "build_date": "2017-02-09T22:05:32.386Z",
    "build_snapshot": false,
    "lucene_version": "6.4.1"
  },
  "tagline": "You Know, for Search"
}
ElasticShell> set port 9201
ElasticShell> get
Method:       GET
URL:          /

{
  "name": "hIzXUZY",
  "cluster_name": "elasticsearch",
  "cluster_uuid": "g5swow-2SHaCA6zPVvf1dQ",
  "version": {
    "number": "5.2.1",
    "build_hash": "db0d481",
    "build_date": "2017-02-09T22:05:32.386Z",
    "build_snapshot": false,
    "lucene_version": "6.4.1"
  },
  "tagline": "You Know, for Search"
}
ElasticShell> exit
```

## Usage with jq

Of course if you feel like combining the power of Elasticsearch with `jq` for response filtering you can do so.

```sh
$ elasticshell GET | jq '.version.number'
"5.2.1"
$ elasticshell GET | jq '.name'
"GNBXbv5"
$ elasticshell -port 9201 GET | jq '.name'
"hIzXUZY"
```

# TODOs

- [ ] CI
- [ ] Bulk API
- [ ] Better help Flag
- [ ] REPL Auto-discover indices
- [ ] Test, test and test
- [ ] Configuration files
