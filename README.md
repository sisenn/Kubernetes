# Kubernetes
# 1. Giriş - Kubernetes’e İlk Adım
## Kubernetes (k8s) Nedir?

- **Tanım**: Kubernetes, konteynerleştirilmiş uygulamaları otomatikleştirmek, dağıtmak ve yönetmek için kullanılan açık kaynak bir konteyner orkestrasyon platformudur.

*Konteyner Orkestrasyonu Nedir?*

Tek bir uygulamayı, birçok küçük ve bağımsız servis olarak çalıştırdığınızı düşünün. Her bir servis, bir konteynerde çalışabilir. Kubernetes, bu konteynerleri otomatik olarak yönetir. Uygulamanızın ihtiyaçlarına göre yeni konteynerler açar, hata olduğunda yeniden başlatır veya konteyner sayısını azaltır.

- **Geliştirici**: Google tarafından Go ile geliştirilmiştir ve daha sonra Cloud Native Computing Foundation (CNCF) tarafından yönetilmektedir.

- **Temel Özellikler**:
  - Uygulamaların sayısını artırıp azaltma: **Scaling**
  - Yük dengeleme: **Load Balancing**
  - Otomatik yeniden başlatma: **Restart Policy**
  - Kendi kendine iyileşme: **Self-Healing**

## Neden Kubernetes?
Kubernetes, modern yazılım geliştirme ve dağıtım süreçlerinde kritik bir rol oynar. Tek bir sunucuda basit bir web uygulaması çalıştırmak kulağa kolay gelse de, gerçek dünya koşullarında işler hızla karmaşıklaşabilir. Uygulamanız büyüdükçe veya trafiğiniz arttıkça manuel olarak sistemleri yönetmek, ölçeklemek ve uygulama sürekliliğini sağlamak zorlaşır. Kubernetes bu noktada devreye girer ve pek çok yaygın zorluğu ortadan kaldırır.

### 1. Manuel Ölçekleme Zorlukları

Bir web uygulamasını tek bir sunucuda çalıştırdığınızı düşünün. Kullanıcı sayınız artarsa, sunucuya binen yük de artacaktır. Bu durumda, uygulamanızın daha fazla sunucuya dağıtılması gerekir. Ancak bunu manuel olarak yapmak, hem zaman alıcı hem de hata yapma olasılığını artıran bir süreçtir.

#### Manuel Ölçekleme Sorunları:

- Uygulamayı manuel olarak birden fazla sunucuya kurmak ve yapılandırmak gerekir.
- Trafik arttığında sunucu eklemek, azaldığında sunucuları kapatmak için sürekli müdahale gerektirir.
- Sunucular arasındaki iş yükü dağılımını ayarlamak zor ve hataya açık bir süreçtir.
- Kubernetes’in Çözümü: Kubernetes, uygulamaları otomatik olarak ölçekleyebilir. Örneğin, kullanıcı trafiğiniz aniden artarsa, Kubernetes pod’larınızı otomatik olarak çoğaltır ve iş yükünü dağıtır. Trafik azalınca gereksiz kaynakları serbest bırakarak kaynak israfını önler.

#### Kubernetes’in Çözümü: 
Kubernetes, uygulamaları otomatik olarak ölçekleyebilir. Örneğin, kullanıcı trafiğiniz aniden artarsa, Kubernetes pod’larınızı otomatik olarak çoğaltır ve iş yükünü dağıtır. Trafik azalınca gereksiz kaynakları serbest bırakarak kaynak israfını önler.

*Örnek Komut:*

`kubectl scale deployment my-app --replicas=5`

Bu komut, uygulamanızın 5 farklı pod üzerinde çalışmasını sağlar. Kubernetes, yük dengelemeyi otomatik olarak yapar ve trafik her pod’a dengeli bir şekilde dağıtılır.

### 2. Yük Dengeleme (Load Balancing)
Birden fazla sunucuda uygulama çalıştırdığınızda, gelen trafiğin her bir sunucuya dengeli bir şekilde dağıtılması gerekir. Bunu manuel yapmak hem karmaşıktır hem de sunucular arasında trafiğin doğru bir şekilde dağılmaması performans sorunlarına yol açabilir.

#### Manuel Yük Dengeleme Sorunları:

- Gelen trafiği sunuculara manuel olarak dağıtmak için ek yük dengeleyici yazılımlar kullanmak gerekir.
- Doğru yapılandırma yapılmadığında bazı sunucular aşırı yüklenirken diğerleri boşta kalabilir.

#### Kubernetes’in Çözümü: 

Kubernetes’in yerleşik bir yük dengeleme sistemi vardır. Kubernetes servisleri, trafiği çalışan pod’lar arasında otomatik olarak dağıtır. Eğer bir pod kapanırsa veya yeni pod’lar açılırsa, Kubernetes yük dengelemeyi otomatik olarak günceller.

*Örnek YAML Tanımı:*

```apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: my-app
```

Bu servis, gelen tüm istekleri uygulamanızın pod’ları arasında dengeli bir şekilde dağıtır.

### 3.Hata Kurtarma
Herhangi bir sistemde sunucuların çökmesi veya uygulamaların durması kaçınılmazdır. Geleneksel yöntemlerde, bir sunucu çöktüğünde uygulamanın yeniden başlatılması ve sistemin normale döndürülmesi zaman alabilir ve manuel müdahale gerektirir.

#### Manuel Hata Kurtarma Sorunları:

