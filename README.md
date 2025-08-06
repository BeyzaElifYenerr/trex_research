# trex_research
.NET Backend Geliştirme – Temel Bilgi ve Kavramlar

# 1. Modern Yazılım Geliştirme Pratikleri 
## Git ve GitHub nedir?
##### Git:
- Yazılım geliştirme sürecinde kullanılan sürüm kontrol sistemidir.
- Yazılan kodlar versiyonlanabilir, önceki sürümlere dönebilir, ekip halinde aynı projede çalışılabilr ve yapılan değişiklikler izlenebilir.
- Proje geçmişi kopyası tutma olanağı sağlanır. Böylelikle çevrimdışı da çalışma imkanı sağlanır.
##### GitHub:
- Git ile yönetilen projeleri bulut ortamında barındırır.
- Proje yönetimi, kod inceleme, otomatik test ve dağıtım yapılabilir.
- Kodlar yedeklenebilir, takım arkadaşlarıyla eş zamanlı çalışılabilir ve toplulukla paylaşılabilir.
## Temel Git Komutları
##### init:
- Yeni bir depo başlatmak için kullanılır.
- Lokal alanda çalışılan projelere start vermek için kullanılır.
- .git uzantılı bir depo açar ve lokal dosyalar buraya kayıt edilir.
##### add:
- Projeyi ya da proje içindeki bir dosyayı depo alanına ekler.
- Tek tek dosyalar aşamalandırılabilir ve birden fazla dosya tek seferde eklenebilir.
##### commit:
- Herhangi bir dosyayı sürüm geçmişine kalıcı olarak kayıt eder.
- Dosyalar son halini alır ve veri tabanına işlenir.
##### push:
- Commitleri yani işlem demetlerini uzak depo alanına aktarır.
##### pull:
- Uzak sunucuda yapılan değişiklikleri çalışma dizinine getirir ve birleştirir.
##### branch:
- Depodaki tüm yerel dalları, sınıfları ve bölümleri listeler.
- Önüne -d ek komutu yazıldığında oluşturulan sınıf silinir.
##### merge:
- Belirtilen uzantıyı veya dalı başka bir uzantıyla birleştirir.
## Merge Conflict Nedir?
Yazılım geliştirme sürecinde iki farklı branchte yapılan değişikliklerin aynı dosyanın aynı satırlarında veya birbirini etkileyen bölgelerinde çakışması durumunda oluşur.
##### Nasıl Çözülür?
- Çatışma fark edilir. Git merge veya Git pull yapıldığında Git “merge conflict” uyarısı verir.
- Çakışmalı dosya açılır. Dosyayı açıldığında iki farklı versiyonun karşılaştırıldığı bir ekran görülür.
- Hangi değişikliklerin korunulacağına karar verilir.
- Çatışma işaretleri kaldırılır.
- Dosya kaydedilir ve Git add ile çatışma çözülür.
## CI/CD Nedir?
Yazılım geliştirme sürecini otomatikleştiren ve yazılım kalitesini artıran iki temel kavramdır.
##### CI:
- Geliştiriciler kodlarını merkezi bir repoya sık sık entegre eder.
- Her entegrasyonda otomatik olarak testler çalıştırılır ve projenin bozulup bozulmadığı kontrol edilir.
- Böylece hatalar erken tespit edilir.
##### CD:
- Testleri geçen kodlar otomatik olarak test ortamına veya doğrudan canlıya (production) aktarılır.
- Bu sayede yeni özellikler daha hızlı ve güvenli bir şekilde son kullanıcılara ulaşır.
##### .NET Projesinde CI/CD Süreci
Bir .NET (ASP.NET Core) projesi için tipik CI/CD pipeline adımları:
1. Kaynak kodun çekilmesi (checkout)
2. .NET SDK kurulumu
3. Bağımlılıkların restore edilmesi (dotnet restore)
4. Derleme (dotnet build)
5. Testlerin çalıştırılması (dotnet test)
6. Publish paketinin oluşturulması (dotnet publish)
7. Ortam (dev, staging, production) deploy işlemleri
##### Azure DevOps ile Pipeline Örneği:
``` 
trigger:
  - main

pool:
  vmImage: 'windows-latest'

steps:
  - task: UseDotNet@2
    inputs:
      packageType: 'sdk'
      version: '8.0.x'

  - script: dotnet restore
    displayName: 'Restore dependencies'

  - script: dotnet build --configuration Release
    displayName: 'Build project'

  - script: dotnet test --no-build --verbosity normal
    displayName: 'Run tests'

  - script: dotnet publish -c Release -o $(Build.ArtifactStagingDirectory)
    displayName: 'Publish project'

  - task: PublishBuildArtifacts@1
    inputs:
      pathToPublish: '$(Build.ArtifactStagingDirectory)'
      artifactName: 'drop'
```
##### GitHub Actions ile Pipeline Örneği:
```
name: .NET CI/CD

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: windows-latest

    steps:
      - uses: actions/checkout@v3

      - name: Setup .NET
        uses: actions/setup-dotnet@v3
        with:
          dotnet-version: '8.0.x'

      - name: Restore dependencies
        run: dotnet restore

      - name: Build
        run: dotnet build --configuration Release --no-restore

      - name: Test
        run: dotnet test --no-build --verbosity normal

      - name: Publish
        run: dotnet publish -c Release -o ./publish
```
## Software Development Life Cycle (SDLC) 
Bir yazılımın fikir aşamasından kullanıma sunulmasına ve bakımına kadar geçen süreci planlayan yapısal bir yöntemdir.
1. Planlama:
- Projenin amacı, kapsamı, bütçesi ve takvimi belirlenir.
- Risk analizi yapılır.
- Kaynaklar (ekip, araçlar, teknoloji) planlanır.
2. Analiz:
- Müşteri gereksinimleri toplanır ve belgelenir.
- İşlevsel (functional) ve işlevsel olmayan (non-functional) gereksinimler belirlenir.
- Gereksinim dokümanları (SRS – Software Requirement Specification) hazırlanır.
3. Geliştirme:
- Kodlama süreci başlar.
- Belirlenen standartlara göre modüller geliştirilir.
- Sürüm kontrol sistemleri (Git vb.) kullanılır.
4. Test:
- Yazılımın hatasız çalışıp çalışmadığı kontrol edilir.
- Birim testi, entegrasyon testi, sistem testi, kullanıcı kabul testi (UAT) yapılır.
- Hatalar düzeltilir.
5. Dağıtım:
- Yazılım testlerden geçtikten sonra canlı (production) ortama alınır.
- Müşteri kullanmaya başlar.
6. Bakım:
- Hata düzeltmeleri, performans iyileştirmeleri yapılır.
- Yeni özellikler eklenir.
##### Agile Nedir?:
Agile (Çevik), yazılım geliştirmede esneklik, hızlı geri bildirim ve sürekli iyileştirme odaklı bir yaklaşımın genel adıdır.
Temel prensipler:
- Müşteri ile sürekli iletişim
- Küçük, sık teslimatlar
- Değişen gereksinimlere hızlı uyum
- Ekip içi iş birliği
##### Scrum Nedir?
Scrum, Agile yaklaşımının en çok kullanılan uygulama çerçevesidir. Belirli roller, seremoniler ve artefaktlar içerir.
Roller:
- Product Owner → Gereksinimleri belirler, önceliklendirir.
- Scrum Master → Sürecin doğru işlemesini sağlar, engelleri kaldırır.
- Development Team → Yazılımı geliştirir.
Seremoniler:
- Sprint Planning (Sprint planlama)
- Daily Scrum (Günlük 15 dk toplantı)
- Sprint Review (Sprint sonunda gösterim)
- Sprint Retrospective (Süreç iyileştirme toplantısı)
Artefaktlar:
- Product Backlog
- Sprint Backlog
- Increment (teslim edilebilir ürün)
##### Kanban Nedir?
Kanban, görsel görev yönetimi yöntemidir. Süreç akışını optimize eder, belirli zaman kutularına (sprint) bağlı kalmaz.
Temel özellikler:
- Kanban panosu kullanılır:
 - To Do → Yapılacak işler
 - In Progress → Devam eden işler
 - Done → Tamamlanan işler
