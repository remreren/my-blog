---
title: 'Monolitik Mimari Mikroservis Mimariye Karşı | Mimari Desenler — 0'
description: 'Bu yazıda monolitik ve mikroservis mimarilerini karşılaştıracağız.'
summary: 'Monolitik sistemler, geliştirmesi en kolay sistemler olmasına karşın proje boyutu büyüdükçe mikroservislere ihtiyaç duyulmaktadır.'
image: https://miro.medium.com/v2/resize:fit:1400/format:webp/1*Ki5t2NcGodzmgPKTdSAAFg.jpeg

date: 2022-05-02T18:00:00+03:00
draft: false

tags: ['monolith', 'microservices', 'architecture']
showTags: true

readTime: true
math: false
hideBackToTop: true
# toc: true
# autonumber: true
# hidePagination: true
---
![Photo by Bernd Feurich](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*Ki5t2NcGodzmgPKTdSAAFg.jpeg#full "[Photo by Bernd Feurich](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*Ki5t2NcGodzmgPKTdSAAFg.jpeg#full)")

Konuya girmeden önce konuyu daha iyi kavrayabilmeniz için üzerinden gideceğim kütüphane örneğini açıklayayım. Kütüphane uygulamamızın kapsamında 4 adet servis bulunuyor. Kitap servisi, yazar servisi, kullanıcı servisi ve ödünç alma servisi. Bu uygulamanın kullanıcı tabanının geniş olduğunu ve kitap servisinin diğerlerine oranla daha fazla, ödünç alma servisinin ise daha az kullanıldığını farz edelim.

Bir konuyu derinlemesine anlamak istiyorsak, en derinden başlayarak incelememiz gerekir. Bu yüzden yazıya monolit kelimesinin anlamından başlayalım. Monolit latincede “tek” ve “taş” kelimelerinin birleşmesinden oluşmaktadır. Monolit kelimesi, kaya gibi sağlam ve yekpare bir paket olarak sunulmasından dolayı seçilmiştir.

![Simple Layered Architecture](https://miro.medium.com/v2/resize:fit:648/format:webp/1*Mtl0XwjaMRWfdZGW5haHvQ.png#small)

Yukarıda da bahsettiğim üzere monolitik mimariyi tüm servislerin bir arada bulunduğu uygulamalar olarak düşünebiliriz. Monolitik mimariye sahip sistemlerde; kullanıcı arayüzü servisi, kitap servisi, yazar servisi gibi servislerin hepsini tek bir paket içerisinde toplar ve sunarız.

![Detailed Layered Architecture](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*duR0FtRi20IC6X-xuAS0IQ.png#small)

Monolitik sistemler, geliştirmesi en kolay sistemlerdir. Günümüzde, sahip olduğumuz kütüphaneler ve yazılım iskeletlerinin de yardımıyla oldukça hızlı bir şekilde uygulama çıkartabiliriz. Örneğin, projesi hazırlanmış bir uygulamanın ilk versiyonunu, monolitik bir mimari kullanarak, 1 hafta gibi kısa bir süre içerisinde çıkarıp kullanıma sunabiliriz.

Monolitik sistemlerin bir diğer kolaylığı ise test edilmeleridir. Monolitik servisler tek paket halinde çalıştığından dolayı, mikroservis mimarisinin aksine, servisler arası iletişimi test etmek zorunda kalmıyoruz.

Monolitik sistemlerin bu yazıdaki son avantajı ise ölçeklendirme kolaylığıdır. Bir uygulama yazıp bunu birden fazla bilgisayara kurduğunuz takdirde sisteminizi yatayda ölçeklenmiş kabul edebilirsiniz. Her ne kadar ileride veritabanından dolayı bir darboğaz yaşayacak olsanız da kısa vadede gayet hızlı biçimde ölçeklemiş oluyorsunuz.

![Scaling-up Concept on Layered Architecture](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*1L0R6y0xujdTg3cfqBBWsw.png#small)

Monolitik sistemler her ne kadar birden çok avantaj sunsa da büyük ölçekte düşündüğümüzde bir o kadar da dezavantajı bulunuyor.

Bu dezavantajların başını ileri düzey uygulamalarda/büyük ekiplerde uygulamayı geliştirme sıkıntısı çekiyor. Örneğin uygulamanız büyüdü ve dolayısıyla sisteminizdeki özellikleri de artırmaya çalışıyorsunuz ya da halihazırda bulunan takımınız artık geliştirme hızınıza yetişemiyor. Böyle bir durumda yeni çalışan almak iyi bir fikir olabilir. Eğer monolitik bir mimariye sahipseniz her şeyin tek uygulama, yani tek kod tabanı, üzerinde çalışılması gerektiği için küçük ekiplere efektif şekilde bölünemezsiniz. Dolayısıyla yeni bir çalışan almak sorununuzu çözmeyebilir, hatta daha da karmaşık hale getirebilir.

![No Asynchronous Development](https://miro.medium.com/v2/resize:fit:1352/format:webp/1*wZN8b-nNrl0HLE2aG7rK7A.png#small)

Monolitik sistemlerde tüm birimler birbirine sıkı şekilde bağlı olarak yazıldığı için büyük ekiplerde bu yapının geliştirilmesinde problemler mutlaka yaşanacaktır.

Bu da yetmezmiş gibi uygulamamızda tek serviste hata oluşması durumunda tüm uygulamanın çökmesi büyük bir problem. Uygulamanın başlangıç süresi kısa olsa dahi böyle bir sorunla karşılaşmak size büyük kayıplar yaşatacaktır.

Birden fazla servisimiz olduğunda bunların kullanımına göre genişletmek isteyebiliriz. Yazının başındaki örnekten ilerleyelim, kitap servisi diğerlerinden daha fazla kullanılırken ödünç alma servisi diğerlerine oranla çok daha az kullanılıyor. Monolitik mimari kullanan bir sistemde kitap servisi ve ödünç alma servisi tek uygulama üzerinde bulunduğundan dolayı bu servisleri ayrı ayrı ölçekleme yapamıyoruz. Bu şekilde gereksiz bir maliyetle karşılaşabiliyoruz.

![Scaling-up Monolithic Architecture](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*ZxAAsb7k2OTtPtn334v_EQ.png#small)

Monolitik yapının bir diğer dezavantajı ise dağıtımda yaşadığımız sıkıntılar. Yine uygulamamızın tek bir paket içinde olmasından dolayı güncelleme işlemleri sırasında uygulamanın tamamen kapalı kalması gerekiyor. Dolayısıyla bu durum kullanıcı deneyimini olumsuz etkileyebilir.

Monolitik sistemlerde gördüğümüz üzere sistemler büyüdükçe sorunları da büyür. Bu sorunlarla baş etmek için büyüyen sistemleri daha küçük parçalara ayırmamız gerekir. Bir sistemi daha kolay yönetilebilen parçalara ayırıp senkron işleri asenkron hale getirebilirseniz, o sistemin daha hızlı işlediğini, yapılan işin ise daha hızlı ilerlediğini fark edersiniz.

![Divide and Conquer](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*YMVltBR2GN5Bm_nrjEDfgw.png#small)

Bir başka örnek vermem gerekirse, insanlar tek başına güçsüzdür. Daha sonra beraber güçlü olduklarını fark ederek çıkarları doğrultusunda gruplaşırlar. Grup büyüdükçe iç çatışmalar artar, yönetim zorlaşır, insanların çıkarları birbiriyle çatışmaya başlar. Bunun üzerine gruplar, iç gruplaşmaya doğru gider. Daha küçük gruplara bölündükten sonra daha hızlı çalışmaya ve daha kolay yönetilmeye başlarlar. Buna bilgisayar bilimlerinde böl ve fethet tekniği denir. Bir problemi daha kolay çözülebilir alt problemlere ayırdığımız zaman işi çözme kolaylığı ve hızı artar.

İşte mikroservisler de tam burada devreye giriyorlar. Büyük, tek parça ve yönetimi zor olan servisleri küçük, kendi içinde otonom parçalara bölerek mikro-servisler oluşturup bunları daha kolay yönetebiliyoruz.

![Scaling-up Concept on Microservices Architecture](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*W-75wQjIhIAyeJ4dWtKC8w.png#small)

Büyük sistemleri mikroservislere bölmek için çeşitli metodlar kullanılır. Biz yukarıdaki örneğimizde benzer veriler üzerinde çalışan servisleri bir araya toplamaya karar verdik. Kitap mikroservisinde, kitapları listeleme, kitap kaydetme, kitabı yazarıyla birlikte gösterme gibi özellikler bir arada bulunurken yazar mikroservisinde ise yazar arama, yazar kaydetme, yazarları listeleme gibi özellikler bir arada bulunuyor.

Örneğin; kitabı yazarıyla birlikte gösterme servisini çağırdığımız zaman, yazarı, kitap mikroservisi içinde elde edemiyoruz. Kitap mikroservisi içinde yazarı elde etmek istediğimiz zaman kod tekrarına düşüp üzerine bir de mikroservis sınırları dışına çıktığımız için bu istenen bir durum değildir. Bu yüzden yazarı, yazar mikroservisinden istememiz gerekir.

![Domain Driven Design (DDD) Sample](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*W4C_5ad3vglIlMbZaIeybA.png#small)

Gördüğünüz üzere bu gibi durumlarda ortaya mikroservislerin birbiriyle haberleşmesi konsepti çıkıyor. İki mikroservis birbiriyle nasıl haberleşir? Tabiki de internet üzerinden! Haberleşme için çeşitli protokoller kullanılabilir ama bunların en popüler ikisi RESTful ve gRPC protokolleridir. İşte iki mikroservis bu protokoller aracılığıyla ihtiyaç duydukları verileri takas ederler.

![Communication Between Microservices (RESTful)](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*c06U9-YWExkTbdBY8cukZA.png#small)

Mikroservis yapısının getirdiği yenilikleri az çok bitirdiğimize göre gelelim mikroservislerin avantajlarına. İlk avantajı, mikroservislerin birbiriyle tamamen bağımsız olması. Birbirinden bağımsız servisler, tek başına çalışabilir ve otonomluk sağlar. Bir mikroserviste oluşan hata diğer mikroservisleri bağlamaz. Bir mikroservisi başlatmak ya da güncellemek istediğimizde tüm uygulamayı durdurup güncellemek zorunda kalmayız, tek bir mikroservisi paketleyip dağıtıma çıkarabiliriz.

Mikroservislerin birbirinden bağımsız olmasının getirdiği bir diğer özellik ise her mikroservisin küçük bir ekip tarafından sahiplenilebilir olmaları. Eğer mikroservis altyapısı kullanıyorsanız ve çok büyük bir ekiple çalışıyorsanız bu ekibi küçük parçalara bölerek birbirinden tamamen bağımsız çalıştırabilirsiniz. Bu yaklaşım proje üzerinde asenkron çalışma özelliği kazandırıp projeyi oldukça hızlandırabilir. Bu proje geliştirme açısından en önemli özelliklerden biridir.

![Sample Release Diagram](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*tNMO0HNtsIkB275dTwt8Ug.png#small)

Mikroservis yapısının bir diğer avantajı ise servis bazında ölçeklenebilmesi. Örneğin elimizde 3 mikroservisli bir proje var. Bu mikroservislerden 1 tanesi diğerlerine oranla çok daha fazla kullanılıyor. Monolitik mimaride uygulamanın bir kopyasını yeniden oluşturup kaynak israfı yapmak yerine en çok kullanılan mikroservisin diğerlerine göre daha fazla kopyasını çıkararak hem kaynak israfını önleyip hem de gereğinden fazla genişletmemiş oluyoruz.

![Scaling-up Microservice Architecture](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*777FOE_5BCD8gcXwAHmrIA.png#small)

Mikroservislerin bir diğer avantajı, platformdan bağımsız bir haberleşme protokolü kullanılırsa, her mikroservis ayrı bir dilde yazılabilir. Örneğin bir mikroservisi yazarken x framework’ü daha iyi performans verirken bir diğer mikroserviste y framework’ü daha iyi performans verir. Bu iki ayrı mikroservis, kendi içinde hangi dilin ya da yazılım iskeletinin kullandığından bağımsız bir şekilde haberleşme özgürlüğüne sahip olur.

![Platform Independent Communication](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*2rKUET_OILGTge19sBI0kQ.png#small)

Mikroservislerin monolitik servislerle kıyaslandığı vakit bir sürü problemi çözdüğünü görsek de mikroservis yapısı bir sürü sorunu da beraberinde getiriyor. Bunlardan ilki ve en önemlisi öğrenme eğrisi. İlk adımı atarken karşılaşacağınız bir sürü sorun var yani sistemi ilk kurmaya çalıştığınız zaman bir sürü sorunla karşılaşacaksınız. Ama sistemi kurmayı başardığınız zaman iş gerçek anlamda çocuk oyuncağına dönüşüyor.

Mikroservis mimarisini kullanan sistemlerin bir diğer sorunu ise konteynerlerin yönetimi ve sağlık takibidir. Elimizde küçük küçük bir sürü mikroservis örneği bulunduğu için bunların yönetimi elle yapılamaz. Konteynerlerin yönetimi sorununu çözdükten sonra önümüze bunların sağlıklarının takip edilmesi sorunu çıkıyor. Bir mikroservis örneği bile düzgün çalışmıyorsa bizim sistemimiz tam verimli çalışıyor diyemeyiz ve dolayısıyla bize maddi kayıp yaşatır.

Mikroservisler hakkında konuşacağımız üçüncü sorun ise ağ gecikmeleri. Normalde tek bir bilgisayar içinde yaptığımız haberleşmeyi mikroservis mimarisine geçtiğimizde bilgisayarlar/konteynerler arası yapmak zorunda kalıyoruz. Dolayısıyla bir bilgisayardan diğerine giderken verinin paketlenmesinde ve iletilmesinde oluşan kayıplar bize ağ gecikmesi olarak yansıyor.

Gördüğünüz üzere her iki mimarinin de iyi ve kötü olduğu alanlar mevcut. Örneğin monolitik mimari küçük projeler ve ekiplerde oldukça kullanışlıyken ekip ve proje büyüdükçe mikroservislerin avantajları artmaya başlıyor. Kullandığınız mimari ister monolitik olsun ister mikroservis olsun kaliteli bir yapı kurmuşsanız ekibinizi paralel biçimde çalıştırıp iyi bir verim alabilirsiniz ama eğer kullanıcı tabanınız genişlemişse ve artık ekibinizin verimi azalmışsa mikroservis mimarisine geçmeniz iyi bir tercih olabilir.

![Microservice Architecture vs Monolithic Architecture](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*YlItsOsZ5PG0AqOb_tfKpg.png#small)

---
Daha fazla oku:
*   [Monolithic application — Wikipedia](https://en.wikipedia.org/wiki/Monolithic_application)
*   [Monolithic Architecture pattern (microservices.io)](https://microservices.io/patterns/monolithic.html)
*   [Layered Architecture | Baeldung on Computer Science](https://www.baeldung.com/cs/layered-architecture)
*   [Layered Architecture](https://web.archive.org/web/20220129090955/https://cs.uwaterloo.ca/~m2nagapp/courses/CS446/1195/Arch_Design_Activity/Layered.pdf)
*   [Monolithic vs. Microservices Architecture | by Anton Kharenko | Microservices Practitioner Articles](https://articles.microservices.com/monolithic-vs-microservices-architecture-5c4848858f59)
*   [Introduction to Software Architecture (Monolithic vs. Layered vs. Microservices) — DEV Community](https://dev.to/zachgoll/introduction-to-software-architecture-monolithic-vs-layered-vs-microservices-452)
*   [Mikroservis Mimarisi Nedir? Monolitik Mimari Nedir? Microservice vs Monolithic | Kampüs Kod (kampuskod.com)](https://www.kampuskod.com/yazilim/mikroservis-mimarisi-nedir-monolitik-mimari-nedir-microservice-vs-monolithic/)
*   [Microservices vs Monolith: which architecture is the best choice? (n-ix.com)](https://www.n-ix.com/microservices-vs-monolith-which-architecture-best-choice-your-business/)
*   [Microservices Nedir ?. Bu yazımda Microservices Nedir… | by Onur Dayıbaşı | Architectural Patterns | Medium](https://medium.com/architectural-patterns/microservice-nedir-73bdfddad197)
*   [Mikroservis Mimarisi Nedir ve Avantajları Nelerdir | by Furkan BEĞEN | Medium](https://medium.com/@furkanbegen/mikroservis-mimarisi-nedir-ve-avantajlar%C4%B1-nelerdir-1369175cc4e6)
*   [Mikroservis Yaklaşımında Servisler Arası İletişim Mimarileri | Devnot](https://devnot.com/2020/mikroservis-yaklasiminda-servisler-arasi-iletisim-mimarileri/)
*   [Kubernetes ile Kesintisiz Deployment ve Uygulama Güncelleme — Levitate (optdcom.net)](https://www.optdcom.net/levitate/tr/kubernetes-ile-kesintisiz-deployment-ve-uygulama-guncelleme/)
*   [Cultural Elements of Microservices: Service Ownership | by Irakli Nadareishvili | Capital One Tech | Medium](https://medium.com/capital-one-tech/cultural-elements-of-microservices-service-ownership-7ff3acea7d92)
*   [Microservices vs SOA: What’s the Difference? | 3Pillar Global](https://www.3pillarglobal.com/insights/microservices-vs-soa-whats-the-difference/)
