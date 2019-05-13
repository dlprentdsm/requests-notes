# requests-notes

Login with requests:

Recall that a simple html form is 
```
<form action="/action_page.php" method="get">
  First name: <input type="text" name="fname"><br>
  Last name: <input type="text" name="lname"><br>
  <input type="submit" value="Submit">
</form>
```
with the action and method at the top.

Go to login page in chrome, 
open dev tools, 
go to Network, 
make sure to select ```Preserve Log```
now click 'Login'.
Scroll back to the beginning of the Network history to find the get or post that made the login.
Select Copy and then Copy as cURL
This site then does a nice conversion:
https://curl.trillworks.com/ [https://github.com/NickCarneiro/curlconverter]
Easy peasy. 
Now we can login to a site using requests, without having to open selenium.

Alternatively just use the mechanize library. However, this is only http. For javascript you would need selenium, which then defeats the point of this exercise, which is to use requests only to be super resource light.
```
import mechanize
from bs4 import BeautifulSoup
import urllib2 
import cookielib

cj = cookielib.CookieJar()
br = mechanize.Browser()
br.set_cookiejar(cj)
br.open("https://id.arduino.cc/auth/login/")

br.select_form(nr=0)
br.form['username'] = 'username'
br.form['password'] = 'password.'
br.submit()

print br.response().read()
```

Passing between selenium and requests, worst case scenario:
First you have to get the cookies from your driver instance:

```cookies = driver.get_cookies()```
This returns a set of cookie dictionaries for your session.

Next, set those cookies in requests:

```
s = requests.Session()
for cookie in cookies:
    s.cookies.set(cookie['name'], cookie['value'])
```