- WIP Limit (Work in Progress): Aynı anda devam eden iş sayısı sınırlanır.
- Sürekli akış vardır, teslimatlar istediğin zaman yapılabilir.
# 2. .NET Ekosistemi 
## .Net Nedir?
- Microsoft tarafından geliştirilen, farklı türde uygulama oluşturmayı sağlayan, ücretsiz ve açık kaynaklı bir yazılım geliştirme platformudur.
- Geliştiricilere farklı cihaz ve işletim sistemlerinde çalışabilecek modern, güvenli ve yüksek performanslı uygulamalar geliştirme imkânı sunmaktır.
- Masaüstü, web, mobil, bulut, IoT ve oyun geliştirmede kullanılabilir.
- Birden fazla programlama dilini, kütüphaneyi ve kitaplığı içerir.
- Modüler bir mimariye sahiptir.
- .Net mimarisi 3 temel katmandan oluşur. Bunlar dil, kütüphane ve çalışma zamanıdır.

| Özellik / Sürüm            | **.NET Framework** | **.NET Core** | **.NET 7/8+** *(Modern .NET)* |
|---------------------------|--------------------|--------------|-----------------------------|
| **Çıkış Yılı**            | 2002               | 2016         | .NET 5 → 2020<br>.NET 8 → 2023 |
| **Platform Desteği**      | Sadece **Windows** | **Cross-platform** (Windows, Linux, macOS) | **Tam cross-platform** (Windows, Linux, macOS, Android, iOS, WebAssembly) |
| **Açık Kaynak**           | ❌ Hayır           | ✅ Evet      | ✅ Evet |
| **Performans**            | Orta               | Yüksek       | Çok yüksek *(JIT optimizasyonları, NativeAOT vb.)* |
| **Geliştirme Dilleri**    | C#, VB.NET, F#     | C#, F#       | C#, F# *(VB.NET sınırlı)* |
| **Kullanım Alanı**        | Eski Windows uygulamaları (WinForms, WPF, ASP.NET) | Modern web, API, konsol, cross-platform uygulamalar | Tüm platformlar için tek kod tabanı *(web, mobil, masaüstü, IoT, bulut)* |
| **Destek Durumu**         | Sadece bakım modu *(yeni özellik yok)* | Destek bitişi: 2022 | Aktif geliştirme (LTS sürümleri: .NET 6, .NET 8) |
| **NuGet Desteği**         | ✅ Evet            | ✅ Evet      | ✅ Evet |
| **Dağıtım**               | Windows Installer  | Self-contained veya framework-dependent | Self-contained, framework-dependent, native |
| **Web Geliştirme**        | ASP.NET            | ASP.NET Core | ASP.NET Core (güncel sürüm) |
| **Masaüstü UI**           | WinForms, WPF      | WinForms, WPF *(Windows’ta)* | WinForms, WPF, **.NET MAUI** |
| **Mobil Geliştirme**      | ❌ Yok             | Xamarin      | **.NET MAUI** (tek kod tabanı ile Android/iOS) |

