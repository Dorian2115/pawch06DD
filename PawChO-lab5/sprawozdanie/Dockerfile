# syntax=docker/dockerfile:1.2

FROM alpine/git AS cloneStage
WORKDIR /app

# Dodanie GitHuba do hostów
RUN mkdir -p -m 0600 ~/.ssh && ssh-keyscan github.com >> ~/.ssh/known_hosts

# Pobranie kodu przez SSH
RUN --mount=type=ssh git clone git@github.com:Dorian2115/pawch06DD.git .

ARG VERSION=1.0
RUN echo "<!DOCTYPE html><html><head><title>Aplikacja Lab 5 i 6</title><meta charset='utf-8'></head><body>" > index.html && \
    echo "<h1>Informacje o kontenerze</h1>" >> index.html && \
    echo "<p><strong>Adres IP serwera:</strong> $(hostname -i)</p>" >> index.html && \
    echo "<p><strong>Nazwa serwera (hostname):</strong> $(hostname)</p>" >> index.html && \
    echo "<p><strong>Wersja aplikacji:</strong> ${VERSION}</p>" >> index.html && \
    echo "</body></html>" >> index.html


# Nginx
FROM nginx:alpine

# Etykieta łącząca obraz z GitHub
LABEL org.opencontainers.image.source=https://github.com/Dorian2115/pawch06DD

# instalacja curla do healthchecka
RUN apk add --update curl && rm -rf /var/cache/apk/*

# Skopiowanie pliku HTML
COPY --from=cloneStage /app/index.html /usr/share/nginx/html/index.html

# Healthcheck
HEALTHCHECK --interval=10s --timeout=3s CMD curl -f http://localhost/ || exit 1

EXPOSE 80
