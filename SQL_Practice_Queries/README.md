# SQL Practice Queries

[← Ana sayfa](../README.md)

Bu klasör, SQL konularını öğrenirken yazdığım temel pratik sorgularını içerir.

Bu dosyalar iş odaklı ana proje değil; SQL konularını öğrenmek ve pekiştirmek için hazırlanmış çalışma dosyalarıdır. Bu pratiklerin iş sorularına uygulanmış hali: [E-Commerce Business SQL Analysis](../Ecommerce_Business_SQL_Analysis/README.md)

## Kullanılan Veriler

- **Olist veri seti:** Sorguların çoğu, [Brazilian E-Commerce Public Dataset by Olist (Kaggle)](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) verisinden oluşturulan `df_` tablolarıyla çalışır. Tabloların nasıl yükleneceği [E-Commerce Business SQL Analysis](../Ecommerce_Business_SQL_Analysis/README.md) sayfasında anlatılıyor.
- **Alıştırma tabloları:** CTE ve subquery dosyaları ile `Joins.sql` içindeki LEFT / FULL / RIGHT JOIN örnekleri, JOIN türleri arasındaki farkı göstermek için küçük bir alıştırma tablo setiyle (`Customers`, `Orders`, `Products`) yazılmıştır.

## Dosyalar

| Dosya | İçerik | Veri |
| --- | --- | --- |
| [`Basic.sql`](Basic.sql) | Temel SELECT, filtreleme, sıralama ve veri kontrolü sorguları | Olist |
| [`Aggregate_Functions.sql`](Aggregate_Functions.sql) | COUNT, SUM, AVG, MIN, MAX gibi aggregate fonksiyon pratikleri | Olist |
| [`Joins.sql`](Joins.sql) | Tabloları INNER, LEFT, FULL ve RIGHT JOIN ile birleştirme çalışmaları | Olist + alıştırma tabloları |
| [`Common_Table_Expression.sql`](Common_Table_Expression.sql) | WITH / CTE kullanımı | Alıştırma tabloları |
| [`Subquery_and_Deep_Filter_Analysis.sql`](Subquery_and_Deep_Filter_Analysis.sql) | Alt sorgu ve detaylı filtreleme pratikleri | Alıştırma tabloları |
| [`Window_Function_Expression.sql`](Window_Function_Expression.sql) | ROW_NUMBER ve window function pratikleri | Olist |
