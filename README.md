# Сайт altyncore.kz

Одна HTML-страница, `robots.txt` и `sitemap.xml`. Сборки нет. Посмотреть локально: `python3 -m http.server -d . 8000`.

## Выкладка

```
bin/deploy
```

Сайт лежит в публичном репозитории [AltynCore/altyncore-site](https://github.com/AltynCore/altyncore-site). `bin/deploy` пушит HEAD в `main` и дёргает вебхук Git-расширения Plesk: сервер сам забирает ветку и разворачивает её в `httpdocs` домена altyncore.kz на `srv-plesk63.ps.kz` (та же подписка, что у adapto.kz). Потом скрипт ждёт, пока файлы на живом сайте совпадут с закоммиченными байт в байт, и проверяет, что `README.md`, `bin/`, `zettel/` отдают 404.

Адрес вебхука хранится в связке ключей macOS под именем `altyncore-site-deploy-hook`; в Plesk он в «altyncore.kz → Git → Настройки репозитория». Паролей деплой не требует: репозиторий публичный.

Plesk пишет только файлы репозитория и не трогает остальное в `httpdocs`. Файл, удалённый из репозитория, удаляется и с сайта.

В корне сайта кроме файлов репозитория лежит `core-continuity/` — отдельное PHP-приложение со своим `index.php`. Его в репозитории нет, Plesk его не трогает. Поэтому в `.htaccess` нельзя задавать `DirectoryIndex index.html`: правило наследуется подпапками, и `core-continuity/` начинает отвечать 403. `bin/deploy` проверяет, что `/core-continuity/login` отдаёт 200.
