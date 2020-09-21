[![](https://img.shields.io/github/license/gsxhnd/owl)](https://opensource.org/licenses/MIT)
![Test](https://github.com/gsxhnd/owl/workflows/Test/badge.svg)

# owl
Read and Put conf to etcd

You can use owl as a command line tool [owl-cli](https://github.com/gsxhnd/owl-cli) or as a  library


## Get configure from etcd

### set etcd client
```bash
# set client use default client config
owl.SetRemoteAddr([]string{"127.0.0.1:2379"})
# set client use custome client config
conf := clientv3.Config{
		Endpoints:        []string{"127.0.0.1:2379"},
		AutoSyncInterval: 0,
		DialTimeout:      5 * time.Second,
	}
owl.SetRemoteAddr(conf)
```

### get value from etcd
```bash
# GetByKey() get conf by key
conf,_:=owl.GetRemote("/conf/test.yaml")
```

### watch value change
```bash
confKey := "conf/test.yaml"
c := make(chan string)
go owl.Watcher(confKey, c)
go func() {
    for i := range c {
        fmt.Println("config value changed: ", i)
    }
}()
```