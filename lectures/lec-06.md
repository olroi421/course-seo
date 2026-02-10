# Лекція 06 Технічне SEO — crawling та indexing

## Вступ

Технічне SEO становить фундамент успішної оптимізації будь-якого вебсайту. Навіть найякісніший контент не принесе результатів, якщо пошукові роботи не зможуть його знайти, обійти та проіндексувати. У цій лекції ми детально розглянемо механізми взаємодії між вебсайтом та пошуковими системами, зосередившись на інструментах контролю crawling та indexing процесів. Розуміння цих концепцій дозволяє SEO-фахівцям ефективно керувати тим, як пошукові системи сприймають та індексують контент сайту.

## 1. Robots.txt — контроль доступу пошукових роботів

### 1.1. Що таке robots.txt

Robots.txt — це текстовий файл, розміщений у кореневій директорії вебсайту, який надає інструкції пошуковим роботам (crawlers, bots, spiders) щодо того, які частини сайту можна обходити, а які ні. Цей файл є стандартом Robots Exclusion Protocol, запровадженим у 1994 році.

Важливо розуміти, що robots.txt є лише рекомендацією, а не механізмом безпеки. Добросовісні пошукові роботи поважають директиви, але зловмисники можуть їх ігнорувати. Для захисту конфіденційної інформації слід використовувати механізми аутентифікації на рівні сервера.

### 1.2. Синтаксис та структура

Базова структура robots.txt складається з директив, організованих у групи:

```
User-agent: *
Disallow: /admin/
Disallow: /private/
Allow: /private/public-info.html

User-agent: Googlebot
Crawl-delay: 10
Disallow: /temp/

Sitemap: https://example.com/sitemap.xml
```

Розглянемо кожну директиву детально.

### 1.3. Директива User-agent

`User-agent` визначає, до якого робота застосовуються наступні правила. Кожна група правил починається з директиви User-agent.

**Значення:**

- `*` — застосовується до всіх роботів (wildcard).
- Конкретні назви: `Googlebot`, `Bingbot`, `Slurp` (Yahoo), `DuckDuckBot`, `Baiduspider` тощо.

Приклад з множинними роботами:

```
User-agent: Googlebot
User-agent: Googlebot-Image
Disallow: /images/private/
```

Обидва роботи Google отримають однакові інструкції.

### 1.4. Директива Disallow

`Disallow` вказує шляхи, які робот не повинен обходити. Шлях є регістрозалежним і може містити wildcards.

**Приклади використання:**

```
# Блокування всього сайту
Disallow: /

# Блокування конкретної директорії
Disallow: /admin/

# Блокування конкретного файлу
Disallow: /secret-page.html

# Блокування всіх URL з параметром
Disallow: /*?sessionid=

# Блокування файлів певного типу
Disallow: /*.pdf$
```

Символи `*` (будь-яка послідовність символів) та `$` (кінець URL) є wildcards, які підтримують сучасні пошукові системи.

### 1.5. Директива Allow

`Allow` створює виняток із правил Disallow, дозволяючи доступ до певних підшляхів всередині заблокованої директорії.

Приклад:

```
User-agent: *
Disallow: /members/
Allow: /members/public/
```

У цьому випадку `/members/` заблокована, але `/members/public/` доступна для crawling.

**Важливо:** специфічність правил має значення. Більш специфічні правила мають пріоритет над загальними:

```
User-agent: *
Disallow: /page
Allow: /page.html
```

Тут `/page.html` буде дозволена, навіть якщо `/page` заблокована.

### 1.6. Директива Crawl-delay

`Crawl-delay` визначає мінімальну затримку (у секундах) між запитами робота до сервера. Використовується для зменшення навантаження на сервер.

```
User-agent: *
Crawl-delay: 10
```

**Зауваження:** Google офіційно не підтримує цю директиву. Для контролю частоти crawling Google використовуйте налаштування у Google Search Console.

