# 🐳 Docker Rehberi

## 🧠 Docker Nedir?

Docker, uygulamaları bağımlılıklarıyla birlikte paketleyip izole **konteynerler (containers)** içinde çalıştırmayı sağlayan bir platformdur. Böylece geliştirme, test ve sunucu ortamları arasında tutarlı bir çalışma ortamı oluşturmayı kolaylaştırır.

### Temel Kavramlar

| Kavram | Açıklama |
|--------|----------|
| **İmaj (image)** | Uygulama kodunu, çalışma ortamını, kütüphaneleri ve gerekli dosyaları içeren, salt okunur bir şablondur. |
| **Konteyner (container)** | Bir imajdan oluşturulan, kendi yazılabilir katmanı ve yapılandırması olan izole çalışma ortamıdır. Çalışıyor veya durmuş olabilir. |
| **Dockerfile** | Bir imajın nasıl oluşturulacağını tarif eden metin dosyasıdır. |
| **Registry** | İmajların saklandığı ve paylaşıldığı servistir. Docker Hub bir registry örneğidir. |
| **Volume** | Konteynerden bağımsız olarak veri saklamak için Docker tarafından yönetilen depolama alanıdır. |
| **Docker Compose** | Servisleri, ağları ve volume'leri bir YAML dosyasında tanımlayıp birlikte yönetmeyi sağlar. |

Bir imajdan birden fazla konteyner oluşturulabilir. Konteynerde yapılan dosya değişiklikleri kaynak imajı değiştirmez.

Konteynerler ayrı bir işletim sistemi çekirdeği çalıştırmak yerine çalıştıkları Docker ortamının çekirdeğini paylaşır. macOS ve Windows'ta Docker Desktop, Linux konteynerleri için bir Linux sanal makinesi kullanır.

Kaynak: [Docker imajları](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-an-image/).

#### Nginx Nedir?

**Nginx** ("engine-x" diye okunur), web sitelerini ve uygulamaları internete sunmak için kullanılan bir **web sunucusudur**. Bu rehberdeki `nginx` imajı, Nginx'i bir Docker konteynerinde çalıştırmanı sağlar.

- **Dosya sunar:** HTML, CSS, JavaScript ve görselleri tarayıcıya gönderir.
- **Reverse proxy (ters vekil) olarak çalışır:** Gelen istekleri arkada çalışan uygulamaya yönlendirir.
- **Yük dengeler:** İstekleri birden fazla uygulama sunucusuna dağıtabilir.
- **HTTPS bağlantısını yönetir:** TLS sertifikalarını kullanarak şifreli bağlantı sağlar.

Bir uygulamanın önünde kullanıldığında istek akışı şöyle olabilir:

```text
Kullanıcı → Nginx konteyneri → Uygulama konteyneri
```

Örneğin uygulaman `3000` portunda çalışırken Nginx, uygun yapılandırmayla `https://site.com` adresine gelen istekleri bu uygulamaya iletebilir. Rehberdeki temel örneklerde ise Nginx, HTML dosyalarını doğrudan sunar.

---

## 💻 Başlangıç ve Kurulum Kontrolü

