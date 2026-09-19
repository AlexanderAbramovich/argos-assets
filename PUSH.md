# _cdn — ВСЕ ассеты сайта для GitHub + jsDelivr (в Tilda только код)

Почему всё, а не только дрон: Tilda не принимает .webp на загрузку (у нас 20 webp), видео/картинки пришлось бы грузить по одному и руками собирать 40 URL. jsDelivr: любые файлы, ≤ 20 МБ файл, ≤ 150 МБ репо, Range 206 + CORS `*` проверены. У нас 161 файл / 19,6 МБ, крупнейший 3,9 МБ.

Папка собирается резаком: `python _src/tilda_build.py --cdn-all --stage _cdn` (пути = как на сайте).

## Команды (после «да» Саши на имя репо; gh залогинен как AlexanderAbramovich)
```bash
cd _cdn
git init -b main
git add -A
git commit -m "argos site assets v1"
gh repo create argos-assets --public --source=. --push
git tag v1 && git push origin v1
```
База: `https://cdn.jsdelivr.net/gh/AlexanderAbramovich/argos-assets@v1/`
Проверка: `curl -sI https://cdn.jsdelivr.net/gh/AlexanderAbramovich/argos-assets@v1/assets/3d/drone.glb` → 200 (первый запрос греет кэш, до минуты).

## Нарезка с CDN
```bash
python _src/tilda_build.py --cdn-all --cdn https://cdn.jsdelivr.net/gh/AlexanderAbramovich/argos-assets@v1/
grep -l "{{" _tilda/*.html   # должно быть пусто
```
Потом `http://localhost:8791/_tilda/preview.html` — та же нарезка в обёртках `.t-rec`, ассеты уже с CDN: если тут 1:1 с локалкой, в Tilda едет проверенный код.

Новая версия файлов → новый тег (`v2`) и новая база: jsDelivr кэширует тег навсегда. Репо публичный (всё это и так уйдёт на публичный сайт).
