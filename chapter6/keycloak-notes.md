### Obtin the token

curl --location --request POST 'http://192.168.76.2:30946/auth/realms/sandbox/protocol/openid-connect/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'grant_type=client_credentials' \
--data-urlencode 'client_id=mi-tienda-gt' \
--data-urlencode 'client_secret=uw3U85kvjhcjxwM3W9OhXGE1v8yMCYWo'


### Do not forget add roles, then assign to the client ID!!
