weed-fs 副本

weed-fs 支持副本功能，但是副本功能的实现，不是在文件级别，实在volume级别实现的。
怎么使用副本?

使副本功能生效只需要下面步骤：
开启 master 服务

开启master服务，并且指定默认的副本类型：
```
./weed master -defaultReplicationType=001
```
启动 volume 服务

启动volume服务：
```
./weed volume -port=8081 -dir=/tmp/1 -max=100
./weed volume -port=8082 -dir=/tmp/2 -max=100
./weed volume -port=8083 -dir=/tmp/3 -max=100
```
其他读写操作跟平常一样。
WeedFS 副本类型的含义

注意：该含义将来可能会变化

||类型||	含义||
||000||	没有副本||
||001||	在同样的 rack 有一个副本||
||010||	在不同的 rack 并且在同样的 data center 里面有一个副本||
||100||	在不同的 data center 里面有一个副本||
||200||	在两个不同的 data center 里面有两个副本||
||110||	在不同的 rack 和不同的 data center 里面 各有一个副本||
||…||	…||

如果把副本来行当做xyz，那么：
类型	含义
```
x	data center数量
y	同一个 data center 中应该保存副本的rack数量
z	在同一个rack中保存副本的数量
x，y，z都可以为0，1，2。
```
配置文件例子

Weed-FS 会默认通过/etc/weedfs/weedfs.conf配置文件类配置系统，该文件的例子如下。
```
<Configuration>
    <Topology>
      <DataCenter name="dc1">
        <Rack name="rack1">
          <Ip>192.168.1.1</Ip>
        </Rack>
      </DataCenter>
      <DataCenter name="dc2">
        <Rack name="rack1">
          <Ip>192.168.1.2</Ip>
        </Rack>
        <Rack name="rack2">
          <Ip>192.168.1.3</Ip>
          <Ip>192.168.1.4</Ip>
        </Rack>
      </DataCenter>
    </Topology>
</Configuration>
```
你也可以再启动volume服务的时候指定data center名称：
```
weed volume -dir=/tmp/1 -port=8080 -dataCenter=dc1
```
weed volume -dir=/tmp/2 -port=8081 -dataCenter=dc2
那么你就可以在请求分配文件id的时候，指定data center了：
http://localhost:9333/dir/assign?dataCenter=dc1