### 1.7. Директива Sitemap

`Sitemap` вказує розташування XML sitemap для допомоги роботам у знаходженні контенту.

```
Sitemap: https://example.com/sitemap.xml
Sitemap: https://example.com/sitemap-images.xml
```

Можна вказати кілька sitemap. Ця директива не прив'язана до конкретного User-agent та застосовується глобально.

### 1.8. Best practices для robots.txt

**Розміщення.** Файл повинен знаходитись у кореневій директорії: `https://example.com/robots.txt`. Розміщення в піддиректоріях не працює.

**Регістр.** Назва файлу має бути в нижньому регістрі: `robots.txt`, а не `Robots.TXT` або `ROBOTS.TXT`.

**Кодування.** Використовуйте UTF-8 кодування, особливо для сайтів з нелатинськими символами.

**Розмір.** Google рекомендує обмежувати розмір файлу 500 KB. Більші файли можуть бути проігноровані.

**Коментарі.** Використовуйте `#` для коментарів:

```
# Блокуємо панель адміністратора
User-agent: *
Disallow: /admin/  # Приватна зона
```

**Тестування.** Використовуйте інструмент перевірки robots.txt у Google Search Console для валідації синтаксису та тестування URL.

### 1.9. Поширені помилки та помилкові практики

**Блокування CSS та JavaScript.** Google потребує доступу до цих ресурсів для правильного рендерингу сторінок:

```
# ПОГАНО: блокує важливі ресурси
User-agent: Googlebot
Disallow: /css/
Disallow: /js/
```

Блокування цих ресурсів може негативно вплинути на mobile-first indexing та Core Web Vitals.

**Використання robots.txt для приховування контенту.** Robots.txt не гарантує, що сторінка не з'явиться в індексі, особливо якщо на неї посилаються інші сайти. Для запобігання індексації використовуйте meta robots noindex.

**Блокування пошукових систем повністю.** Помилкове залишення тестових налаштувань на продакшні:

```
# УВАГА: блокує весь сайт!
User-agent: *
Disallow: /
```

### 1.10. Приклад оптимального robots.txt для блогу

```
# Дозволити всім роботам сканування сайту
User-agent: *
Disallow: /wp-admin/
Allow: /wp-admin/admin-ajax.php

# Блокуємо сторінки пошуку та фільтрів
Disallow: /*?s=
Disallow: /filter/
Disallow: /page/

# Блокуємо дублювання контенту
Disallow: /tag/
Disallow: /author/
Disallow: /comments/feed/

# Дозволяємо доступ до ресурсів
Allow: /wp-content/uploads/

# Вказуємо sitemap
Sitemap: https://example.com/sitemap.xml
Sitemap: https://example.com/news-sitemap.xml
```

## 2. XML Sitemap — карта для пошукових роботів

### 2.1. Що таке XML Sitemap

XML Sitemap — це файл у форматі XML, який містить список URL сайту разом з додатковими метаданими (дата останньої модифікації, частота оновлення, пріоритет). Sitemap допомагає пошуковим системам ефективніше знаходити та індексувати контент, особливо для великих сайтів або сайтів з поганою внутрішньою структурою посилань.

### 2.2. Структура XML Sitemap

Базова структура відповідає протоколу sitemaps.org:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://example.com/</loc>
    <lastmod>2025-02-07</lastmod>
    <changefreq>daily</changefreq>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://example.com/about</loc>
    <lastmod>2025-01-15</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.8</priority>
  </url>
  <url>
    <loc>https://example.com/blog/seo-guide</loc>
    <lastmod>2025-02-05</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.9</priority>
  </url>
