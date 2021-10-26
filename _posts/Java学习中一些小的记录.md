---
title: 2021-10-25-记录第二个开发任务
tags: TeXt
layout: home
---

## 一些数据类型  

List：有序的元素集合，可以通过索引访问；元素是可以重复的  

ArrayList：长度可变的数组，可以对元素进行随机的访问，插入和删除元素的速度较慢  

LinkList：采用链表数据结构，插入和删除速度很快，但是访问较慢  

Set：无序、不可重复的数据,同样允许null值得存在，但是最多只能有一个null值  

HashSet：按照哈希算法取集合中的对象，存取速度比较快  

TreeSet：实现了SortSet接口，能够对集合中的对象进行排序

Map：常用的实现类是Hashmap和Treemap  

HashMap：基于散列表实现，插入和查询的开销是固定的，可以通过构造设置容量和负载因子来调整容器的性能  

TreeMap：基于红黑树实现。查看<k,v>时，会被排序。TreeMap是唯一带有subMap()方法的Map，可以返回一个子树。  

## 一个golang学习的网站——https://www.golangroadmap.com/

原始文件格式  
```
PART                     TYPE                     PIN=NETNAME                                       NAIL
S5000                    ANALOG                   1=BUTTON_RST_IN_N,                                F180
S5000                    ANALOG                   2=_GND,                                           F3
S5000                    ANALOG                   3=_GND,                                           F3
S5000                    ANALOG                   4=_GND,                                           F3
S5000                    ANALOG                   5=_GND,                                           F3
S5000                    ANALOG                   6=_GND;                                           F3
R4000                    RES                      1=_22N5209,                                       
R4000                    RES                      2=_22N5205;                                       
U2                       HYBRID                   1=ADS7953_GPIO2,                                  F597
U2                       HYBRID                   2=ADS7953_GPIO3,                                  F598
U2                       HYBRID                   3=_GND,                                           F3
U2                       HYBRID                   4=_75I11032_AD7953_REF,                           F535
U2                       HYBRID                   5=_75I11032V3V3ADS7953,                           F527
U2                       HYBRID                   6=_GND,                                           F3
U2                       HYBRID                   7=_75I11032_7_U5501,                              F614
U2                       HYBRID                   8=_75I11032_7_U5501,                              F614
U2                       HYBRID                   9=_GND,                                           F3
U2                       HYBRID                   10=_GND,                                          F3


目标格式

PART                     TYPE                     PIN=NETNAME                                       NAIL
S5000                    ANALOG                   1=BUTTON_RST_IN_N,                                F180
                                                  2=_GND,                                           F3
                                                  3=_GND,                                           F3
                                                  4=_GND,                                           F3
                                                  5=_GND,                                           F3
                                                  6=_GND;                                           F3
R4000                    RES                      1=_22N5209,                                       
                                                  2=_22N5205;                                       
U2                       HYBRID                   1=ADS7953_GPIO2,                                  F597
                                                  2=ADS7953_GPIO3,                                  F598
                                                  3=_GND,                                           F3
                                                  4=_75I11032_AD7953_REF,                           F535
                                                  5=_75I11032V3V3ADS7953,                           F527
                                                  6=_GND,                                           F3
                                                  7=_75I11032_7_U5501,                              F614
                                                  8=_75I11032_7_U5501,                              F614
                                                  9=_GND,                                           F3
                                                  10=_GND,                                          F3   
                                                  
```
代码
```java   

public class GetPinsTest {
    @Test
    public void test() throws Exception {
        SqliteDao sqliteDao = new SqliteDao("D:\\Program Files\\SQLiteStudio\\197480242133209088_ts.db3");
        List<TelPinEntity> telPin = sqliteDao.getDao(TelPinEntity.class).queryForAll();
        List<TelNetEntity> telNet = sqliteDao.getDao(TelNetEntity.class).queryForAll();
        List<ValPinEntity> valPin = sqliteDao.getDao(ValPinEntity.class).queryForAll();
        List<NetNailEntity> NetNail = sqliteDao.getDao(NetNailEntity.class).queryForAll();
        Map<String, String> newNetName =
                telNet.stream()
                        .collect(
                                Collectors.toMap(
                                        TelNetEntity::getNetName, TelNetEntity::getNewNetName, (e1, e2) -> e1));
        Map<String, String> nameToNail =
                telPin.stream()
                        .collect(
                                Collectors.toMap(
                                        TelPinEntity::getNetName, TelPinEntity::getPinNoDetail, (e1, e2) -> e1));
        Map<String, String> nailToConn =
                NetNail.stream()
                        .collect(
                                Collectors.toMap(
                                        NetNailEntity::getNailNum, NetNailEntity::getConn));
        Set<String> partRef = valPin.stream().map(ValPinEntity::getPartRef).distinct().collect(Collectors.toSet());
        Field[] field = ValPinEntity.class.getDeclaredFields();

        List<String> oriPin =
                valPin.stream()
                        .map(Objects::toString)
                        .collect(Collectors.toList());   
                        
```
