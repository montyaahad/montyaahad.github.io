---
layout: post
title: Azure Deployment Slots with Backward Compatible DB Migration!
---

When I was using Azure deployment slot, it was cool until I faced issue with my database migration while I tried to swap back to my previous code version. This article will explain how to tackle issues related to database compatibility when we are using deployment slot. 

## Deployment Slot

1. Deploy app to staging slot
2. Test the app on staging slot
3. You are happy with the changes, Swap the slots
4. Messed up in Production! Roll back your deployment. Simply Swap back to reverse the effect.

![_config.yml]({{ site.baseurl }}/images/deployment-slot/azure-slots-diagram.png)

### Pros
- Zero-downtime deployments
- Verify new version of an app before it goes live
- Free!

### Facts
- Full-fledged App Service instance
- Run within the same App Service and its App Service Plan
- Have a different URL. For instance, “staging” can become http://website-staging.azurewebsites.net.

## How to handle DB Migration?

### Backward Compatible database migration
- New table columns should be nullable or have default values
- Renaming column is not permitted
- Dropping column is not permitted
### Plan for destructive database changes
- The new version of app should be released, which removes the dependency on renamed/dropped columns
- And additional release is needed to perform the destructive database changes

## Backward Compatible DB migration
For understanding purpose consider the below example -

```SQL
CREATE TABLE PERSON (
    id BIGINT GENERATED BY DEFAULT AS IDENTITY,
    first_name VARCHAR(255) NOT NULL,
    last_name VARCHAR(255) NOT NULL
);
 
INSERT INTO PERSON (first_name, last_name) VALUES ('Dave', 'Syer');
```

- The app stores the Person data into a column called last_name
- We need to change the field name from last_Name to surname


## Deploy version 1.0.0 of the application
- with v1 of db schema (column name = last_name)

## Deploy version 2.0.0 of the application
- that saves data to last_name and surname columns.
- The app reads from surname column. If null read from lastname
- Db is in version v2 containing both last_name and surname columns.
- The surname column is a copy of the last_namecolumn. (NOTE: this column must not have the not null constraint)

## Deploy version 3.0.0 of the application
- that saves data only to surname and reads from surname.
- As for the db the final migration of last_name to surname takes place.
- Also the NOT NULL constraint is dropped from last_name. Db is now in version v3

## Deploy version 4.0.0 of the application
- there are no changes in the code.
- Deploy db in v4 that first performs a final migration of last_name to surname and removes the last_name column.
- Here you can add any missing constraints

#### References
- https://stackify.com/azure-deployment-slots/
- https://spring.io/blog/2016/05/31/zero-downtime-deployment-with-a-database
- https://stackoverflow.com/questions/44809152/azure-deployment-slots-and-database-migrations
