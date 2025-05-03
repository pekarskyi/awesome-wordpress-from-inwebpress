# Заголовки безпеки

Заголовки безпеки (Security Headers) — це спеціальні директиви HTTP-заголовків, які налаштовують браузер для захисту веб-сайту від різних типів атак. Вони передаються у відповіді сервера клієнту і вказують браузеру, як саме обробляти контент сторінки з точки зору безпеки.

## Основні заголовки безпеки:

### Content-Security-Policy (CSP)
**Призначення:** Захист від XSS-атак шляхом визначення дозволених джерел завантаження ресурсів.

**Приклад:**
```
Content-Security-Policy: default-src 'self'; script-src 'self' trusted-scripts.com; img-src *;
```
Цей приклад дозволяє завантажувати скрипти лише з власного домену та trusted-scripts.com, зображення — з будь-яких джерел, а все інше — лише з власного домену.

### X-XSS-Protection
**Призначення:** Активує вбудований в браузер фільтр XSS-атак.

**Приклад:**
```
X-XSS-Protection: 1; mode=block
```
Значення "1" вмикає фільтр, а "mode=block" вказує повністю блокувати сторінку у випадку виявлення атаки.

### X-Frame-Options
**Призначення:** Захист від Clickjacking-атак шляхом контролю відображення сайту у фреймах.

**Приклад:**
```
X-Frame-Options: SAMEORIGIN
```
Дозволяє відображати сторінку у фреймах лише на тому ж домені.

### X-Content-Type-Options
**Призначення:** Запобігає MIME-сніффінгу браузерами (визначення типу вмісту, що відрізняється від заявленого).

**Приклад:**
```
X-Content-Type-Options: nosniff
```
Примушує браузер слідувати MIME-типу, вказаному в заголовку Content-Type.

### Strict-Transport-Security (HSTS)
**Призначення:** Примусове використання HTTPS замість HTTP.

**Приклад:**
```
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
```
Вказує браузеру використовувати HTTPS протягом року (31536000 секунд), включаючи піддомени.

### Referrer-Policy
**Призначення:** Контролює, яка інформація про джерело переходу передається при переході за посиланнями.

**Приклад:**
```
Referrer-Policy: strict-origin-when-cross-origin
```
Передає повний URL при переході в межах домену, але тільки джерело (без шляху) при переході на інші домени через HTTPS.

### Feature-Policy / Permissions-Policy
**Призначення:** Обмежує використання певних API браузера.

**Приклад:**
```
Permissions-Policy: camera=(), microphone=(), geolocation=(self)
```
Забороняє використання камери і мікрофона, але дозволяє визначення геолокації на власному домені.

## Як додати заголовки безпеки в WordPress:

### 1. Через файл .htaccess (для Apache):
```apache
<IfModule mod_headers.c>
  Header set Content-Security-Policy "default-src 'self';"
  Header set X-XSS-Protection "1; mode=block"
  Header set X-Frame-Options "SAMEORIGIN"
  Header set X-Content-Type-Options "nosniff"
  Header set Referrer-Policy "strict-origin-when-cross-origin"
  Header set Strict-Transport-Security "max-age=31536000; includeSubDomains"
</IfModule>
```

### 2. Через конфігурацію Nginx:
```nginx
add_header Content-Security-Policy "default-src 'self';" always;
add_header X-XSS-Protection "1; mode=block" always;
add_header X-Frame-Options "SAMEORIGIN" always;
add_header X-Content-Type-Options "nosniff" always;
add_header Referrer-Policy "strict-origin-when-cross-origin" always;
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
```

### 3. Через функції WordPress (functions.php):
```php
function add_security_headers() {
    header("Content-Security-Policy: default-src 'self';");
    header("X-XSS-Protection: 1; mode=block");
    header("X-Frame-Options: SAMEORIGIN");
    header("X-Content-Type-Options: nosniff");
    header("Referrer-Policy: strict-origin-when-cross-origin");
    header("Strict-Transport-Security: max-age=31536000; includeSubDomains");
}
add_action('send_headers', 'add_security_headers');
```

### 4. За допомогою плагінів WordPress:
- Sucuri Security
- iThemes Security
- Wordfence
- WP Security Headers

Правильно налаштовані заголовки безпеки значно підвищують захист WordPress-сайту від поширених атак, таких як XSS, clickjacking та інших вразливостей на стороні клієнта.