</urlset>
```

**Елементи:**

- `<loc>` — повний URL сторінки (обов'язковий).
- `<lastmod>` — дата останньої модифікації у форматі ISO 8601 (YYYY-MM-DD).
- `<changefreq>` — очікувана частота змін (always, hourly, daily, weekly, monthly, yearly, never).
- `<priority>` — відносний пріоритет URL на сайті (0.0 до 1.0).

### 2.3. Обмеження та специфікації

**Розмір файлу.** XML Sitemap не може перевищувати 50 MB (некомпресований) або містити більше 50,000 URL. Для більших сайтів використовуйте Sitemap Index.

**Кодування.** Файл має використовувати UTF-8 кодування.

**Екранування URL.** Спеціальні символи в URL повинні бути екрановані:

```xml
<loc>https://example.com/product?id=123&amp;color=red</loc>
```

Символ `&` замінюється на `&amp;`, `<` на `&lt;`, `>` на `&gt;`, `'` на `&apos;`, `"` на `&quot;`.

### 2.4. Типи Sitemap

**Sitemap Index.** Для сайтів з багатьма URL використовується індексний файл, який посилається на кілька окремих sitemap:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<sitemapindex xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <sitemap>
    <loc>https://example.com/sitemap-posts.xml</loc>
    <lastmod>2025-02-07</lastmod>
  </sitemap>
  <sitemap>
    <loc>https://example.com/sitemap-pages.xml</loc>
    <lastmod>2025-02-01</lastmod>
  </sitemap>
  <sitemap>
    <loc>https://example.com/sitemap-products.xml</loc>
    <lastmod>2025-02-06</lastmod>
  </sitemap>
</sitemapindex>
```

**Image Sitemap.** Розширення для зображень:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
        xmlns:image="http://www.google.com/schemas/sitemap-image/1.1">
  <url>
    <loc>https://example.com/photo-gallery</loc>
    <image:image>
      <image:loc>https://example.com/images/photo1.jpg</image:loc>
      <image:caption>Опис зображення</image:caption>
      <image:title>Назва зображення</image:title>
    </image:image>
    <image:image>
      <image:loc>https://example.com/images/photo2.jpg</image:loc>
    </image:image>
  </url>
</urlset>
```

**Video Sitemap.** Розширення для відео:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
        xmlns:video="http://www.google.com/schemas/sitemap-video/1.1">
  <url>
    <loc>https://example.com/videos/tutorial</loc>
    <video:video>
      <video:thumbnail_loc>https://example.com/thumbs/video-thumb.jpg</video:thumbnail_loc>
      <video:title>SEO Tutorial</video:title>
      <video:description>Повний посібник з SEO для початківців</video:description>
      <video:content_loc>https://example.com/video123.mp4</video:content_loc>
      <video:duration>600</video:duration>
      <video:publication_date>2025-02-01T00:00:00+00:00</video:publication_date>
    </video:video>
  </url>
</urlset>
```

**News Sitemap.** Спеціальний формат для новинних сайтів, які хочуть потрапити в Google News:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
        xmlns:news="http://www.google.com/schemas/sitemap-news/0.9">
  <url>
    <loc>https://example.com/news/article-123</loc>
    <news:news>
      <news:publication>
        <news:name>Назва видання</news:name>
        <news:language>uk</news:language>
      </news:publication>
      <news:publication_date>2025-02-07T10:00:00+00:00</news:publication_date>
      <news:title>Заголовок новини</news:title>
    </news:news>
  </url>
</urlset>
```

### 2.5. Автоматизація генерації Sitemap

Більшість CMS мають вбудовані або плагінні рішення для автоматичної генерації sitemap.

**WordPress.** Вбудована функціональність (з версії 5.5) або популярні плагіни:

- Yoast SEO — автоматична генерація з налаштуванням типів контенту.
- RankMath — розширені можливості для окремих sitemap.
- Google XML Sitemaps — спеціалізований плагін.

**JavaScript frameworks (Next.js, Nuxt).** Програмна генерація під час build:

