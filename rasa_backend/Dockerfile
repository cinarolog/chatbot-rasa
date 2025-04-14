FROM rasa/rasa:2.8.1-full

WORKDIR /app

COPY . /app

# Gerekli olan tek bir komut
RUN rasa train

CMD ["rasa", "run", "--enable-api", "--cors", "*", "--debug"]
