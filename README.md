# Data Engineer Test

## Instructions

- Build an ETL process that would transform the data from `data/location.json` and `https://53c1a51d-616b-4aea-9e2a-89b2c8024b72-bluemix.cloudant.com/events` to the desired schemas and store them in a SQL database.

- Desired schema 1:

  | Column     | Description                                               |
  | ---------- | --------------------------------------------------------- |
  | id         | Primary key                                               |
  | user_id    | User id                                                   |
  | event_type | Event type                                                |
  | location   | Location i.e. where the user was when the event triggered |
  | timestamp  | Date time                                                 |

- Desired schema 2:

  | Column      | Description                                           |
  | ----------- | ----------------------------------------------------- |
  | date        | Date                                                  |
  | user_id     | User id                                               |
  | location    | Location e.g. `zone-1`                                |
  | time_spent  | Total time spent at location in minutes               |
  | visit_count | Count of how many times the user was at that location |

- You can use a SQLite database if you do not want to set up a proper SQL database for this test.

- Please write this code as though it will be deployed to a production environment.

## Submitting your solution

Please archive the entire test directory and email your solution back to us. Please do not fork the project on git or submit a pull request with your solution as we would prefer it to remain private.

```shell
$ git archive -o <candidate-name>-submission.zip HEAD
```

We would like it if you would submit your solution as multiple git commits with descriptive commit messages.

## Data source

### JSON file

Location: `data/location.json`

| Key       | Description                |
| --------- | -------------------------- |
| user_id   | User id                    |
| location  | User's location            |
| timestamp | Unix epoch in milliseconds |

- Each record indicates a change in the user location. E.g.

  ```json
  // User "01G29P9SP04M59QJ8YNKAT1Y0Z" was detected at location "zone-2" at
  // "1641024008859" i.e. 2022-01-01T08:00:08.859Z
  {
    "user_id": "01G29P9SP04M59QJ8YNKAT1Y0Z",
    "location": "zone-2",
    "timestamp": 1641024008859
  }

  // At "1641024029364" i.e. 2022-01-01T08:00:29.364Z, the same user has
  // moved to "zone-3"
  {
    "user_id": "01G29P9SP04M59QJ8YNKAT1Y0Z",
    "location": "zone-3",
    "timestamp": 1641024029364
  }
  ```

### CouchDB

Location: `https://53c1a51d-616b-4aea-9e2a-89b2c8024b72-bluemix.cloudant.com/events`

| Key        | Description                                    |
| ---------- | ---------------------------------------------- |
| \_id       | Document id                                    |
| \_rev      | Document revision                              |
| event_type | Event type e.g. ENTER_GEOFENCE, HIGH_HEAT      |
| user_id    | User id                                        |
| timestamp  | ISO 8601 date time e.g. `2022-05-05T08:44:28Z` |

- Document id and revision are CouchDB implementation details and are not pertinent for this test.

- Each record indicates a triggered event

  ```json
  // User "01G29PB08CC2G72K9JF7FJ6F2N" entered a geofenced zone at
  // 2022-01-01T08:02:10.263Z
  {
    "_id": "4c026ba25bd3c7ed5a4290271c02862d",
    "_rev": "1-1f9339a905e0d68f6040c22019fad7cc",
    "event_type": "ENTER_GEOFENCE",
    "user_id": "01G29PB08CC2G72K9JF7FJ6F2N",
    "timestamp": "2022-01-01T08:02:10.263Z"
  }
  ```

- To fetch all the records in the `events` database

  ```shell
  $ curl "https://53c1a51d-616b-4aea-9e2a-89b2c8024b72-bluemix.cloudant.com/events/_all_docs?include_docs=true"
  ```
