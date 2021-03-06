# Configure edge destinations and projections using APIs

This tutorial provides step-by-step instructions for working with edge destinations and projections using the [Real-time Customer Profile API](../../../../../../acpdr/real-time-customer-profile.yaml), including:

* [Listing existing destinations](#list-destinations)  
* [Creating a destination](#create-a-destination)  
* [Listing projection configurations](#list-projection-configurations)  
* [Creating a projection configuration](#create-a-projection-configuration)
    * [Configuring selector parameters](#selector)

## Getting started

This tutorial requires a working understanding of the Experience Platform services involved in edge destinations and projections. Before beginning this tutorial, please review the documentation for the following services:

* [Real-time Customer Profile](../../technical_overview/unified_profile_architectural_overview/unified_profile_architectural_overview.md): Provides a unified, real-time consumer profile based on aggregated data from multiple sources.
* [Experience Data Model (XDM)](../../technical_overview/schema_registry/xdm_system/xdm_system_in_experience_platform.md): The standardized framework by which Platform organizes customer experience data.

The following sections provide additional information that you will need to know in order to successfully make calls to the Platform APIs.

### Reading sample API calls

This tutorial provides example API calls to demonstrate how to format your requests. These include paths, required headers, and properly formatted request payloads. Sample JSON returned in API responses is also provided. For information on the conventions used in documentation for sample API calls, see the section on [how to read example API calls](../../technical_overview/platform_faq_and_troubleshooting/platform_faq_and_troubleshooting.md#how-do-i-format-an-api-request) in the Experience Platform troubleshooting guide.

### Gather values for required headers

In order to make calls to Platform APIs, you must first complete the [authentication tutorial](../../tutorials/authenticate_to_acp_tutorial/authenticate_to_acp_tutorial.md). Completing the authentication tutorial provides the values for each of the required headers in all Experience Platform API calls, as shown below:

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

All resources in Experience Platform are isolated to specific virtual sandboxes. All requests to Platform APIs require a header that specifies the name of the sandbox the operation will take place in:

* x-sandbox-name: `{SANDBOX_NAME}`

> **Note:** For more information on sandboxes in Platform, see the [sandbox overview documentation](../../technical_overview/sandboxes/sandboxes-overview.md). 

Requests that contain a payload (POST, PUT, PATCH) require an additional `Content-Type` header. The following types are used in this tutorial:

- Content-Type: application/json
- Content-Type: application/vnd.adobe.platform.projectionDestination+json; version=1

## List destinations 

You can list the edge destinations that have already been configured for your organization by making a GET request to the `/config/destinations` endpoint.

#### API format

```http
GET /config/destinations
```

#### Request

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/config/destinations \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

#### Response

If edge destinations have been configured for your organization, the `projectionDestinations` array returns the details for each destination as an individual object within the array. If no projections have been configured, the `projectionDestinations` array will be empty.

```json
{
    "_links": {
        "self": {
            "href": "/data/core/ups/config/destinations",
            "templated": false
        }
    },
    "_embedded": {
        "projectionDestinations": [
            {
                "_links": {
                    "self": {
                        "href": "/data/core/ups/config/destinations/9d66c06e-c745-480c-b64c-1d5234d25f4b",
                        "templated": false
                    }
                },
                "id": "9d66c06e-c745-480c-b64c-1d5234d25f4b",
                "type": "EDGE",
                "topic": "ups-edgeprofile-dev-va711-eciobanu",
                "version": 1
            }
        ]
    }
}
```

## Create a destination

If the destination you require does not already exist, you can create a new projection destination by making a POST request to the `/config/destinations` endpoint. 

#### API format

```http
POST /config/destinations
```

#### Request

The following request creates a new edge destination. 

> **Note:** The POST request to create a destination requires a specific `Content-Type` header, as shown below. Using an incorrect `Content-Type` header results in an HTTP Status 415 (Unsupported Media Type) error.

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/config/destinations \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/vnd.adobe.platform.projectionDestination+json; version=1' \
  -d '{
        "type": "EDGE",
        "dataCenters": [ 
          "OR1" 
        ],
        "ttl": 3600,
        "replicationPolicy": REACTIVE
      }'
```

* **type**: **(Required)** The type of destination to be created. The only accepted value, "EDGE", creates an edge destination.
* **dataCenters**: **(Required)** A string array that lists the edges toward which projections are to be routed. May contain one or more of the following values:
    * "OR1" - Western United States
    * "VA5" - Eastern United States
    * "NLD1" - EMEA
* **ttl**: **(Required)** Specifies projection expiration. Accepted value range: 600 to 604800. Default value: 3600.
* **replicationPolicy**: **(Required)** Defines the behavior of the data replication from the hub to the edges. Default value: REACTIVE. Supported values:
    * PROACTIVE
    * REACTIVE

#### Response

A successful response returns the details of the newly created edge destination, including the read-only, system-generated unique ID (`"id"`).

```json
{
    "self": {
        "href": "/data/core/ups/config/destinations/cc5a3bd1-f2b9-4965-b9bd-4e7416a02cd4",
        "templated": false
    },
    "id": "cc5a3bd1-f2b9-4965-b9bd-4e7416a02cd4",
    "type": "EDGE",
    "dataCenters": [
       "OR1"
    ],
    "ttl": 3600,
    "version": 1
}
```

* **self.href**: This path is used to lookup (GET) the destination directly and can also be used for updating (PUT) or deleting (DELETE) the destination.
* **id**: The read-only, system-generated unique ID for the destination. This ID is used to reference the destination directly and when creating projection configurations.
* **version**: This read-only value shows the current version of the destination. When a destination is updated, the version number automatically increments.

## List projection configurations

You can lookup and list projection configurations that have been created for your organization by making a GET request to the `/config/projections` endpoint. You can also add optional parameters to the request path to access projection configurations for a particular schema or lookup an individual projection by its name.

#### API format

```http
GET /config/projections
GET /config/projections?schemaName={SCHEMA_NAME}
GET /config/projections?schemaName={SCHEMA_NAME}&name={PROJECTION_NAME}
```
* `{SCHEMA_NAME}`: The name of the schema associated with the projection configuration you want to access.
* `{PROJECTION_NAME}`: The name of the projection configuration you want to access.

> **Note:** `schemaName` is required when using the `name` parameter, as a projection configuration name is only unique in the context of a schema.

#### Request

The following request lists all projection configurations associated with the XDM Individual Profile schema.

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/config/projections?schemaName=_xdm.context.profile \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

#### Response

A successful response returns a list of projection configurations within the root `_embedded` attribute, contained within the `projectionConfigs` array. If no projection configurations have been made for your organization, the `projectionConfigs` array will be empty.

```json
{
    "_links": {
        "self": {
            "href": "/data/core/ups/config/projections",
            "templated": false
        }
    },
    "_embedded": {
        "projectionConfigs": [
            {
                "_links": {
                    "destination": {
                        "href": "/data/core/ups/config/destinations/a689248a-5d2b-44af-bb70-c8f17f97011b",
                        "templated": false
                    },
                    "self": {
                        "href": "/data/core/ups/config/projections/99aed0bc-c183-4997-ada7-7843642e08f6",
                        "templated": false
                    }
                },
                "_embedded": {
                    "destination": {
                        "self": {
                            "href": "/data/core/ups/config/destinations/a689248a-5d2b-44af-bb70-c8f17f97011b",
                            "templated": false
                        },
                        "id": "a689248a-5d2b-44af-bb70-c8f17f97011b",
                        "type": "EDGE",
                        "ttl": 1000,
                        "dataCenters": [
                            "OR1"
                        ],
                        "version": 1
                    }
                },
                "selector": "strategy",
                "version": 2,
                "id": "99aed0bc-c183-4997-ada7-7843642e08f6",
                "schemaName": "_xdm.context.profile",
                "name": "adcloud_rlsa",
                "destinationId": "a689248a-5d2b-44af-bb70-c8f17f97011b",
                "mergePolicyId": "90b2977c-afb0-4b93-aea3-2eea5f26131d"
            },
        ]
    }
}
```

## Create a projection configuration

You can create (POST) a new projection configuration that will dictate which XDM fields are made available on the edges to be persisted or published by Projection Service.

#### API format

```http
POST /config/projections?schemaName={SCHEMA_NAME}
```

* `{SCHEMA_NAME}`: The name of the schema associated with the XDM fields you want to configure for projection.

#### Request

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/config/projections?schemaName=_xdm.context.profile \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "selector": "emails,person(firstName)",
        "name": "my_test_projection",
        "destinationId": "cc5a3bd1-f2b9-4965-b9bd-4e7416a02cd4",
        "mergePolicyId": "90b2977c-afb0-4b93-aea3-2eea5f26131d"
      }'
```

* `selector`: A string containing a list of properties within the schema that are to be replicated to the edges. This creates a mask to apply on top of Real-time Customer Profile data to generate the projection. To learn more about how to construct selectors, see the [selector](#selector) section below.
* `name`: A descriptive name for the new projection configuration.
* `destinationId`: The identifier for the edge destination the data will be projected to.

#### Response

A successful response returns the details of the newly created projection configuration.

```json
{
    "_links": {
        "destination": {
            "href": "/data/core/ups/config/destinations/cc5a3bd1-f2b9-4965-b9bd-4e7416a02cd4",
            "templated": false
        },
        "self": {
            "href": "/data/core/ups/config/projections/87fcd0bc-c183-4997-daf9-7843642g95a1",
            "templated": false
        }
    },
    "_embedded": {
        "destination": {
            "self": {
                "href": "/data/core/ups/config/destinations/cc5a3bd1-f2b9-4965-b9bd-4e7416a02cd4",
                "templated": false
            },
            "id": "cc5a3bd1-f2b9-4965-b9bd-4e7416a02cd4",
            "type": "EDGE",
            "ttl": 1000,
            "dataCenters": [
                "OR1"
            ],
            "version": 1
        }
    },
    "selector": "emails,person(firstName)",
    "version": 2,
    "id": "87fcd0bc-c183-4997-daf9-7843642g95a1",
    "schemaName": "_xdm.context.profile",
    "name": "my_test_projection",
    "destinationId": "cc5a3bd1-f2b9-4965-b9bd-4e7416a02cd4",
    "mergePolicyId": "90b2977c-afb0-4b93-aea3-2eea5f26131d"
}
```

## Selector

A selector is a comma-separated list of XDM field names. In a projection configuration, the selector designates the properties to be included in projections.

The format of the `selector` parameter value is loosely based on XPath syntax. Supported syntax is summarized below, with additional examples provided for reference.

### Supported syntax

* Use commas to select multiple fields. Do not use spaces.
* Use dot-notation to select nested fields. 
    * For example, to select a field named `field` which is nested within a field named `foo`, use the selector `foo.field`.
* When including a field that contains sub-fields, all sub-fields are also projected by default. However, you can filter the sub-fields returned by using parentheses `"( )"`.
    * For example, `addresses(type,city.country)` returns only the address type and the country in which the address city is located for each `addresses` array element.
    * The above example is equivalent to `addresses.type,addresses.city.country`.

> **Note:** Both dot-notation and parenthetical notation are supported for referencing sub-fields. However, it is best practice to use dot-notation because it is more concise and provides a better illustration of field hierarchy.

* Each field in a selector is specified relative to the root of the response.
    * If the data is a collection of resources, the projection will include an array of resources.
    * If the data is a single resource, the projection will include fields that are relative to that resource.
    * If the field you select is (or is part of) an array, the projection will include the selected portion of all elements in the array.

### Examples of the selector parameter

The following examples show sample `selector` parameters, followed by the structured values they represent.

**"person.lastName"**

Returns the `lastName` sub-field of the `person` object in the requested resource.

```json
{
  "person": {
    "lastName": "Smith"
  }
}
```

**"addresses"**

Returns all elements in the `addresses` array, including all fields in each element, but no other fields.	

```json
{
  "addresses": [
    {
      "type": "home",
      "street1": "100 Great Mall Parkway",
      "city": {
        "name": "San Jose",
        "country": "United States"
      }
    },
    {
      "type": "work",
      "street1": "1 Main Street",
      "city": {
        "name": "San Jose",
        "country": "United States"
      }
    }
  ]
}
```

**"person.lastName,addresses"**

Returns the `person.lastName` field and all elements in the `addresses` array.	

```json
{
  "person": {
    "lastName": "Smith"
  },
  "addresses": [
    {
      "type": "home",
      "street1": "100 Great Mall Parkway",
      "city": {
        "name": "San Jose",
        "country": "United States"
      }
    },
    {
      "type": "work",
      "street1": "1 Main Street",
      "city": {
        "name": "San Jose",
        "country": "United States"
      }
    }
  ]
}
```

**"addresses.city"**

Returns only the city field for all elements in the addresses array.

```json
{
  "addresses": [
    {
      "city": {
        "name": "San Jose",
        "country": "United States"
      }
    },
    {
      "city": {
        "name": "San Jose",
        "country": "United States"
      }
    }
  ]
}
```

> **Note:** Whenever a nested field is returned, the projection includes the enclosing parent objects. The parent fields do not include any other child fields unless they are also selected explicitly.

**"addresses(type,city)"**

Returns only the values of the `type` and `city` fields for each element in the `addresses` array. All other sub-fields contained in each `addresses` element are filtered out.

```json
{
  "addresses": [
    {
      "type": "home",
      "city": {
        "name": "San Jose",
        "country": "United States"
      }
    },
    {
      "type": "work",
      "city": {
        "name": "San Jose",
        "country": "United States"
      }
    }
  ]
}
```

## Next Steps

After completing this tutorial you should understand how to configure edge destinations and projections for your organization, including how to properly format the `selector` parameter. You can now create additional edge destinations and projections or review the [Real-time Customer Profile overview](../../technical_overview/unified_profile_architectural_overview/unified_profile_architectural_overview.md) to learn how to take additional actions using the Profile API and UI.