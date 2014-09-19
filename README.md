# RedTask

This repository contains a bash command line tool which can interact with the redmine API,
in order to create and modify issues.

### Dependencies

In order to run this tool you must install [jq](http://stedolan.github.io/jq/).

## Usage

    redtask <options> <ids or collections> <filters> <command> <reports>

### options

    -p <project id>
        project id is the numeric id of a certain project

### ids or collections

    100,101,102
        ids can be provided as comma separated list of ids

    ids:<custom command>
        ids can be created by custom commands which perform a custom query on the
        redmine in order to receive some ids

    json:<custom command>
        json content can be provided by an external command which will perform a 
        query on the redmine api

### filters

    assign:<user id>
        filter for issues which are assigned to the specified user

### commands

    done
        set status of issue to done

### reports

    lines
        print a line for each issue


### hints

#### recover lost data from journals
      redtask json:backlog feature load_details pretty >all_features.json
      cat all_features.json | jq -r -c '.[] | "{\"id\":\(.id),\"payload\":\([.journals[] | .details[0].new_value | select(contains("deliverable")) ] | .[-1:]|.[])},"' > lost_values.json
      cat lost_values.json | jq 'map( del(.payload.tags))' > cleand_lost.json
      cat cleand_lost.json  | jq 'map(.deliverable=.payload.deliverable |.requirements=.payload.requirements) | map(del(.payload))' > lost3.json
      cat lost3.json | jq -r '.[]|"redtask \(.id) mo deliver:\"\(.deliverable)\""' >commands
      sh commands
      cat lost3.json | jq -r '.[]|"redtask \(.id) mo require:\"\(.requirements)\""' > commands
      sh commands
