# BJIR

An example how I use the nginx-vod-module to stream video from a directory.

## Running locally

```
% docker build --load -t nginx-vod-module .
% docker run -d --name vod-server -p 3030:80 -v $PWD/videos:/opt/static/videos -v $PWD/nginx.conf:/usr/local/nginx/conf/nginx.conf nginx-vod-module
```
then run the `index.html`.

url:
- HLS: http://localhost:3030/hls/devito,360p.mp4,480p.mp4,720p.mp4,.en_US.vtt,.urlset/master.m3u8
- Dash: http://localhost:3030/dash/devito,360p.mp4,480p.mp4,720p.mp4,.en_US.vtt,.urlset/manifest.mpd
- Thumbnail: http://localhost:3030/thumb/devito360p.mp4/thumb-1000.jpg

## URI Encryption

To test with uri encryption please run the encryptionUrl.php file

```sh
    php encryptUrl.php <base url> <encrypted part> <encryption key> <encryption iv>
```

for example,

```php
    php encryptUrl.php "/hls/" "devito,360p.mp4,480p.mp4,720p.mp4,.en_US.vtt,.urlset/master.m3u8" "000102030405060708090a0b0c0d0e0f101112131415161718191a1b1c1d1e1f" "00000000000000000000000000000000" 
```

the url output will be:

```
    /hls/_Jt6EDpqR4ZJdEKgpJOoQYiXflVRKh5UaoVl7S01x4G-r1biX6p0WarMqihUyg56xVVOfsYUx4xPVLG8aUMtdAr5JGxMSC7PMd4Ds6gYldY"
```

## Generate Encryption

Generate encryption key:

```sh
   % dd if=/dev/urandom bs=1 count=32 2> /dev/null | xxd -p -c32
   6f924323bf82b14e0d633ff7fdec9140494e4125e6f99c4294cf1b81df9c3648
```

Generate encryption iv:

```sh
   % dd if=/dev/urandom bs=1 count=16 2> /dev/null | xxd -p -c32
   0b04618b0e67bc5aef49b0d81f3a4762
```
