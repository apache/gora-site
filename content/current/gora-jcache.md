Title: Gora JCache Module

## Overview
This is the main documentation for the gora-jcache module. gora-jcache
module enables [Hazelcast JCache](https://hazelcast.com/use-cases/caching/jcache-provider) caching support for Gora.
This implementation is based on Hazelcast JCache provider. This dataStore can act as wrapped caching layer for any other 
persistent Apache Gora persistent dataStore.

[TOC]

## gora.properties

* <code>gora.cache.datastore.default</code> <code>( Mandatory )</code> - Caching dataStore to be used with persistent dataStore. If JCache dataStore is used as caching store
assigned value should be <b>org.apache.gora.jcache.store.JCacheStore</b>

* <code>gora.datastore.default</code> <code>( Mandatory )</code> - Persistent back-end dataStore to be used with JCache caching dataStore.

* <code>gora.datastore.jcache.provider</code> <code>( Mandatory )</code> - Two possible values, whether to start JCache dataStore in Server mode or Client mode,
  * Server Mode
     <b>com.hazelcast.cache.impl.HazelcastServerCachingProvider</b>
  * Client Mode
     <b>com.hazelcast.client.cache.impl.HazelcastClientCachingProvider</b>

* <code>gora.datastore.jcache.hazelcast.config</code> - If JCache datastore is started in,
  * Server Mode
     This property to should point to Hazelcast Cluster member network configuration file related to
     forming Hazelcast cluster using members. Please see <a href="http://docs.hazelcast.org/docs/3.5/manual/html/networkconfiguration.html">Network configuration</a>.
  * Client Mode
     This property to should point s to Hazelcast client configuration file related to connecting related to already formed Hazelcast cluster.
     Please see <a href="http://docs.hazelcast.org/docs/3.5/manual/html/javaclientconfiguration.html#java-client-configuration">Client configuration</a>  <code>( Mandatory )</code>

* <code>jcache.auto.create.cache</code> - Whether force creating the cache at time JCache dataStore creation. Default is set to <b>false</b>.

* <code>jcache.cache.inmemory.format</code> - In memory for format for persistent bean resides in cache. Possible values,
  <b>BINARY, OBJECT, NATIVE</b> Please see [In memory format](http://docs.hazelcast.org/docs/3.5/manual/html/map-inmemoryformat.html).

* <code>jcache.read.through.enable</code> - Whether to fetch a missing cache entry from backend persistent dataStore. Default value is <b>true</b>.

* <code>jcache.write.through.enable</code> - Whether to push change of a cache entry to backend persistent dataStore. Default value is <b>true</b>.

* <code>jcache.statistics.enable</code> - Statistics like cache hits and misses are collected. Default value is <b>false</b>.

* <code>jcache.management.enable</code> - JMX beans are enabled and collected statistics are exposed over the beans.It doesn't automatically enables statistics collection.
Default is set to false. Default JMX port opens on <b>9999</b>.

* <code>jcache.store.by.value.enable</code> - Whether to store key and values of data beans in means of store by value or store by reference. Default is <b>true</b> that means store by <b>value</b>.

* <code>jcache.cache.namespace</code> - Cache manager scope URI. This will allow different cache manager instances to share data among them if they are aligned with same scope.
  On opposite having two different scopes means such that each cache manager can isolate each other’s owned caches without any conflict. 
  Please see <a href="http://docs.hazelcast.org/docs/3.5/manual/html/jcache-icache.html">Scopes and Namespaces</a>

* <code>jcache.expire.policy</code> - Cache entry expiry policy. Possible values <b> ACCESSED, CREATED, MODIFIED, TOUCHED </b>
  Please see <a href="http://docs.hazelcast.org/docs/3.5/manual/html/jcache-expirepolicy.html">JCache expiry policy</a>

* <code>jcache.expire.policy.duration</code> - Cache entry expiry timeout in seconds.

* <code>jcache.eviction.policy</code> - Cache entry eviction policy. Possible values <b> LRU, LFU, NONE, RANDOM </b>
  Please see <a href="http://docs.hazelcast.org/docs/3.5/manual/html/jcache-eviction.html">Hazelcast eviction policy</a>
                                        
* <code>jcache.eviction.max.size.policy</code> - Measure of maximum cache size to apply eviction policy. 
  <b> ENTRY_COUNT, USED_NATIVE_MEMORY_SIZE, USED_NATIVE_MEMORY_PERCENTAGE, FREE_NATIVE_MEMORY_SIZE, FREE_NATIVE_MEMORY_PERCENTAGE </b>
  
* <code>jcache.eviction.size</code> - Maximum size as integer as a measure of max size policy criteria.