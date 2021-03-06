# جریان‌های QUIC و پروتکل HTTP/3

پروتکل HTTP/3 برای QUIC ساخته شده است، در نتیجه از مزایای جریان‌ها در QUIC
بهره کامل می‌برد، در حالی که HTTP/2 مجبور بود تا کل جریان و مفهوم
multiplexing خود را بر روی TCP طراحی کند.

درخواست‌های انجام شده‌ی HTTP بر روی HTTP/3 از یک مجموعه‌ی بخصوص
از جریان‌ها استفاده می‌کنند.

## قاب‌های HTTP/3

پروتکل HTTP/3 به معنای راه اندازی جریان‌های QUIC و ارسال دسته‌ای از
قاب‌ها به پایانه‌ی دیگر است. گرچه تعداد ثابت کمی (در واقع ۹ عدد در ۱۸
دسامبر ۲۰۱۸!) از قاب‌های شناخته شده در HTTP/3 وجود دارد. که مهم‌ترین
آنها احتمالاً به شرح ذیل‌اند:

- قاب HEADERS، که سرایند‌های فشرده‌ی HTTP را ارسال می‌کند
- قاب DATA، محتوای داده‌ی دودویی ارسال می‌کند
- قاب GOAWAY، خواهشمند است این اتصال را پایان دهید

## درخواست پروتکل انتقال ابرمتن

کارخواه درخواست HTTP خود را روی یک جریان دوطرفه‌ی QUIC آغاز شده از سمت خود
ارسال می‌کند.

یک درخواست از یک قاب HEADERS تشکیل می‌شود و ممکن است بطور اختیاری با یک یا
دو قاب دیگر نیز همراه باشد: یک مجموعه از قاب‌های DATA و احتمالاً یک قاب
HEADERS نهایی برای پشت‌بند‌هایش.

کارخواه پس از پس از ارسال یک درخواست، جریان را برای ارسال می‌بندد.

## پاسخ پروتکل انتقال ابرمتن

سرور پاسخ HTTP خود را روی جریان دوطرفه برمی‌گرداند. یک قاب HEADERS،
مجموعه‌ای از قاب‌های DATA و احتمالاً پشت‌بندش یک قابِ HEADERS.

## سرایندهای QPACK

قاب‌های HEADERS شامل سرایند‌های فشرده شده‌ی HTTP با استفاده از
الگوریتم QPACK هستند. پروتکل QPACK در روش شبیه به فشرده‌سازی HPACK ([RFC
7541](https://httpwg.org/specs/rfc7541.html)) در HTTP/2 است، اما اصلاح شده برای
کار با جریان‌های دریافت شده‌ی خارج از ترتیب.

لازم به ذکر که QPACK خودش از دو جریان اضافی QUIC  یک‌طرفه مابین دو پایانه
استفاده می‌کند. آنها، در هر دو جهت، برای حمل کردن اطلاعات جدول پویا استفاده
می‌شوند.
