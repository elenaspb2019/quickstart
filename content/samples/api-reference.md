---
title: "Reference: Retrieve Temperature Data for a Specific City"
date: 2025-02-02
---

The HTTP GET method retrieves historical temperature data for a specified city. The city name is provided as a path parameter in the URL.

## Request

```txt
GET /cities/{city}/temperatures
```

### Request Parameters

| **Parameter** | **Type** | **Description** | **Required** |
|---------------|----------|-----------------|--------------|
| **city**      | string   | The name of the city for which to retrieve temperature data. | Yes |

## Response

A successful request will return a JSON array containing temperature data for the specified city. Each object in the array represents a specific timestamp and its corresponding temperature.

### Example Response:

```json
[
  {
    "timestamp": "2023-11-22T12:34:56Z",
    "temperature": 15.2
  },
  {
    "timestamp": "2023-11-23T08:00:00Z",
    "temperature": 12.8
  }
]
```

### Response Parameters

The API returns a JSON array containing temperature data for the specified city. Each object in the array has the following properties:

| **Property**    | **Type** | **Description** |
|-----------------|----------|-----------------|
| **timestamp**   | string   | The timestamp indicates the time at which the temperature was recorded. The format is ISO 8601. |
| **temperature** | number   | The temperature reading in Celsius. |

## Error Codes

| **Status Code** | **Description** |
|-----------------|-----------------|
| **400 Bad Request** | Invalid request format or missing parameters. |
| **404 Not Found**   | The specified city was not found. |
| **500 Internal Server Error** | An unexpected error occurred on the server. |
