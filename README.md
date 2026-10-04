# PasarGuard Rathole Relay برای Railway

این سرویس فقط Rathole server است و باید روی Railway اجرا شود. Node اصلی PasarGuard روی Runsite/Render اجرا می‌شود و با یک اتصال outbound به این Relay وصل می‌شود.

## Environment variables

```text
RATHOLE_TOKEN=<یک secret طولانی و تصادفی>
RATHOLE_CONTROL_PORT=2333
RATHOLE_DATA_PORT=62050
RATHOLE_USER_PORT=8443
```

همین `RATHOLE_TOKEN` باید در سرویس Node روی Runsite هم تنظیم شود.

## TCP Proxyهای Railway

سه TCP Proxy برای Service بساز:

| کاربرد | پورت داخلی |
|---|---:|
| Rathole control | 2333 |
| PasarGuard Node API | 62050 |
| User traffic | 8443 |

آدرس Proxy کنترل را در `RATHOLE_SERVER_ADDR` سرویس Runsite قرار بده. آدرس و public port Proxy دادهٔ 62050 را در پنل PasarGuard به‌عنوان Address و Port وارد کن.

## نکات مهم

- این سرویس HTTP نیست؛ برای آن Health Check HTTP تنظیم نکن.
- Relay نباید Sleep شود و باید روی حالت همیشه‌فعال/پایدار Railway اجرا شود.
- `RATHOLE_TOKEN` را در Repository یا Log عمومی قرار نده.
- `EXPOSE` در Dockerfile به‌تنهایی Proxy عمومی Railway نمی‌سازد؛ TCP Proxyها را از داشبورد Railway ایجاد کن.
- این Relay فقط دو mapping دارد: API نود و ترافیک کاربر. اگر پنل برای data-plane پورت‌های دیگری بسازد، برای هر پورت باید mapping و Proxy جداگانه اضافه شود.