##### .Net --info Çıktısı Örneği:
```
.NET SDK:
 Version:   8.0.100
 Commit:    123abc456

Runtime Environment:
 OS Name:     Windows
 OS Version:  10.0.19045
 OS Platform: Windows
 RID:         win10-x64
 Base Path:   C:\Program Files\dotnet\sdk\8.0.100\

Host:
  Version:      8.0.0
  Architecture: x64
  Commit:       789xyz123
```
- .NET SDK: Yüklü SDK sürümünü gösterir (geliştirme için kullanılır).
- Runtime Environment: OS bilgisi, işlemci mimarisi ve RID (Runtime Identifier) gösterilir.
- Host: .NET’in çalıştırıcı (runtime) sürümünü ve mimarisini gösterir.
##### Senkron Programlama:
- İşlemler ardışık olarak çalışır.
- Bir işlem bitmeden diğeri başlamaz.
- Uzun süren işlemler (ör. dosya okuma, API çağrısı) sırasında uygulama bekler ve kullanıcı arayüzü (UI) donar.
##### Asenkron Programlama:
- Uzun süren işlemler başka bir iş parçacığında veya non-blocking şekilde çalıştırılır.
- İşlem bitene kadar uygulama diğer işlere devam edebilir.
- UI donmaz, kullanıcı deneyimi iyileşir.
##### .NET’te Asenkron Programlamanın Temel Kavramları
 ##### Task:
  - Asenkron bir işlemi temsil eder. Task (void yerine) veya Task<T> (geriye değer döndüren) olarak kullanılır.
 ##### async:
  - Bir metodun asenkron çalışacağını belirtir.
 ##### await:
  - Asenkron işlemin tamamlanmasını bekler, ancak UI’ı veya ana thread’i kilitlemez.
 ##### ConfigureAwait(bool):
  - Await sonrası kaldığın thread’e dönüp dönmeyeceğini kontrol eder. ConfigureAwait(false) genelde UI dışı kodlarda kullanılır (performans için).
