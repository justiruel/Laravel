
### Konfigurasi Sendmail .ENV
```
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=skyparking.visionet@gmail.com
MAIL_PASSWORD=mazvflxnzqdxrmxk
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=noreply@skyparking.co.id
MAIL_FROM_NAME="Sky Parking"
```

### Jika error saat sendmail
- ikuti langkah di sini : https://stackoverflow.com/a/42558904
- "php artisan config:cache" to update the env variables!
Jika masih error, buka "D:\PROJECT\VISIONET\WEB\BACKEND\GITLAB\ParkingSystemAPI\config\mail.php", tambahkan
```
return [
...
  'stream' => [
      'ssl' => [
            'allow_self_signed' => true,
            'verify_peer' => false,
            'verify_peer_name' => false,
       ],
    ],
]
```