```javascript
// next-sitemap.config.js
module.exports = {
  siteUrl: 'https://example.com',
  generateRobotsTxt: true,
  exclude: ['/admin/*', '/private/*'],
  robotsTxtOptions: {
    policies: [
      { userAgent: '*', allow: '/' },
      { userAgent: '*', disallow: '/admin' }
    ]
  }
};
```

**Python (для статичних сайтів).** Скрипт для створення sitemap:

```python
from datetime import datetime
import xml.etree.ElementTree as ET

def generate_sitemap(urls):
    urlset = ET.Element('urlset', xmlns="http://www.sitemaps.org/schemas/sitemap/0.9")

    for url_data in urls:
        url = ET.SubElement(urlset, 'url')
        loc = ET.SubElement(url, 'loc')
        loc.text = url_data['loc']

        lastmod = ET.SubElement(url, 'lastmod')
        lastmod.text = datetime.now().strftime('%Y-%m-%d')

        priority = ET.SubElement(url, 'priority')
        priority.text = str(url_data.get('priority', 0.5))

    tree = ET.ElementTree(urlset)
    tree.write('sitemap.xml', encoding='UTF-8', xml_declaration=True)

urls = [
    {'loc': 'https://example.com/', 'priority': 1.0},
    {'loc': 'https://example.com/about', 'priority': 0.8}
]

generate_sitemap(urls)
```

### 2.6. Submission та валідація

**Google Search Console.** Додавання sitemap:

1. Відкрити розділ "Sitemaps".
2. Ввести URL sitemap (наприклад, `sitemap.xml`).
3. Натиснути "Submit".

Google періодично перевірятиме sitemap на наявність нових або оновлених URL.

**Валідація.** Перевірка коректності XML перед submission:

- Вбудований валідатор у Search Console.
- Онлайн-інструменти: XML Sitemap Validator.
- Локальна перевірка через xmllint: `xmllint --noout sitemap.xml`.

### 2.7. Best practices

**Включайте лише канонічні URL.** Sitemap повинен містити тільки ті URL, які ви хочете бачити в індексі. Виключіть сторінки з noindex, редірект-сторінки, дублікати.

**Оновлюйте lastmod точно.** Вказуйте реальну дату останньої зміни, інакше пошукові системи можуть перестати довіряти цій інформації.

**Не покладайтеся на changefreq та priority.** Google офіційно заявив, що ігнорує ці параметри. Використовуйте їх для документації, але не очікуйте впливу на crawling.

**Стискайте великі sitemap.** Використовуйте gzip compression для зменшення розміру файлу та прискорення завантаження роботами:

```bash
gzip -c sitemap.xml > sitemap.xml.gz
```

Розмір gzip-архіву також не повинен перевищувати 50 MB.

## 3. Meta robots tags — контроль на рівні сторінки

### 3.1. Що таке meta robots tags

Meta robots tags — це HTML-теги, які надають інструкції пошуковим роботам безпосередньо на рівні окремої сторінки. На відміну від robots.txt, який контролює доступ до crawling, meta robots визначають, чи повинна сторінка індексуватися та як обробляти посилання.

### 3.2. Синтаксис та розміщення

Meta robots tag розміщується в секції `<head>` HTML-документа:

```html
<head>
  <meta name="robots" content="noindex, nofollow">
</head>
```

**Атрибути:**

- `name` — визначає цільового робота (`robots` для всіх або конкретний, наприклад `googlebot`).
- `content` — містить директиви, розділені комами.

### 3.3. Основні директиви

**index / noindex.** Контроль індексації сторінки:

- `index` — дозволити індексацію (за замовчуванням).
- `noindex` — заборонити індексацію; сторінка не з'явиться в результатах пошуку.

```html
<meta name="robots" content="noindex">
```

**follow / nofollow.** Контроль передачі link equity:

- `follow` — переходити за посиланнями на сторінці (за замовчуванням).
- `nofollow` — не переходити за посиланнями; не передавати PageRank.

```html
<meta name="robots" content="nofollow">
```

**Комбінування директив:**

