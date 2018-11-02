# MapNYC 2018 Data

StreetCred held a contest in NYC in the Fall of 2018 to allow participants to
compete for Bitcoin rewards by collecting POI data in New York City.
Included in this repository is the resulting data.
Read more about the contest goals and outcomes [here](https://mapnyc.streetcred.co)

You can also learn more about the thinking behind making this dataset public in
this [blog post](https://www.streetcred.co/blog/2018/11/2/opening-mapnyc-data).

### License
MapNYC data is made available under [CDLA - Sharing - Version 1.0](LICENSE.md). CDLA is an open data license created by the Linux Foundation. [Read more here](https://cdla.io/). Additionally, StreetCred Labs, Inc. has no objections to geodata derived from MapNYC being incorporated into OpenStreetMap and released under the ODbL 1.0.

StreetCred Labs, Inc. may also offer this and other datasets under different licenses in the future. Questions? [Get in touch here](mailto:hello@streetcred.co).

### Stats
Record Count: 20,441

### Schema

##### `id` : `UUID string`
StreetCred data aims to provide a stable ID for each record that can be
traced from creation and through the lifetime of each corresponding record.

#### `name` : `string`
Full name of the place. In future schema version we will produce a richer
name object with alternate names and languages support, but for the
purposes of MapNYC and the release of this data we have truncated the name
property to a single string.

#### `categories` : `[integers]`
Currently only a single category is specified for each record, however
the property is an array of integer to allow for flexibility of multiple
categories in the future.

For more information about StreetCred category taxonomy and the canonical
listing of all the possible categories see the
[streetcredlabs/categories](https://github.com/streetcredlabs/categories) repository.

#### `images` : `[object]`
Images are hosted on [S3](https://s3.amazonaws.com/production.images.app.streetcred.co)
and are available under the same License as the data in this repository.

At least one image is required per record. Images property is an array of
image objects with the following format.

```json
{
    "location": "https://s3.amazonaws.com/production.images.app.streetcred.co/857b58d0-c355-11e8-bb4d-55ec4a2369ed",
    "image_id": "857b58d0-c355-11e8-bb4d-55ec4a2369ed"
}
```

To access the thumbnail version of any image append `_thumb` to the end of the `location` property value.

#### `status` : `string`
Status represents whether or not the place is still pending consensus or has been
approved by at least two validators. Values are either `approved` or `pending`.

_Places that were rejected by the community are not included in this data-set._

#### `created_at` : `unix timestamp`
Timestamp representing time of record creation (original submission).

#### `updated_at` : `unix timestamp`
Timestamp representing the last time the record or any of its properties were updated.
Generally represents the time at which the record was approved by the community
and the status was changed from `pending` to `approved`.
If no changes have been made since creation, `updated_at` will equal `created_at`.

#### `attributes` : `object`
The attributes object stores all additional properties of a place, typically
dictated by the place category.

##### `wheelchairAccessible` : `boolean`

##### `phone` : `string`
Properly formated US 10-digit phone number.

_Not required_

_Only available for some categories_

##### `website` : `string`

_Not required_

_Only available for some categories_

##### `outdoorSeating` : `string`

_Required for some categories_

_Only available for some categories_

##### `hours` : `object`
Hours are in local time: EDT in the case of MapNYC data. Hours object may contain only one of the following options.

_Required for some categories_

_Only available for some categories_

`"twentyFourSeven": true`

OR

`"notApplicable": true`

OR

an object with a property for every day of the week, `sunday` - `saturday`

```JSON
{
    "sunday": {
      "open": true,
      "hours": [
        {
          "start": "1100",
          "end": "1900"
        }
      ]
    },
    "monday": {
      "open": true,
      "hours": [
        {
          "start": "1000",
          "end": "2000"
        }
      ]
    },
    ...
}
```