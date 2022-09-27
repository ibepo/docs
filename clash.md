# clash
```shell
docker pull haishanh/yacd
docker run -p 1234:80 -d haishanh/yacd
docker pull dreamacro/clash
cd ~/dockerVolume/clash
wget -O config.yaml '订阅地址'
docker run --name Clash -d -v /home/ibepo/dockerVolume/clash/config.yaml:/root/.config/clash/config.yaml --network="host" --privileged dreamacro/clash
```

```shell
cd ~/dockerVolume/clash
#注意挑选专属于clash的订阅路径
wget -O config.yaml '订阅地址'
```
