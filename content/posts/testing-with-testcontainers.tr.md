---
title: 'Testcontainers ile Test Etmek'
description: "Bu yazıda neden H2 yerine Testcontainers'ın kullanılması gerektiği ve hangi sorunları çözdüğünü anlatacağım."
summary: 'Her ne kadar H2 gibi araçlarla testler yazabilsek de taklitler gerçeklerin yerini tutmuyor. Testcontainers da tam burada ortaya çıkıyor.'

preview:
  title: 'Testcontainers ile Test Etmek'
  description: 'Her ne kadar H2 gibi araçlarla testler yazabilsek de taklitler gerçeklerin yerini tutmuyor. Testcontainers da tam burada ortaya çıkıyor.'
  image: 'https://miro.medium.com/v2/resize:fit:1400/format:webp/1*_foQAb0bTEbESsdKk8atnA.jpeg'

date: 2022-08-24T12:00:00+03:00
draft: false

tags: ['testing', 'testcontainers', 'integration test', 'acceptance test', 'unit test']
showTags: true

readTime: true
math: false
hideBackToTop: true
---

![Photo by Pixabay](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*_foQAb0bTEbESsdKk8atnA.jpeg#full "[Photo by Pixabay](https://www.pexels.com/photo/astronaut-graffiti-on-semi-trailers-163811/)")

# Giriş

[Bir önceki yazımızda](https://medium.com/emlakjet/unit-test-ve-test-türleri-3659574fec8d) belirttiğimiz üzere bir uygulamayı yazdıktan sonra o uygulamanın özelliklerini test etmek için otomatize edilmiş testler uygulanır. Bu testlerden bazıları unit, integration ve acceptance testlerdir.

Bu testleri uygularken gerekli komponentleri tek tek kurup, test ettikten sonra da silmek oldukça kaynak, zaman ve efor harcayan bir iş olduğundan zaman içinde daha az kaynak gerektiren çözümler gerekmektedir.

# Halihazırda bulunan çözümler niye işe yaramıyor?

Halihazırda bulunan çözümlerden birisi o komponenti taklit eden farklı, daha hafif ve kullanımı daha kolay bir komponent bulup tüm testleri bu komponent üzerinde gerçekleştirmektir. Bu her ne kadar yeterli bir çözüm gibi görünse de büyük projelerde o komponentin her özelliği kullanılması gerektiğinde aslından farklı bir özelliğe illa rastlanacaktır. Bu da testlerin güvenilirliğini azaltacaktır.

Bir örnek vermemiz gerekirse [şurada](https://stackoverflow.com/questions/71268596/h2-postgresql-dialect-not-working-with-using-keyword) göreceğiniz üzere asıl komponente gelen yenilik eşzamanlı olarak test için kullandığımız komponente gelmiyor. Bu da yeni özellikler sayesinde kodumuzda yapacağımız iyileştirmeleri engelliyor. Ya teknolojiyi geriden takip etmek zorunda kalıyorunuz ya da iki ortamda da çalışabilmesi için daha düşük performanslı ya da en iyi başarımı sağlamayan yöntemi tercih etmek zorunda kalıyorsunuz.

# Testcontainers’ın çıkışı

Bunun gibi sıkıntıları fark eden Richard North, Docker’ın da yardımıyla taklit sistemler yerine gerçek sistemlerde testleri çalıştırmamızı sağlayan bir araç geliştirdiler. Bu aracın adı ise Testcontainers.

Testcontainers nedir? Testcontainers, junit ile kullanılan, hafif, kullanılıp atılabilir yapıda bir test platformudur. Kullanım amacınıza göre integration ve acceptance testlerinizi taklit yöntemiyle test etmeye oranla daha güvenilir hale getirir.

Daha güvenilir hale getirmesinin sebebi ise yukarıda belirttiğimiz gibi taklit yönteminin tam olarak gerçek komponentin yerini tutmamasıdır. Test için kullanılan komponent %90 oranında gerçeğine yakın çalışsa da bizim testlerimizin güvenilir olması için aynı şekilde çalışması gerekmektedir.

# Testcontainers’ın çalışma prensibi

Testcontainers temelinde Docker’ı kullanır. Dolayısıyla docker bilgisayarımızda yüklü olmalı. Üzerinde testlerimizi çalıştırmak istediğimiz komponentin docker imajını test edeceğimiz sınıfın içinde tanımlayarak başlatırız ve üzerinde testlerimizi uygularız. Testlerimiz bittiğinde ise varsayılan olarak konteynerimiz kapatılır ve silinir.

# Kullanım detayları

Projemizde Testcontainers kurulumu yapmak için öncelikle projemize Testcontainers kütüphanesini ve Testcontainers’ın resmi olarak destekliyorsa, desteklediği komponentleri eklemeliyiz. Eğer sizin kullanacağınız bir komponentin resmi desteği gözükmüyorsa GenericContainer sayesinde manuel olarak parametre vererek konteynırlarınızı çalıştırabilirsiniz.

Ben projemde Postgresql kullandığım ve resmi olarak desteklediği için onu da bağımlılıklara ekliyorum. Bunun için pom.xml dosyamıza aşağıdaki kodu ekliyoruz. Tabiki en güncel versiyonu kullanmayı unutmayın. Örnek projeyi kaynakça kısmına ekledim.

![Dependencies](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*3HVNETP0GX2i20UIznOStw.png)

Testlerimizi yapmak için bir test sınıfı açmalıyız. Ama ben bunu yapmadan önce örnek olması açısından bir repository oluşturup okuma yazma testini bu repository üzerinden gerçekleştireceğim.

![Repository](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*TkAHAWukIceVi8T_uvWDOw.png)

Repository açtıktan sonra bunun için bir test sınıfı oluşturuyoruz. Testleri önce ayrı bir veritabanına sahip olduğumuz versiyonuyla yapıp daha sonra Testcontainers haline getireceğim. Test ortamında aktif olan veritabanının bilgilerini application.yml üzerinde belirttikten sonra aşağıdaki testi çalıştırdığımız zaman sonucunu göreceğiz.

![Test without Testcontainers](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*lqcpAol02FMhmMIxlW9jeQ.png)

Peki ya test ortamındaki veritabanımız değiştiğinde ne olacak? Örneğin veritabanında olan bir değere göre test yazdığımızda ve birisi gidip o değeri sildiğinde ne olacak? Bu gibi durumlarda örnek verilerimizi kullanarak oluşturduğumuz bir Postgresql konteynırı işimizi fazlasıyla görecektir.

Şimdi bu kodun Testcontainers versiyonuna geçelim. Bir adet veritabanı konteynırı oluşturup bunun bilgilerini bizim ayarlarımızın üstüne yazmalıyız. Tabi bir diğer seçenek olarak bir veritabanı ismi, giriş bilgisi ve kullanıcı bilgilerini bu yeni oluşturacağımız veritabanına verip onunla açılmasını da isteyebiliriz ama ilk bahsettiğim seçenek daha basit oluyor Testcontainers ile.

![Test with Testcontainers](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*s6-iDg-tYYL0ter5cF3gQw.png)

Böylece Testcontainers kullanarak ilk testimizi yazmış olduk! Bu yapıyı biraz daha geliştirip testler arası geçiş yaparken konteynırların kapanıp açılması gibi sorunları giderebiliriz, böylece performans düşüşü sağlamamış oluruz. Ya da örnek veri için bir sql dosyası oluşturup bunu konteynır açıldığında konteynıra yükleyerek içindeki verilerle testlerimizi çalıştırabiliriz.

---
# Kaynakça
- [Unit Test ve Test Türleri](https://medium.com/emlakjet/unit-test-ve-test-türleri-3659574fec8d)
- [Don't use In-Memory Databases (H2, Fongo) for Tests](https://phauer.com/2017/dont-use-in-memory-databases-tests-h2/)
- [Testcontainers](https://www.testcontainers.org/)
- [How to use Testcontainers with integration tests](https://proitcsolution.com.ve/how-to-use-testcontainers-with-integration-tests/)
- [GitHub - remreren/testcontainers-demo](https://github.com/remreren/testcontainers-demo)