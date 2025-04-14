Harika bir başlangıç yapmışsın Çınar! Aşağıda, yazdığın README içeriğini daha profesyonel ve okunabilir hale getirilmiş bir versiyonuyla paylaşıyorum. Markdown biçimine uygun başlıklar, kod blokları, açıklamalar ve bağlantılar eklendi:

markdown
Kopyala
Düzenle
# 🤖 RASA Chatbot Projesi

Bu repo, [Rasa](https://rasa.com/) ile geliştirilmiş bir chatbot projesinin Docker destekli çalışmasını ve basit bir web arayüzü ile entegrasyonunu içerir.

## 📁 Proje Klasör Yapısı

chatbot-rasa/ │ ├── data/ # NLU eğitim verileri │ └── nlu.yml ├── domain.yml # Bot'un domain tanımı ├── config.yml # Pipeline ve policy konfigürasyonu ├── rules.yml # Kural tabanlı diyaloglar ├── stories.yml # Eğitim hikayeleri │ ├── actions/ # Custom action'lar │ └── init.py │ ├── models/ # Eğitilmiş modellerin kaydedildiği klasör ├── credentials.yml # Kanal konfigürasyonları ├── endpoints.yml # Action server ayarları │ ├── docker/ # Docker konfigürasyonları │ ├── docker-compose.yml │ └── Dockerfile │ ├── templates/ # Web arayüz şablonları ├── web_ui/ # Statik web dosyaları │ ├── .gitignore ├── .gitattributes └── README.md

yaml
Kopyala
Düzenle

---

## ⚙️ Sanal Ortam Kurulumu

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# Linux/macOS
python3 -m venv venv
source venv/bin/activate

# Rasa kurulumu
pip install rasa
🧠 Model Eğitimi
bash
Kopyala
Düzenle
rasa train
Eğitim tamamlandıktan sonra model models/ klasörüne kaydedilir.

🧪 Lokal Test
A. Sadece NLU:
bash
Kopyala
Düzenle
rasa shell nlu
B. Tam Konuşma:
bash
Kopyala
Düzenle
rasa shell
C. Action Server ile:
bash
Kopyala
Düzenle
# Bir terminalde:
rasa run actions

# Diğer terminalde:
rasa shell
🌐 Webhook Testi
bash
Kopyala
Düzenle
rasa run --enable-api --cors "*" --debug
Test için örnek cURL komutu:
bash
Kopyala
Düzenle
curl -X POST http://localhost:5005/webhooks/rest/webhook \
-H "Content-Type: application/json" \
-d '{"sender": "test_user", "message": "Merhaba"}'
🐳 Docker ile Çalıştırma
1. Docker Dosyaları
docker/Dockerfile

dockerfile
Kopyala
Düzenle
FROM rasa/rasa:3.6.2

USER root
RUN mkdir -p /app/models
WORKDIR /app
docker/docker-compose.yml

yaml
Kopyala
Düzenle
version: "3.9"
services:
  rasa:
    build: .
    ports:
      - "5005:5005"
    volumes:
      - ../:/app
    command: rasa run --enable-api --cors "*"

  action_server:
    image: rasa/rasa-sdk:3.6.2
    ports:
      - "5055:5055"
    volumes:
      - ../actions:/app/actions

  web_ui:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - ../web_ui:/usr/share/nginx/html
2. Başlatma
bash
Kopyala
Düzenle
cd docker
docker compose up --build
🔗 Erişim Noktaları
Chatbot REST API: http://localhost:5005

Action Server: http://localhost:5055

Web Arayüzü: http://localhost:8080

Not: Eğer models/ klasöründe izin hatası alırsanız:

bash
Kopyala
Düzenle
sudo chmod -R 777 models/
