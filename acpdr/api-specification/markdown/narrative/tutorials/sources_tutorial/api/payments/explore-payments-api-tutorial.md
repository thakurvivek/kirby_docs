# Explore your payments application using the Flow Service API

Flow Service is used to collect and centralize customer data from various disparate sources within Adobe Experience Platform. The service provides a user interface and RESTful API from which all supported sources are connectable.

This tutorial uses the Flow Service API to explore payments application and includes the following steps:

- [Explore your data tables](#explore-your-data-tables)
- [Inspect the structure of a table](#inspect-the-structure-of-a-table)

## Getting started

This guide requires a working understanding of the following components of Adobe Experience Platform:

- [Sources](https://docs.adobe.com/content/help/en/experience-platform/source-connectors/home.html): Experience Platform allows data to be ingested from various sources while providing you with the ability to structure, label, and enhance incoming data using Platform services.
- [Sandboxes](https://docs.adobe.com/content/help/en/experience-platform/sandbox/home.html): Experience Platform provides virtual sandboxes which partition a single Platform instance into separate virtual environments to help develop and evolve digital experience applications.

The following sections provide additional information that you will need to know in order to successfully connect to a payments application using the Flow Service API.

### Gather required credentials

This tutorial requires you to have a valid connection with the third-party payments application you wish to ingest data from. A valid connection involves your application's connection specification ID and connection ID. More information about creating a payments connection and retrieving these values can be found in the [create a source in the API](../sources-api-tutorial.md) tutorial.

### Reading sample API calls

This tutorial provides example API calls to demonstrate how to format your requests. These include paths, required headers, and properly formatted request payloads. Sample JSON returned in API responses is also provided. For information on the conventions used in documentation for sample API calls, see the section on [how to read example API calls](https://docs.adobe.com/content/help/en/experience-platform/landing/troubleshooting.html#reading-example-api-calls) in the Experience Platform troubleshooting guide.

### Gather values for required headers

In order to make calls to Platform APIs, you must first complete the [authentication tutorial](https://docs.adobe.com/help/en/experience-platform/tutorials/authentication.html). Completing the authentication tutorial provides the values for each of the required headers in all Experience Platform API calls, as shown below:

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

All resources in Experience Platform, including those belonging to the Flow Service, are isolated to specific virtual sandboxes. All requests to Platform APIs require a header that specifies the name of the sandbox the operation will take place in:

- x-sandbox-name: `{SANDBOX_NAME}`

All requests that contain a payload (POST, PUT, PATCH) require an additional media type header:

- Content-Type: `application/json`

## Explore your data tables

Using the connection ID for your payments system, you can explore your data tables by performing GET requests. Use the following call to find the path of the table you wish to inspect or ingest into Platform.

#### API format

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=root
```

- `{BASE_CONNECTION_ID}`: The connection ID of your payments system.

#### Request

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connections/24151d58-ffa7-4960-951d-58ffa7396097/explore?objectType=root' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

#### Response

A successful response returns an array of tables from your payments system. Find the table you wish to bring into Platform and take note of its `path` property, as you are required to provide it in the next step to inspect its structure.

```json
[
    {
        "type": "table",
        "name": "PayPal.Billing_Plans",
        "path": "PayPal.Billing_Plans",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "table",
        "name": "PayPal.Billing_Plans_Payment_Definition",
        "path": "PayPal.Billing_Plans_Payment_Definition",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "table",
        "name": "PayPal.Billing_Plans_Payment_Definition_Charge_Models",
        "path": "PayPal.Billing_Plans_Payment_Definition_Charge_Models",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "table",
        "name": "PayPal.Catalog_Products",
        "path": "PayPal.Catalog_Products",
        "canPreview": true,
        "canFetchSchema": true
    }
]
```

## Inspect the structure of a table

To inspect the structure of a table from your payments system, perform a GET request while specifying the path of a table as a query parameter.

#### API format

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=table&object={TABLE_PATH}
```

| Parameter | Description |
| --------- | ----------- |
| `{BASE_CONNECTION_ID}` | The connection ID of your payments system. |
| `{TABLE_PATH}` | The path of a table within your payments system.|

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connections/24151d58-ffa7-4960-951d-58ffa7396097/explore?objectType=table&object=test1.Mytable' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

#### Response

A successful response returns the structure of the specified table. Details regarding each of the table's columns are located within elements of the `columns` array.

```json
{
    "format": "flat",
    "schema": {
        "columns": [
            {
                "name": "Product_Id",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "Product_Name",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "Description",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "Type",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
        ]
    }
}
```

## Next steps

By following this tutorial, you have explored your payments system, found the path of the table you wish to ingest into Platform, and obtained information regarding its structure. You can use this information in the next tutorial to [retrieve data from your payments system and bring it into Platform](./retrieve-payments-data-api-tutorial.md).