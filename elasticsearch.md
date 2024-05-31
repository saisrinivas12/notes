


1. Elasticsearch is a highly scalable, open-source search and analytics engine that enables you to store, search, and analyze large volumes of data quickly and in near real-time.
2. It provided a distributed full text search engine which is exposed over HTTP web interface
3. It is a schema free json documents
it uses a concept called inverted index where it scans the word that needs to be searched in the store and gets the documents based on that instead of going to every page and scanning the word
like word -> pages to keyword centric data structure pages-> word

page centric data structure (page to words ) to keyword centric data structure ( words to page)

   important things of elastic search

1.Index: it contains one or more documents 
2. Documents : you can think it as a row in RDBMS
3. shards: split up indices into shards
4. replica : one or more copy of index 
   
