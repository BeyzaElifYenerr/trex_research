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









































































































