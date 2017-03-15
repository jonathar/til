# Elasticsearch Cheatsheet

## Cat apis

`/`

```
{
  "status" : 200,
  "name" : "hostname",
  "cluster_name" : "cluster_name",
  "version" : {
    "number" : "1.5.2",
    "build_hash" : "62ff9868b4c8a0c45860bebb259e21980778ab1c",
    "build_timestamp" : "2015-04-27T09:21:06Z",
    "build_snapshot" : false,
    "lucene_version" : "4.10.4"
  },
  "tagline" : "You Know, for Search"
}
```

`_clusther/health`

```
{
  "cluster_name" : "cluster_name",
  "status" : "green",
  "timed_out" : false,
  "number_of_nodes" : 11,
  "number_of_data_nodes" : 8,
  "active_primary_shards" : 20,
  "active_shards" : 40,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 0,
  "number_of_pending_tasks" : 0
}
```

`_cat/recovery`
```
index           shard time     type    stage source_host                 target_host                 repository snapshot files files_percent bytes        bytes_percent total_files total_bytes  translog translog_percent total_translog
index_1 0     19096369 replica done  hostname-e2 hostname-e4 n/a        n/a      727   100.0%        727804654283 100.0%        727         727804654283 0        100.0%           0
index_1 0     50572    replica done  hostname-e4 hostname-e2 n/a        n/a      0     100.0%        0            100.0%        727         727804654283 0        100.0%           0
index_1 1     52347    replica done  hostname-e2 hostname-e3 n/a        n/a      0     100.0%        0            100.0%        783         728495061170 0        100.0%           0
index_1 1     50620    replica done  hostname-e3 hostname-e2 n/a        n/a      0     100.0%        0            100.0%        783         728495061170 0        100.0%           0
```

`_cat/shards`

```
index                  shard prirep state        docs   store ip           node
index_201703 8     r      STARTED   2711141   9.5gb 127.0.1.1    hostname01
index_201703 8     p      STARTED   2711141   9.5gb 127.0.1.1    hostname02
index_201703 0     p      STARTED   2711407   9.5gb 127.0.1.1    hostname03
index_201703 0     r      STARTED   2711407   9.5gb 127.0.1.1    hostname04
```

`_cat/nodes`

```
host                        ip           heap.percent ram.percent load node.role master name
hostname-e3 172.31.3.139           60          40 0.00 d         *      hostname-e3
hostname-e2 172.31.4.248           75          39 0.00 d         m      hostname-e2
hostname-e4 172.31.4.42            50          29 0.00 d         m      hostname-e4
hostname-e5 172.31.2.126            4           4 0.00 c         -      hostname-e5/t1
hostname-e1 172.31.2.79            56          40 0.00 d         m      hostname-e1
```

`_cat/indices`

```
health status index                  pri rep docs.count docs.deleted store.size pri.store.size
green  open   index_1         10   1 2464291953     63317525     13.2tb          6.6tb
green  open   index_201703  10   1   27127171        84628    193.2gb         95.7gb
```

`_cat/aliases`

```
alias                index                  filter routing.index routing.search
alias1 index_1        -      -             -
alias2 index_201703 -      -             -
```

## Reroute Shards Manually

```
curl -XPOST 'localhost:9200/_cluster/reroute' -d '{
    "commands" : [ {
        "move" :
            {
              "index" : "test", "shard" : 0,
              "from_node" : "node1", "to_node" : "node2"
            }
        },
        {
          "allocate" : {
              "index" : "test", "shard" : 1, "node" : "node3"
          }
        }
    ]
}'
```

##  Log all queries

```
curl -XPUT http://localhost:9200/index_name/_settings -d '
{
  "index.search.slowlog.threshold.query.debug": "0s"
}
'
```