- Bir sunucu veya uygulama çöktüğünde, uygulamayı elle yeniden başlatmak gerekir.
- Sürekli izleme ve manuel müdahale gerektirir.
- Kesinti süreleri artar ve müşteri memnuniyeti düşer.

#### Kubernetes’in Çözümü: 
Kubernetes, uygulamalarınızın yüksek erişilebilirliğini otomatik olarak sağlar. Eğer bir pod kapanırsa, Kubernetes hemen yeni bir pod açarak kesinti süresini minimuma indirir. Aynı zamanda pod’ları sürekli izler ve sorunları tespit ederek otomatik iyileştirme yapar.

*Örnek Komut:*

`kubectl get pods`

Bu komutla çalışan pod'larınızı görebilirsiniz. Eğer bir pod kapanırsa Kubernetes, yenisini otomatik olarak başlatır.

### 4.Dağıtım Kolaylığı

Bir uygulamayı güncellerken ya da yeni bir sürüm dağıtırken, eski sürümü kesintiye uğratmadan yenisini dağıtmak genellikle zordur. Geleneksel yöntemlerle bu işlem, sunucuların manuel olarak yeniden başlatılmasını ve uygulamanın birkaç dakikalık kesintiye uğramasını gerektirebilir.

#### Kubernetes’in Çözümü: 
Kubernetes, **rolling update** özelliği ile yeni sürümü eski sürümü kapatmadan dağıtır. Böylece uygulamanız kesintisiz olarak güncellenir. Eski sürümde bir hata varsa, Kubernetes rollback işlemi ile önceki sürüme hızla geri dönebilir.

*Örnek Komut:*

`kubectl rollout status deployment/my-app`

Bu komutla, uygulamanızın yeni sürümünün dağıtım durumunu izleyebilirsiniz.

### Sonuç

Kubernetes, modern yazılım geliştirme süreçlerinde yaşanan manuel ölçekleme, yük dengeleme ve hata kurtarma gibi zorlukları otomatik hale getirerek uygulamaların daha stabil, ölçeklenebilir ve yönetilebilir olmasını sağlar. Bu avantajları sayesinde Kubernetes, büyük ve karmaşık uygulamaların sorunsuz bir şekilde çalışmasını sağlar ve geliştiricilerin daha az manuel müdahale ile daha fazla iş yapabilmelerine olanak tanır.

## Kubernetes Bileşenlerine Giriş: Konteynerler ve Docker'ı Anlamak 

Kubernetes'i tam anlamıyla kavrayabilmek için önce konteyner ve Docker teknolojisinin temellerini anlamak önemlidir. 

### *Konteyner Teknolojisi Nedir?*

Konteynerlar, uygulamalarınızı ve bu uygulamaların ihtiyaç duyduğu tüm dosyaları, kütüphaneleri ve bağımlılıkları bir araya toplayan hafif, taşınabilir ve izole edilmiş paketlerdir.

*Gerçek Hayattan Bir Örnek:*

Bir uygulamanın geliştirilip farklı ortamlarda (örneğin, geliştirici bilgisayarı, test sunucusu veya üretim ortamı) çalıştırılması genellikle bazı sorunlara yol açar. Örneğin:

- Geliştirici bilgisayarında çalışan bir uygulama, üretim sunucusunda çalışmayabilir. Çünkü geliştirme ortamında farklı kütüphaneler veya bağımlılıklar olabilir.

Bu durumu, bir ürünü farklı ülkelerde üretilen parçalarla monte etmeye benzetebiliriz. Her ülkenin standartları farklı olduğu için, parçalar bir araya geldiğinde uyumsuzluklar olabilir. Konteyner teknolojisi, tüm bu parçaların (uygulama ve bağımlılıkların) bir kutuya yerleştirilmesini sağlar ve bu kutu (container) dünyanın her yerinde aynı şekilde çalışır.

### Neden Konteyner Teknolojisi?

**Taşınabilirlik:** Bir konteynerde bulunan uygulama, nerede çalıştırılırsa çalıştırılsın, aynı şekilde çalışır. Geliştirici bilgisayarında çalışan bir uygulama, test ve üretim ortamında da aynı şekilde çalışır.

**Yalıtım:** Her konteyner, uygulamanın ve onun ihtiyaç duyduğu tüm bileşenlerin birbirinden izole edilmesini sağlar. Bu sayede farklı uygulamalar, aynı sunucuda birbiriyle çakışmadan çalışabilir.

**Hafiflik:** Konteynerler, sanal makinelerden (VM) çok daha hafiftir. Bir sunucuda onlarca sanal makine çalıştırmak zor olabilirken, aynı sunucuda yüzlerce konteyner çalıştırabilirsiniz.

### Konteynerler ve Sanal Makineler Arasındaki Fark

Konteynerler genellikle sanal makinelerle karıştırılır. İkisi de bir uygulamayı izole bir ortamda çalıştırır, ancak temel farkları şunlardır:

#### *Sanal Makineler (Virtual Machines - VM):*

- Bir sanal makine, bir sunucunun tamamını simüle eder. Her sanal makinenin kendine ait bir işletim sistemi vardır.
- Bu işletim sistemi, uygulamayı çalıştırmak için gerekli olan kütüphaneleri ve bileşenleri içerir.
- Sanal makineler, oldukça ağırdır ve çok fazla kaynak kullanır (CPU, RAM vb.).

#### *Konteynerler:*

