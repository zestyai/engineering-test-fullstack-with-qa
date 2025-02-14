# Zesty.ai Full-Stack Engineering Test

- [Background](#background)
- [Assignment](#assignment)
- [Feature List](#feature-list)
- [Setup](#setup)
- [API Specification](#api-specification)
- [Submission Instructions](#submission-instructions)

## Background

Full-stack engineers at Zesty.ai develop our web applications end-to-end, working with modern front-end frameworks, APIs (ours and third parties'), and many kinds of data and imagery.

This test is an opportunity for you to demonstrate your comfort with developing UI and API services, similar to a day-to-day project you might encounter working on our team.


## Assignment

Your goal is to create a full-stack web application that allows users to search for and retrieve information about real estate properties (see [Feature List](#feature-list)). Using your language(s) and framework(s) of choice, you will need to create a front-end and back-end (see [API Specification](#api-specification)) for your application and connect to the provided PostgreSQL database (see [Setup](#setup)). Your UI and API should both be packaged as containerized services (Docker images).

Note that some features are more difficult than others, and you will be evaluated on more than just the number of
features completed. Quality is preferred over quantity. Design, organize, and comment your code as you would a typical 
production project. Be prepared to discuss decisions you made.

## Feature List

* **List all properties:** Display, in a tabular format, all properties and their geographic location (longitude and 
  latitude).

* **Map view:** Implement an interactive map view using Mapbox, Leaflet, or similar package, initially showing all the properties as pins on the map. This map should be tied to the search functionality (see next feature), and filter the pins based on the search results. Selecting a pin on the map should display the property detail view for that location.
  
* **Property detail view:** Show detailed information about a given property, including its image, geographic location, 
  and statistics (if applicable). This can be either a dedicated page or a popup/info modal in the map view.

* **Search by coordinates:** Prompt the user for a longitude, latitude, and search radius (default 10000 meters) and 
  display, in a tabular format, the results of the search, including the properties' geographic location (longitude and 
  latitude). Pan the interactive map to the entered location and filter the pins to the returned search results. Also visually indicate the search radius on the map.

* **Image overlays:** Add polygonal overlays to property images (in the property detail page) to represent either the parcel, building, or both 
  (`parcel_geo` and `buildings_geo` fields in the database). Add a mechanism that allows for parcel and building overlays to be toggled on/off independently on the image view.

## Non-Functional Requirements

* **Containerization:** Include Docker image(s) of your application when submitting your final code.

* **End-to-end Testing:** Implement automated E2E tests using Playwright (or similar tool) to cover critical user flows: property browsing, search functionality, and detail page navigation.

## Setup

### Development environment requirements
You will need to install [Docker](https://www.docker.com/products/docker-desktop) and 
[`docker-compose`](https://docs.docker.com/compose/install/) to run the example database.

### Database startup
From the repo root folder, run `docker-compose up -d` to start the PostgreSQL database needed for this example. The 
database server will be exposed on port **5555**. If this port is not available on your computer, feel free to change 
the port in the `docker-compose.yml` file.

In the test database, there is a single table called `properties` (with 5 sample rows), where each row represents a 
property or address. There are three geography<sup>*</sup> fields and one field with an image URL pointing to an image on [Google Cloud Storage](https://cloud.google.com/storage/).

<sup>*</sup> *If you are not familiar with [PostgreSQL](https://www.postgresql.org/) or [PostGIS](https://postgis.net/), you may need to read up beforehand.*

## API Specification
The API you will be implementing for this project must adhere to the following API specification:

### GET /display/:id

*Fetches and displays property tile by ID.*

`example: GET localhost:1235/display/f853874999424ad2a5b6f37af6b56610`

##### Request Parameters
- "id" | description: Property ID | type: string | required: true | validation: length greater than 0

##### Response
JPEG image

***

### GET /properties
*Lists all properties.*

`example: GET localhost:1235/properties`

##### Response
JSON array of property objects


***
### POST /find
*Finds properties within X meters away from provided geojson point.*

`example: POST localhost:1235/find`

##### Request Body
- geojson object with x-distance property

```
example:

{
  "type": "Feature",
  "geometry": {
  "type": "Point",
  "coordinates": [-80.0782213, 26.8849731]
  },
  "x-distance": 1755000
}
```

##### Response
JSON array of property IDs

## Submission instructions

Send us your completed application's code by email, or create and give us access to a new private GitHub repository.

Include instructions on how to run your app, and a list of what features you implemented. Add any comments or things you 
want the reviewer to consider when looking at your submission. You don't need to be too detailed, as there will likely 
be a review done with you where you can explain what you've done.
