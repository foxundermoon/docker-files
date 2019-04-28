
# source code

[https://github.com/xtaci/kcptun](https://github.com/xtaci/kcptun)

#  Usage

## server

```bash
docker run -d -p 4000:4000 foxundermoon/kcptun-server -t "TARGET_IP:8388" -l ":4000" -mode fast3 -nocomp -sockbuf 16777217 -dscp 46

``` 

# client

```bash
docker run -d -p 8388:8388 foxundermoon/kcptun-client -r "KCP_SERVER_IP:4000" -l ":8388" -mode fast3 -nocomp -autoexpire 900 -sockbuf 16777217 -dscp 46
```

