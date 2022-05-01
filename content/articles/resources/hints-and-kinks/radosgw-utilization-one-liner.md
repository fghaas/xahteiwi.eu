Title: A Python one-liner for pretty-printing radosgw utilization
Tags: Ceph, Python
Date: 2015-12-17
Slug: radosgw-utilization-one-liner
Summary: In case you need a quick overview of how many radosgw objects live in your Ceph cluster, here‘s how you do that in one (slightly involved) line of Python.

In case you need a quick overview of how many radosgw objects live in
your Ceph cluster, your first step is normally this command:

    radosgw-admin bucket stats

When used *without* the `--bucket=<name>` argument, this command lists
a bunch of statistics for *all* your radosgw buckets, in a somewhat
convoluted JSON format. If you only want a simple list of all your
buckets and the number of objects they contain, you can use the
following bit of Python list comprehension magic:

```bash
radosgw-admin bucket stats | \
  python -c 'import json; import sys; print "\n".join(["%s: %s" % (str(x["bucket"]), ", ".join(["%s: %s" % (k, v["num_objects"]) for k,v in x["usage"].iteritems()])) for x in json.load(sys.stdin) if isinstance(x,dict)])'
```

And while the above is all on one line so you can easily copy and
paste, here are the Python bits in a slightly more legible format:

```python
import json
import sys

data = json.load(sys.stdin)

print "\n".join(["%s: %s" % (str(x["bucket"]),
                             ", ".join(["%s: %s" % (k,
                                                    v["num_objects"])
                                        for k, v in x["usage"].iteritems()]))
                 for x in data
                 if isinstance(x, dict)])
```
    
Of course, you'll need to substitute `print()` for `print` if your
system runs only Python 3.

* * *

This article originally appeared on the `hastexo.com` website (now defunct).
