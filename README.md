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
- Yazılım geliştirme sürecinde iki farklı branchte yapılan değişikliklerin aynı dosyanın aynı satırlarında veya birbirini etkileyen bölgelerinde çakışması durumunda oluşur.
##### Nasıl Çözülür?
- Çatışma fark edilir. Git merge veya Git pull yapıldığında Git “merge conflict” uyarısı verir.
- Çakışmalı dosya açılır. Dosyayı açıldığında iki farklı versiyonun karşılaştırıldığı bir ekran görülür.
- Hangi değişikliklerin korunulacağına karar verilir.
- Çatışma işaretleri kaldırılır.
- Dosya kaydedilir ve Git add ile çatışma çözülür.
## CI/CD Nedir?
- Yazılım geliştirme sürecini otomatikleştiren ve yazılım kalitesini artıran iki temel kavramdır.
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














































































































