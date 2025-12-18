Bu .gitlab-ci.yml dosyası; ASP.NET Core API projelerinizin sürekli entegrasyon (CI) ve sürekli dağıtım (CD) süreçlerini yönetmek için tasarlanmıştır. Temel amacı, kodunuzu güvenli bir şekilde derlemek ve hedef IIS (Internet Information Services) sunucusuna, çalıştığınız dala (branch) göre otomatik veya kontrollü bir şekilde aktarmaktır.

1. Dallara Göre Ayrılmış Senaryo (Branch Management)

Dosya, hangi kodun hangi ortama gideceğini rules (kurallar) mekanizmasıyla belirler:
	
 - Main Branch (Geliştirme): main dalına bir push yapıldığında süreç tamamen otomatiktir. Kod derlenir (build-dev) ve hemen ardından geliştirme sunucusuna (deploy:dev) basılır.
	
 - Prod Branch (Canlı): prod dalına bir push yapıldığında kod derlenir (build-prod), ancak canlıya çıkış işlemi (deploy:prod) için manuel onay (when: manual) beklenir. Bu, canlı ortamın güvenliğini sağlar.

2. Build (Derleme) Aşaması

Bu aşamada API projeniz yayınlanabilir (publish) hale getirilir:

  - dotnet publish komutu kullanılarak projenin tüm kütüphaneleri ve dosyaları bir klasörde toplanır.

  - Environment Specific: Derleme yapılırken projenin hangi ortamda (Development veya Production) çalışacağı /p:EnvironmentName parametresiyle koda enjekte edilir.

3. Deploy (IIS Sunucusuna Dağıtım) Aşaması

Derlenen dosyalar, PowerShell komutları aracılığıyla doğrudan IIS sunucusuna aktarılır:

  - Hizmet Durdurma: Dosyaların üzerine yazılabilmesi için IIS servisleri (W3SVC) geçici olarak durdurulur.

  - Temizlik ve Kopyalama: Belirlenen DEPLOY_PATH üzerindeki eski dosyalar silinir ve yeni derlenen (build edilen) dosyalar buraya kopyalanır.

  - Yeniden Başlatma: Dağıtım bittikten sonra servis tekrar başlatılarak API'nin yeni versiyonuyla yayına girmesi sağlanır.
