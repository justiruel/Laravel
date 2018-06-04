# ERROR MESSAGE
```
The page has expired due to inactivity. 
Please refresh and try again.
```
tambahkan ini didalam tag form
```
{{ Form::hidden('_method', 'POST') }}
{{  Form::hidden('_token', csrf_token()) }}
```