- Konteynerler, işletim sistemi çekirdeğini (kernel) paylaşır ve sadece uygulamanın çalışması için gereken dosyaları ve bağımlılıkları içerir.
- Aynı sunucuda çalışan diğer konteynerlerle çekirdek ve bazı dosyaları paylaşır, bu da onları daha hafif yapar.
- Çok daha az kaynak kullanır, bu nedenle daha fazla konteyner çalıştırmak mümkündür.

### Docker Nedir?

Docker, konteyner teknolojisinin popüler bir uygulamasıdır. Docker, uygulamalarınızı konteyner adı verilen hafif ve taşınabilir paketler halinde çalıştırmanızı ve dağıtmanızı sağlar. Docker'ın sunduğu araçlar ve platform sayesinde, uygulamalarınızın taşınabilirliği ve yönetimi kolaylaşır.

#### Docker Nasıl Çalışır?
Docker, uygulamalarınızı çalıştırmak için bir Docker Engine kullanır. Bu motor, uygulamalarınızı konteyner olarak paketler ve bu konteynerlerin aynı şekilde her ortamda çalışmasını sağlar.

#### Docker Image ve Container:

#### *Docker Image (Görüntü):*
Uygulamanızın ve tüm bağımlılıklarının paketlendiği bir şablon gibidir. Bir image, uygulamanızı ve ihtiyaç duyduğu tüm kütüphaneleri içerir.

#### *Container (Konteyner):*
Bir Docker image’inden başlatılan çalışan bir örnektir. Yani bir image, uygulamanın çalışabilir halidir ve bir sunucuda birden fazla container oluşturulabilir.

#### Docker ile Uygulama Geliştirme Senaryosu

#### *Örnek Senaryo 1:* 

Geliştirici Ortamında Bir Web Uygulaması

Bir web geliştiricisi olduğunuzu düşünün. Yerel bilgisayarınızda Python ile bir web uygulaması geliştirdiniz. Uygulama yerel ortamınızda sorunsuz çalışıyor, ancak aynı uygulamayı test sunucusuna gönderdiğinizde bazı hatalar alıyorsunuz. Bu, iki ortamın farklı kütüphaneler veya işletim sistemi sürümleri kullanmasından kaynaklanabilir.

Docker ile bu sorunu çözmek oldukça kolaydır. Uygulamanız için bir Dockerfile oluşturursunuz. Bu dosya, uygulamanızın çalışması için hangi işletim sistemi, hangi bağımlılıklar ve hangi ayarların gerektiğini içerir. Docker bu dosyaya göre bir image oluşturur. Bu image’i her yerde çalıştırabilirsiniz: kendi bilgisayarınızda, test sunucusunda ya da üretim ortamında.

Docker sayesinde, uygulamanız her yerde aynı şekilde çalışır, çünkü her ortamda aynı image kullanılır.

## Kubernetes Bileşenleri

Kubernetes bileşenleri, Kubernetes'in dağıtılmış sistem mimarisini anlamanızı ve Kubernetes'in nasıl çalıştığını daha iyi kavramanızı sağlar. Bir Kubernetes kümesi (cluster), **master** (kontrol düzlemi) ve **worker** düğümleri olmak üzere iki ana bileşenden oluşur. Bu bileşenler birlikte çalışarak uygulamalarınızı container'lar (kapsayıcılar) içinde çalıştırır ve yönetir.

### 1. Master Node (Control Plane Components)

Kubernetes'in master bileşenleri, kümenin yönetiminden ve denetlenmesinden sorumludur. Bu bileşenler, uygulamaların nerede çalışacağını belirler, iş yüklerini dağıtır, worker nodeları izler ve gerektiğinde ölçeklendirme ve hata kurtarma gibi işlemleri yürütür. Master Node'un 4 temel bileşeni vardır: 

#### 1.1 API Server

- Tüm kontrol işlemlerini sağlar ve Kubernetes'in "merkezi yönetim noktası" olarak çalışır.

- Master sunucusuna gelen tüm requestlerin yönetilmesinden sorumlu yapıdır.

- Kullanıcılar kubernetes API Server'a komutlar göndererek clusterı yönetir. 

- API sunucusu, her türlü istek ve güncellemenin doğrulanmasından ve işlenmesinden sorumludur.

- Cluster'daki tüm bileşenler, Kube-API Server ile etkileşime geçerek bilgi alır ya da veri günceller.

#### 1.2 etcd

- Kubernetes'in dağınık yapılandırma veritabanıdır. Tüm Kubernetes kümesi verilerini tutar. 

- etcd, Kubernetes kümesinin durumunu depolayan bir dağıtılmış anahtar-değer veri deposudur. API sunucusu (Kube-API Server) bu verileri kullanarak kümeyi yönetir.

- Cluster'ın durum bilgilerini tutar (örneğin, hangi podların nerede çalıştığı).

#### 1.3 Scheduler

- Yeni podların hangi node üzerinde çalışacağını belirler.

- Bir pod oluşturulduğunda, Scheduler uygun bir node seçer ve podun bu node üzerinde çalışmasını sağlar.
- Scheduler, node'ların mevcut kaynakları, taahhüt edilen yükler ve kısıtlamalara (örn. affinity, tolerations) göre karar verir.

- Yük dengeleme ve kaynak optimizasyonunu sağlar.

---> *İyi performans sağlamak için podların uygun node'lara yerleştirilmesi kritiktir.*

#### 1.4 Controller Manager

