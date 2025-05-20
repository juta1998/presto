
# presto
presto

java download: https://www.oracle.com/java/technologies/downloads/?er=221886#jdk24-windows


راهنمای نصب Presto روی ویندوز بدون استفاده از Docker
در این راهنما، شما را گام به گام برای نصب Presto روی یک سیستم ویندوزی بدون استفاده از Docker راهنمایی می‌کنم.

پیش‌نیازها
سیستم عامل: ویندوز 10 یا 11 (یا نسخه‌های جدیدتر)

Java JDK 11 یا 17 (نسخه 64 بیتی)

حداقل 4GB RAM (8GB یا بیشتر توصیه می‌شود)

حداقل 2GB فضای دیسک خالی

مرحله 1: نصب Java JDK
آخرین نسخه Java JDK را از وبسایت اوراکل دانلود کنید

فایل اجرایی را نصب کنید

متغیرهای محیطی را تنظیم کنید:

JAVA_HOME را به مسیر نصب JDK تنظیم کنید (مثلاً C:\Program Files\Java\jdk-17.0.1)

مسیر bin را به متغیر PATH اضافه کنید (%JAVA_HOME%\bin)

مرحله 2: دانلود Presto
به صفحه دانلود Presto در prestodb.io بروید

آخرین نسخه پایدار را دانلود کنید (مثلاً presto-server-0.280.tar.gz)

فایل را در مسیر مورد نظر (مثلاً C:\presto) اکسترکت کنید

مرحله 3: پیکربندی Presto
در مسیر نصب Presto، پوشه‌های زیر را ایجاد کنید:

C:\presto\etc
C:\presto\etc\catalog
C:\presto\data
فایل node.properties را در پوشه etc ایجاد کنید با محتوای:

properties
node.environment=production
node.id=ffffffff-ffff-ffff-ffff-ffffffffffff
node.data-dir=C:/presto/data
فایل jvm.config را در پوشه etc ایجاد کنید با محتوای:

config
-server
-Xmx2G
-XX:+UseG1GC
-XX:G1HeapRegionSize=32M
-XX:+UseGCOverheadLimit
-XX:+ExplicitGCInvokesConcurrent
-XX:+HeapDumpOnOutOfMemoryError
-XX:+ExitOnOutOfMemoryError
فایل config.properties را در پوشه etc ایجاد کنید با محتوای:

properties
coordinator=true
node-scheduler.include-coordinator=true
http-server.http.port=8080
query.max-memory=1GB
query.max-memory-per-node=1GB
query.max-total-memory-per-node=2GB
discovery-server.enabled=true
discovery.uri=http://localhost:8080
برای تست، یک کاتالوگ نمونه ایجاد کنید. فایل etc/catalog/tpch.properties را با محتوای زیر ایجاد کنید:

properties
connector.name=tpch
مرحله 4: اجرای Presto
خط فرمان (CMD) را به عنوان administrator باز کنید

به مسیر نصب Presto بروید:

cd C:\presto
سرور Presto را اجرا کنید:

bin\launcher run
مرحله 5: نصب CLI (اختیاری)
برای اتصال به Presto از خط فرمان:

فایل presto-cli-0.280-executable.jar را از صفحه دانلود Presto دانلود کنید

آن را به presto-cli.jar تغییر نام دهید

برای اجرا:

java -jar presto-cli.jar --server localhost:8080 --catalog tpch --schema default
مرحله 6: تست نصب
در CLI، یک کوئری تستی اجرا کنید:

sql
SELECT * FROM tpch.tiny.nation;
باید نتیجه کوئری را مشاهده کنید

نکات اضافی
برای استفاده از کانکتورهای دیگر (مثل MySQL، PostgreSQL و...) فایل‌های properties مربوطه را در پوشه etc/catalog ایجاد کنید

برای اجرای Presto به عنوان سرویس، می‌توانید از ابزارهایی مثل NSSM استفاده کنید

لاگ‌ها در مسیر C:\presto\data\var\log ذخیره می‌شوند

اگر در هر مرحله به مشکل برخوردید، لاگ‌ها را بررسی کنید و مطمئن شوید که Java به درستی نصب و پیکربندی شده است.


