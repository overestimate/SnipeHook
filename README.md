# SnipeHook
SnipeHook is a free (as in freedom) webhook for sniping coming soon. <!--hosted at https://snipehook.herokuapp.com.-->

## Why?
Ease of use and a consolidated service. This makes announcing easier.

# Non-Applicable Sectors
All sections that aren't applicable until launch are here; they will remain until launch. This is basically pre-documentation.
## Can't someone just spam the endpoint?
<h4> NO. </h4>
In order to prevent spamming, the service has the following restrictions:
 - requires a JWT from Mojang so that the name and UUID are verified correctly
   - hence, this is open-source. the heroku is deployed directly from this repo
 - only allows requests for that particular name 15 minutes after it is claimed
 - name becomes unclaimable afterwards for 30 minutes (long enough that it can't be ran again)
 - internally, the verification has a lock object per-username
 - a rate-limit of 30 seconds is put in place (`Retry-After` will contain the remaining time in seconds)

## Verification
### Request
To verify a name, send a POST request as below:
<details>
  <summary>Click here to expand request.</summary>

All headers provided are **required**. Your request will be denied (see below) unless you provide all headers.
```http
POST https://snipehook.herokuapp.com/verify/$profileName HTTP/1.1
Content-Type: application/json
Authorization: JWT <token>
Accept: application/json

{"name": "$profileName", "id": "$profileID", "wh": {"name": "$sniperName", "icon": "$iconURL"}}
```
</details>
  
### Response
<details>
  <summary>Click to expand responses.</summary>
  
A list of possible responses is found below.
  - Success
    - HTTP Status 200
    - Content of `{}`
  - Invalid Request Error
    - HTTP Status 400
    - Content of `{"msg": "Invalid request. Try again later."}`
  - Authentication Failure
    - HTTP Status 401
    - Content of `{"msg": "Invalid authentication."}`
  - Verification Failure
    - HTTP Status 403
    - Content of `{"msg": "Verification failed." "add": "Check that the account owns the username and try again."}`
  - Webhook Failure
    - HTTP Status 500
    - Content of `{"msg": "Webhook link invalid."`}
      - This is a fuck-up on the service's end, open an issue if this occurs.
  </details>
