# ERROR MESSAGE
```
The page has expired due to inactivity. 
Please refresh and try again.
```
# Solusi
tambahkan ini didalam tag form
```
{{ Form::hidden('_method', 'POST') }}
{{  Form::hidden('_token', csrf_token()) }}
```
# Solusi lain
hapus kode ini di Kernel.php
```
\App\Http\Middleware\VerifyCsrfToken::class,
```
## Tambahan
html hanya dapat menerima methodc POST dan GET, untuk method lain didalam form tag tambahkan
```
<input name="_method" type="hidden" value="PUT">
<input name="_token" type="hidden" value="yeVnhaht6XSi4EpAFfUIDKpFIblwNGcB9DYkpVsZ">
atau
{{ Form::hidden('_method', 'PUT') }}
{{  Form::hidden('_token', csrf_token()) }}
```

