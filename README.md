# Telco Customer Churn

IBM’in örnek telekom verisiyle müşteri ayrılmasını sınıflandıran bir makine öğrenmesi projesi. Amaç, ayrılan müşterileri mümkün olduğunca yakalarken oluşan yanlış alarmları da ölçmek.

## Final test sonucu

Lojistik regresyon 5.634 eğitim kaydıyla eğitildi ve ayrılmış 1.409 test kaydında değerlendirildi. Karar eşiği, yalnızca eğitim verisindeki kat dışı tahminlerde F2’yi en yüksek yapan değer olarak seçildi (yaklaşık **0,14956**).

| Recall | Precision | F2 | Average precision |
| ---: | ---: | ---: | ---: |
| %91,71 | %43,97 | 0,754 | 0,634 |

| Gerçek durum | Tahmin: Ayrılmadı | Tahmin: Ayrıldı |
| --- | ---: | ---: |
| Ayrılmadı | 598 | 437 |
| Ayrıldı | 31 | 343 |

Model, ayrılan 374 müşterinin **343’ünü yakaladı** ve **437 yanlış alarm** verdi. Sonuçlar örnek veri kümesindeki performansı gösteriyor. Test ayrımı zaman sıralı olmadığından gelecekteki müşteri kaybını öngörme başarısı bu çalışmayla ölçülmedi.

## Yöntem

1. Veriyi denetle ve sınıf oranını koruyarak %20’sini final test için ayır.
2. Eğitim verisinde keşifsel analiz yap. Ön işlemeyi modelin pipeline’ına ekle.
3. Referans model, lojistik regresyon, karar ağacı ve Random Forest modellerini aynı beş doğrulama katıyla karşılaştır.
4. Seçilen model için eşiği eğitim verisinde belirle ve final testte ölç.

[Notebook](main.ipynb) veri denetimini, grafikleri ve model sonuçlarını içerir. [Kısa raporda](telco_customer_churn_raporu.md) model seçimi ve eşik değişikliğinin etkisi açıklanır.

## Yerelde çalıştırma

Python **3.12.14** ile doğrulandı. Depoyu klonladıktan sonra proje klasöründe bir sanal ortam oluştur ve bağımlılıkları kur.

```sh
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

Windows PowerShell’de sanal ortamı `py -3.12 -m venv .venv` ile oluşturup `.\.venv\Scripts\Activate.ps1` ile etkinleştirebilirsin.

[IBM örnek CSV dosyasını](https://raw.githubusercontent.com/IBM/telco-customer-churn-on-icp4d/d5371f5d83a446ad5673cbcca3b814b926491f8a/data/Telco-Customer-Churn.csv) `Telco-Customer-Churn.csv` adıyla `main.ipynb` ile aynı klasöre kaydet. Kaynak ve veri açıklaması [IBM’in özgün deposunda](https://github.com/IBM/telco-customer-churn-on-icp4d/tree/d5371f5d83a446ad5673cbcca3b814b926491f8a) bulunuyor.

```sh
python -m jupyterlab
```

`main.ipynb` dosyasını aç ve **Restart Kernel and Run All Cells** seçeneğiyle çalıştır.
