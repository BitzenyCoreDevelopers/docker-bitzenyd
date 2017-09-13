Bitzenyd Docker Image
=====================

How to run (3 steps)
----------

### Note

Bash is used in this instuction.
Do it on the directory you have R/W permission.
Of course, you must have permissions for execute `docker` command. ;)

### At the first

Make the directory for blockchain data.

```
mkdir bitzeny
```

### Second

Create file at `bitzeny/bitzeny.conf`.

```
rpcuser=rpc
rpcpassword=rpcpass
rpcallowip=localhost
rpcport=9302
server=1
gen=0
addnode=108.61.151.163
addnode=120.75.131.7
addnode=125.1.118.31
addnode=153.120.5.171
addnode=157.7.222.158
addnode=27.87.232.227
addnode=27.96.51.15
addnode=36.8.68.37
addnode=64.137.196.189
```

You should change `rpcuser`, `rpcpassword`, `rpcallowip`.
You may need to add `addnode` in case your bitzenyd failed to find nodes.

In addition, you don't add `daemon=1` to this file.

### Finally

Run the docker image.

```
docker run --name bitzenyd -d -v $(pwd)/bitzeny:/root/.bitzeny monacoex/bitzenyd
```

You can view logs. Like below.

```
docker exec -it bitzenyd tail -f /root/.bitzeny/debug.log
```

Enjoy.