```html
<!-- Індексувати сторінку, але не переходити за посиланнями -->
<meta name="robots" content="index, nofollow">

<!-- Не індексувати і не переходити за посиланнями -->
<meta name="robots" content="noindex, nofollow">
```

**none.** Еквівалент `noindex, nofollow`:

```html
<meta name="robots" content="none">
```

### 3.4. Розширені директиви

**noarchive.** Забороняє Google показувати кешовану копію сторінки в результатах пошуку:

```html
<meta name="robots" content="noarchive">
```

**nosnippet.** Забороняє показувати текстовий snippet або відеопревью в результатах:

```html
<meta name="robots" content="nosnippet">
```

Якщо встановлено `nosnippet`, Google також не показуватиме кешовану копію, оскільки для неї потрібен snippet.

**max-snippet:[number].** Обмежує максимальну довжину snippet в символах:

```html
<meta name="robots" content="max-snippet:160">
```

Значення:

- `-1` — немає ліміту (за замовчуванням).
- `0` — еквівалент `nosnippet`.
- Число — максимальна кількість символів.

**max-image-preview:[setting].** Контроль розміру preview зображення:

```html
<meta name="robots" content="max-image-preview:large">
```

Значення:

- `none` — не показувати preview.
- `standard` — стандартний розмір.
- `large` — великий preview (рекомендовано для AMP та rich results).

**max-video-preview:[number].** Максимальна тривалість відео-preview в секундах:

```html
<meta name="robots" content="max-video-preview:30">
```

**notranslate.** Забороняє пропонувати переклад сторінки в результатах:

```html
<meta name="robots" content="notranslate">
```

**noimageindex.** Забороняє індексацію зображень на сторінці:

```html
<meta name="robots" content="noimageindex">
```

### 3.5. Специфічні директиви для окремих роботів

Можна надавати різні інструкції різним роботам:

```html
<!-- Загальні інструкції -->
<meta name="robots" content="index, follow">

<!-- Специфічні для Googlebot -->
<meta name="googlebot" content="noindex">

<!-- Для Googlebot-Image -->
<meta name="googlebot-image" content="noimageindex">
```

У випадку конфлікту специфічна директива має пріоритет над загальною.

### 3.6. Комбінування з robots.txt

Важливо розуміти взаємодію між robots.txt та meta robots:

- Якщо сторінка заблокована в robots.txt, робот не зможе побачити meta robots tag.
- Для застосування `noindex` сторінка має бути доступна для crawling.

**Неправильно:**

```
# robots.txt
Disallow: /private/
```

```html
<!-- /private/page.html -->
<meta name="robots" content="noindex">
```

Робот не дістанеться до сторінки через блокування в robots.txt, тому не побачить `noindex`. Якщо на цю сторінку є зовнішні посилання, Google може проіндексувати URL без контенту.

**Правильно:** Дозволити crawling, але заборонити індексацію:

```
# robots.txt
Allow: /private/
```

```html
<!-- /private/page.html -->
<meta name="robots" content="noindex, nofollow">
```

### 3.7. Приклади використання

**Сторінка "Дякуємо за реєстрацію".** Не має цінності для пошуку:

```html
<meta name="robots" content="noindex, nofollow">
```

**Архівні сторінки блогу.** Індексувати, але обмежити snippet для заохочення кліків:

```html
<meta name="robots" content="index, follow, max-snippet:100">
```

**Конфіденційні документи.** Повна заборона індексації:

```html
<meta name="robots" content="noindex, nofollow, noarchive">
```

## 4. X-Robots-Tag HTTP headers

### 4.1. Що таке X-Robots-Tag

X-Robots-Tag — це альтернатива meta robots tags, яка надається через HTTP-заголовки замість HTML. Особливо корисна для неHTML-ресурсів (PDF, зображення, відео), де неможливо додати HTML-теги.

### 4.2. Синтаксис

