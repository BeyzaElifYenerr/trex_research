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

##### Startup.cs / Program.cs Middleware Sırası Örneği:
```
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.UseRouting();          // 1. Rotalama bilgisi hazırlanır
app.UseAuthentication();   // 2. Kimlik doğrulama yapılır
app.UseAuthorization();    // 3. Yetkilendirme yapılır
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllers();
});

app.Run();
```
**NOT: Middleware sırası çok önemlidir. Örneğin UseAuthorization() kimlik doğrulamadan önce gelirse yetkilendirme çalışmaz.**
## Worker ve Background Job Nedir?
##### Worker Service
- .NET uygulamalarında arka planda kesintisiz çalışan servislerdir.
- Genellikle zamanlanmış görevler, mesaj kuyruğu dinleyicileri veya sürekli izleme işlemleri için kullanılır.
##### Background Job 
- ASP.NET Core içinde tanımlanan, genellikle kullanıcı etkileşimi sonrası tetiklenen, kısa süreli asenkron işlemlerdir.
- Örneğin: Bir kullanıcı kayıt olduğunda ona e-posta göndermek, bir dosya yüklenince thumbnail oluşturmak gibi görevlerdir.

| Özellik           | Worker Service                           | Background Job                                  |
|-------------------|------------------------------------------|------------------------------------------------|
| **Çalışma Süresi**| Sürekli                                  | Tetiklendiğinde çalışır                        |
| **Web’e Bağımlılık** | Bağımsız çalışabilir                    | Genelde ASP.NET uygulamasına bağlıdır          |
| **Kullanım Alanı**| Cron job, loglama, queue işleme          | E-posta, PDF üretme, görsel sıkıştırma         |
| **Oluşturma Şekli**| `dotnet new worker` ile proje oluşturulur| `IHostedService`, `BackgroundService`, Hangfire|
| **Yaygın Örnek**  | Docker'da sürekli çalışan servis         | Web API içinde gecikmeli işlem                 |
## Dependency Injection (DI) Nedir? 
- Bir sınıfın ihtiyaç duyduğu bağımlılıkları (örneğin servisler, veritabanı erişimi) kendi içinde oluşturmaması, bunun yerine dışarıdan alması prensibidir.
- ASP.NET Core, yerleşik bir DI altyapısı ile birlikte gelir. Bu yapı sayesinde uygulamalar daha esnek, test edilebilir ve modüler hale gelir.
```
// Interface
public interface IProductService
{
    List<string> GetProducts();
}

// Service
public class ProductService : IProductService
{
    public List<string> GetProducts() => new List<string> { "Laptop", "Telefon" };
}

// Controller
public class ProductController : ControllerBase
{
    private readonly IProductService _productService;
    
    public ProductController(IProductService productService) // DI ile geliyor
    {
        _productService = productService;
    }

    [HttpGet]
    public IActionResult Get() => Ok(_productService.GetProducts());
}
```
Program.cs’da DI Kaydı:
```
builder.Services.AddScoped<IProductService, ProductService>();
```
Çalışma Mantığı:
 1. Kullanıcı GET /product isteği gönderir.
 2. ASP.NET Core, ProductController’ı oluşturur.
 3. Controller’ın constructor’ında IProductService istenir → DI Container ProductService’i oluşturur ve verir.
 4. Get() metodu çağrılır → Servisten veri alınır → JSON olarak döner.
