# HydraProxy API Documentation v1.0

### API General Usage Info

- **All POST data digit parameters must be sent as **strings of digits** (NOT integers).**
- **Only order management is allowed via the API (updating whitelisted IPs, changing location, etc). Order placement (buy or extending a proxy order) must be performed manually from your account dashboard at: [app.hydraproxy.com](app.hydraproxy.com)app.hydraproxy.com**
- All requests must be sent via HTTPS
- API Key must be included in the **Authorization** header with the format `Token api_token_string`
- **GET** or **POST** method must be specified. URLs for **POST** methods must end with a forward slash **/**
- The API recognizes requests sent only from whitelisted IP addresses (you can provide a maximum of three IP addresses from where the API will be called)
- You can update the whitelisted IPs at anytime in the future (provided a 30 minutes change lockdown after the last update)
- Whitelisting an IP address is done manually. You can request a change at any time
- All dates/hours/ timestamps are in UTC time
- API responses are in json format
- The first item of the response is the request status ("OK" or "ERROR")

**ERROR Response Example**

```
{  "status":"ERROR",
    "url": "api/proxy_details/",
    "message":"unauthorised_request"
}
```

# How to use the API

### Required Headers

All API calls must include the following two headers.

```
Accept: application/json
Authorization: Token extended_api_token_string
```

1. [GET **api/get-account-info/**](#api/get-account-info)
2. [POST **api/view-usage-history/**](#api/view-usage-history)

---

## api/get-account-info

Returns the details of your HydraProxy.com account

**GET** ```https://app.hydraproxy.com/api/get-account-info/```

*Headers*
```
Accept: application/json
Authorization: Token extended_api_token_string
```

**Response**

```
{
    "status": "OK",
    "username": "acc_username",
    "email": "you@domain.com",
    "balance_usd": 180.0,
}
```


## api/view-usage-history

**NOTE 1**: For current day usage history there is a delay in displaying the exact usage. Final usage for the day can be retrived at the end of day (UTC time).

**NOTE 2**: The History usage won’t be available for all the data set. An order's old history gets deleted to avoid large API responses, so you'll probably need to save it in the database. If you have opted for the extended usage log, you will be able to retrieve all your order's usage since extended usage activation.

**POST** ```https://app.hydraproxy.com/api/view_usage_history/```

*Headers*
```
Accept: application/json
Authorization: Token extended_api_token_string
```
*Accepted key:value Data*

| Key (low caps)  | Value Type (CAPS or digits)   | Options     | Description  |
| -------         | -------           | ---------------------   | -----------  |
| order_id        | 123XX             | 1 to XXXX | Insert the order ID *as string*. |

**Response for unused bandwidth**
```
{
    "status": "OK",
    "order_id": 43440,
    "usage": null
}
```

**Response for used bandwidth**
```
{
    "status": "OK",
    "order_id": 37798,
    "usage": [
        {
            "date": "2022-04-25",
            "bandwidth_upload_mb": 345.83,
            "bandwidth_download_mb": 839.63,
            "bandwidth_total_mb": 1185.46
        },
        {
            "date": "2022-04-23",
            "bandwidth_upload_mb": 375.58,
            "bandwidth_download_mb": 609.80,
            "bandwidth_total_mb": 985.38
        }
        {
            "date": "2022-04-22",
            "bandwidth_upload_mb": 140.44,
            "bandwidth_download_mb": 1359.63,
            "bandwidth_total_mb": 1500.07
        }
    ]
}
```
