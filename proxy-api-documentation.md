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

## Endpoints

1. [GET **api/get-account-info/**](#get-account-info)
2. [POST **api/view-usage-history/**](#proxy-details)
3. [POST **api/update-whitelist-ip/**](#update-whitelist-ip)
4. [POST **api/update-location/**](#update-location)
5. [POST **api/update-rotation/**](#update-rotation)
6. [GET **api/get-locations-list/**](#get-locations-list)
7. [POST **api/view-usage-history/**](#view-usage-history)
 
---

## get-account-info

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

## proxy-details

**NOTE 1**: Order ID must be specified (as a string, not integers).

**POST** ```https://app.hydraproxy.com/api/proxy-details/```

*Headers*
```
Accept: application/json
Authorization: Token extended_api_token_string
```

*Accepted key:value Data*

| Key (low caps)  | Value Type (CAPS or digits)   | Options     | Description  |
| -------         | -------           | ---------------------   | -----------  |
| order_id        | 29139             | 1 to XXXX | Insert the order ID value *as string*. |

All proxy orders will return the following data common to all your orders:

```
{
    "status": "OK",
    "url": "api/proxy_details/",
    "order_id": 29139,
    "order_status": "Active",
    "order_info": {
        "network": "Residential",
        "date_start": "2022-05-21 15:47:35",
        "date_end": "",
        "ports": 1,
        "price_usd": 50.0,
        "usage": "Scraping test"
    }
}
```

**Response for individual networks**

**Residential proxies**

```
{
    "status": "OK",
    "url": "api/proxy_details/",
    "order_id": 29139,
    "order_status": "Active",
    "order_info": {
        "network": "Residential",
        "date_start": "2022-05-21 15:47:35",
        "date_end": "",
        "ports": 1,
        "price_usd": 50.0,
        "usage": "Scraping test"
    },
    "proxy_info": {
        "protocol": "HTTPS",
        "username": "proxyuser",
        "password": "proxypass",
        "bandwidth_start_gb": 10,
        "bandwidth_available_gb": 17
    },
    "proxy": {
        "hostname": "proxy.hydraproxy.com",
        "server_ip": "12.34.56.78",
        "port": 1234
    }
}

```

**Mobile proxies**

```whitelist_ip_delta_minutes```, ```location_delta_minutes```, ```next_ip_rotation_delta_minutes``` express the amount of minutes left until a certain order update can be performed. A particular update can be performed only after its delta displays 0 (zero).

```
{
    "status": "OK",
    "url": "api/proxy_details/",
    "order_id": 29139,
    "order_status": "Active",
    "order_info": {
        "network": "Mobile",
        "subnetwork": "4G Mobile",
        "date_start": "2022-05-27 06:58:04",
        "date_end": "2022-05-28 06:58:04",
        "ports": 2,
        "days": 1,
        "price_usd": 5.9,
        "usage": "smm"
    },
    "proxy_info": {
        "protocol": "HTTPS",
        "whitelist_ip": "1.22.3.44",
        "whitelist_ip_unlock_time": "2022-05-27 07:28:04",
        "whitelist_ip_delta_minutes": 0,
        "location": "Random",
        "location_unlock_time": "2022-05-27 07:28:04",
        "location_delta_minutes": 0,
        "isp": "Random",
        "rotation_auto": true,
        "next_ip_rotation": "2022-05-27 07:59:03",
        "next_ip_rotation_delta_minutes": 17
    },
    "proxy": {
        "hostname": "proxy.hydraproxy.com",
        "server_ip": "12.34.56.78",
        "port": [
            "1235",
            "1236"
        ]
    }
}
```

**Static mobile proxies**
```
{
    "status": "OK",
    "url": "api/proxy_details/",
    "order_id": 29139,
    "order_status": "Active",
    "order_info": {
        "network": "Static",
        "date_start": "2022-05-27 06:59:02",
        "date_end": "2022-05-28 06:59:05",
        "ports": 1,
        "days": 1,
        "price_usd": 3.0,
        "usage": "test geo"
    },
    "proxy_info": {
        "protocol": "HTTPS",
        "whitelist_ip": "1.22.3.44",
        "whitelist_ip_unlock_time": "2022-05-27 07:29:05",
        "whitelist_ip_delta_minutes": 0,
        "public_ip": "99.88.77.66",
        "connection": "Wi-Fi",
        "country": "United States",
        "country_code": "US",
        "state": "Alabama",
        "state_code": "AL",
        "city": "Millbrook",
        "isp": "",
        "zip": "",
        "spam": "Clean"
    },
    "proxy": {
        "hostname": "proxy.hydraproxy.com",
        "server_ip": "12.34.56.78",
        "port": 1234
    }
}
```

## update-whitelist-ip

**NOTE 1**: Order ID must be specified (as a string, not integers).

**POST** ```https://app.hydraproxy.com/api/update-whitelist-ip/```

*Headers*
```
Accept: application/json
Authorization: Token extended_api_token_string
```

*Accepted key:value Data*

| Key (low caps)  | Value Type (CAPS or digits)   | Options     | Description  |
| -------         | -------           | ---------------------   | -----------  |
| order_id        | 29139             | 1 to XXXX | Insert the order ID value *as string*. |
| whitelist_ip        | 22.22.2.11             | IPv4 Format | Only IPv4 IPs are allowed for whitelisting. |

All responses will return both ```status``` and ```update_status```. 
```status``` - for all requests that reached the API server. 
```update_status``` - informs you if the update/change has been succesfully applied.

**Response for update applied successfully**

```
{
    "status": "OK",
    "url": "api/update-whitelist-ip/",
    "order_id": 29139,
    "update_info": {
        "update_status": "OK",
        "whitelist_ip": "22.22.2.11",
        "whitelist_ip_unlock_time": "2022-05-27 08:24:13",
        "whitelist_ip_delta_minutes": 30
    }
}
```

**Response for update error**

```
{
    "status": "OK",
    "url": "api/update-whitelist-ip/",
    "order_id": 29139,
    "update_info": {
        "update_status": "ERROR",
        "whitelist_ip": "11.11.1.22",
        "whitelist_ip_unlock_time": "2022-05-23 11:00:09",
        "whitelist_ip_delta_minutes": 28,
        "message": "Whitelist IP not editable yet."
    }
}

```

## update-location

**NOTE 1**: Order ID must be specified (as a string, not integers).

**NOTE 2**: Available only for mobile proxies (subnetworks: 4G, Wi-Fi and 4G & Wi-Fi).

**POST** ```https://app.hydraproxy.com/api/update-location/```

*Headers*
```
Accept: application/json
Authorization: Token extended_api_token_string
```

*Accepted key:value Data*

| Key (low caps)  | Value Type (CAPS or digits)   | Options     | Description  |
| -------         | -------           | ---------------------   | -----------  |
| order_id        | 29139             | 1 to XXXX | Insert the order ID value *as string*. |
| location        | FL          | US state | Only US state 2-letter code. |

A list of available US states can be found by calling the endpoint [**api/get-locations-list/**](#get-locations-list).

All responses will return both ```status``` and ```update_status```. 
```status``` - for all requests that reached the API server. 
```update_status``` - informs you if the update/change has been succesfully applied.

**Response for update applied successfully**

```
{
    "status": "OK",
    "url": "api/update-location/",
    "order_id": 29139,
    "update_info": {
        "update_status": "OK",
        "location": "FL",
        "location_unlock_time": "2022-05-27 08:19:44",
        "location_delta_minutes": 30
    }
}
```

**Response for update error**

```
{
    "status": "OK",
    "url": "api/update-location/",
    "order_id": 29139,
    "update_info": {
        "update_status": "ERROR",
        "location": "FL",
        "location_unlock_time": "2022-05-27 08:19:44",
        "location_delta_minutes": 29,
        "message": "Location not editable yet."
    }
}

```

**Response for trying to submit an order_id from a proxy other than mobile one.**

```
{
    "status": "OK",
    "url": "api/update-location/",
    "order_id": 29139,
    "update_info": {
        "update_status": "ERROR",
        "message": "Update error for this proxy type."
    }
}
```

## update-rotation

**NOTE 1**: Order ID must be specified (as a string, not integers).

**NOTE 2**: Available only for mobile proxies (all subnetworks: 4G, Wi-Fi and 4G & Wi-Fi and Mobile Carrier).

**POST** ```https://app.hydraproxy.com/api/update-rotation/```

*Headers*
```
Accept: application/json
Authorization: Token extended_api_token_string
```

*Accepted key:value Data*

| Key (low caps)  | Value Type   | Options     | Description  |
| -------         | -------           | ---------------------   | -----------  |
| order_id        | 29139             | 1 to XXXX | Insert the order ID value *as string*. |
| rotation_auto        | true          | true, false | use true of force 30min rotation and false for extended rotation. |

All responses will return both ```status``` and ```update_status```. 
```status``` - for all requests that reached the API server. 
```update_status``` - informs you if the update/change has been succesfully applied.

**Response for ```false``` extended rotation**

```
{
    "status": "OK",
    "url": "api/update-rotation/",
    "order_id": 29139,
    "update_info": {
        "update_status": "OK",
        "rotation_auto": false,
        "next_ip_rotation": ""
    }
}
```

**Response for ```true``` (30min auto rotation)**

```
{
    "status": "OK",
    "url": "api/update-rotation/",
    "order_id": 29139,
    "update_info": {
        "update_status": "OK",
        "rotation_auto": true,
        "next_ip_rotation": "2022-05-27 08:22:48"
    }
}

```

## get-locations-list

Returns all US states and the available network in each one.

**GET** ```https://app.hydraproxy.com/api/get-locations-list/```

*Headers*
```
Accept: application/json
Authorization: Token extended_api_token_string
```

**Response**

```
{
    "status": "OK",
    "url": "api/get-locations-list/",
    "all_geo": [
        {
            "state_name": "Alabama",
            "state_code": "AL",
            "available_ports_count": 53,
            "subnetwork_count": {
                "mobile": 20,
                "wifi": 33
            }
        },
        {
            "state_name": "Wisconsin",
            "state_code": "WI",
            "available_ports_count": 107,
            "subnetwork_count": {
                "mobile": 13,
                "wifi": 94
            }
        },
    ]
}

```

## view-usage-history

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
    "order_id": 29139,
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
