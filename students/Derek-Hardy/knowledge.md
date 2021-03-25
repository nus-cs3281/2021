### Full-text search
* Definition
* Application in modern search engine
* Solr Java client "SolrJ"
* Advantages of using search engine for read-only operation

**Resource:**
- [Full-text search](https://en.wikipedia.org/wiki/Full-text_search)
- [Inverted index](https://nlp.stanford.edu/IR-book/html/htmledition/a-first-take-at-building-an-inverted-index-1.html)
- [Solr vs ES](https://solr-vs-elasticsearch.com/)
- [SolrJ documentation](https://solr.apache.org/guide/8_1/using-solrj.html)
- [Search engine vs Relational database](https://dzone.com/articles/search-engine-solr-vs-relational-database)

### Cloud Storage
* Understand various aspects of the Google Cloud Storage such as:
  * Blob storage
  * KeyFactory
  * ancestor filter and non-ancestor query
  * datastore query data consistency
  * search API concepts and practices (document, index, query, cursor...)
  * GCP data buckets import & export 

**Resource:**
- [Blob reference](https://googleapis.dev/java/google-cloud-storage/latest/index.html)
- [KeyFactory reference](https://cloud.google.com/appengine/docs/standard/java/javadoc/com/google/appengine/api/datastore/KeyFactory)
- [Query reference](https://cloud.google.com/appengine/docs/standard/java/datastore/queries)
- [Data consistency](https://cloud.google.com/appengine/docs/standard/java/datastore/data-consistency#data_consistency_levels)
- [Search API documentation](https://cloud.google.com/appengine/docs/standard/java/search)

### Datastore emulator
* understand the usage of local datastore emulator for testing
* usage of LocalDatastoreHelper in Cloud Storage unit test setup
* data import/export between the emulator and staging server

**Resource:**
- [Datastore emulator doc](https://cloud.google.com/datastore/docs/tools/datastore-emulator)
- [LocalDatastoreHelper API](https://googleapis.dev/java/google-cloud-clients/0.89.0-alpha/com/google/cloud/datastore/testing/LocalDatastoreHelper.html)
- [Entity import/export](https://cloud.google.com/datastore/docs/export-import-entities)
- [Emulator import/export](https://cloud.google.com/datastore/docs/tools/emulator-export-import)

### Cloud Logging
* Understand the internal working of Google Cloud Logging.

**Resource:** 
- [Logging v1 Doc](https://cloud.google.com/logging/docs/reference/v2/rpc/google.appengine.logging.v1)
- [Logging v2 Doc](https://cloud.google.com/logging/docs/reference/api-overview)

### Objectify v5, v6
* Common operations on entity schema, queries and migration
* Interactions with Google Cloud Datastore in different versions (v5, v6)
* Objectify unit test environment setup
* Cloud storage key update
* Learned incompatible issues between versions and workarounds

**Resource:** 
- [Objectify wiki](https://github.com/objectify/objectify/wiki)
- [Stackoverflow](https://stackoverflow.com/questions/32628124/how-to-use-objectifyservice-in-junit-testing)
- [Update PR](https://github.com/Derek-Hardy/teammates/pull/9)
- [Key incompatibility between v5 and v6](https://github.com/objectify/objectify/issues/410)

### Adaptive and responsive design
* definition, use cases
* tradeoff and how to make choice

**Resource:** 
- [Responsive Design vs. Adaptive Design: What’s the Best Choice for Designers?](https://www.uxpin.com/studio/blog/responsive-vs-adaptive-design-whats-best-choice-designers/)

### GitHub Action
* how to configure CI workflow to the existing project

**Resource:** 
- [GitHub Action documentation](https://docs.github.com/en/actions/quickstart)

### Angular test framework
* available components and best practices to test Angular components

**Resource:** 
- [the official Angular documentation](https://angular.io/guide/testing)

### CSS
* different use and functionalities of flexbox layout

**Resource:** 
- [CSS-Tricks](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)

### Git
**Problem:** permission denied when pushing to remote repo

**Solution:**
```
git remote rm origin
git remote add origin git@github.com…git (SSH key)
cat ~/.ssh/id_rsa.pub (view key)
https://github.com/settings/ssh (add key into GitHub account)  
```

**Resource:** 
- [Stackoverflow](https://stackoverflow.com/questions/13674647/cannot-push-to-git-repository-permission-denied)

**Problem:** delete undesirable commit history

**Solution:**
```
Git reset --hard <SHA>
```

**Resource:** Stackoverflow

### GitHub

**Problem:** "refusing to allow an OAuth App to create or update workflow" on git push

**Solution:** we need to enable `workflow` scope on our GitHub repository to allow any modifications on GitHub Action in the project.
Otherwise, the request will likely be declined.

**Resource:** 
- [Stackoverflow](https://stackoverflow.com/questions/64059610/how-to-resolve-refusing-to-allow-an-oauth-app-to-create-or-update-workflow-on)

### Java/OS
How to switch between Java versions on the PC for working with different project environment?

**Resource:**
- [Medium article](https://medium.com/@devkosal/switching-java-jdk-versions-on-macos-80bc868e686a)
