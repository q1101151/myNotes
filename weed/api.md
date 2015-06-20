master 服务接口

所有的HTTP API都可以通过添加&pretty=y参数来格式化json输出。
分配文件 id
```
# 简单用法：
curl http://localhost:9333/dir/assign
{"count":1,"fid":"3,01637037d6","url":"127.0.0.1:8080",
 "publicUrl":"localhost:8080"}

# 分配文件 id 并且指定副本类型
curl "http://localhost:9333/dir/assign?replication=001"

# 指定需要分配多少个文件 id
curl "http://localhost:9333/dir/assign?count=5"

# 指定指定的 data center
curl "http://localhost:9333/dir/assign?dataCenter=dc1"
```
查找 volume

我们需要找到volume服务是否已经转移了地方。
```
curl "http://localhost:9333/dir/lookup?volumeId=3&pretty=y"
{
  "locations": [
    {
      "publicUrl": "localhost:8080",
      "url": "localhost:8080"
    }
  ]
}
# 其他用法:
# 你可以通过一个文件 id 来查找
curl "http://localhost:9333/dir/lookup?volumeId=3,01637037d6"

# 如果你知道 collection，指定collection会变得快一点。
curl "http://localhost:9333/dir/lookup?volumeId=3&collection=turbo"
```
强制垃圾回收

如果你的系统有大量的文件删除。那些删除文件的空间不会同时被删除。
有一个后台任务在检查volume空间的使用率。如果空白的空间比率超过一定的额度（默认值是0.3）时，该任务就会让volume变成只读，然后把没有存在的文件拷到另外一个新的volume中。
如果你觉得不耐烦，你可以通过该方法来回收那些不用的方法。
```
curl "http://localhost:9333/vol/vacuum"
curl "http://localhost:9333/vol/vacuum?garbageThreshold=0.4"
```
garbageThreshold参数是可选的，并不会改变该默认值。你通可以通过在启动时添加garbageThreshold参数来指定该值。
提前创建 volumes

volume只能同时写一个文件，如果你需要并发写，你可以提前创建多个volume。
```
curl "http://localhost:9333/vol/grow?replication=000&count=4"
{"count":4}

# 指定collection
curl "http://localhost:9333/vol/grow?collection=turbo&count=4"

# 指定 data center
curl "http://localhost:9333/vol/grow?dataCenter=dc1&count=4"
```
检查系统状态
```
curl "http://10.0.2.15:9333/cluster/status?pretty=y"
{
  "IsLeader": true,
  "Leader": "10.0.2.15:9333",
  "Peers": [
    "10.0.2.15:9334",
    "10.0.2.15:9335"
  ]
}
curl "http://localhost:9333/dir/status?pretty=y"
{
  "Topology": {
    "DataCenters": [
      {
        "Free": 3,
        "Id": "dc1",
        "Max": 7,
        "Racks": [
          {
            "DataNodes": [
              {
                "Free": 3,
                "Max": 7,
                "PublicUrl": "localhost:8080",
                "Url": "localhost:8080",
                "Volumes": 4
              }
            ],
            "Free": 3,
            "Id": "DefaultRack",
            "Max": 7
          }
        ]
      },
      {
        "Free": 21,
        "Id": "dc3",
        "Max": 21,
        "Racks": [
          {
            "DataNodes": [
              {
                "Free": 7,
                "Max": 7,
                "PublicUrl": "localhost:8081",
                "Url": "localhost:8081",
                "Volumes": 0
              }
            ],
            "Free": 7,
            "Id": "rack1",
            "Max": 7
          },
          {
            "DataNodes": [
              {
                "Free": 7,
                "Max": 7,
                "PublicUrl": "localhost:8082",
                "Url": "localhost:8082",
                "Volumes": 0
              },
              {
                "Free": 7,
                "Max": 7,
                "PublicUrl": "localhost:8083",
                "Url": "localhost:8083",
                "Volumes": 0
              }
            ],
            "Free": 14,
            "Id": "DefaultRack",
            "Max": 14
          }
        ]
      }
    ],
    "Free": 24,
    "Max": 28,
    "layouts": [
      {
        "collection": "",
        "replication": "000",
        "writables": [
          1,
          2,
          3,
          4
        ]
      }
    ]
  },
  "Version": "0.47"
}
```
上面操作会创建4个空白的volume。
volume 服务接口

上传文件到weedfs
```
curl -F file=@/home/www/myphoto.jpg http://127.0.0.1:8080/3,01637037d6
{"size": 43234}
```
返回的大小是文件保存在 weed-fs 的大小，有时候文件会根据文件类型自动压缩。
直接上传文件
```
curl -F file=@/home/chris/myphoto.jpg http://localhost:9333/submit
{"fid":"3,01fbe0dc6f1f38","fileName":"myphoto.jpg","fileUrl":"localhost:8080/3,01fbe0dc6f1f38","size":68231}
```
这 API 只是为了方便使用，master服务会受到一个文件id并且把文件保存到正确的volume上，该接口不支持其他参数。
删除文件
```
curl -X DELETE http://127.0.0.1:8080/3,01637037d6
```
在指定的 volume 服务创建指定的 volume
```
curl "http://localhost:8080/admin/assign_volume?replication=000&volume=3"
```
该命令会创建volume 3在该volume服务上。
如果你使用了副本类型（replication），那么你需要在在其他volume服务上创建同样的镜像文件。
检查 volume 服务的状态
```
curl "http://localhost:8080/status?pretty=y"
{
  "Version": "0.34",
  "Volumes": [
    {
      "Id": 1,
      "Size": 1319688,
      "RepType": "000",
      "Version": 2,
      "FileCount": 276,
      "DeleteCount": 0,
      "DeletedByteCount": 0,
      "ReadOnly": false
    },
    {
      "Id": 2,
      "Size": 1040962,
      "RepType": "000",
      "Version": 2,
      "FileCount": 291,
      "DeleteCount": 0,
      "DeletedByteCount": 0,
      "ReadOnly": false
    },
    {
      "Id": 3,
      "Size": 1486334,
      "RepType": "000",
      "Version": 2,
      "FileCount": 301,
      "DeleteCount": 2,
      "DeletedByteCount": 0,
      "ReadOnly": false
    },
    {
      "Id": 4,
      "Size": 8953592,
      "RepType": "000",
      "Version": 2,
      "FileCount": 320,
      "DeleteCount": 2,
      "DeletedByteCount": 0,
      "ReadOnly": false
    },
    {
      "Id": 5,
      "Size": 70815851,
      "RepType": "000",
      "Version": 2,
      "FileCount": 309,
      "DeleteCount": 1,
      "DeletedByteCount": 0,
      "ReadOnly": false
    },
    {
      "Id": 6,
      "Size": 1483131,
      "RepType": "000",
      "Version": 2,
      "FileCount": 301,
      "DeleteCount": 1,
      "DeletedByteCount": 0,
      "ReadOnly": false
    },
    {
      "Id": 7,
      "Size": 46797832,
      "RepType": "000",
      "Version": 2,
      "FileCount": 292,
      "DeleteCount": 0,
      "DeletedByteCount": 0,
      "ReadOnly": false
    }
  ]
}
```
