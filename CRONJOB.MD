Cronjob

# Tutorial Lengkap
```
https://gosigitgo.wordpress.com/2010/03/18/tutorial-penggunaan-crontab-scheduler-di-ubuntu/
```

## Langkah
- Menjadikan .sh executable
```
chmod +x /home/sysadmin/Parking_Scheduler/CheckExtendBooking/CheckExtendBooking.sh
```

- ketik sudo crontab -e 
```
Tambahkan line (dieksekusi 5 menit sekali):
*/5 * * * * /home/sysadmin/Parking_Scheduler/CheckExtendBooking/CheckExtendBooking.sh
```

- cek list of cronjob
```
sudo crontab -l
```

# Catatan
after edit file di crontab -e, cronjob akan otomatis tereksekusi tanpa harus restart atau apapun
