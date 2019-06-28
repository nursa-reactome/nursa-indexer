Nursa Indexer
=============
Indexes the Nursa dataset meta-data for Solr search.

Installation Guide
------------------
The prerequisite is a running SolR server, as described in the Reactome
[Search Indexer](https://github.com/reactome/search-indexer/).

The Nursa Solr core is named `nursa`. The `nursa` core is indexed
from the Reactome Nursa JSON document cache at
`/usr/local/reactomes/Reactome/production/nursa/datasets/10.1621`.
Here, `10.1621` is the `nursa.org` DOI authority. The directory contents
are JSON documents for datasets fetched from `nursa.org`. These documents
are indexed on the `doi`, `name` and `description` fields.

The `/usr/local/reactomes/Reactome/production/nursa/datasets`
cache can be refreshed with new Nursa dataset content by running this
project's `populate.sh` program. Run `populate.sh --help` for the command
syntax.

Create a new Solr core named `nursa`, if necessary, using the
[Solr Dashboard](http://localhost:8983/solr) Core Admin. The Reactome
Nursa Solr schema is `conf/schema.xml` in this project's source
distribution.

The index can be created manually from an existing
`/usr/local/reactomes/Reactome/production/nursa/datasets` cache as follows:

1. Clear the existing Nursa core:

       curl --user <userid>:<password> \
            http://localhost:8983/solr/nursa/update?commit=true \
            -d '<delete><query>*:*</query></delete>'

   where <userid>:<password> is the Solr authorization.

2. Index the datasets using the Solr `post` command:

       /path/to/solr/post -u <userid>:<password> -c nursa -filetypes json \
            /usr/local/reactomes/Reactome/production/nursa/datasets/*/*/*.json

   where `/path/to/solr` is the Solr installation directory.
