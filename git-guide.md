## 🧠 Git & GitHub Nedir?

- **Git**, Linus Torvalds tarafından geliştirilmiş bir versiyon kontrol sistemidir.
- **GitHub**, Git projelerini internet ortamında saklamak ve paylaşmak için kullanılan bir platformdur.
- **Commit**, yapılan değişiklikleri kaydederek projenin belirli bir noktasını sabitler.
- Git, günümüzde yazılım dünyasında en yaygın kullanılan versiyon kontrol sistemidir ve endüstri standardı haline gelmiştir.

---

## 💻 Terminal Komutları

| Komut | Açıklama |
|-------|----------|
| `ls` | Klasördeki içerikleri listeler |
| `ls -la` | Gizli dosyalar dahil her şeyi listeler |
| `pwd` | Bulunduğun dizini gösterir |
| `cd KLASÖR_ADI` | Belirtilen klasöre geçiş yapar |
| `cd ..` | Bir üst dizine çıkar |
| `mkdir KLASÖR_ADI` | Yeni klasör oluşturur |
| `touch DOSYA_ADI` | Yeni boş dosya oluşturur |
| `rm DOSYA_ADI` | Dosyayı siler |
| `rm -rf KLASÖR_ADI` | Klasörü içeriğiyle birlikte zorla siler |
| `vi DOSYA_ADI` | Dosyayı terminalde düzenlemeye açar |
| `:wq` | Dosyayı kaydedip düzenleyiciden çıkar |
| `cat DOSYA_ADI` | Dosya içeriğini terminalde görüntüler |

---

## 🔧 Git Komutları

### Genel Yapılandırma

- `git --version` → Git sürümünü gösterir  
- `git config --global user.name "Ad"` → Kullanıcı adını ayarlar  
- `git config --global user.email "email"` → E-posta adresini ayarlar  
- `git config --global core.editor "code --wait"` → VS Code'u varsayılan editör yapar

### Temel Kullanım

- `git init` → Mevcut klasörü Git deposuna çevirir  
- `git status` → Değişiklik durumlarını listeler  
- `git add .` → Tüm dosyaları sahneye ekler  
- `git add README.md` → Belirtilen dosyayı sahneye ekler  
- `git commit -m "mesaj"` → Değişiklikleri commit eder  
- `git commit -a` → Sahneye almadan direkt commit eder  
- `git log` → Commit geçmişini listeler  
- `touch .gitignore` → .gitignore dosyası oluşturur  

---

## 🌿 Branch (Dal) İşlemleri

- `git branch branch_ismi` → Yeni branch oluşturur  
- `git switch branch_ismi` → Belirtilen branch’e geçiş yapar  
- `git checkout -b yeni_branch` → Yeni branch oluşturup geçiş yapar  
- `git merge branch_ismi` → Aktif branch'e diğer branch'i birleştirir  
- `git merge conflict` → Çakışma varsa manuel çözüm gerekir  
- `git fast forwarding` → Ana branch’te değişiklik yoksa doğrudan birleştirme yapılır  

---

## ♻️ Git Restore (Geri Alma)

- `git restore dosya_adı` → Dosyayı son commit haline döndürür

---

## 🧳 Git Stash (Geçici Saklama)

- `git stash` → Değişiklikleri geçici olarak saklar  
- `git stash pop` → Son stash’i geri getirir ve siler  
- `git stash apply stash@{0}` → Belirli stash’i geri getirir ama silmez  
- `git stash list` → Stash listesini gösterir  
- `git stash show` → Son stash’in ne içerdiğini özetler  
- `git stash show -p` → Satır bazında değişiklikleri gösterir  
- `git stash drop stash@{0}` → Belirli stash’i siler  
- `git stash clear` → Tüm stash’leri temizler  

---

## 🕹️ Commit Geçmişi ve Geri Alma

- `git checkout hash` → Belirli commit'e geçer (detached HEAD)  
- `git reset hash` → Commit’leri belirtilen commit’e kadar geri alır  
- `git reset --hard hash` → Commit’leri ve dosya değişikliklerini siler  
- `git revert hash` → Belirtilen commit’i geri alır (yeni bir commit ile)  

---

## ☁️ GitHub ile Çalışmak

- `git remote add origin <repo_linki>` → Uzak repo bağlantısı ekler  
- `git branch -M main` → Branch ismini "main" yapar  
- `git push -u origin main` → İlk push işlemi (origin’e main branch gönderimi)  
- `git fetch origin main` → Uzak değişiklikleri çeker (birleştirmez)  
- `git pull origin main` → Fetch + Merge işlemi yapar  

---

## 🔍 Farkları Karşılaştırma

- `git diff` → Çalışma dizinindeki değişikliklerle son commit farkını gösterir  
- `git diff HEAD` → HEAD ile mevcut halin farkını gösterir  
- `git diff hash1 hash2` → İki commit arasındaki farkı gösterir  
- `git diff branch1 branch2` → İki branch arasındaki farkı gösterir  

---

## ⚠️ Git Rebase (Dikkatli Kullanılmalı)

- `git rebase` → Commit geçmişini temizler, merge commitlerini kaldırır  
⚠️ Paylaşılmış projelerde rebase kullanımı önerilmez; başkalarının geçmişini bozabilir.

---

## 🔐 SSH Anahtarları

- `cd ~/.ssh` → SSH klasörüne geçer  
- `pbcopy < ~/.ssh/id_ed25519.pub` → Public SSH anahtarını panoya kopyalar  
- `open ~/.ssh` → SSH klasörünü açar  
