# DAX Measures

[← Proje sayfası](../README.md) · [Ana sayfa](../../README.md)

Bu dosya, Brazil Olist E-Commerce Dashboard projesinde kullanılan temel DAX ölçülerini içerir.

## Core KPI Measures

Dashboard'daki temel KPI kartlarını hesaplamak için kullanılmıştır.  
Customers müşteri sayısını, Orders sipariş sayısını, Payment Value toplam ödeme tutarını, AOV ise ortalama sipariş değerini gösterir.

```dax
Customers =
DISTINCTCOUNT(df_Customers[customer_id])

Orders =
COUNTROWS(df_Orders)

Payment Value =
SUM(df_Payments[payment_value])

AOV =
DIVIDE(
    [Payment Value],
    [Orders]
)
```

## Previous Month and Growth Measures

KPI kartlarında önceki ay değeri, büyüme oranı ve pozitif/negatif renk bilgisini göstermek için kullanılmıştır.
```dax
AOV PM =
CALCULATE(
    [AOV],
    PREVIOUSMONTH('Calendar'[Date])
)

AOV Growth =
DIVIDE(
    [AOV] - [AOV PM],
    [AOV PM]
)

AOV Color =
IF(
    [AOV Growth] >= 0,
    "Green",
    "Red"
)
```

Aynı Previous Month, Growth ve Color mantığı Customers, Orders ve Payment Value KPI kartları için de uygulanmıştır.

## Highlighted Month Details

Seçili ayların toplam ödeme tutarı içindeki oranını dinamik metin olarak göstermek için kullanılmıştır.

```dax
Highlighted Month Details =
VAR _selected_months =
    VALUES('Month Param'[Month Param])

VAR _selected_months_PaymentValue =
    CALCULATE(
        [Payment Value],
        'Calendar'[Monthnum] IN _selected_months
    )

VAR _percent_of_total =
    FORMAT(
        DIVIDE(
            _selected_months_PaymentValue,
            [Payment Value],
            0
        ),
        "percent"
    )

VAR _result =
    IF(
        ISFILTERED('Month Param'[Month Param]),
        "Highlighted months Payment Value: "
            & FORMAT(_selected_months_PaymentValue, "#,0,,.00 Mi ( ")
            & _percent_of_total
            & " ) in Payment Value "
            & FORMAT([Payment Value], "#,0,,.00 Mi"),
        "Payment Value : "
            & FORMAT([Payment Value], "#,0,,.00 Mi")
    )

RETURN
    _result
```

## Month Highlight Color

Seçilen ayları grafikte farklı renkle göstermek için kullanılmıştır.

```dax
Month Highlight Color =
VAR _selected_months =
    VALUES('Month Param'[Month Param])

VAR _result =
    IF(
        ISFILTERED('Month Param'[Month Param])
            && MAX('Calendar'[Monthnum]) IN _selected_months,
        "#FF8310",
        "#4063FF"
    )

RETURN
    _result
```
## Month Switch

Ay numarasını kısa ay adına çevirmek için kullanılmıştır.


```dax
Month Switch =
SWITCH(
    SELECTEDVALUE('Month Param'[Month Param]),
    1, "Jan",
    2, "Feb",
    3, "Mar",
    4, "Apr",
    5, "May",
    6, "Jun",
    7, "Jul",
    8, "Aug",
    9, "Sep",
    10, "Oct",
    11, "Nov",
    12, "Dec"
)
```
## Previous Month

KPI kartlarında önceki ay bilgisini dinamik olarak göstermek için kullanılmıştır.

```dax
Previous Month =
VAR myPrevMonth =
    CALCULATE(
        SELECTEDVALUE('Calendar'[Month]),
        PREVIOUSMONTH('Calendar'[Date])
    )

RETURN
    "vs. " & myPrevMonth & ":"
```

## Calendar Table

Tarih bazlı analizler, önceki ay karşılaştırmaları ve ay filtreleri için kullanılan takvim tablosudur.

```dax
Calendar =
ADDCOLUMNS(
    CALENDARAUTO(),
    "Year", YEAR([Date]),
    "Month", FORMAT([Date], "mmm", "en-US"),
    "Monthnum", MONTH([Date]),
    "Day", DAY([Date]),
    "Weekday", FORMAT([Date], "ddd"),
    "Weeknum", WEEKDAY([Date]),
    "Qtr", "Q- " & FORMAT([Date], "Q"),
    "Weektype", IF(
        WEEKDAY([Date]) = 1 || WEEKDAY([Date]) = 7,
        "Weekend",
        "Weekday"
    )
)
