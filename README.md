# math2-records

This repository holds the full catalogue of mathematical resources of [tamnd/math2](https://github.com/tamnd/math2). The move follows RS12.3 of the resources spec. The catalogue is too large for the math2 repository, so math2 keeps only the records of the tiers T1, T2 and T5 and a pin of a commit of this repository in `manifests/records.yaml`. The id registers stay in math2.

## Layout

The layout is the same as in math2. Each file is a shard of records in the Markdown record format of math2, compressed with zstd.

| Path | Contents |
|---|---|
| `records/resources/<shard>.md.zst` | the resource records, one shard for the first three characters of the id |
| `records/resources/cites/<shard>.md.zst` | the cites edges, in shards by the citing resource |
| `records/resources/structure/*.md.zst` | the structure files of the works |
| `records/resources/index.md` | each shard with its blake3 hash and its size before compression |
| `records/resources/resources.parquet` | one flat row for each resource, for queries |

## How to update

Run these commands from a working tree of math2-agent. The first command writes the shards to this tree, and the last command pins the new commit in math2.

```
m2 res status -root MATH2
m2 res records export -root MATH2 -out MATH2_RECORDS
git -C MATH2_RECORDS commit -am "Update the catalogue"
m2 res records keep -root MATH2
m2 res records pin -root MATH2 -out MATH2_RECORDS -commit $(git -C MATH2_RECORDS rev-parse HEAD)
```

## Licences

Each record names its sources. The records of a source keep the licence and the attribution of that source, as `manifests/sources.yaml` of math2 gives them (RS14.3).