- Kubernetes'te çeşitli kontrol süreçleri (controller) mevcuttur ve her biri kümeyi istenen durumda tutmakla görevlidir.

- Kube-Controller Manager bu kontrol süreçlerini tek bir yürütülebilir dosyada birleştirir.

##### **Önemli Kontrolcüler:**

*Replication Controller:* Pod'ların istenen sayıda olduğundan emin olur.

*Node Controller:* Node'ların çalışma durumunu izler ve yönetir.

*Endpoint Controller:* Hizmetler ve podlar arasındaki bağlantıyı düzenler.

### 2. Worker Node 

Worker node’lar, Kubernetes kümesi içinde uygulamalarınızın konteyner'ler halinde çalıştığı yerdir. Bu node’larda podlar bulunur ve bu bileşenler podların sağlıklı çalışmasını sağlar. 

#### *Pod: Konteyner'lerin Çalıştığı Alan*

- Pod, Kubernetes'in temel çalışma birimi olup, bir veya daha fazla konteyneri içerebilir ve bu konteynerler birlikte çalışarak uygulamanın belirli bir görevini yerine getirir.
- Podlar, bir uygulamanın çalıştığı ortamı oluşturur ve genellikle bir uygulamanın birbirine bağlı bileşenlerini barındırır. 
- Podlar, uygulamanın çalışması için gerekli kaynakları (CPU, hafıza, ağ) sağlar.
- Her pod, ortak bir ağ ve depolama alanı paylaşır, böylece konteynerler arasında iletişim ve veri paylaşımı kolaylaşır.

 

Her worker node'un kendi bileşeni vardır:

#### 2.1 Kubelet

- Her node’da podların çalışmasını ve yönetilmesini sağlar.

- API sunucusundan aldığı talimatları izler ve bu talimatlara göre node üzerinde podların oluşturulmasını ve çalıştırılmasını sağlar.

-  Podlar çalışırken, podların durumu hakkında bilgi toplar ve bu bilgileri API sunucusuna geri bildirir.

- Konteyner'leri başlatır ve kapsayıcıların canlılığını izler.

#### 2.2 Kube-Proxy

- Kubernetes hizmetleri (services) için ağ bağlantılarını ve yük dengeleme işlemlerini yönetir.

- Kube-Proxy, bir pod'a yönlendirilen ağ trafiğini yönetir.

- Kubernetes'te hizmetler (services), pod'lar arasında ağ iletişimini yönetir ve Kube-Proxy, ağ paketlerini doğru pod'a yönlendirir.

- Podlara IP adresi proxy sayesinde atanır. 

### 3. Container Runtime

- Container runtime, Kubernetes'in container’ları çalıştırmak için kullandığı yazılımdır.

- Kubernetes’in varsayılan olarak kullandığı container runtime Docker veya containerd olabilir.

- Container'ların oluşturulmasından, çalıştırılmasından ve yönetilmesinden sorumludur.

## Kubernetes Terminolojisi

Kubernetes ile ilgili bazı temel terimler ve anlamları aşağıda açıklanmıştır:

### 1. Deployment
**- Uygulamayı başlatır:** Uygulamayı podlar içinde başlatır ve çalıştırır.

**- Ölçeklendirme:** Uygulamanın birden fazla kopyasını (podunu) aynı anda çalıştırmak istediğinizde deployment daha fazla pod ekler ve podlar arasında yükü dengeler.

**- Güncelleme:** Uygulamanın yeni bir sürümü geldiğinde deployment eski sürümü kapatıp yeni sürümü kolay ve güvenli bir şekilde dağıtmanıza olanak tanır.

Deployment, güncellemeleri gerçekleştirmek için iki ana strateji sunar:

---> *Rolling Update (Aşamalı Güncelleme):*

Bu, varsayılan güncelleme stratejisidir. Aşamalı güncelleme, mevcut pod'ların bir kısmını kaldırırken yenilerini ekler. Bu sayede, uygulamanızda her zaman belirli bir sayıda pod aktif kalır.
Örneğin, eğer 5 pod’unuz varsa, güncelleme sırasında önce 1 pod güncellenir, ardından yeni pod aktif hale geldikten sonra diğer pod’lar güncellenir. Bu, kesintisiz h
izmet sağlar.

---> *Recreate (Yeniden Oluşturma):*

Bu stratejide, mevcut pod'lar tamamen kaldırılır ve ardından yeni pod'lar oluşturulur. Bu yöntem, güncelleme sırasında uygulamanızda kısa süreli bir kesintiye yol açabilir, bu nedenle dikkatli kullanılmalıdır.

**- Kendi Kendini İyileştirme:** Bir pod çökerse veya sorun yaşarsa deployment bunu fark eder ve yeni bir pod başlatarak uygulamanın kesintisiz çalışmasını sağlar.

**Örnek Kullanım:** Bir web uygulamasını dağıtmak istediğinizde, bir Deployment oluşturarak istenen pod sayısını belirtebilir ve gerektiğinde uygulamanızı güncelleyebilirsiniz.

### 2. Service

Kubernetes'te çalışan podların dış dünyaya ya da küme içindeki diğer uygulamalar tarafından erişilebilir olmasını sağlayan bir soyutlama kavramıdır.

Service, pod'ların dinamik IP adresleri değişebileceği için, onlara sabit bir erişim noktası sağlar. Pod'lar yeniden başlatıldığında veya yeni pod'lar eklendiğinde, Service bu değişiklikleri yönetir ve dışarıdan gelen istekleri doğru pod'lara yönlendirir.