##### Senkron Örnek:
```
public void GetData()
{
    var data = GetFromApi(); // Bu işlem bitene kadar bekler
    Console.WriteLine(data);
}
```
##### Asenkron Programlama:
```
public async Task GetDataAsync()
{
    var data = await GetFromApiAsync(); // Bekler ama UI thread’i kilitlemez
    Console.WriteLine(data);
}
```
##### Arrow Function (Lambda) Kullanımı (=>)
- Lambda expression olarak bilinir ve özellikle kısa fonksiyonlar tanımlar.
- LINQ sorguları ve event handler’larda yaygındır.
# 3. Backend Geliştirme Temelleri
## Back-end Nedir?
- Bir web sitesinin veya uygulamanın arka planda çalışan, kullanıcının doğrudan görmediği ancak site veya uygulamanın işlevselliğini sağlayan kısımdır.
- Sunucu tarafında çalışan uygulama mantığını, veri tabanını ve sunucular arasındaki iletişimi içerir.
- Web sitesinin arka planında çalışan bu bölüm, verilerin işlenmesi, saklanması ve sunucu ile istemci arasındaki veri alışverişinin yönetilmesi gibi önemli işlevleri yerine getirir.
##### Front-end ve Back-end Arasındaki Farklar
- Front-end, kullanıcının gördüğü ve etkileşimde bulunduğu her şeyden sorumluyken, Back-end bu işlemlerin arka planda nasıl gerçekleştiğiyle ilgilenir.
- Front-end geliştiricileri, görsel tasarım ve kullanıcı deneyimine odaklanırken, Back-end geliştiricileri veri işleme, güvenlik ve performans konularında çalışır.
- Front-end ve Back-end geliştiricileri, kullanıcıların ihtiyaçlarını karşılayan, performanslı ve güvenli web siteleri oluşturmak için birlikte çalışır.
## Web Sunucusu Nedir?
- İstemcilerden gelen istekleri alıp işleyerek uygun yanıtları döndüren yazılımsal ya da donanımsal bir sistemdir.
- En yaygın kullanılan web sunucuları arasında IIS (Windows), Apache ve Nginx yer alır.
- Bir web sunucusu, HTTP protokolü üzerinden gelen istekleri alır, arka planda ilgili işlemleri yapar ve sonuçları tekrar istemciye iletir.
## API nedir? 
- İki yazılım bileşeninin birbirleriyle iletişim kurmasını sağlayan bir arayüzdür.
- API’ler sayesinde arayüz ve iş mantığı birbirinden ayrılır, bu da sistemlerin modüler, esnek ve kolay yönetilebilir olmasını sağlar.
##### API Türleri:
- RESTful API: HTTP protokolü ve JSON formatı ile çalışır, modern ve yaygın bir yaklaşımdır. GET, POST, PUT, DELETE gibi HTTP metodlarını kullanır.
- SOAP API: XML tabanlıdır, daha katıdır ve genellikle kurumsal sistemlerde tercih edilir. Veri taşıma daha karmaşıktır.
- GraphQL API: Kullanıcıya ihtiyacı kadar veri alma esnekliği tanır. Tek bir endpoint üzerinden çeşitli veri sorguları yapılabilir.
- gRPC: Google tarafından geliştirilen, performansı yüksek, ikili veri protokolü (binary) kullanan bir API türüdür. Mikroservislerde yaygınlaşmaktadır.
##### REST vs SOAP vs GraphQL: Temel Karşılaştırma
| Özellik          | REST                                   | SOAP                                         | GraphQL                                        |
|------------------|----------------------------------------|----------------------------------------------|------------------------------------------------|
| **Veri Formatı** | JSON (genelde)                         | XML                                          | JSON                                           |
| **Endpoint Kullanımı** | Her kaynak için ayrı endpoint          | Tek endpoint                                 | Tek endpoint                                   |
| **Performans**   | Yüksek (özellikle mobilde uygun)        | Düşük (XML nedeniyle ağır)                   | Yüksek (veri aşımı olmaz)                      |
| **Esneklik**     | Orta                                   | Düşük                                        | Çok yüksek (istediğin alanı çekersin)          |
| **Güvenlik**     | Temel (HTTPS, JWT)                     | Gelişmiş (WS-Security, XML Signature)        | Client güvenliği geliştirilmeli                |
| **Kullanım Alanı**| Web/Mobil API’ler                      | Finans, bankacılık, devlet sistemleri        | Modern web, frontend tabanlı sistemler         |
| **Öğrenme Eğrisi**| Kolay                                  | Zor                                          | Orta                                           |