##### ASP.NET Core Katmanlı Mimari
`````
[Presentation Layer] → Controller / View / API
↓
[Business Layer] → Servisler (iş mantığı)
↓
[Data Access Layer] → Repository, Entity Framework
↓
[Database] → SQL Server, PostgreSQL, vb.
`````
## 5. Veritabanı ve ORM
## SQL Nedir?
SQL (Structured Query Language), veritabanları ile iletişim kurmak için kullanılan standart sorgu dilidir.
- Veri ekleme, silme, güncelleme, sorgulama işlemlerinde kullanılır.
- MySQL, SQL Server, PostgreSQL, Oracle gibi sistemlerde kullanılır.
## İlişkisel ve İlişkisel Olmayan Veritabanı Farkları
| Özellik         | İlişkisel Veritabanı (RDBMS)  | İlişkisel Olmayan Veritabanı (NoSQL) |
| --------------- | ----------------------------- | ------------------------------------ |
| Veri Yapısı     | Tablo (satır/sütun)           | JSON, doküman, anahtar-değer, grafik |
| Şema            | Sabit şema                    | Esnek şema                           |
| Örnek Sistemler | SQL Server, MySQL, PostgreSQL | MongoDB, Redis, Cassandra            |
| Kullanım Alanı  | Finans, ERP, CRM              | Büyük veri, gerçek zamanlı analiz    |
| Sorgu Dili      | SQL                           | Kendi sorgu dili veya API            |
## ORM Nedir?
- ORM (Object Relational Mapping), veritabanı tablolarını nesneler olarak temsil eden bir teknolojidir.
- SQL yazmadan, nesneler üzerinden veri işlemleri yapmamızı sağlar.
## Entity Framework Core Nedir?
- Entity Framework Core (EF Core), Microsoft’un .NET için geliştirdiği modern ORM aracıdır.
- Cross-platform çalışır.
- LINQ desteği vardır.
- Code-First ve Database-First yaklaşımlarını destekler.
## DbContext Nedir, Nasıl Kullanılır?
- DbContext, EF Core’da veritabanı ile uygulama arasındaki köprüdür.
- Tablolar için DbSet<T> tanımlanır.
- CRUD işlemleri buradan yapılır.
##### Örnek:
```
public class AppDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }

    public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) { }
}
```
## LINQ Nedir?
- LINQ (Language Integrated Query), C# içinde veri sorgulama imkanı sunar.
- SQL benzeri sorgular, koleksiyonlar ve veritabanları üzerinde çalışabilir.
##### Yaygın LINQ Örnekleri:
```
// Filtreleme (Where)
var expensive = products.Where(p => p.Price > 1000);

// Sıralama (OrderBy / OrderByDescending)
var sorted = products.OrderBy(p => p.Name);

// İlk/tek kayıt (First / Single)
var first = products.First(p => p.Id == 1);

// Projeksiyon (Select)
var names = products.Select(p => p.Name);
```
##### LINQ → SQL Karşılaştırması
| LINQ                                  | SQL                                          |
| ------------------------------------- | -------------------------------------------- |
| `products.Where(p => p.Price > 1000)` | `SELECT * FROM Products WHERE Price > 1000;` |
| `products.OrderBy(p => p.Name)`       | `SELECT * FROM Products ORDER BY Name ASC;`  |
| `products.Select(p => p.Name)`        | `SELECT Name FROM Products;`                 |
| `products.First(p => p.Id == 1)`      | `SELECT TOP 1 * FROM Products WHERE Id = 1;` |
## Code-First
- Önce kod tarafında (C# sınıfları ile) veri modelleri tanımlanır.
- Entity Framework (EF) bu modellere göre veritabanı şemasını otomatik oluşturur.
- Veritabanı migration (göç) komutlarıyla güncellenir.
## Database-First
- Önce veritabanı (SQL Server, MySQL, vb.) oluşturulur.
- EF, mevcut veritabanı şemasına göre C# sınıflarını otomatik üretir.
- Bu işlem için Scaffold-DbContext komutu kullanılır.

| Özellik                  | Code-First           | Database-First              |
| ------------------------ | -------------------- | --------------------------- |
| **Başlangıç Noktası**    | Kod (C# sınıfları)   | Mevcut veritabanı           |
| **Veritabanı Oluşturma** | EF migration ile     | Önceden tasarlanmış         |
| **Esneklik**             | Yüksek (kod odaklı)  | Orta (DB odaklı)            |
| **En Uygun Olduğu Yer**  | Yeni projeler        | Mevcut DB ile entegrasyon   |
| **Bağımlılık**           | Kod tabanına bağımlı | Veritabanı şemasına bağımlı |
## Temel SQL sorguları: SELECT, INSERT, UPDATE, DELETE
- SELECT (Veri Çekme)
```
SELECT * FROM Products;
SELECT Name, Price FROM Products WHERE Price > 1000;
```
- INSERT (Veri Ekleme)
```
INSERT INTO Products (Name, Price) VALUES ('Laptop', 15000);
```
- UPDATE (Veri Güncelleme)
```
UPDATE Products SET Price = 14000 WHERE Id = 1;
```
- DELETE (Veri Silme)
```
DELETE FROM Products WHERE Id = 1;
```
# 6. Güvenlik ve Performans
## Authentication
- Kullanıcının kim olduğunu doğrulama süreci.
- Örnek: Kullanıcı adı/şifre ile giriş yapması.
## Authorization
-  Doğrulanan kullanıcının hangi işlemleri yapabileceğini belirleme süreci.
-  Örnek: Admin kullanıcı ürün silebilir ama normal kullanıcı silemez.
## JWT (JSON Web Token) Nedir?
- İstemci ile sunucu arasında güvenli bilgi taşımak için kullanılan, JSON tabanlı bir token formatıdır.
- Genellikle Bearer Token olarak HTTP Authorization header'ında gönderilir.
- Stateless çalışır (sunucu tarafında session saklanmaz).
##### Header
- JWT’nin türünü ve imzalama algoritmasını belirtir.
- Alanlar:
  - alg → İmzalama algoritması (HS256 = HMAC-SHA256, RS256 = RSA-SHA256 vb.)
  - typ → Token tipi (genelde "JWT")
- Örnek JSON:
```
{
  "alg": "HS256",
  "typ": "JWT"
}
```
##### Payload
- Kullanıcıya ait bilgileri (claims) taşır.
- Claims Türleri:
  - Registered claims (standart alanlar): sub (subject), iat (issued at), exp (expiration time) vb.
  - Public claims (genel alanlar)
  - Private claims (uygulamaya özel alanlar)
- Örnek JSON:
```
{
  "userId": "1234",
  "role": "Admin",
  "exp": 1735689600
}
```
**exp değeri token’ın ne zaman geçersiz olacağını Unix zaman damgası olarak belirtir.**
##### Signature
- Token’ın değiştirilip değiştirilmediğini doğrulamak.
- Oluşumu:
```
Signature = HMACSHA256(
    Base64UrlEncode(Header) + "." + Base64UrlEncode(Payload),
    SecretKey
)
```
- Mantık: Header + Payload birleşip gizli anahtar ile imzalanır. İmzayı doğrulamak için aynı işlem sunucuda yapılır, uyuşmazsa token geçersizdir.
##### JWT Akış Mantığı
 1. Kullanıcı giriş yapar, kimlik doğrulama başarılı olursa sunucu JWT üretir.
 2. Sunucu JWT’yi istemciye gönderir.
 3. İstemci sonraki isteklerde JWT’yi Authorization: Bearer <token> başlığı ile gönderir.
 4. Sunucu JWT’nin imzasını ve süresini kontrol eder.
 5. Token geçerliyse işlem yapılır.
## OAuth, OAuth2.0, OpenID Connect, OpenIddict
- OAuth → Üçüncü taraf uygulamalara belirli kaynaklara erişim yetkisi verme standardı. (ör: "Google ile Giriş Yap" → senin şifreni paylaşmadan Google hesabına erişim izni vermek)
- OAuth 2.0 → OAuth'un modern ve daha güvenli versiyonu.
- OpenID Connect (OIDC) → OAuth 2.0 üzerine kimlik doğrulama katmanı ekler.
- OpenIddict → ASP.NET Core uygulamalarında OAuth 2.0 ve OpenID Connect tabanlı kimlik doğrulama için kullanılan bir kütüphane.
## Performans artımı için ne yapılabilir?
- AsNoTracking
  - EF Core sorgularında AsNoTracking() kullanmak, EF’nin entity'leri takip etmesini engeller.
  - Okuma odaklı sorgularda performans artışı sağlar.
```
var products = context.Products.AsNoTracking().ToList();
```
- IAsyncEnumerable
  - Verileri streaming şeklinde (parça parça) çekerek bellekte yüklenmeyi azaltır.
  - Özellikle büyük veri setlerinde daha az bellek kullanır.
```
await foreach (var product in context.Products.AsAsyncEnumerable())
{
    Console.WriteLine(product.Name);
}
```
- Caching (Önbellekleme)
  - Sık kullanılan verileri bellek, Redis gibi sistemlerde saklamak, tekrar tekrar veritabanına gitmeyi engeller.
  - Örnek: MemoryCache, DistributedCache, Redis Cache.
- Profiling
  - Performans darboğazlarını tespit etmek için sorgu ve işlem sürelerini izleme.
  - EF Core LogTo ile SQL sorgularını görebilir veya MiniProfiler kullanabilirsin.
- Redis
  - Dağıtık ve yüksek hızlı cache sistemi.
  - API yanıtlarını veya session verilerini saklamak için kullanılabilir.
## OWASP Top 10
| No     | Açık Adı                                     | Kısa Açıklama                                                                               |
| ------ | -------------------------------------------- | ------------------------------------------------------------------------------------------- |
| **1**  | **Broken Access Control**                    | Yetkilendirme hataları → Kullanıcı, erişmemesi gereken verilere/işlemlere erişebilir.       |
| **2**  | **Cryptographic Failures**                   | Zayıf veya yanlış şifreleme → Hassas veriler korunmaz, çalınabilir.                         |
| **3**  | **Injection (SQL Injection)**                | Kullanıcı girdisi filtrelenmeden sorguya eklenir, saldırgan veri tabanına komut çalıştırır. |
| **4**  | **Insecure Design**                          | Başta güvenlik düşünülmeden yapılan tasarım → Açıklara sebep olur.                          |
| **5**  | **Security Misconfiguration**                | Yanlış güvenlik ayarları (varsayılan şifreler, açık portlar) → Sistemi savunmasız bırakır.  |
| **6**  | **Vulnerable Components**                    | Eski ve güvenlik açığı olan kütüphanelerin/projelerin kullanılması.                         |
| **7**  | **Identification & Authentication Failures** | Kimlik doğrulama mekanizmaları zayıfsa, saldırgan başkası gibi giriş yapabilir.             |
| **8**  | **Software & Data Integrity Failures**       | Kod veya verinin yetkisiz şekilde değiştirilmesi → Zararlı kod çalıştırma riski.            |
| **9**  | **Security Logging & Monitoring Failures**   | Olayların kaydedilmemesi → Saldırılar fark edilmez.                                         |
| **10** | **Server-Side Request Forgery (SSRF)**       | Sunucu, saldırganın istediği bir dış kaynağa istek gönderir → Veri sızıntısı.               |
##### SQL Injection 
- Kullanıcı girişine girilen zararlı SQL kodu veritabanında çalışır.
- Örnek (yanlış kullanım):
```
SELECT * FROM Users WHERE Username = '" + userInput + "'";
```
- Saldırgan şunu girebilir: ' OR '1'='1 → Tüm kullanıcılar listelenir.
##### XSS (Cross-Site Scripting) 
- Web sayfasına zararlı JavaScript kodu eklenir, kullanıcı tarayıcısında çalışır.
##### CSRF (Cross-Site Request Forgery) 
- Kullanıcı farkında olmadan başka bir site üzerinden yetkili işlemler yaptırılır (örneğin banka havalesi).
##### Broken Authentication
- Kimlik doğrulama hataları → Şifre tahmini, oturum çalma gibi sorunlar.
#####  ASP.NET Core ile Alınabilecek Önlemler
| Açık Türü                 | ASP.NET Core Önlemleri                                                                            |
| ------------------------- | ------------------------------------------------------------------------------------------------- |
| **SQL Injection**         | Parametreli sorgular veya LINQ kullan, `FromSqlInterpolated` ile güvenli SQL yaz.                 |
| **XSS**                   | Razor otomatik HTML encode yapar, kullanıcı girdilerini encode et.                                |
| **CSRF**                  | Formlarda `@Html.AntiForgeryToken()` kullan, `ValidateAntiForgeryToken` ekle.                     |
| **Broken Authentication** | ASP.NET Identity veya JWT ile güçlü kimlik doğrulama, şifre karma (hash) kullan.                  |
| **Model Validation**      | `[Required]`, `[MaxLength]`, `[EmailAddress]` gibi Data Annotations ile giriş verilerini doğrula. |
| **Input Sanitization**    | HTML, JavaScript gibi zararlı girdileri temizle (ör. HtmlSanitizer).                              |
# 7. Logging ve Hata Yönetimi
## Neden Loglama Yapılır?
- Loglama, uygulamanın çalışması sırasında önemli olayları, hataları ve sistem bilgilerini kayıt altına alma işlemidir.
- Amaçları:
  - Hata ayıklama (debugging)
  - Uygulama davranışını izleme
  - Güvenlik olaylarını takip etme
  - Performans sorunlarını tespit etme
  - Kullanıcı aktivitelerini inceleme
## Log Seviyesi Nedir?
Log seviyeleri, kaydın önem derecesini belirler. ASP.NET Core’da en yaygın log seviyeleri:
| Seviye          | Açıklama                                                        | Örnek                       |
| --------------- | --------------------------------------------------------------- | --------------------------- |
| **Trace**       | En detaylı loglar, genellikle geliştirme aşamasında kullanılır. | Metot giriş-çıkış bilgileri |
| **Debug**       | Hata ayıklama amaçlı, normalde production’da kapalıdır.         | Değişken değerleri          |
| **Information** | Uygulamada gerçekleşen normal olaylar.                          | Kullanıcı giriş yaptı       |
| **Warning**     | Potansiyel sorunlar.                                            | Disk alanı azalıyor         |
| **Error**       | Hata oluştu ama uygulama çalışmaya devam edebilir.              | API isteği başarısız        |
| **Critical**    | Ciddi hata, uygulamanın durmasına sebep olabilir.               | Veritabanı bağlantısı koptu |
## ASP.NET Core’da Logging Altyapısı
- ASP.NET Core’da yerleşik Microsoft.Extensions.Logging kütüphanesi vardır.
- Varsayılan olarak Console, Debug, EventSource gibi sağlayıcılar eklenebilir.
- Üçüncü taraf sağlayıcılar (Serilog, NLog, log4net) ile gelişmiş loglama yapılabilir.
Program.cs Örneği:
```
var builder = WebApplication.CreateBuilder(args);

// Log seviyesini ayarlama
builder.Logging.ClearProviders();
builder.Logging.AddConsole();
builder.Logging.SetMinimumLevel(LogLevel.Information);

var app = builder.Build();
```
## Global Exception Handling
- Uygulamada oluşan tüm beklenmeyen hataları tek bir noktadan yakalamak için kullanılır.
- Bunun amacı:
 - Hataları merkezi bir yerde loglamak,
 - Kullanıcıya güvenli ve standart bir cevap dönmek,
 - Teknik detayları dışarı sızdırmamak.
##### UseExceptionHandler Nedir?
- ASP.NET Core’da global exception handling yapmak için kullanılan yerleşik bir middleware’dir.
- Uygulamada beklenmeyen bir hata olduğunda, istek akışını özel belirlediğin bir endpoint’e yönlendirir.
- Bu endpoint’te hatayı yakalayıp loglayabilir ve kullanıcıya standart bir cevap dönebilirsin.
- Production ortamında önerilir çünkü kullanıcıya hata detaylarını göstermez.
```
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddControllers();

var app = builder.Build();

// Global hata yakalama endpoint’ine yönlendir
app.UseExceptionHandler("/error");

app.MapControllers();

app.Run();
```
##### ILogger Nedir?
- ASP.NET Core’un yerleşik loglama arayüzüdür (Microsoft.Extensions.Logging içinde).
- Farklı log seviyelerinde (Trace, Debug, Information, Warning, Error, Critical) log yazmanı sağlar.
- Console, Debug, dosya, veri tabanı veya üçüncü taraf (Serilog, NLog) ile kullanılabilir.
```
public class HomeController : ControllerBase
{
    private readonly ILogger<HomeController> _logger;

    public HomeController(ILogger<HomeController> logger)
    {
        _logger = logger;
    }

    [HttpGet]
    public IActionResult Index()
    {
        _logger.LogInformation("Index metodu çalıştı.");
        try
        {
            throw new Exception("Test hatası");
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Bir hata oluştu.");
        }
        return Ok();
    }
}
```
##### UseExceptionHandler + ILogger Birlikte Kullanım
UseExceptionHandler ile yönlendirdiğin hata endpoint’inde ILogger kullanarak hatayı loglayabilir.
```
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddControllers();
builder.Logging.ClearProviders();
builder.Logging.AddConsole(); // Console’a log yazmak için

var app = builder.Build();

// Global exception handling
app.UseExceptionHandler("/error");

// Hata yakalama endpoint’i
app.Map("/error", (HttpContext context) =>
{
    var exceptionFeature = context.Features.Get<IExceptionHandlerFeature>();
    var logger = context.RequestServices.GetRequiredService<ILogger<Program>>();

    // Hata detaylarını logla
    logger.LogError(exceptionFeature?.Error, "Global hata yakalandı");

    // Kullanıcıya standart bir cevap dön
    return Results.Problem(
        title: "Bir hata oluştu",
        detail: "Sistem yöneticisi ile iletişime geçin.",
        statusCode: StatusCodes.Status500InternalServerError
    );
});

app.MapControllers();
app.Run();
```
- Çalışma Mantığı
  1. Uygulamada beklenmeyen bir hata oluşur.
  2. UseExceptionHandler devreye girer ve isteği /error endpoint’ine yönlendirir.
  3. /error endpoint’i IExceptionHandlerFeature ile hatayı alır.
  4. ILogger ile hata detaylarını loglar (Console, dosya, veri tabanı vb.).
  5. Kullanıcıya güvenli, standart bir hata yanıtı döner.
# 8. Yazılım Geliştirme Prensipleri
## SOLID Prensipleri
##### S – Single Responsibility Principle (Tek Sorumluluk İlkesi)
- Bir sınıfın tek bir sorumluluğu olmalı, değişmesi için tek bir sebebi olmalı.
- Kodun bakımı ve genişletilmesi kolay olur.
- Örnek:
```
// Kötü Örnek: Hem kullanıcı kaydı hem e-posta gönderme yapıyor
public class UserService
{
    public void Register(string username, string password) { /* kayıt işlemi */ }
    public void SendEmail(string email) { /* e-posta gönderme */ }
}

// İyi Örnek: Görevler ayrı sınıflara ayrıldı
public class UserService
{
    public void Register(string username, string password) { /* kayıt işlemi */ }
}
public class EmailService
{
    public void SendEmail(string email) { /* e-posta gönderme */ }
}
```
##### O – Open/Closed Principle (Açık/Kapalı İlkesi)
- Sınıflar yeni özellikler eklemeye açık, mevcut kodu değiştirmeye kapalı olmalıdır.
- Yeni özellik eklerken mevcut kodu bozmamak.
- Örnek:
```
// Kötü Örnek: Yeni ödeme yöntemi eklemek için kodu değiştirmek gerekiyor
public class PaymentService
{
    public void Pay(string method)
    {
        if (method == "CreditCard") { /* kredi kartı işlemi */ }
        else if (method == "PayPal") { /* PayPal işlemi */ }
    }
}

//  İyi Örnek: Yeni ödeme eklemek için yeni sınıf yazılır
public interface IPaymentMethod { void Pay(); }
public class CreditCardPayment : IPaymentMethod { public void Pay() { } }
public class PayPalPayment : IPaymentMethod { public void Pay() { } }
```
##### L – Liskov Substitution Principle (Liskov’un Yerine Geçme İlkesi)
- Türetilmiş sınıflar, temel sınıfların yerine sorunsuz geçebilmelidir.
- Kalıtım ilişkisi doğru kullanılmalı.
- Örnek:
```
// Kötü Örnek: Türeyen sınıf, temel sınıfın davranışını bozuyor
public class Bird { public virtual void Fly() { } }
public class Penguin : Bird { public override void Fly() { throw new Exception("Penguen uçamaz"); } }

// İyi Örnek: Ortak davranışlar uygun sınıflara taşındı
public class Bird { }
public class FlyingBird : Bird { public void Fly() { } }
public class Penguin : Bird { }
```
#####  I – Interface Segregation Principle (Arayüz Ayrımı İlkesi)
- Büyük ve kapsamlı arayüzler yerine, ihtiyaca uygun küçük arayüzler oluşturulmalı.
- Gereksiz metod implementasyonunu engellemek.
- Örnek:
```
// Kötü Örnek: Her yazıcı tarama yapmak zorunda
public interface IPrinter
{
    void Print();
    void Scan();
}

public class SimplePrinter : IPrinter
{
    public void Print() { }
    public void Scan() { throw new NotImplementedException(); }
}

//  İyi Örnek: Ayrı arayüzler tanımlandı
public interface IPrint { void Print(); }
public interface IScan { void Scan(); }
```
##### D – Dependency Inversion Principle (Bağımlılıkların Tersine Çevrilmesi İlkesi)
- Yüksek seviye modüller, düşük seviye modüllere değil, soyutlamalara (interface/abstract) bağımlı olmalıdır.
- Modüller arası bağımlılığı azaltmak.
- Örnek:
```
// Kötü Örnek: Doğrudan somut sınıfa bağımlılık
public class ReportService
{
    private readonly PdfExporter _exporter = new PdfExporter();
}

// İyi Örnek: Interface’e bağımlılık
public interface IExporter { void Export(); }
public class PdfExporter : IExporter { public void Export() { } }
public class ReportService
{
    private readonly IExporter _exporter;
    public ReportService(IExporter exporter) { _exporter = exporter; }
}
```













  













































































































