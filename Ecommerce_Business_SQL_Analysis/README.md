# E-Commerce Business SQL Analysis

[← Ana sayfa](../README.md)

Bu proje, Olist Brazil e-ticaret veri seti üzerinde SQL Server ile hazırlanmış iş odaklı veri analizi çalışmasıdır.

Amaç sadece temel SQL komutlarını göstermek değil; müşteri, sipariş, ürün, ödeme, satıcı, gelir ve veri kalite kontrolleri gibi iş sorularını SQL ile cevaplamaktır.

## Proje Konumu

Bu çalışma gerçek şirket deneyimi iddiası taşımaz. Portföy amaçlı bir vaka çalışmasıdır.

Projenin hedefi:

- İş sorusunu SQL sorgusuna çevirebilmek
- Tablolar arasındaki ilişkileri kullanmak
- Gelir, ödeme ve müşteri analizleri yapmak
- Veri kalite problemlerini kontrol etmek
- Okunabilir ve açık SQL dosyaları hazırlamak

## Veri Kaynağı

[Brazilian E-Commerce Public Dataset by Olist (Kaggle)](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

Aynı veri seti Power BI ile de görselleştirildi: [Brazil Olist E-Commerce Dashboard](../02-Brazil-Olist-Ecommerce-Dashboard/README.md)

## Kullanılan Tablolar

| Tablo | Kaynak CSV dosyası | Açıklama |
| --- | --- | --- |
| `df_Customers` | `olist_customers_dataset.csv` | Müşteri şehir ve eyalet bilgileri |
| `df_Orders` | `olist_orders_dataset.csv` | Sipariş ve müşteri ilişkisi |
| `df_OrderItems` | `olist_order_items_dataset.csv` | Ürün, satıcı, fiyat ve kargo ücreti bilgileri |
| `df_Payments` | `olist_order_payments_dataset.csv` | Ödeme tipi, taksit sayısı ve ödeme tutarı |
| `df_Products` | `olist_products_dataset.csv` | Ürün kategorisi ve ürün ölçü bilgileri |

## Nasıl Çalıştırılır

1. Veri setini Kaggle'dan indirin.
2. SQL Server'da `E_commerce` adında bir veritabanı oluşturun.
3. Yukarıdaki CSV dosyalarını tablodaki adlarla bu veritabanına aktarın (ör. SSMS → Tasks → Import Flat File).
4. Önce `00_Data_Preparation.sql`, ardından diğer dosyaları numara sırasıyla çalıştırın.

## Dosyalar

| Dosya | İçerik |
| --- | --- |
| [`00_Data_Preparation.sql`](00_Data_Preparation.sql) | Sayısal analiz için veri tipi hazırlığı ve `product_weight_g` temizliği |
| [`01_Business_Aggregate_Analysis.sql`](01_Business_Aggregate_Analysis.sql) | COUNT, SUM, AVG, MIN, MAX, GROUP BY ve CASE WHEN ile temel iş analizleri |
| [`02_Business_Join_Analysis.sql`](02_Business_Join_Analysis.sql) | Müşteri, sipariş, ürün ve ödeme tablolarını JOIN ile birleştiren analizler |
| [`03_Business_CTE_Analysis.sql`](03_Business_CTE_Analysis.sql) | WITH / CTE kullanarak ara sonuç tabloları ile iş analizleri |
| [`04_Business_Window_Function_Analysis.sql`](04_Business_Window_Function_Analysis.sql) | ROW_NUMBER ile müşteri, kategori, satıcı, ödeme türü, şehir ve eyalet sıralama analizleri |
| [`05_Data_Quality_Checks.sql`](05_Data_Quality_Checks.sql) | Tekrar eden ID, negatif tutar, boş kategori ve eşleşmeyen kayıt kontrolleri |

## Cevaplanan İş Soruları

Bu projede aşağıdaki iş sorularına SQL ile cevap verilmiştir:

- Veri setinde kaç sipariş ve kaç müşteri vardır?
- En çok müşteri hangi eyalet ve şehirlerdedir?
- Ürün geliri, kargo geliri ve toplam müşteri ödeme tutarı nedir?
- Ödeme tipine göre toplam ödeme tutarı nedir?
- Hangi ürün kategorileri en çok gelir getiriyor?
- Hangi satıcılar en yüksek satış gelirini oluşturuyor?
- Müşteriler toplam harcamaya göre nasıl sıralanıyor?
- Şehir ve eyalet bazında satış geliri nasıl dağılıyor?
- Tekrar eden müşteri veya sipariş ID var mı?
- Fiyat, kargo ve ödeme tutarlarında hatalı değer var mı?
- Ürün kategorisi boş olan kayıtlar var mı?
- Sipariş ve sipariş kalemleri ilgili ana tablolarla eşleşiyor mu?

## Örnek Veri Kalitesi Bulguları

Bu proje kapsamında yapılan veri kalite kontrollerinde aşağıdaki bulgular elde edilmiştir:

- `product_category_name` alanında `NULL` kayıt bulunmadı.
- `product_category_name` alanında 308 kayıt boş metin (`EMPTY_STRING`) olarak tespit edildi.
- `product_weight_g` alanında sayısal tipe çevrilemeyen 15 kayıt `NULL` olarak işaretlendi.
- Ortalama ağırlık hesapları geçerli, sayısal ve `NULL` olmayan ağırlık değerleri üzerinden yapıldı.

## Kullanılan SQL Konuları

- `COUNT`, `COUNT(DISTINCT)`
- `SUM`, `AVG`, `MIN`, `MAX`
- `GROUP BY`, `HAVING`
- `CASE WHEN`
- `INNER JOIN`, `LEFT JOIN`
- `WITH` / CTE
- `ROW_NUMBER() OVER(...)`
- `WHERE`, `IS NULL`, boş metin kontrolü
- Veri kalite kontrolleri

## Not

Bu proje iş odaklı SQL portföy çalışmasıdır. Kodlar, temel SQL bilgisini gerçekçi iş sorularına uygulama mantığıyla hazırlanmıştır.