## HTTP Nedir?
-  İstemci (client) ile sunucu (server) arasında veri iletişimi sağlayan bir protokoldür.
-  Web tarayıcıları, mobil uygulamalar ve API’ler bu protokolü kullanır.
-  HTTP istekleri belirli metotlarla yapılır.
##### HTTP Metodları ve Örnekleri:
| Metot      | Amaç                        | Örnek Senaryo                | Örnek HTTP İsteği              |
| ---------- | --------------------------- | ---------------------------- | ------------------------------ |
| **GET**    | Veri çekmek                 | Kullanıcı listesi alma       | `GET /api/users`               |
| **POST**   | Yeni veri eklemek           | Yeni kullanıcı oluşturma     | `POST /api/users` + JSON body  |
| **PUT**    | Var olan veriyi güncellemek | Kullanıcı bilgisi değiştirme | `PUT /api/users/5` + JSON body |
| **DELETE** | Veri silmek                 | Kullanıcı silme              | `DELETE /api/users/5`          |
## RESTful Servislerin Çalışma Mantığı
HTTP protokolü üzerine kurulu bir mimari stildir.
 - Özellikleri:
  - Kaynaklar (resources) URL ile tanımlanır.
  - HTTP metodları kullanılır (GET, POST, PUT, DELETE).
  - Stateless (her istek bağımsızdır, önceki isteklerin bilgisi tutulmaz).
  - Genellikle JSON veri formatı kullanılır.
- Örnek:
  - GET /api/products/10 → ID’si 10 olan ürünü getirir.
  - DELETE /api/products/10 → ID’si 10 olan ürünü siler.
## JSON Veri Formatı ve Kullanım Amacı
Veri değişimi için hafif ve okunabilir bir formattır.
- Özellikleri:
  - Anahtar-değer çiftlerinden oluşur.
  - İnsan ve makine tarafından kolayca anlaşılır.
  - Web API’lerde en yaygın veri formatıdır.
##### JSON Örneği:
```
{
  "id": 101,
  "ad": "Ahmet Yılmaz",
  "yas": 28,
  "email": "ahmet@example.com",
  "aktif": true,
  "adres": {
    "sehir": "İstanbul",
    "postaKodu": "34000"
  },
  "telefonlar": ["05321234567", "05419876543"]
}
```
- Satır Satır Açıklama:
  - id: Tam sayı (int)
  - ad: Metin (string)
  - yas: Tam sayı
  - email: Metin
  - aktif: Boolean (true/false)
  - adres: İç içe nesne (nested object)
  - telefonlar: String dizisi (array)
- Kullanım Amaçları:
  - Web API’lerde veri alışverişi
  - Konfigürasyon dosyaları (appsettings.json)
  - NoSQL veritabanlarında veri depolama (MongoDB vb.)
