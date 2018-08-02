# Tinker (sql dengan `query builder` di CLI)
```
php artisan tinker
```
```
>>> DB:table('user_customer')->toSql()
=> "select * from "user_customer"
```
```
>>> DB::table('user_customer')->first()
=> {#901
     +"id": 1,
     +"user_name": "janurqq",
     +"pscode": null,
     +"email": "janur.qq@gmail.com",
     +"phonenumber": "0856787612",
     +"address": "jl jayasrana",
     +"password": "$2y$10$zueNIrgEbFmxztDWlZTy9uX4JuXvN9hAudctNgd7BOChnJjURVSlW",
     +"gender": "m",
     +"fullname": "janur qindi",
     +"id_role": 17,
     +"date_of_birth": "2017-09-13 00:00:00",
     +"id_identity_card_type_id": null,
     +"id_identity_card": null,
     +"sms_activation_code": null,
     +"sms_confirmation": null,
     +"email_activation_code": null,
     +"email_confirmation": null,
     +"created_on": null,
     +"created_by": null,
     +"modified_on": null,
     +"modified_by": null,
     +"status_flag": "1",
     +"created_at": "2017-11-08 16:07:15",
     +"updated_at": null,
     +"is_verification": null,
     +"code_verification": null,
     +"id_membership": null,
     +"expired_code": null,
   }

```

