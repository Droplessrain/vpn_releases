# Orbit VPN — релизы APK

Публичное (или полупубличное) хранилище **только готовых APK** для кнопки «Скачать» на лендинге.  
Репозиторий: `git@github.com:Droplessrain/vpn_releases.git`  
Ветка: `main`

Исходники Android здесь **не** хранятся — только файлы в `releases/`.

## Структура

```text
releases/
  OrbitVPN-X.Y.Z-client-release.apk   # обычно один «актуальный» файл
```

Прямая ссылка для лендинга:

```text
https://raw.githubusercontent.com/Droplessrain/vpn_releases/main/releases/OrbitVPN-X.Y.Z-client-release.apk
```

> GitHub предупреждает, если APK > 50 MB. Для soft launch это ок; при росте — GitHub Releases / LFS / CDN.

## Как выложить новый APK

На ноутбуке (после сборки client-release):

```bash
# 1) Собрать
cd /home/user/IdeaProjects/VPN/V2ray-Android
./gradlew clean :app:assembleClientRelease

APK=app/build/outputs/apk/client/release/OrbitVPN-*-client-release.apk
ls -lh $APK

# 2) Положить в releases (оставить только свежий файл)
cd /home/user/IdeaProjects/vpn_releases
rm -f releases/OrbitVPN-*-client-release.apk
cp -f /home/user/IdeaProjects/VPN/V2ray-Android/app/build/outputs/apk/client/release/OrbitVPN-*-client-release.apk \
  releases/

# 3) Commit + push
git add -A releases/
git status
git commit -m "Keep only latest OrbitVPN X.Y.Z APK in releases."
git push origin main
```

## После пуша

1. Обновить URL в лендинге (`orbit-vpn-landing`: `main.js` + `index.html` + `dist/`) на новый файл.
2. `git push` лендинга → Render deploy.
3. Проверить скачивание с телефона / инкогнито.

## Откат

```bash
cd /home/user/IdeaProjects/vpn_releases
git log --oneline -5
# вернуть нужный commit файла или снова скопировать старый APK и push
```

## Не коммитить

- keystore, `orbit.local.properties`, исходники клиента
- несколько старых APK «на всякий» без нужды (репо раздувается; политика soft launch — один актуальный файл)
