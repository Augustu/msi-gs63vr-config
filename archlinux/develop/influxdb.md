### InfluxDB

#### Start

```bash
docker run --name influxdb \
	-p 127.0.0.1:8086:8086 \
	-v /home/augustu/Mount/Develop/data/influxdb/etc:/etc/influxdb/ \
	-v /home/augustu/Mount/Develop/data/influxdb/data:/var/lib/influxdb/data \
	-d influxdb


```





ref: https://docs.influxdata.com/influxdb/v2.0/get-started/

