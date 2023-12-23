# Oyunlar Feedback Analiz Modeli
İlk denememde sonuçalar biraz düşük olduğu için yeni bir veri setiyle tekrar denedim. O yüzden ilk denememdeki dosyalar İlk Deneme klasöründe ikinci denemem ise ikinci deneme klasörümde.
## Veri Setinin oluşturulması
- Öncelikle steamden seçtiğim popüler oyunların mağaza sayfasından Müşteri İncelemelerinden verileri tek tek kopyalayıp excel'e kayıt ettim. Daha sonra Olumlu ise 1 - Olumsuz ise 0 yazarak etiketledim. + Olabildigince gördüğüm argo kelimeleri sansürlemeye çalıştım.
<img src="pictures/Dataset.png" width =600 height = 300>

## Veri Setini Temizleme
Bende derslerde kullandığımız stop_word dosyasını kullandım. Ve temizlemek için clean_text fonksiyonunu kullandım. Veri setimizi aşağıdaki fotoğraftaki gibi görebiliyoruz.  
<img src="pictures/dataset_v1.png" width =600 height = 600>  
Daha sonra ``` df['clean'] = df['yorumlar'].apply(clean_text) ``` ile bütün yorumalar satırları üzerinde clean_text fonksiyonumuzu uygulayıp clean isimli sütuna kayıt ediyoruz.  
 ``` df.to_excel('temizlenmis_veriler.xlsx', index=False) ``` ile de yeni dosyamızı excel olarak tekrar kayıt ediyoruz.  
Temizlenmiş verileri kontrol etmek için derste kullandığımız fonksiyonu kullanıyorum.   
<img src="pictures/dataset_v2.png" width =600 height = 600>  

## Modelin Oluşturulması

### Bayes Modeli
İlk önce **clean** ve **etiket** sütunlarımızı numpy array olarak dönüştürüyoruz.  
```
X = df.clean.to_numpy()
y = df.etiket.to_numpy()
```
Derste yaptığımız gibi test %20'lik kısmını test setine kayıt ediyoruz. ``` X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42) ```  
Öğretici verilerimizin bulunduğu **X_train**'i veri setimize fit ederken test setimiz olan **X_test** verimizi transform ediyoruz. Daha sonra Bayes modelimizi eğitiyoruz.
```
vectorizer = TfidfVectorizer()

X_train = vectorizer.fit_transform(X_train)
X_test = vectorizer.transform(X_test)

model_NB = MultinomialNB()
model_NB.fit(X_train, y_train)
```
Eğitilmiş Bayes modelimizin direkt **F1 score**'unu aşağıdaki gibi hesaplatarak modelimizin doğruluk oranını öğrenebiliriz.
```
print("NB train accuracy:", model_NB.score(X_train, y_train))
print("NB test accuracy:", model_NB.score(X_test, y_test))

predictions_train = model_NB.predict(X_train)
print("NB Train F1:", f1_score(y_train, predictions_train))

predictions_test = model_NB.predict(X_test)
print("NB Test F1:", f1_score(y_test, predictions_test))
```

### DecisionTree Modeli 

Öğretici modellerimizi fit ettik.
```
model_DT = DecisionTreeClassifier()
model_DT.fit(X_train, y_train)
```
DT modelinde de hazırda bulunan fonksiyonlarla **F1Score0**'u hesaplattık.
```
print("DT train accuracy:", model_DT.score(X_train, y_train))
print("DT test accuracy:", model_DT.score(X_test, y_test))

predictions_train = model_DT.predict(X_train)
print("DT Train F1:", f1_score(y_train, predictions_train))

predictions_test = model_DT.predict(X_test)
print("DT Test F1:", f1_score(y_test, predictions_test))
```

### Grid Search Modeli




## Modelde Test Edilmesi
Derste yaptığımız NB ,Desicion Tree, Grid Search örneklerini kendi veri setime göre düzelterek yazdım.  
### Farklı Modellerdeki Sonuçlar:  
NB Train F1: 0.9428007889546351  
NB Test F1: 0.7401574803149606  
DT Train F1: 0.9979035639412999    
DT Test F1: 0.6666666666666666     
Grid Search Train: 0.7275  
Grid Search Test: 0.55  
Sonuçlar ne yazıkki beklediğimden kötü çıktı. Bunun sebebinin veri setimin kötü ve küçük olmasından kaynaklı olduğunu düşünüyorum.


# Tekrardan
Buradan sonrası tekrar bir veri seti bulup onu temizleyip aynı modellerimde test edilmesini anlatıyor.
## Veri setinin oluşturulması
İnternetten daha büyük bir veri seti bulamaya çalıştım. Ve aşağıdaki linkte türkçe yorumların olduğunu da gördüm.  
[veri setinin linkine buraya tıklayarak ulaşabilirsiniz](https://www.kaggle.com/datasets/sridharstreaks/game-reviews-dataset?select=review_info.csv)   
İlk olarak languages sütunundan turkish olarak işaretlenmiş yorumları ayıklayıp bir excel dosyasına kayıt ettim. 21320 adet Türkçe yorum varmış.  
<img src="pictures/v2.1.png" width =800 height = 600>  
Daha sonra aldığım hata yüzünden review satırını stringe dönüştürüp her satıra clean_text fonksiyonunu uyguladım.  
<img src="pictures/v2.2.png" >  
Daha elle tutulur bir veri seti:  
<img src="pictures/v2.3.png" width =600 height = 600>  

### Verilerin etiketlenmesi
- Verilerin recomended satırında DOĞRU ve YANLIŞ olarak etiketlendiğini gördüm. O yüzden o etiketleri kullanarak yeni bir etiket satırı açıp DOĞRU için 1, YANLIŞ için 0 olarak etiketledim. Daha sonra yazdığım kodun işe yaramadığını gördüm çünkü recommended sütunu boolean olarak True ve False olarak işaretlenmiş. Kodlarımı ona göre değiştim ve etiketleme işlemini de tamamladım.
- ```df['etiket'] = df['recommended'].apply(lambda x: 1 if x == True else 0)```

## Modellerimizdeki Sonuçlar:

NB Train F1: 0.915979466799139  
NB Test F1: 0.91053257565746  
DT Train F1: 0.9967115961406424  
DT Test F1: 0.9028392067341989   
Grid Search Train: 0.8585968738640495  
Grid Search Test:0.8507390356190938  

Sanırım sonuçlar bir önceki veri setime göre daha iyi. Okuduğunuz için teşekkür ederim. :)

