Nursa Indexer
=============
Indexes the Nursa dataset meta-data for Solr search.

Installation Guide
------------------
The prerequisite is a running SolR server, as described in the Reactome
[Search Indexer](https://github.com/reactome/search-indexer/).

The Nursa Solr core is named `nursa`. The `nursa` core is indexed
from the Reactome Nursa JSON document cache at
`/usr/local/reactome/nursa/datasets/10.1621`. Here, `10.1621` is
the `nursa.org` DOI authority. The directory contents are JSON
documents for datasets fetched from `nursa.org`. These documents are
ndexed on the `doi`, `name` and `description` fields.

Create a new Solr core named `nursa`, if necessary, using the
[Solr Dashboard](http://localhost:8983/solr) Core Admin. The Reactome
Nursa Solr schema is `conf/schema.xml` in this project's source
distribution.

Clear an existing Nursa core as follows:

    curl --user <user>:<password> \
         http://localhost:8983/solr/nursa/update?commit=true \
         -d '<delete><query>*:*</query></delete>'

Index the datasets using the Solr `post` command:

    post -u <userid>:<password> -c nursa -filetypes json \
         /usr/local/reactome/nursa/datasets/*
