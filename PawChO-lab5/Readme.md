# Sprawozdanie z Laboratorium 5 - PAwChO

**Imię i nazwisko:** Dominik Długołencki
**Grupa:** IO6

---

## 1. Treść utworzonego pliku Dockerfile

Zgodnie z wymaganiami, pierwszy etap buduje środowisko od zera i generuje stronę HTML z informacjami o hoście oraz wersją. Etap drugi kopiuje ten plik na serwer WWW.

```dockerfile
# ETAP 1
FROM scratch AS builder


ADD alpine-minirootfs-3.19.1-x86_64.tar.gz /


ARG VERSION=1.0

WORKDIR /app

RUN echo "<!DOCTYPE html><html><head><title>Aplikacja Lab 5</title><meta charset='utf-8'></head><body>" > index.html && \
    echo "<h1>Informacje o kontenerze</h1>" >> index.html && \
    echo "<p><strong>Adres IP serwera:</strong> $(hostname -i)</p>" >> index.html && \
    echo "<p><strong>Nazwa serwera (hostname):</strong> $(hostname)</p>" >> index.html && \
    echo "<p><strong>Wersja aplikacji:</strong> ${VERSION}</p>" >> index.html && \
    echo "</body></html>" >> index.html

# ETAP 2
FROM nginx:alpine

RUN apk add --update curl && \
    rm -rf /var/cache/apk/*

COPY --from=builder /app/index.html /usr/share/nginx/html/index.html

HEALTHCHECK --interval=10s --timeout=3s \
  CMD curl -f http://localhost/ || exit 1

EXPOSE 80
```

## 2. Reszta informacji potrzebnych do sprawozdania

Zrzuty ekranu budowy obrazu, uruchomienia kontenera oraz wyniku polecenia curl znajduja się w folderze /sprawozdanie/zrzutyEkranu
