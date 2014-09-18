# RedTask

This repository contains a command line tool which can interact with redmine issues.

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
