# urlabuse-api-doc

## URLAbuse - API Documentation

URLAbuse (urlabuse.com) provides several different types of API end-points for different purposes. You can select the right API end-point based on the provided key and your purpose. Here is the list of API endpoint explained in details:

### Table of Content
- [API Report for trusted parties](#api-report)
- [API Report in General](#get-url)


### api-report

This API is only given to the trusted third parties. One can report URL directly to our database (without manual verification from our side). URLAbuse, assumes that each single entry coming from this end-point is guaranteed to be malicious (phishing, hacked, malware delivery) and then, we will automatically collect the necessary data for the reported URL.

To get access to this API, usually, you must be part of a national CERT, one of the URLAbuse administrator or a trusted person (after having online meeting).

* API URL: [https://urlabuse.com/api_report](https://urlabuse.com/api_report)
* Method: POST
* parameters:
    

|#| Name| Mandatory?| Type | Description|
|-----------|----------|----------------|----------------|----------------|
|1| token| YES| STRING| Contact us to receive the token for authentication|
|2|url|YES|STRING|The malicious URL you want to report|
|3|rtype|YES| STRING| one of the 'phishing', 'malware', 'hacked'|
|4|reporter|NO|STRING|Your name/alias which will appear in the list as reporter|
|5|target|NO|STRING|target of the attack or malware name or a related keyword|
|6|confirmed|YES|INTEGER|either 0 or 1 (1 means you are 100% sure it's malicious)|

Notes:

1. `url` parameter must starts with _https://_ or _http://_
2. `rtype` only supports one of the mentioned 3 values.
3. `reporter` can only have alphanumeric characters. It's a good practice to use your Twitter/X handle or your company name.
4. `target` is the target of the attack in phishing, malware type in case of malware delivery or just simple a keyword like 'compromised' in case of hacked domain name. Use [Title case](https://en.wikipedia.org/wiki/Title_case) style for targets when dealing with phishing.
5. `confirmed` field is very sensitive. Only send **1** if your **100%** sure that you are sending a malicious URL with the correct target. If you are only 99% sure, then set `confirmed` to 0.

#### Warning:
Sending malformed data or false-positive will result in loosing access to the API without any notice. URLAbuse is all about accuracy. So let's keep the data clean and accurate.

#### Example in Python
```python

import json
import requests

API_URL = "https://urlabuse.com/api_report"

url = "https://iamphishing.com/login.php"
token = "<PUT-YOUR-TOKEN-HERE>"
reporter = "mycompany"
target = "Bank of America"
confirmed = 1

params = {"url": url, "token": token, "reporter": reporter,
          "target": target, "confirmed": confirmed}

r = requests.post(API_URL, data=params, timeout=10)

print(r.text)
```

### get-url

This API end-point is easier to use as you just need to know the URL. We don't need target information from you as we will use our own detection system to detect the target.

It's also OK to have some false-positives as our system will detect them. However, please do not submit dead domain name or dead URL (just to have your name appear in the reporter list). A big amount of dead domain name or dead URL will result in loosing access without prior notice.

* API URL: https://urlabuse.com/get_url
* Method: GET and POST
* Parameters:

|#|Name | Mandatory?| description|
|--|----|----------|------------|
|1|token|YES| contact us for the authentication token|
|2|url|YES| The URL you want to report|
|3|reporter|NO|Your name to appear in the list|

Notes:

1. **ONLY** send **phishing** URLs to this end-point.

Example:

```bash
API_URL="https://urlabuse.com/get_url"
TOKEN="YOUR-API-TOKEN"
URL="https://iamphishing.com/login.php"
REPORTER=mycompany
curl -X POST $API_URL -d "token=$TOKEN" -d 'url=$URL' -d 'reporter=$REPORTER'
```

Or you can use the Python code provided for the previous example.







