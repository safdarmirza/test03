# Azure Custom Role

Author: Safdar Mirza

Version: 0.0.01

Date: 2019-09-24


## Creating custom roles by using Azure CLI 

** Requirement: **
To create a custom role, you must have the Microsoft.Authorization/roleDefinitions/write permission on all AssignableScopes, such as Owner or User Access Administrator

### Step One: List available subscriptions 

```
az account list --output table
```

### Step Two: Create a JSON file with following format: 

When you create a custom role, you need to know the resource provider operations that are available to define your permissions. To view the list of operations, see the [Azure Resource Manager resource provider operations](https://docs.microsoft.com/en-us/azure/role-based-access-control/resource-provider-operations). You will add the operations to the *Actions* or *NotActions* properties of the [role definition](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-definitions). If you have data operations, you will add those to the *DataActions* or *NotDataActions* properties.


** Skeleton  JSON File. **

```
{
  "Name": "UNIQUE NAME FOR YOUR CUSTOM ROLE",
  "IsCustom": true,
  "Description": "Description about your custom role …",
  "Actions": [],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": []
}
```



Sample JSON File: 

```
{
  "Name": "Sample Role",
  "IsCustom": true,
  "Description": "Safdar Test - Can Read Subscription.",
  "Actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.EventGrid/eventSubscriptions/read",
        "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
        "Microsoft.EventGrid/locations/eventSubscriptions/read",
        "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read"
  ],
  "NotActions": [
        "Microsoft.Authorization/*/Delete",
        "Microsoft.Authorization/*/Write",
        "Microsoft.Authorization/elevateAccess/Action",
        "Microsoft.Blueprint/blueprintAssignments/write",
        "Microsoft.Blueprint/blueprintAssignments/delete"
],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000",
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}

```

In above example, replace subscriptions with output result found in ** step 1. **


** Step Three: Create a custom role:  **

Save above listed file with some name such as myfile.json and execute following command. 

If JSON file is under same working directory: 
```
az role definition create --role-definition @myfile.json 
```

if JSON file is under a different path: Json file with alternate file path. 
```
az role definition create --role-definition /path/to/file/myfile.json
```


References: 

[Custom Roles](https://docs.microsoft.com/en-us/azure/role-based-access-control/custom-roles)

[Role Assignments Template](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-template)

[Tutorial Custom Role CLI](https://docs.microsoft.com/en-us/azure/role-based-access-control/tutorial-custom-role-cli)

[RBAC Resource Provider Operations](https://docs.microsoft.com/en-us/azure/role-based-access-control/resource-provider-operations)
