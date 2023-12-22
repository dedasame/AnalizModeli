# Oyun Feedback Analiz Modeli

## Veri Setinin oluşturulması
- Öncelikle steamden seçtiğim popüler oyunların mağaza sayfasından Müşteri İncelemelerinden verileri tek tek kopyalayıp excel'e kayıt ettim. (Toplam 500 adet verimiz var.)
- Daha sonra Olumlu ise 1 - Olumsuz ise 0 yazarak etiketledim. + Olabildigince gördüğüm argo kelimeleri sansürlemeye çalıştım.
<img src="pictures/Dataset.png" width =600 height = 300>
## Veri Setinin Temizlenmesi
#### CleanDataSet.ipynb dosyasından kodlara ulaşabilirsiniz.

- Bende derste kullandığımız stop_word dosyasını kullandım. Ve temizlemek için clean_text fonksiyonunu kullandım.
- Veri setimizi aşağıdaki fotoğraftaki gibi görebiliyoruz.
<img src="pictures/dataset_v1.png" width =600 height = 300>

- Daha sonra ``` df['clean'] = df['yorumlar'].apply(clean_text) ``` ile bütün yorumalar satırları üzerinde clean_text fonksiyonumuzu uygulayıp clean isimli sütuna kayıt ediyoruz.
- ``` df.to_excel('temizlenmis_veriler.xlsx', index=False) ``` ile de yeni dosyamızı excel olarak tekrar kayıt ediyoruz.
- Temizlenmiş verileri kontrol etmek için derste kullandığımız fonksiyonu kullanıyorum.
<img src="pictures/dataset_v2.png" width =600 height = 300>

## Modelin Oluşturulması

- NB Test F1 Score: 0.7401574803149606
- DT Test F1 Score: 0.6346153846153846
- Grid Search: 0.54
- Sonuçlar ne yazıkki beklediğimden kötü çıktı. Bunun sebebinin veri setimin kötü ve az olmasından kaynaklı olduğunu düşünüyorum.

- Aşağıdaki linkten turkce yorumlari cektim.
- https://www.kaggle.com/datasets/sridharstreaks/game-reviews-dataset?select=review_info.csv