X-Robots-Tag додається до HTTP-відповіді:

```
HTTP/1.1 200 OK
X-Robots-Tag: noindex, nofollow
Content-Type: application/pdf
```

### 4.3. Налаштування на рівні сервера

**Apache (.htaccess):**

```apache
# Noindex для всіх PDF
<FilesMatch "\.pdf$">
  Header set X-Robots-Tag "noindex, nofollow"
</FilesMatch>

# Noindex для конкретної директорії
<Directory /var/www/html/private>
  Header set X-Robots-Tag "noindex"
</Directory>
```

**Nginx:**

```nginx
location /private/ {
  add_header X-Robots-Tag "noindex, nofollow";
}

location ~* \.(pdf|zip)$ {
  add_header X-Robots-Tag "noindex";
}
```

**Node.js (Express):**

```javascript
app.use('/private', (req, res, next) => {
  res.set('X-Robots-Tag', 'noindex, nofollow');
  next();
});

app.get('/document.pdf', (req, res) => {
  res.set('X-Robots-Tag', 'noarchive');
  res.sendFile('document.pdf');
});
```

### 4.4. Переваги X-Robots-Tag

**Універсальність.** Працює для будь-яких типів файлів, не лише HTML.

**Централізоване управління.** Налаштування на рівні сервера або CDN замість редагування кожного файлу.

**Комбінування з meta tags.** Можна використовувати обидва методи; директиви об'єднуються.

### 4.5. Приклад для зображень

Запобігання індексації конфіденційних зображень:

```apache
<FilesMatch "\.(jpg|jpeg|png|gif)$">
  <If "%{REQUEST_URI} =~ m#/confidential/#">
    Header set X-Robots-Tag "noindex, noimageindex"
  </If>
</FilesMatch>
```

## 5. HTTPS та SSL сертифікати

### 5.1. Вплив HTTPS на SEO

З 2014 року Google офіційно визнав HTTPS як фактор ранжування. Хоча вплив невеликий (lightweight signal), безпека сайту стала обов'язковою вимогою.

**Переваги HTTPS:**

- Захист даних користувачів через шифрування.
- Запобігання man-in-the-middle атакам.
- Необхідність для сучасних веб-функцій (Service Workers, HTTP/2, Geolocation API).
- Довіра користувачів — браузери позначають HTTP-сайти як "Not Secure".

### 5.2. SSL/TLS сертифікати

SSL (Secure Sockets Layer) та його наступник TLS (Transport Layer Security) забезпечують шифрування з'єднання між браузером та сервером.

**Типи сертифікатів:**

