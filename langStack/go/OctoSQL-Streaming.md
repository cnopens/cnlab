#OctoSQL-Streaming.md

-Tools - SQL
 OctoSQL is a query tool that allows you to join, analyse and transform data from multiple databases and file formats using SQL.
Topics


Why the name?

OctoSQL stems from Octopus SQL.

Octopus, because octopi have many arms, so they can grasp and manipulate multiple objects, like OctoSQL is able to handle multiple datasources simultaneously.
Installation

Either download the binary for your operating system (Linux, OS X and Windows are supported) from the Releases page, or install using the go command line tool:

GO111MODULE=on go get -u github.com/cube2222/octosql/cmd/octosql

Quickstart

Let's say we have a csv file with cats, and a redis database with people (potential cat owners). Now we want to get a list of cities with the number of distinct cat names in them and the cumulative number of cat lives (as each cat has up to 9 lives left).

First, create a configuration file (Configuration Syntax) For example:

dataSources:
  - name: cats
    type: csv
    config:
      path: "~/Documents/cats.csv"
  - name: people
    type: redis
    config:
      address: "localhost:6379"
      password: ""
      databaseIndex: 0
      databaseKeyName: "id"

Then, set the OCTOSQL_CONFIG environment variable to point to the configuration file.

export OCTOSQL_CONFIG=~/octosql.yaml

You can also use the --config command line argument.

Finally, query to your hearts desire:

octosql "SELECT p.city, FIRST(c.name), COUNT(DISTINCT c.name) cats, SUM(c.livesleft) catlives
FROM cats c JOIN people p ON c.ownerid = p.id
GROUP BY p.city
ORDER BY catlives DESC
LIMIT 9"