İşletim sistemine uygun kurulum adımları için [Docker kurulum belgelerini](https://docs.docker.com/get-started/get-docker/) kullan. Docker Desktop kullanıyorsan komutları çalıştırmadan önce uygulamayı başlat.

| Komut | Açıklama |
|-------|----------|
| `docker --version` | Docker CLI sürümünü gösterir; tek başına Docker Engine'in çalıştığını doğrulamaz. |
| `docker version` | İstemci ve erişilebiliyorsa sunucu (Engine) sürümünü gösterir. |
| `docker info` | Docker Engine ve çalışma ortamı hakkında bilgi verir. |
| `docker compose version` | Compose sürümünü gösterir. |
| `docker run --rm hello-world` | Test imajını gerekirse indirir, konteyneri çalıştırır ve işlem bitince konteyneri siler. |

---

## 📦 İmaj Komutları

İmaj komutlarında imaj adı veya ID kullanılabilir; `docker pull` için registry üzerindeki imaj adı gerekir. Registry adresi belirtilmeyen bu örneklerde Docker Hub kullanılır.

Aşağıdaki örneklerde `nginx:alpine`, `nginx` imajının `alpine` etiketini belirtir. Etiket verilmezse varsayılan olarak `latest` kullanılır. `latest`, her zaman en yeni sürüm anlamına gelmez; etiketler zaman içinde farklı imajları gösterebilir.

| Komut | Açıklama |
|-------|----------|
| `docker search nginx` | Docker Hub üzerinde imaj arar. |
| `docker pull nginx` | Docker Hub'dan `nginx:latest` imajını indirir. |
| `docker pull nginx:alpine` | Docker Hub'dan `nginx` imajının `alpine` etiketini indirir. |
| `docker images` veya `docker image ls` | Bilgisayarındaki yerel imajları listeler. |
| `docker image inspect nginx:alpine` | İmajın ayrıntılarını gösterir. |
| `docker image history nginx:alpine` | İmaj katmanlarının geçmişini gösterir. |
| `docker build -t rehber-web:1.0 .` | Geçerli dizini build context olarak kullanarak imaj oluşturur; örnek aşağıdadır. |
| `docker rmi rehber-web:1.0` veya `docker image rm rehber-web:1.0` | Yerel imaj etiketini kaldırır; başka etiketi kalmazsa imajı da siler. Bir konteyner tarafından kullanılıyorsa işlem engellenebilir. |

---

## ▶️ Konteyner Çalıştırma

Yerel bir web sunucusu başlat:

```bash
docker run -d --name rehber-nginx -p 127.0.0.1:8080:80 nginx:alpine
```

Tarayıcıda `http://localhost:8080` adresini aç.

- `-d`: Konteyneri arka planda çalıştırır.
- `--name rehber-nginx`: Konteynere isim verir.
- `-p 127.0.0.1:8080:80`: Bilgisayarın yerel `8080` portunu konteynerin `80` portuna yönlendirir.
- `nginx:alpine`: Kullanılacak imajı belirtir; yerelde yoksa varsayılan olarak indirilir.

Port eşlemesi `HOST_IP:HOST_PORT:CONTAINER_PORT` biçimindedir. `-p 8080:80` kullanıldığında port varsayılan olarak tüm ağ arayüzlerinde yayımlanır. Yukarıdaki örnek erişimi yerel makineyle sınırlar. Ayrıntılar: [Port yayımlama](https://docs.docker.com/engine/network/port-publishing/).

### Konteyner Yönetimi

Bu komutlar yukarıda oluşturulan `rehber-nginx` konteynerini kullanır:

| Komut | Açıklama |
|-------|----------|
| `docker ps` | Çalışan konteynerleri listeler. |
| `docker ps -a` | Durmuş olanlar dahil tüm konteynerleri listeler. |
| `docker logs -f rehber-nginx` | Logları gösterir ve yeni logları takip eder; `Ctrl+C` takibi sonlandırır. |
| `docker exec -it rehber-nginx sh` | Çalışan konteynerde etkileşimli bir shell açar; `exit` ile çıkılır. |
| `docker inspect rehber-nginx` | Konteynerin ayrıntılarını gösterir. |
| `docker stop rehber-nginx` | Konteyneri durdurur. |
| `docker start rehber-nginx` | Durmuş konteyneri yeniden başlatır. |
| `docker restart rehber-nginx` | Konteyneri yeniden başlatır. |
| `docker rm rehber-nginx` | Durmuş konteyneri siler; önce `docker stop` kullan. |

`docker run` her seferinde yeni bir konteyner oluşturur; `docker start` mevcut konteyneri başlatır. Aynı isimde bir konteyner varsa yeni `docker run --name ...` komutu hata verir.

`--rm`, konteyner durduğunda onu otomatik siler; imajı silmez. Konteynerin ana işlemi bittiğinde konteyner de durur. Kaynak: [docker run başvurusu](https://docs.docker.com/reference/cli/docker/container/run/).

---

## 🛠️ Dockerfile ile İmaj Oluşturma

Boş bir örnek klasöründe `index.html` dosyası oluştur:

```html
<!doctype html>
<html lang="tr">
  <meta charset="utf-8">
  <title>Docker Rehberi</title>
  <h1>Merhaba Docker!</h1>
</html>
```

Aynı klasörde `Dockerfile` adında, uzantısız bir dosya oluştur:

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

- `FROM`: Temel imajı belirler.
- `COPY`: Build context içindeki dosyayı imaja kopyalar.
- `EXPOSE`: Uygulamanın kullandığı portu belgeler; tek başına portu bilgisayara açmaz.
- `RUN`: İmaj oluşturulurken komut çalıştırır.
- `CMD`: Konteyner başladığında kullanılacak varsayılan komutu veya argümanları tanımlar. Bu örnek, temel Nginx imajının başlatma ayarlarını kullanır.

Dosyaların bulunduğu klasörde çalıştır:

```bash
docker build -t rehber-web:1.0 .
docker run -d --name rehber-web -p 127.0.0.1:8081:80 rehber-web:1.0
```

`http://localhost:8081` adresinde kendi sayfanı görebilirsin. `index.html` değiştiğinde imajı yeniden oluşturup konteyneri yeni imajdan yeniden oluşturman gerekir.

Build context'e eklenmesini istemediğin dosyaları `.dockerignore` ile hariç tutabilirsin. Kaynak: [Dockerfile başvurusu](https://docs.docker.com/reference/dockerfile/).

---

## 💾 Kalıcı Veri: Volume ve Bind Mount

Konteynerin yazılabilir katmanındaki veriler konteyner durdurulduğunda korunur, konteyner silindiğinde kaybolur. Kalıcı veri için volume veya bind mount kullanılır.

**Volume**, Docker tarafından yönetilir. Aşağıdaki örnekte ilk konteyner silinse bile ikinci konteyner dosyayı okuyabilir:

```bash
docker volume create rehber-veri
docker run --rm --mount type=volume,source=rehber-veri,target=/veri alpine sh -c 'echo Merhaba > /veri/not.txt'
docker run --rm --mount type=volume,source=rehber-veri,target=/veri alpine cat /veri/not.txt
```

**Bind mount**, bilgisayardaki belirli bir dosya veya klasörü konteynere bağlar. Önceki örnekteki `index.html` dosyasının bulunduğu klasörde çalıştır:

```bash
docker run --rm --name rehber-canli -p 127.0.0.1:8082:80 \
  --mount type=bind,source="$(pwd)/index.html",target=/usr/share/nginx/html/index.html,readonly \
  nginx:alpine
```

`http://localhost:8082` adresini aç. Yerel dosyayı düzenleyip sayfayı yenilediğinde değişiklik görünür. `readonly`, konteynerin bağlı dosyaya yazmasını engeller. `Ctrl+C` ile sunucuyu durdurabilirsin.

| Komut | Açıklama |
|-------|----------|
| `docker volume ls` | Volume'leri listeler. |
| `docker volume inspect rehber-veri` | Volume ayrıntılarını gösterir. |
| `docker volume rm rehber-veri` | Kullanımda olmayan volume'ü **içindeki verilerle birlikte siler**. |

Kaynak: [Docker volume belgeleri](https://docs.docker.com/engine/storage/volumes/).

---

## 🧩 Docker Compose

Önceki `Dockerfile` ve `index.html` ile aynı klasöre `compose.yaml` dosyasını ekle:

```yaml
services:
  web:
    build: .
    ports:
      - "127.0.0.1:8083:80"
```

Bu örnek tek servis içerir; aynı dosyaya veritabanı gibi başka servisler de eklenebilir.

| Komut | Açıklama |
|-------|----------|
| `docker compose config` | Yapılandırmayı doğrular ve çözümlenmiş halini gösterir. |
| `docker compose up -d --build` | İmajı oluşturur, servisleri arka planda başlatır; değişen servisleri gerektiğinde yeniden oluşturur. |
| `docker compose ps` | Servislerin durumunu gösterir. |
| `docker compose logs -f` | Servis loglarını takip eder. |
| `docker compose exec web sh` | Çalışan `web` servisinin konteynerinde shell açar. |
| `docker compose stop` | Konteynerleri silmeden servisleri durdurur. |
| `docker compose down` | Projenin konteynerlerini ve Compose tarafından oluşturulan ağlarını kaldırır. |

Komutları `compose.yaml` dosyasının bulunduğu klasörde çalıştır. Uygulamaya `http://localhost:8083` adresinden erişebilirsin.

`docker compose down` varsayılan olarak named volume'leri silmez. `docker compose down -v`, Compose dosyasında tanımlanan named volume'leri ve konteynerlere bağlı anonim volume'leri de **verileriyle birlikte siler**; external volume'leri kaldırmaz.

Kaynaklar: [Compose başlangıç rehberi](https://docs.docker.com/compose/gettingstarted/), [docker compose down](https://docs.docker.com/reference/cli/docker/compose/down/).