- **Domain Validation (DV)** — базова перевірка права володіння доменом; видається автоматично (Let's Encrypt).
- **Organization Validation (OV)** — підтвердження існування організації.
- **Extended Validation (EV)** — найвищий рівень перевірки; відображає назву компанії в браузері (зелена адресна панель у старих браузерах).

Для SEO DV-сертифіката цілком достатньо.

### 5.3. Міграція з HTTP на HTTPS

**Кроки міграції:**

1. Отримання SSL-сертифіката (безкоштовно через Let's Encrypt або комерційні CA).
2. Встановлення сертифіката на сервер.
3. Налаштування 301 редіректів з HTTP на HTTPS:

```apache
# Apache
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteRule ^(.*)$ https://%{HTTP_HOST}/$1 [R=301,L]
```

```nginx
# Nginx
server {
  listen 80;
  server_name example.com www.example.com;
  return 301 https://$host$request_uri;
}
```

4. Оновлення внутрішніх посилань на HTTPS.
5. Оновлення canonical URLs та hreflang на HTTPS.
6. Зміна адреси сайту в Google Search Console на HTTPS-версію.
7. Повторна подання sitemap з HTTPS URL.

### 5.4. Запобігання змішаному контенту

Mixed content виникає, коли HTTPS-сторінка завантажує HTTP-ресурси. Браузери блокують або попереджають про такий контент.

**Виявлення:** Console в Chrome DevTools показує помилки mixed content.

**Виправлення:**

- Використовувати protocol-relative URLs: `//example.com/image.jpg` (автоматично підбирає HTTP/HTTPS).
- Краще: явно вказувати HTTPS: `https://example.com/image.jpg`.
- Content Security Policy для примусового HTTPS:

```html
<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests">
```

Ця CSP-директива автоматично перетворює HTTP-запити на HTTPS.

### 5.5. HTTP Strict Transport Security (HSTS)

HSTS примушує браузери завжди використовувати HTTPS, навіть якщо користувач вводить HTTP.

Налаштування через HTTP-заголовок:

```
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
```

**Параметри:**

- `max-age` — тривалість дії політики в секундах (1 рік = 31536000).
- `includeSubDomains` — застосовувати до всіх піддоменів.
- `preload` — додавання до HSTS preload list браузерів.

**Preload list:** Публічний список доменів з HSTS, вбудований у браузери. Submission на https://hstspreload.org/.

## 6. Canonical URLs

### 6.1. Проблема дублікатів контенту

Дублікати контенту виникають, коли ідентичний або дуже схожий контент доступний за кількома URL. Пошукові системи можуть розділити link equity між дублікатами, зменшуючи ефективність SEO.

**Причини дублікатів:**

- Варіації протоколів: `http://` vs `https://`.
- Варіації домену: `example.com` vs `www.example.com`.
- URL-параметри: `page.html?ref=twitter` vs `page.html?ref=facebook`.
- Session IDs: `page.html?sessionid=abc123`.
- Trailing slash: `page.html` vs `page.html/`.

### 6.2. Canonical tag — self-declaration

Canonical link element вказує пошуковим системам пріоритетну (канонічну) версію URL:

```html
<link rel="canonical" href="https://example.com/page">
```

Цей тег розміщується в `<head>` HTML-документа.

**Приклад використання:** Товар доступний у кількох категоріях:

```
https://example.com/shoes/nike-air
https://example.com/sneakers/nike-air
https://example.com/products/nike-air
```

Всі три сторінки мають однаковий canonical:

```html
<link rel="canonical" href="https://example.com/products/nike-air">
```

### 6.3. Self-referencing canonical

Навіть для унікальних сторінок рекомендується додавати self-referencing canonical для запобігання проблем з параметрами:

```html
<!-- На сторінці https://example.com/article -->
<link rel="canonical" href="https://example.com/article">
```

Якщо хтось додасть параметр (`?utm_source=twitter`), canonical вкаже оригінальну версію.

### 6.4. Cross-domain canonical

Canonical може посилатися на інший домен, що корисно для синдикації контенту:

```html
<!-- На сайті syndication.com -->
<link rel="canonical" href="https://original-site.com/article">
```

Google розумітиме, що original-site.com є джерелом контенту.

### 6.5. Canonical vs 301 redirect

**Canonical** — сигнал для пошукових систем, але користувачі досі можуть відвідати неканонічний URL.

**301 redirect** — примусове перенаправлення на рівні сервера; користувачі та роботи автоматично переходять на нову адресу.

**Коли використовувати canonical:**

- Потрібно зберегти доступ до URL для користувачів (наприклад, параметри сортування).
- Синдикований контент на сторонніх сайтах.

**Коли використовувати 301:**

- Постійна зміна структури URL.
- Міграція на новий домен.
- Консолідація дублікатів без потреби в доступі до старих URL.

### 6.6. Best practices

**Абсолютні URL.** Завжди використовуйте повні URL з протоколом та доменом:

```html
<!-- ПОГАНО -->
<link rel="canonical" href="/page">

<!-- ДОБРЕ -->
<link rel="canonical" href="https://example.com/page">
```

**Один canonical на сторінку.** Наявність кількох canonical створює конфлікт; Google вибере один або проігнорує обидва.

**Canonical лише для схожого контенту.** Не використовуйте canonical для принципово різних сторінок; це може призвести до деіндексації.

**Перевірка через PageSpeed Insights або інспекцію URL.** Переконайтеся, що Google розпізнає canonical правильно.

## 7. Pagination — rel="next" та rel="prev"

### 7.1. Проблема пагінації

Пагінований контент (списки статей, каталоги товарів) розподілений на кілька сторінок. Пошукові системи можуть не розуміти зв'язок між сторінками або індексувати кожну окремо, розбиваючи link equity.

### 7.2. Історичний контекст: rel="next" та rel="prev"

До 2019 року Google рекомендував використовувати `rel="next"` та `rel="prev"` для вказання послідовності сторінок:

```html
<!-- Сторінка 1 -->
<link rel="next" href="https://example.com/articles?page=2">

<!-- Сторінка 2 -->
<link rel="prev" href="https://example.com/articles?page=1">
<link rel="next" href="https://example.com/articles?page=3">

<!-- Сторінка 3 -->
<link rel="prev" href="https://example.com/articles?page=2">
```

У березні 2019 року Google офіційно заявив, що більше не використовує ці теги для ранжування. Однак інші пошукові системи (Bing, Yandex) можуть їх підтримувати.

### 7.3. Сучасні рекомендації

**View All page.** Створення окремої сторінки з повним контентом:

```html
<!-- Пагіновані сторінки -->
<link rel="canonical" href="https://example.com/articles/all">

<!-- View All сторінка -->
<link rel="canonical" href="https://example.com/articles/all">
```

Всі пагіновані сторінки вказують canonical на View All.

**Компроміс:** View All може мати проблеми з продуктивністю для великих списків.

**Self-referencing canonical.** Дозволити індексацію кожної сторінки окремо:

```html
<!-- Сторінка 2 -->
<link rel="canonical" href="https://example.com/articles?page=2">
```

Кожна сторінка є канонічною для себе. Google індексуватиме кожну окремо.

**Infinite scroll з SEO-friendly fallback.** Використання пагінації для роботів та infinite scroll для користувачів:

```html
<a href="https://example.com/articles?page=2" class="load-more">
  Load More
</a>

<script>
// JavaScript замінює link на infinite scroll
</script>
```

### 7.4. Додавання noindex до глибоких сторінок

Для запобігання індексації менш релевантних сторінок (page 10+):

```html
<!-- Сторінка 15 -->
<meta name="robots" content="noindex, follow">
```

Це дозволяє роботам переходити далі, але не індексувати сторінки з низькою цінністю.

## Висновки

Технічне SEO становить фундаментальний рівень оптимізації, без якого навіть найкращий контент не зможе реалізувати свій потенціал у пошукових системах. Розуміння механізмів crawling та indexing дозволяє SEO-фахівцям ефективно керувати тим, як пошукові роботи взаємодіють з вебсайтом.

Robots.txt надає макрорівень контролю, визначаючи загальні правила доступу до різних частин сайту. XML Sitemap служить картою для ефективного виявлення контенту, особливо для великих або складних сайтів. Meta robots tags та X-Robots-Tag забезпечують мікрорівень контролю на рівні окремих сторінок та ресурсів.

HTTPS перестав бути опціональним покращенням і став базовою вимогою як для безпеки користувачів, так і для SEO. Canonical URLs вирішують проблему дублікатів контенту, консолідуючи ranking signals на пріоритетних версіях сторінок.

Важливо пам'ятати, що технічне SEO вимагає постійної уваги та моніторингу. Регулярні аудити через Google Search Console, тестування robots.txt та sitemap, перевірка правильності canonical та meta robots tags повинні стати рутинною практикою. Технічні помилки можуть мати катастрофічний вплив на видимість сайту в пошуку, тому превентивний підхід та систематична перевірка є критично важливими для успіху SEO-стратегії.