# ASP.NET
## ASP.NET Nedir?
- Microsoft tarafından geliştirilen, .NET Framework üzerine kurulu bir web uygulama geliştirme platformudur.
- Sadece Windows üzerinde çalışır.
- Web Forms, MVC, Web API gibi farklı geliştirme modelleri içerir.
- Kullanım Alanları:
  - Web siteleri
  - Web tabanlı uygulamalar
  - Web servisleri (SOAP/REST)
## ASP.NET Core Nedir?
- Microsoft’un geliştirdiği yeni nesil, açık kaynak, cross-platform web geliştirme platformudur.
- .NET Core ve .NET 5/6/7/8+ üzerinde çalışır.
- Windows, Linux, macOS gibi farklı platformlarda çalışabilir.
- Performans, esneklik ve bulut uyumluluğu yüksek olacak şekilde yeniden tasarlandı.
- Kullanım Alanları:
  - Modern web uygulamaları
  - RESTful API’ler
  - Gerçek zamanlı uygulamalar (SignalR)
  - Mikroservis mimarisi
## Avantajlar ve Farklar
| Avantaj               | ASP.NET                 | ASP.NET Core                                     |
| --------------------- | ----------------------- | ------------------------------------------------ |
| **Performans**        | Orta                    | Çok yüksek (Kestrel web sunucusu, modern mimari) |
| **Platform Desteği**  | Sadece Windows          | Cross-platform (Windows, Linux, macOS)           |
| **Açık Kaynak**       | ❌ Hayır                 | ✅ Evet                                           |
| **Modülerlik**        | Daha az                 | Çok modüler (kütüphaneler paketler halinde)      |
| **Bulut Uyumu**       | Orta                    | Yüksek (Azure, AWS, Docker uyumlu)               |
| **Geliştirme Modeli** | Web Forms, MVC, Web API | MVC, Razor Pages, Blazor, Minimal API            |
| **Güncel Destek**     | Sadece bakım modu       | Aktif geliştirme ve yeni özellikler              |
## Model-View-Controller (MVC) Nedir?
- Verinin farklı görselleştirme yöntemleriyle kullanıcıya sunulduğu, uygulamaların geliştirilmesinde kullanılan bir yazılım kalıbıdır.
- Yazılan uygulamanın iş mantığı ile kullanıcı arayüzünün, farklı hedefleri olan kısımlarının birbirinden ayrılmasını sağlar.
- Milyonlarca verinin olduğu karmaşık uygulamalarda verileri somutlaştırarak ve kodları birbirinden ayırarak geliştirmeyi daha kolay hale getirir.
- Masaüstü, web ve mobil uygulamaların hepsinde kullanılabilir. Nesne yönelimli programlama ile çalışabilir.
## MVC Çalışma Mantığı
Mvc mimarisi üç parçadan oluşur. Bunlar Model, View ve Controller olarak parçalara ayrılır. MVC mimarisi şu şekilde çalışır: Tarayıcıdan View sayfasına istek yapıldığında, View katmanı Controller’a gider. Controller isteği gerçekleştirmek üzere Model katmanına gider. Daha sonra Model'den alınan veriler, View’a gönderilerek istenilen verilerin görüntülenmesi sağlanır. En basit anlamda MVC, bir uygulamayı üç alana ayırma çabasıdır. 
- Model: MVC mimarisi içinde verilerin tutulduğu, veri tabanına erişimin sağlandığı, tüm data işlemlerinin gerçekleştiği yer model katmanıdır.
- View: Model katmanının görselleştirilmiş, kullanıcının uygulamayı gördüğü halidir.
- Controller: Model ve View katmanları arasındaki işlemleri gerçekleştirir. Yani View katmanından aldığı veri tabanı işlemleri ve hesaplamalar gibi tüm işlemleri Model katmanına taşır. Bir nevi arada köprü görevi görür.
## Middleware Nedir, Nasıl Çalışır?
ASP.NET Core’da istek (request) ve yanıt (response) sürecinde araya giren küçük yazılım bileşenleridir.
- Her middleware:
  1. İsteği işler (gerekirse değiştirir),
  2. Bir sonraki middleware’e iletir,
  3. Dönüşte (response) de veriyi değiştirebilir.
![Middleware Cycle](https://github.com/user-attachments/assets/b75fa3de-8786-4d87-975e-53807055a864)


  













































































































