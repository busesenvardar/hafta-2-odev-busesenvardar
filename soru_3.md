#Soru 3) "sample.pageview": tablosunda 1 gün içerisinde trendyol.com a gelen tüm ziyaretlerin logu var.
```SQL
DECLARE initial TIMESTAMP DEFAULT "2020-03-03 00:00:00";
CREATE TABLE IF NOT EXISTS `buse_senvardar.answerofq3` (
  `tarih` TIMESTAMP,
  `kullanici_sayisi` INT64
);
WHILE initial < "2020-03-04 00:00:00" DO
  INSERT INTO `buse_senvardar.answerofq3` (tarih, kullanici_sayisi)
        SELECT timestamp_add(initial,INTERVAL 5 MINUTE) as tarih, HLL_COUNT.extract(HLL_COUNT.init(deviceid)) as kullanici_sayisi FROM buse_senvardar.pageview
          WHERE view_ts BETWEEN initial AND timestamp_add(initial,INTERVAL 5 MINUTE);
  SET initial = timestamp_add(initial, INTERVAL 1 MINUTE);
END WHILE;
```
