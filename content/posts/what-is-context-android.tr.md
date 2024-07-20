---
title: 'Context Nedir? — Onlar her yerde.'
description: 'Bu yazıda Android uygulamalarında sıkça kullanılan Context sınıfının ne olduğunu ve nasıl kullanıldığını anlatacağım.'
summary: 'Context, uygulamamızın bağlamı ve özellikleri hakkında bilgi verir. Uygulamanın tercihleri burada saklanır, uygulama özelindeki kaynaklara ve fonksiyonlara bu arayüz üzerinden erişiriz.'
image: https://miro.medium.com/v2/resize:fit:4800/format:webp/1*ka8-i-vA0oJA6dPvStcsDw.jpeg

date: 2021-04-03T12:00:00+03:00
draft: false

tags: ['android', 'context', 'application', 'activity', 'service']
showTags: true

readTime: true
math: false
hideBackToTop: true
---

![Sam Kolder adlı kişinin Pexels’daki fotoğrafı](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*ka8-i-vA0oJA6dPvStcsDw.jpeg#full "[Sam Kolder adlı kişinin Pexels’daki fotoğrafı](https://www.pexels.com/tr-tr/fotograf/selalelerin-yaninda-duran-uc-adam-2387873/)")

# “Context… O… Her yerde”

Context dediğimiz şey, her yerde önümüze çıkıyor. Gerek uygulamamıza gömdüğümüz **resimleri almak** istediğimizde, gerek **bir arayüz elemanı oluşturmak** istediğimizde, gerekse de **yeni bir aktivite** açacak olduğumuzda bizden context istiyor Android sistemi.

# “Context, Android sisteminin özüdür”

Context, uygulamanın **bağlam** bilgilerini global olarak barındıran bir **soyut sınıftır**. Bu sınıf uygulamalara Android sistemi tarafından sağlanır ve **intent başlatabilmek**, **bazı servisleri çalıştırabilmek**, **kaynaklara erişebilmek** gibi bir çok işlemi yaptırabilir. Context’i bunlara açılan bir kapı veya bunların kullanılmasını sağlayan bir **aracı** olarak düşünebiliriz.

Android sisteminde **neredeyse tüm sınıflar**, dolaylı ya da direkt olarak, bu sınıf üzerine kurulmuştur ve özelliklerinden faydalanır. Context üzerine kurulmuş en çok kullandığımız 3 sınıf şunlardır; **Activity, Application ve Service.**

![Activity-Context Sınıfları Arasındaki İlişki](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*h8xRIQmweLBQt_ktA6U0fw.jpeg)
![Application-Context Sınıfları Arasındaki İlişki](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*5V32sRKOSiFcQOgRZC4DIg.jpeg)
![Service-Context Sınıfları Arasındaki İlişki](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*15LQI6yqyhFHKnPFEDMn2A.jpeg)

# Peki ya Context’i nerelerde/nasıl kullanıyoruz?

Context sınıfı birçok fonksiyonu hazır bulunduruyor. Bir resmi çekmek istediğimiz zaman eğer Context’e uzanan bir sınıf içerisindeysek (örn; Activity, Application, Service vs.) direkt olarak `getString(int)` ya da `getDrawable(int)` yapmamız yeterli oluyor fakat Context sınıfına uzanmayan bir sınıf içerisindeysek (örn; Adapter, ViewHolder vs.) bir Context objesi bulup onun üzerinden `context.getString(int)` ya da `context.getDrawable(int)` şeklinde işlem yapmamız gerekiyor. Nasıl kullandığımız fark etmeden bunun gösterimini şu şekilde yapacağım `Context#getString(int)` . Benim çoğunlukla kullandığım 3 yer ise şu şekilde;

## 1- Bir intent başlatırken

Yukarıda bahsettiğim üzere bulunduğumuz yere göre `Context#startActivity(Intent)` şeklinde kullanabiliyoruz. Bu komut yeni bir **Activity başlatıyor** ve Activity yığınının en tepesine ekliyor.*

## 2- Bir kaynak kullanmak istediğimizde

Uygulamamıza gömdüğümüz bir **dizi ya da resmi** çekmek istediğimizde direkt olarak `Context#getString(int)` ya da `Context#getDrawable(int)` şeklinde çekebiliyoruz.

## 3- Bir sistem servisine ulaşmak istediğimizde

Örneğin bildirim oluşturmak istiyoruz, bildirim oluşturmak için **Android sisteminin sağladığı bir servisi** kullanmamız gerekiyor. Bu servise Context üzerinden ulaşmamız gerekiyor. Bunu da şu şekilde kullanıyoruz `Context#getSystemService(Class)`. Bildirim oluşturmak sadece bir örnek tabii ki. Daha fazla servisi keşfetmek isterseniz [buraya tıklayabilirsiniz.](https://developer.android.com/reference/android/content/Context#getSystemService(java.lang.Class%3CT%3E))

# Kullandığım pratikler neler?

Diğer geliştiricilerden aman aman farklı bir şey yapmıyorum aslında. Bazı değişik yaptığımı düşündüğüm şeyler şunlar;

İlk olarak gereksiz Context parametrelerinden bahsedeyim. RecyclerView içinde `Adapter#onCreateViewHolder(ViewGroup, int)` fonksiyonu içinde bir arayüz elemanı oluştururken **ilk parametre ile bize verilen parent içindeki Context**’i kullanıyorum Adapter oluştururken yeniden gönderip boşuna hafıza kullanmak yerine. ViewHolder içinde ise yine bize verilen **view içindeki Context**’i kullanıyorum herhangi bir işlem yapacak olduğumda. Böylece tüm Adapter yapılarıma her seferinde Context vermek zorunda kalmıyorum.

İkinci ve son olarak da kodumuzun daha güzel gözükmesi için bir öneri. Context altındaki bazı fonksiyonlar kullanınca kodun okuması zorlaşabiliyor. Bunu engellemek için kullanabildiğim kadar **ContextCompat** kütüphanesini kullanmaya çalışıyorum. Örneğin `mContext.getColor(int)` yazmak yerine `ContextCompat.getColor(Context, int)` şeklinde yazıyorum. Böylece kod çok daha okunaklı gözüküyor.

---
Daha fazla oku:
* [Android — Context ve Application Kavramları](https://medium.com/@tugcekolcu/android-context-ve-application-kavramlar%C4%B1-7f33d0b0bcc6)
* [Context - Android Developers](https://developer.android.com/reference/android/content/Context)