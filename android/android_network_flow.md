# Android网络流量优化

## 是什么

    流量是哪里产生的？怎样定位到问题？

## 怎么做

1. 如果仅统计应用总流量，TrafficStats 类提供的接口就可以。
2. 需要分析流量的成分，TrafficStats类的方法setThreadStatsTag(int)可以标记线程，Android monitor里面获取的流量数据是包含有tag的，Android系统将放流量数据信息保存在文件/proc/net/xt_qtaguid/stats

通过adb命令可以查看流量内容 adb shell cat /proc/net/xt_qtaguid/stats

iface：网络性质［vlan－wifi流量 lo－本地流量 rmnet0－3g/2g流量 ...］

acct_tag_hex： 线程标记，也就是上面我们做的标记

uid_tag_int： 应用UID，据此判断是否是某应用统计的流量数据

cnt_set： 应用前后台标志位：1-前台，0-后台

rx_bytes： read 总接受字节

tx_bytes：  transfer总发送字节

其中acct_tag_hex 为0x0的一行为某应用开机以来产生的总流量；

这里统计的流量是开机之后所有的流量统计

```java

try {
            FileReader stream = new FileReader(FILE_NAME;
            BufferedReader in = new BufferedReader(stream, 500);
            String line;
            String[] dataStrings;
            while ((line = in.readLine()) != null) {
                dataStrings = line.split(" ");
                if (StringUtils.equals(uid, dataStrings[3])) {
                    TrafficData dataTotal = new TrafficData();
                    TrafficData dataDiff = new TrafficData();

                    dataTotal.iface = dataDiff.iface = dataStrings[1];
                    dataTotal.moduleTag = dataDiff.moduleTag = dataStrings[2];
                    dataTotal.cnt_set = dataDiff.cnt_set = dataStrings[4];

                    dataTotal.rx_bytes = Integer.parseInt(dataStrings[5]);
                    dataTotal.tx_bytes = Integer.parseInt(dataStrings[6]);

                    if (dataTotal.rx_bytes + dataTotal.tx_bytes > 0) {
                        trafficDataList.add(dataTotal);
                        //根据存储值计算增量
                    }
                }
            }
            FileStoreProxy.setListValue(trafficDataList);
        } catch (Exception e) {
        } finally {
            in.close();
            stream.close();
        }

```