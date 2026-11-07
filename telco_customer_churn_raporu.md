# Telco Customer Churn — Proje raporu

## Amaç

Ayrılan müşterileri sınıflandırdık. Ayrılanları kaçırmamaya öncelik verdik ve bunun kaç yanlış alarma yol açtığına baktık.

## Veri

IBM örnek verisinde 7.043 müşteri ve 21 sütun vardı. Sınıf oranını koruyarak 5.634 müşteriyi eğitime, 1.409 müşteriyi final teste ayırdık. Müşteri kimliğini model girdisi olarak kullanmadık.

## Yöntem

Eksik değer doldurma ve kategorik kodlama işlemlerini modelin işlem akışına aldık. Lojistik regresyon için sayısal değerleri de ölçekledik. Modelleri aynı beş doğrulama katında karşılaştırdık.

## Model karşılaştırması

| Model | Ortalama AP |
| --- | ---: |
| Referans model | 0,265 |
| Karar ağacı | 0,624 |
| Random Forest | 0,666 |
| Lojistik regresyon | 0,662 |

Random Forest ile AP farkı küçük olduğu ve varsayılan eşikte recall ile F2 daha yüksek çıktığı için lojistik regresyonu seçtik. Referans modelin recall değeri sıfırdı.

## Eşik seçimi

Her eğitim müşterisi için onu görmemiş modelin tahminini kullandık. F2’yi en yüksek yapan **0,1495644981** eşiğinde F2 **0,7523** çıktı. 0,50 eşiğine göre 547 ek müşteri yakaladık ve 1.311 ek yanlış alarm verdik.

## Final sonuçlar

| Gerçek durum | Tahmin No | Tahmin Yes |
| --- | ---: | ---: |
| No | 598 | 437 |
| Yes | 31 | 343 |

Recall **%91,71**, precision **%43,97**, F2 **0,754**, AP **0,634**. 374 ayrılan müşterinin 343’ünü yakaladık. 31 ayrılan müşteriyi kaçırdık ve 437 yanlış alarm verdik.

## Sınırlamalar

Bu bir örnek veri kümesi ve test ayrımı zaman sıralı değil. Sonuçlar bu veri kümesindeki performansı gösteriyor. Gözlenen ilişkiler nedensellik kanıtı oluşturmuyor ve gelecekteki performansı ölçmüyor.

## Öğrenme notları

En dikkat gerektiren kısım, eşiği eğitim tahminleriyle seçip final testi ayrı tutmaktı.

1. Çoğunluk sınıfını seçmek yüksek accuracy verebilirken ayrılan müşterilerin tamamını kaçırabiliyor.
2. Eşiği düşürmek daha fazla müşteriyi yakalayabiliyor ve yanlış alarmları da artırabiliyor.
3. Birden fazla ağacı birleştiren Random Forest bu veride tek ağaca göre daha yüksek AP verdi.
