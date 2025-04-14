RASA CHATBOT PROJESİ
Bu proje, Rasa ile geliştirilmiş bir chatbot’un Docker destekli çalışmasını ve basit bir web arayüzü ile entegrasyonunu içerir.

PROJE KLASÖR YAPISI

chatbot-rasa/
├── data/ → NLU eğitim verileri (nlu.yml)
├── domain.yml → Bot’un domain tanımı
├── config.yml → Pipeline ve policy ayarları
├── rules.yml → Kural tabanlı diyaloglar
├── stories.yml → Eğitim hikayeleri
├── actions/ → Custom action dosyaları (init.py)
├── models/ → Eğitilmiş modellerin kaydedildiği klasör
├── credentials.yml → Kanal konfigürasyonları
├── endpoints.yml → Action server ayarları
├── docker/ → Dockerfile ve docker-compose.yml içerir
├── templates/ → Web arayüz şablonları
├── web_ui/ → Statik web dosyaları
├── .gitignore, .gitattributes
└── README.txt

SANAL ORTAM KURULUMU

Windows:
python -m venv venv
venv\Scripts\activate

Linux/macOS:
python3 -m venv venv
source venv/bin/activate

Rasa kurulumu:
pip install rasa

MODEL EĞİTİMİ

rasa train

(Eğitim tamamlandıktan sonra model models/ klasörüne kaydedilir.)

LOKAL TEST

Sadece NLU test:
rasa shell nlu

Tam konuşma testi:
rasa shell

Action server ile test:

Terminal 1:
rasa run actions

Terminal 2:
rasa shell

WEBHOOK TESTİ

API aktif çalıştırmak için:
rasa run --enable-api --cors "*" --debug

Örnek cURL isteği:

curl -X POST http://localhost:5005/webhooks/rest/webhook \
-H "Content-Type: application/json" \
-d '{"sender": "test_user", "message": "Merhaba"}'

DOCKER İLE ÇALIŞTIRMA

Dockerfile (docker/Dockerfile):

bash
Kopyala
Düzenle
FROM rasa/rasa:3.6.2  
USER root  
RUN mkdir -p /app/models  
WORKDIR /app  
docker-compose.yml (docker/docker-compose.yml):

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
Başlatmak için:
cd docker
docker compose up --build

ERİŞİM NOKTALARI

Chatbot REST API → http://localhost:5005
Action Server → http://localhost:5055
Web Arayüzü → http://localhost:8080

İZİN HATASI ÇÖZÜMÜ
models klasöründe hata alırsan:
sudo chmod -R 777 models/