#### 2.1 Service Özellikleri:

##### **2.1.1. Podlara Sabit Erişim** 

Podlar geçici olduğu için, yeniden başlatıldıklarında yeni bir IP adresi alabilirler.

Service, podların dinamik IP'leri gizleyip, sabit bir DNS adı ve veya IP adresi sağlar. Böylece uygulamalar service aracılığıyla podlara kesintisiz erişebilir.

##### **2.1.2 Yük Dengeleme (Load Balancing)**

Bir servis aynı anda birden fazla poda erişim sağlar. BU sayede uygulama trafiği bu podlar arasında dengelenir. Bu yük dengeleme sistemdeki trafiği optimize eder ve podların dengeli bir şekilde iş yükü almasını sağlar.

##### **2.1.3 Küme İçi İletişim (Cluster Internal Communicaiton)**

Service, pod'ların bir grup sabit IP adresi ve DNS adı aracılığıyla küme içindeki diğer bileşenlerle iletişim kurmasına olanak tanır. Bu sayede, Kubernetes üzerinde dağıtık çalışan uygulamalar sorunsuz şekilde haberleşebilir.

Service, bir uygulamanın çeşitli bileşenlerinin birbirine ihtiyaç duyduğu senaryolarda özellikle önemlidir. Örneğin, bir frontend servisi, backend servisi ile veya bir uygulama, bir veritabanı ile konuşmak isteyebilir. Kubernetes'te servisler bu iletişimi birkaç farklı şekilde sağlar.

Küme içi iletişimde en yaygın kullanılan servislere bir örnek:

###### **2.1.3.1 ClusterIP Servisi:**

ClusterIP, bir servisin yalnızca Kubernetes kümesinin içinden erişilebilir olmasını sağlar. Bu servis tipi, küme dışından erişim gerektirmeyen ve yalnızca diğer pod'lar tarafından kullanılacak hizmetler için idealdir.
Örnek kullanım: Bir veritabanı servisi sadece küme içindeki uygulama pod'ları tarafından erişilebilecekse ClusterIP servisi kullanılır.

```
yaml
----------------------
apiVersion: v1
kind: Service
metadata:
  name: my-database
spec:
  selector:
    app: my-database
  ports:
    - protocol: TCP
      port: 3306
      targetPort: 3306
  type: ClusterIP

```

##### **2.1.3.4 Harici Erişim (External Access)**

Kubernetes kümeleri genellikle iç kaynaklar arasındaki iletişimle sınırlı olsa da, bazı durumlarda dış dünya ile de iletişim kurması gerekebilir. Uygulamanın dış kullanıcılardan gelen talepleri işleyebilmesi için küme dışından erişime açık hale getirilmesi gerekmektedir. Bu duruma harici erişim (external access) denir.

Kubernetes, uygulamaların dış dünyaya açılmasını sağlayan birkaç yöntem sunar:

##### **Harici Erişim Sağlayan Servis Türleri**

**NodePort Servisi**

- NodePort, pod'ları dışarıya açmak için en temel servis türüdür.
- Bu servis, her Kubernetes düğümünde (node) belirli bir portu açar ve bu port üzerinden gelen trafiği pod'lara yönlendirir.
- NodePort servisinin atanmış bir portu vardır (genellikle 30000–32767 arasında). 
- Dış kullanıcılar, kümenin IP adresi ve bu port üzerinden erişim sağlar.


```
yaml
---------------------------
apiVersion: v1
kind: Service
metadata:
  name: my-nodeport-service
spec:
  type: NodePort
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 8080
      nodePort: 30007
```

**Senaryo: Bir web uygulamasını dış dünyaya açmak için NodePort servisi kullanılır. Örneğin, http://<node-ip>:30007 adresinden bu web uygulamasına dışarıdan erişilebilir.**

**2. LoadBalancer Servisi**

- LoadBalancer, daha geniş bir harici erişim ihtiyacı olduğunda kullanılır. 
- Bu servis türü, genellikle bulut ortamlarında (AWS, GCP, Azure gibi) kullanılır ve dış dünyaya bir IP adresi atanarak otomatik olarak bir yük dengeleyici (load balancer) oluşturulur.
- LoadBalancer servisi, trafik yoğunluğunu birden fazla düğüm arasında dağıtarak yüksek ölçeklenebilirlik ve erişilebilirlik sağlar.

```
yaml
------------------------------
apiVersion: v1
kind: Service
metadata:
  name: my-loadbalancer-service
spec:
  type: LoadBalancer
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 8080
```

**Senaryo: Bir e-ticaret platformu gibi yüksek trafikli bir uygulama, LoadBalancer servisi kullanılarak dış dünyaya açılır. Bu servis, küme dışından gelen talepleri ilgili pod'lara yönlendirir ve trafiği dengeler. Örneğin, http://<external-ip> adresi üzerinden dış dünyadan erişim sağlanır.** 

**3. Ingress**

- Ingress, Kubernetes'teki daha gelişmiş bir harici erişim yöntemidir. 
- Ingress, HTTP ve HTTPS taleplerini yönetir ve genellikle farklı servisleri dışarıya açarken daha iyi bir yönetim sağlar. 
- Bu yapı, load balancer veya NodePort servislerinden daha esnek bir çözüm sunar.
- Ingress, bir veya daha fazla servise harici HTTP(S) erişimi sağlayabilir, aynı zamanda yük dengeleme, SSL/TLS sonlandırma ve sanal ana bilgisayar bazlı yönlendirme gibi özellikleri destekler.

