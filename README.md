RASA Projesi Hızlı Başlangıç Dokümanı
1. Klasör Yapısı
project/
│
├── data/
│   └── nlu.yml
├── domain.yml
├── config.yml
├── rules.yml
├── stories.yml
├── actions/
│   └── __init__.py
├── models/
├── credentials.yml
├── endpoints.yml
├── docker/
│   └── docker-compose.yml
│   └── Dockerfile
└── web_ui/
    └── (statik web arayüzü dosyaları)
2. Sanal Ortam Kurulumu
python -m venv venv
venv\Scripts\activate  # Windows
# veya
source venv/bin/activate  # Linux/macOS

pip install rasa
3. Model Eğitimi
rasa train
# Eğitilen model models/ klasörüne kaydedilir.
4. Lokal Test
A. NLU Test:
rasa shell nlu

B. Tam Konuşma:
rasa shell

C. Action Server ile:
rasa run actions  # başka terminalde
rasa shell
5. Webhook Testi
rasa run --enable-api --cors "*" --debug

Test:
curl -X POST http://localhost:5005/webhooks/rest/webhook \
-H "Content-Type: application/json" \
-d '{"sender": "test_user", "message": "Merhaba"}'
6. Docker Dosyaları
docker/docker-compose.yml:
[... docker-compose içeriği ...]

docker/Dockerfile:
FROM rasa/rasa:3.6.2
USER root
RUN mkdir -p /app/models
WORKDIR /app
7. Docker ile Çalıştırma
cd docker
docker compose up --build

Sonuç:
- Chatbot REST API: http://localhost:5005
- Action server: http://localhost:5055
- Web arayüzü: http://localhost:8080

Not: Permission hatası için:
sudo chmod -R 777 models/
