# s3rsyncs3
Rsync from one S3 service to another S3.  

* Requires an auth.json file, with the server urls, and keys
* Requires a conf.json, with the source buckets, and destination bucket

An object is transferred if there is no correcsponding object at the destination; Or if the ETag fields differ. Currently, objects deleted from the source store, are not deleted from the destination store.

The ETag can be an AWS style multipart MD5, but it will only match if the source and destination objects where both uploaded with the same chunk size. This code assumes 1GiB (1073741824), to match the value used for uploading instrument data. Objects of size less than, or equal to the chunk size are uploaded without using chunks, so have a standard MD5 in the ETag.

## Help
```
usage: s3rsyncs3.py [-h] [-?] [-d DEBUG_LVL] [-n] [-c CONF_FILE] [-a AUTH_FILE]

rsync from source s3 bucket to dest s3 bucket

optional arguments:
  -h, --help                        show this help message and exit
  -?                                show this help message and exit
  -d DEBUG_LVL, --debug DEBUG_LVL   0: Off, 
                                    1: Copy/Mismatch messages, 
                                    2: Exists messages,
                                    3: Dest ls
  -n, --no_rsync                    Use with Debugging. Default is to perform s3 rsync
  -c CONF_FILE, --conf CONF_FILE    Specify JSON conf file for source and destination
  -a AUTH_FILE, --auth AUTH_FILE    Specify JSON auth file containing s3 keys
```

## Configuration

### conf/s3_auth.json
```json
{
  "src_endpoint": "https://a.b",
  "src_s3_keys": {
    "access_key_id": "xxxxxxxx",
    "secret_access_key": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
   },

  "dest_endpoint": "https://c.d",
  "dest_s3_keys": {
    "access_key_id": "xxxxxxxx",
    "secret_access_key": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
  }
}
```

### conf/s3_conf.json
```json
{
  "src_buckets": [ "aaaaaa", "bbbbbb", "ccccccc"],
  "dest_bucket": "xxxxxx",
  "chunk_size": 1073741824
}
```