```
yaml
--------------------------------
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
    - host: my-app.example.com
      http:
        paths:
        - path: /
          pathType: Prefix
          backend:
            service:
              name: my-app-service
              port:
                number: 80
```
**Senaryo: Bir şirketin farklı alt alan adlarına (subdomain) sahip birkaç servisi olabilir, örneğin, app.example.com ve shop.example.com. Ingress kullanılarak, gelen talepler ilgili servislere yönlendirilir. SSL desteğiyle HTTPS güvenliği de sağlanabilir.**

**---> Ingress vs. Service**

**Service (NodePort/LoadBalancer):** Daha düşük seviyede bir ağ yönlendirme çözümüdür. Her bir servis için ayrı ayrı yük dengeleyiciler veya açık portlar kullanılır.

**Ingress:** Tek bir giriş noktası sağlayarak HTTP(S) isteklerinin daha verimli ve yönetilebilir şekilde yönlendirilmesini sağlar.


##### **3. Namespace**

Namespace, Kubernetes kümesinde kaynakların mantıksal olarak ayrılmasını sağlar. Farklı projeler veya takımlar için ayrı ortamlar oluşturmak için kullanılır. Her namespace, kendi kaynaklarına ve erişim kontrolüne sahiptir.

*Örnek Kullanım:* Farklı projeler üzerinde çalışıyorsanız, her proje için ayrı bir namespace oluşturarak kaynakları daha düzenli bir şekilde yönetebilirsiniz.

##### **4. Volume**

Volume, pod'lar arasında veri paylaşımı ve kalıcı depolama sağlamak için kullanılır. Pod'lar silindiğinde bile verilerin kaybolmaması için kalıcı bir depolama alanı sağlar.

*Örnek Kullanım:* Veritabanı verilerinin depolanması için bir volume oluşturabilir ve bu volume'u veritabanı konteynerinize bağlayarak veri kaybını önleyebilirsiniz.

##### **5. Config Map**

ConfigMap, uygulama yapılandırmalarını depolamak için kullanılır. Uygulama koduna sıkı sıkıya bağlı olmayan yapılandırma verilerini dışarıda tutarak, uygulamaların daha esnek olmasını sağlar.

*Örnek Kullanım:* Uygulamanızın çalışma ortamına göre farklı ayar dosyaları kullanıyorsanız, bu ayarları ConfigMap ile yönetebilirsiniz.

##### **6. Secret**

