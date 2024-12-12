# Fauna Driver Performance Test Utilities
This repo contains common files that Fauna employs to run performance tests across our drivers so we can monitor for performance regressions.

The file structure is as follows:
- fauna/
  - main.fsl
  - seeddata.fql
- init.sh
- queries.json
- README.md (this file)
- teardown.sh

## init.sh and teardown.sh
These two shell scripts handle setup and teardown of the performance test database. Before running these files, the runtime environment must meet this prerequisites:
- Required environment variables:
  - `FAUNA_ENDPOINT` (the full URI to Fauna, e.g. https://db.fauna.com) 
  - `FAUNA_SECRET` (a database key with write privileges)
- Fauna CLI (v2.x) must be installed (see [documentation](https://docs.fauna.com/fauna/current/build/cli/))
  - `npm install -g fauna-shell@^2.0.0`

## fauna/main.fsl
This file contains Fauna Schema Language (FSL) that defines the two Collections used in the performance tests, `Product` and `Manufacturer`. This database schema is "pushed" to the test database using the Fauna CLI `fauna schema push` command by the `init.sh` script. For more information on managing schema with FSL, see our [documentation](https://docs.fauna.com/fauna/current/learn/schema/).

## fauna/seeddata.fql
This file defines a Fauna Query Language (FQL) query that is run using the Fauna CLI to prepopulate the test database's Collections with documents. This FQL file is called during setup in the `init.sh` script after the database schema is ready. For more information on querying Fauna with FQL, see our [documentation](https://docs.fauna.com/fauna/current/learn/query/).

## queries.json
This file contains a JSON object that defines the various performance test queries and metadata; this object gets consumed by the language-specific performance test cases and drives the inputs for the test scenarios. The top-level field, `queries`, is an array of objects with the following fields:
- `name` - the name of the test (e.g. `basic_add`)
- `parts` - an array of strings that compose the query
- `response.typed` - whether the query result is of the `Product` collection type (mainly interesting in typed languages)
- `response.paged` - whether the query result will be a Page of results
