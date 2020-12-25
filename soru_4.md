Bizim isteğimiz sample.content_category ve sample.content_category_20201222_00_59 tablolarını karşılaştırarak
insert edilen yeni kayıtları sample.content_category tablosunada eklemek,
update edilen kayıtlar var ise sample.content_category tablosunda da update etmek ve
silinmiş olan kayıtların sample.content_category tablosundaki karşılıklarının is_deleted alanını true olarak güncellemek ve silmeden saklamaktır.
Ve bunu yaparken tek bir create or replace table ya da merge statementı kullanarak yapmak istiyoruz.
``` SQL
Merge buse_senvardar.content_category AS ccb  
Using buse_senvardar.content_category_20201222_00_59 As ccs
on (ccb.id=ccs.id)

When Matched and ccb.cdc_date <> ccs.cdc_date Or 
	ccb.is_deleted <> ccs.is_deleted  or ccb.category<>ccs.category Then 
Update Set ccb.cdc_date=ccs.cdc_date,ccb.is_deleted=ccs.is_deleted ,ccb.category=ccs.category

When Not Matched By Target Then 
Insert (cdc_date,is_deleted,id,category)
Values (ccs.cdc_date,ccs.is_deleted,ccs.id,ccs.category)

```