Secret, hassas bilgileri (şifreler, token'lar, vs.) saklamak için kullanılır. Secret'lar, verilerin güvenli bir şekilde depolanmasını sağlar.

*Örnek Kullanım:* Bir veritabanına erişim için gerekli olan şifreleri Secret olarak tanımlayarak, uygulamanızın bu verilere güvenli bir şekilde erişmesini sağlayabilirsiniz.

##### **7. ReplicaSet** 

ReplicaSet, belirli sayıda pod'un her zaman çalışmasını sağlamak için kullanılır. Bir Deployment'ın bir parçası olarak, otomatik ölçeklendirme ve hata yönetimi sağlar.

*Örnek Kullanım:*  Uygulamanızın her zaman en az üç örneği çalışsın istiyorsanız, bir ReplicaSet kullanarak bu durumu güvence altına alabilirsiniz.

## Kubernetes'in Çalışma Şekli: Aşama Aşama İnceleme

Kubernetes, bir komut alındığında bu komutun nasıl işlendiğine dair bir dizi aşamadan oluşan bir iş akışına sahiptir. İşte bu sürecin aşama aşama açıklaması:

### Aşama 1: Komut Gönderimi

**Kullanıcı veya Uygulama İle Etkileşim:** Kullanıcı, bir komut gönderir. Bu komut, genellikle **kubectl** komutu aracılığıyla terminalden gönderilir. Örneğin, bir pod oluşturmak için kubectl apply -f pod.yaml komutu kullanılır.

**kubectl**, Kubernetes ile etkileşim kurmak için kullanılan komut satırı aracıdır. Kubernetes cluster'ınızı yönetmek için gerekli komutları vermenizi sağlayan bir araçtır.

### Aşama 2: API Server’a İletim

**API Server:** Kullanıcının gönderdiği komut, Kubernetes API Server'a ulaşır. API Server, kontrol düzleminin merkezi bileşenidir ve gelen istekleri işler. API Server, istekleri doğrular ve uygun kaynak türüne (pod, deployment, service vb.) yönlendirir.

### Aşama 3: Durum Güncellemeleri

**Etcd:** API Server, gelen komutu bir durumu güncelleme olarak etcd veritabanına kaydeder. etcd, kümenin tüm durum bilgilerini tutan bir anahtar-değer deposudur. Bu aşamada, istekler bir sıraya alınır ve daha sonra işlenmek üzere bekletilir.

### Aşama 4: Kontrol Düzlemi Yönetimi

**Kontrol Yöneticileri:** API Server, belirli bir kaynak türü için uygun kontrol yöneticisine (controller) yönlendirme yapar. Örneğin, bir deployment oluşturulması durumunda, Deployment Controller devreye girer. Kontrol yöneticisi, istenen durumu mevcut durumla karşılaştırır.

### Aşama 5: Node’ların Güncellenmesi

**Kubelet ve Node:** Kontrol yöneticisi, yeni bir pod oluşturma gerekliliği varsa, ilgili node’lara komut gönderir. Bu, kubelet aracılığıyla gerçekleştirilir. Kubelet, her node'da çalışan ve Kubernetes’in yönlendirdiği pod'ları yöneten bir ajandır.

### Aşama 6: Pod Oluşturma

**Konteynerin Başlatılması:** Kubelet, belirtilen özelliklere (imaj, ağ, depolama vb.) göre pod'u oluşturur. Pod oluşturma süreci, ilgili konteyner görüntüsünü çekmek ve konteyneri başlatmak için gerekli adımları içerir.

### Aşama 7: Ağ ve Servis Ayarları

**Ağ Yapılandırması:** Pod başlatıldığında, Kubernetes otomatik olarak pod'a bir IP adresi atar ve gerekirse service bileşenini kullanarak ağ yapılandırmasını sağlar. Bu, pod'lar arasında iletişim kurulmasını ve dış dünya ile bağlantı sağlanmasını mümkün kılar.

### Aşama 8: Durumun İzlenmesi

**Sağlık Kontrolleri:** Kubelet, başlatılan pod’un durumunu izler. Pod'un sağlık durumunu kontrol etmek için belirlenen sağlık kontrollerini (liveness ve readiness probes) kullanır. Eğer pod düzgün çalışmıyorsa, kubelet yeni bir pod başlatır.

### Aşama 9: Geri Bildirim

**Durum Güncellemesi:** Kullanıcıya, pod'un başarıyla oluşturulduğuna dair bir geri bildirim sağlanır. Bu, **kubectl** komutunun çıktısı olarak terminalde gösterilir.

### Aşama 10: Sürekli İzleme ve Güncellemeler

**Otomatik Yönetim:** Kubernetes, sürekli olarak küme durumunu izler ve belirli koşullara göre otomatik güncellemeler yapar. Örneğin, bir pod beklenmedik bir şekilde kapanırsa, kontrol düzlemi bunu algılar ve otomatik olarak yeni bir pod başlatır.

![Kubernetes control plane ve worker node bileşenleri](image.png)




## Kubernetes ile Etkili Çalışma: Önemli Komutlar

### Konteyner ve Pod Yönetimi

#### - Tüm Pod'ları Listele:

`kubectl get pods`

#### - Bir Pod'un Detaylarını Görüntüle:

`kubectl describe pod <pod_adı>`

#### - Belirli Bir Namespace İçindeki Pod'ları Listele:

`kubectl get pods -n <namespace_adı>`

#### - Pod Oluştur:

`kubectl create -f <pod_yml_dosyası.yml>`

#### - Podu Sil:

`kubectl delete pod <pod_adı>`

### Uygulama Dağıtımı

#### - Deployment Oluştur:

`kubectl create deployment <deployment_adı> --image=<image_adı>`

#### - Tüm Dağıtımları Listele:

`kubectl get deployments`

#### - Dağıtımı Güncelle:

`kubectl set image deployment/<deployment_adı> <container_adı>=<yeni_image_adı>`

#### - Dağıtımı Sil:

`kubectl delete deployment <deployment_adı>`

### Hizmet Yönetimi

#### - Tüm Servisleri Listele:

`kubectl get services`

#### - Servis Oluştur:

`kubectl expose deployment <deployment_adı> --type=LoadBalancer --port=80`

#### - Servisi Sil:

`kubectl delete service <servis_adı>`

### Küme Durumunu İzleme

#### - Küme Bilgilerini Görüntüle:

`kubectl cluster-info`

#### - Tüm Düğümleri Listele:

`kubectl get nodes`

#### - Düğüm Detaylarını Görüntüle:

`kubectl describe node <düğüm_adı>`

###  Kubernetes Kayıtları

#### - Node Kayıtlarını İzle:

`kubectl top nodes`

#### - Pod Kayıtlarını Görüntüle:

`kubectl logs <pod_adı>`

#### - Podların Durumunu İzle:

`kubectl top pods`

#### - Pod Kayıtlarını İzle:

`kubectl logs -f <pod_adı>`

### Kaynak Yönetimi

#### - Kubernetes Kaynaklarını YAML Formatında Görüntüle:

`kubectl get <kaynak_türü> <kaynak_adı> -o yaml`

#### - Kaynakları Güncelle:

`kubectl apply -f <yml_dosyası.yml>`

### Config ve Secret Yönetimi

#### - ConfigMap Oluştur:

`kubectl create configmap <configmap_adı> --from-file=<dosya_adı>`

#### - Secret Oluştur:

`kubectl create secret generic <secret_adı> --from-literal=<anahtar>=<değer>`

### Namespace Yönetimi

#### - Tüm Namespace'leri Listele:

`kubectl get namespaces`

#### - Yeni Namespace Oluştur:

`kubectl create namespace <namespace_adı>`

### Yardımcı Komutlar

#### Yardım Al:

`kubectl help`

## Kubernetes Best Practices: Örnek Senaryolarla Adım Adım Kılavuz

### 1. Yapılandırma Yönetimi: ConfigMap ve Secret Kullanımı

**Senaryo: Bir web uygulaması geliştiriyorsunuz ve uygulamanın veritabanı bağlantı ayarlarını ve API anahtarlarını yönetmeniz gerekiyor.**

- **Çözüm:** Veritabanı bağlantı detaylarını bir ConfigMap içinde saklayın. API anahtarlarını ise daha güvenli olan Secret nesnesi kullanarak yönetin.

- **Neden Önemli?** Böylece, uygulama yapılandırmasını güncellerken Pod’ları yeniden başlatmanız gerekmez ve hassas bilgileri güvenli bir şekilde saklayabilirsiniz.

```
yaml
---------------------------------------------
apiVersion: v1
kind: Secret
metadata:
  name: api-secrets
data:
  api-key: c2VjcmV0X2tleQ==  # base64 encoded

```

### 2. Kaynak Limitleri ve Talepleri

**Senaryo: Farklı kaynaklar kullanan mikro servislerinizi Kubernetes üzerinde çalıştırıyorsunuz, ancak bazıları çok fazla bellek tüketiyor ve düzensiz çalışıyor.**

**Çözüm:** Mikro servislerin her birine uygun CPU ve bellek limitleri koyun. Örneğin, bir servisin en fazla 500Mi bellek kullanmasına izin verebilirsiniz.

**Neden Önemli?** Bu sayede, bir uygulamanın aşırı kaynak tüketmesinin diğer servisleri etkilemesini önlersiniz.

```
yaml
-------------------
resources:
  requests:
    memory: "128Mi"
    cpu: "500m"
  limits:
    memory: "500Mi"
    cpu: "1"

```

###  3. Rolling Update ile Kesintisiz Güncelleme

**Senaryo: Kullanıcılar tarafından aktif olarak kullanılan bir e-ticaret uygulamasını güncellemeniz gerekiyor.**

**Çözüm:** Bir Deployment oluşturup rolling update stratejisini kullanarak uygulamanızı yeni sürüme güncelleyin. Böylece eski sürüm çalışmaya devam ederken yeni sürüm yavaşça yayına girer.
Neden Önemli? Kullanıcılar güncelleme sırasında uygulamayı kesintisiz kullanmaya devam eder.

```
yaml
---------------------
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 1
    maxSurge: 1
```

### 4. Network Policies ile Güvenlik

**Senaryo: Mikro servis tabanlı bir uygulamanız var ve her mikro servis sadece belirli servislerle iletişim kurmalı.**

**Çözüm:** Network Policy kullanarak Pod’lar arasında izin verilen ağ trafiğini kısıtlayın. Örneğin, sadece belirli Pod’ların bir veritabanına erişmesine izin verebilirsiniz.

**Neden Önemli?** Mikro servisler arası gereksiz ve riskli ağ trafiğini engelleyerek güvenliği artırırsınız.

```
yaml
--------------------------------
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-access
spec:
  podSelector:
    matchLabels:
      role: database
  ingress:
  - from:
    - podSelector:
        matchLabels:
          role: backend

```

### 5. Monitoring ve Logging ile Performans Takibi

**Senaryo: Kubernetes üzerinde çalışan bir web uygulamanız var ve performans sorunları yaşanıyor.**

**Çözüm:** Prometheus ve Grafana ile tüm sistem performansını izleyin, ayrıca ELK Stack ile log’ları merkezi bir yerde toplayıp analiz edin.

**Neden Önemli?** Performans sorunlarını ve hataları hızlıca tespit edip çözüm bulmanızı sağlar.

### 6. Helm Kullanımı ile Dağıtım Kolaylığı

**Senaryo:** Birden fazla servis içeren karmaşık bir uygulamanız var ve bu uygulamayı farklı ortamlara (test, staging, production) dağıtmanız gerekiyor.

**Çözüm:** Helm Chart kullanarak uygulamanızın tüm bileşenlerini paketleyin ve kolayca farklı ortamlara dağıtın.

**Neden Önemli?** Uygulamanızın her seferinde elle yapılandırılması yerine, paketlenmiş bir çözümle kolay dağıtım ve güncelleme yapabilirsiniz.

### 7. RBAC ile Erişim Kontrolü

**Senaryo: Birden fazla ekip Kubernetes kümesinde çalışıyor ve herkesin farklı düzeyde erişim hakkına sahip olması gerekiyor.**

**Çözüm:** RBAC (Role-Based Access Control) kullanarak her ekip üyesine veya servise sadece ihtiyaç duyduğu kaynaklara erişim hakkı verin.

**Neden Önemli?** Güvenliği artırarak sadece yetkilendirilmiş kişilerin kritik işlemler yapabilmesini sağlarsınız.

```
yaml
----------------------------------------
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: dev
  name: developer-role
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "create"]
```

### 8. Persistent Volume Kullanımı ile Veri Yönetimi

**Senaryo: Bir veritabanı uygulaması Kubernetes üzerinde çalışıyor ve veriler kaybolmamalı.**

**Çözüm:** Verileri kalıcı olarak saklamak için bir Persistent Volume (PV) oluşturun ve bu PV’yi Persistent Volume Claim (PVC) ile Pod’a bağlayın.

*Neden Önemli?** Pod yeniden başlatılsa bile veriler kalıcı olarak saklanır ve veri kaybı önlenir.

```
yaml
---------------------------
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: database-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
```

### 9. CI/CD Entegrasyonu ile Otomatik Dağıtım

**Senaryo: Uygulamanız sürekli güncelleniyor ve manuel dağıtım yapmak zaman alıyor.**

**Çözüm:** Jenkins veya GitLab CI gibi bir CI/CD aracı kullanarak kod değişikliklerini otomatik olarak Kubernetes’e dağıtın.

**Neden Önemli?** Sürekli dağıtım yaparak yeni özellikleri hızlıca yayına alabilirsiniz.


