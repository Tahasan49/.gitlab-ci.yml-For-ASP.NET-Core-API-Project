Project CI/CD Documentation

This .gitlab-ci.yml file is designed to manage the Continuous Integration (CI) and Continuous Deployment (CD) processes for your ASP.NET Core API projects. Its primary purpose is to securely build your code and deploy it to the target IIS (Internet Information Services) server in either an automated or controlled manner, depending on the active branch.

1. Branch Management

The file uses the rules mechanism to determine which code is deployed to which environment:

  - Main Branch (Development): When a push is made to the main branch, the process is fully automated. The code is built (build-dev) and immediately deployed to the development server (deploy:dev).
	
  - Prod Branch (Production): When a push is made to the prod branch, the code is built (build-prod), but the deployment process (deploy:prod) requires manual approval (when: manual). This ensures the security and stability of the production environment.

2. Build Stage

During this stage, your API project is prepared for publication:

  - The dotnet publish command is used to collect all necessary libraries and files into a single folder.

  - Environment Specific: The target environment (Development or Production) is injected into the code during the build process using the /p:EnvironmentName parameter.

3. Deploy Stage (IIS Deployment)

The compiled files are transferred directly to the IIS server via PowerShell commands:

  - Service Suspension: The IIS service (W3SVC) is temporarily stopped to allow files to be overwritten.

  - Cleanup and Copying: Existing files at the defined DEPLOY_PATH are deleted, and the newly built files are copied to this location.

  - Restart: Once the deployment is complete, the service is restarted, making the new version of the API live.

***************************************************
For Turkish
***************************************************

Proje CI/CD Dokümantasyonu

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
