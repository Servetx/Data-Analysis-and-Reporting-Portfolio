# DAX Measures

[← Proje sayfası](../README.md) · [Ana sayfa](../../README.md)

Bu dosya, Training E-Commerce Dashboard projesinde kullanılan DAX ölçülerini içerir.

## Core KPI Measures

Dashboard'daki temel KPI kartlarını hesaplamak için kullanılmıştır.  
Customers müşteri sayısını, Orders sipariş sayısını, Sales toplam satış tutarını, AOV ise ortalama sipariş değerini gösterir.

```dax
Customers =
DISTINCTCOUNT(facteCommerce[Customer ID])

Orders =
COUNTROWS(facteCommerce)

Sales  =
SUM(facteCommerce[Total Sale Amount])

AOV =
DIVIDE(
    [Sales],
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

Aynı Previous Month, Growth ve Color mantığı Customers, Orders ve Sales KPI kartları için de uygulanmıştır.

## Country Sales Percentage

Ülke bazlı satış oranını hesaplamak için kullanılmıştır.

```dax
Country Sales % =
VAR _allvalue =
    CALCULATE(
        [Sales],
        ALL(dimCountry)
    )
RETURN
    DIVIDE([Sales], _allvalue)
```

## Continent Rounded Bar Chart

Kıta bazlı satış oranını SVG bar chart olarak göstermek için kullanılmıştır.

```dax
Continent Rounded Bar Chart =
VAR W =
    ROUND([Country Sales %] * 500, 0)

VAR Pct =
    FORMAT([Country Sales %], "0.0%")

RETURN
    "data:image/svg+xml;utf8,"
        & "<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 270 40'>"
        & "<rect x='0' y='8' width='" & W & "' height='24' rx='12' ry='12' fill='#2196F3' />"
        & "<text x='" & (W + 10) & "' y='26' font-size='28' fill='black'>"
        & Pct
        & "</text>"
        & "</svg>"
```

## Highlighted Month Details

Seçili ayların toplam satış içindeki oranını dinamik metin olarak göstermek için kullanılmıştır.

```dax
Highlighted Month Details =
VAR _selected_months =
    VALUES('Month Param'[Month Param])

VAR _selected_months_Sales =
    CALCULATE(
        [Sales],
        'Calendar'[Monthnum] IN _selected_months
    )

VAR _percent_of_total =
    FORMAT(
        DIVIDE(
            _selected_months_Sales,
            [Sales],
            0
        ),
        "percent"
    )

VAR _result =
    IF(
        ISFILTERED('Month Param'[Month Param]),
        "Highlighted months Sales : "
            & FORMAT(_selected_months_Sales, "#,0,,.00 Mi ( ")
            & _percent_of_total
            & " ) in Total Sales "
            & FORMAT([Sales], "#,0,,.00 Mi"),
        "Total Sales : "
            & FORMAT([Sales], "#,0,,.00 Mi")
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
