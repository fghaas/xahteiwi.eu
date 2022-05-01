Title: Removing buckets in radosgw (and their contents)
Date: Dec 23 11:34:34 2015 +0100
Tags: Ceph

Every once in a while you'll want to remove a bucket in radosgw,
including all the objects contained in that bucket.

Now you might use a utility like [`s3cmd`](http://s3tools.org/s3cmd)
for that purpose:

    s3cmd rb --recursive s3://mybucket

The advantage to this approach is that your users can do it, using
just the regular S3 API. But this approach may be slow, particularly
if you have previously created your objects with `rest-bench`,
`cosbench`, or another benchmarking tool.

So in the event that you want to remove buckets, and their objects,
directly from `radosgw`, you can do so with

    radosgw-admin bucket rm --bucket=mybucket --purge-objects

This is usually the faster approach.

If, at any time, you want to nuke all buckets owned by a particular
user, there is a command for that, as well. Use this one with care:

    radosgw-admin user rm --uid=[username] --purge-data

* * *

This article originally appeared on the `hastexo.com` website (now defunct).
