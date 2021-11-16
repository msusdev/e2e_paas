```bash
az group create --name '<resource-group>' --location '<location>'
```

```bicep
resource StorageAccount 'Microsoft.Storage/storageAccounts@2021-06-01' = {
  name: 'sidneyandrewsstorage'
  location: 'eastus2'
  kind: 'StorageV2'
  sku: {
    name: 'Premium_LRS'
  }
}
```

```bash
az deployment group create --resource-group '<resource-group>' --template-file '<bicep-file>'
```

```bicep
resource StorageAccount 'Microsoft.Storage/storageAccounts@2021-06-01' = {
  name: 'sidney${uniqueString(resourceGroup().id)}'
  location: resourceGroup().location
  kind: 'StorageV2'
  sku: {
    name: 'Standard_LRS'
  }
}
```

```bash
az deployment group create --resource-group '<resource-group>' --template-file '<bicep-file>' --name '<deployment-name>'
```

```bicep
var storageAccountName = 'sidney${uniqueString(resourceGroup().id)}'

resource StorageAccount 'Microsoft.Storage/storageAccounts@2021-06-01' = {
  name: storageAccountName
  location: resourceGroup().location
  kind: 'StorageV2'
  sku: {
    name: 'Standard_LRS'
  }
}
```

```bicep
param storageAccountPrefix string

var storageAccountName = '${storageAccountPrefix}${uniqueString(resourceGroup().id)}'

resource StorageAccount 'Microsoft.Storage/storageAccounts@2021-06-01' = {
  name: storageAccountName
  location: resourceGroup().location
  kind: 'StorageV2'
  sku: {
    name: 'Standard_LRS'
  }
}
```

```bash
az deployment group create --resource-group '<resource-group>' --template-file '<bicep-file>' --parameters name='<value>'
```

```bicep
param storageAccountPrefix string

var storageAccountName = '${storageAccountPrefix}${uniqueString(resourceGroup().id)}'

resource StorageAccount 'Microsoft.Storage/storageAccounts@2021-06-01' = {
  name: storageAccountName
  location: resourceGroup().location
  kind: 'StorageV2'
  sku: {
    name: 'Standard_LRS'
  }
}

resource StorageAccountBlobServices 'Microsoft.Storage/storageAccounts/blobServices@2021-06-01' = {
  name: 'default'
  parent: StorageAccount
}

resource StorageAccountContainer 'Microsoft.Storage/storageAccounts/blobServices/containers@2021-06-01' = {
  name: 'logs'
  parent: StorageAccountBlobServices
}
```


```bicep
param storageAccountPrefix string

var storageAccountName = '${storageAccountPrefix}${uniqueString(resourceGroup().id)}'

resource StorageAccount 'Microsoft.Storage/storageAccounts@2021-06-01' = {
  name: storageAccountName
  location: resourceGroup().location
  kind: 'StorageV2'
  sku: {
    name: 'Standard_LRS'
  }
  resource StorageAccountBlobServices 'blobServices@2021-06-01' = {
    name: 'default'
    resource StorageAccountContainer 'containers@2021-06-01' = {
      name: 'logs'
    }
  }
}
```


```bicep
param storageAccountPrefix string

var storageAccountName = '${storageAccountPrefix}${uniqueString(resourceGroup().id)}'

resource StorageAccount 'Microsoft.Storage/storageAccounts@2021-06-01' = {
  name: storageAccountName
  location: resourceGroup().location
  kind: 'StorageV2'
  sku: {
    name: 'Standard_LRS'
  }
  resource StorageAccountBlobServices 'blobServices@2021-06-01' = {
    name: 'default'
    resource StorageAccountContainer 'containers@2021-06-01' = {
      name: 'logs'
    }
  }
}

output storageEndpoint string = StorageAccount.properties.primaryEndpoints.blob
```

===

```bicep
 resource AppServicePlan 'Microsoft.Web/serverfarms@2020-06-01' = {
  name: 'webhostingplan'
  location: resourceGroup().location
  kind: 'linux'
  sku: {
    name: 'F1'
  }
  properties: {
    reserved: true
  }
}

resource WebApp 'Microsoft.Web/sites@2020-06-01' = {
  name: 'webapp${uniqueString(resourceGroup().id)}'
  location: resourceGroup().location
  properties: {
    serverFarmId: AppServicePlan.id
    siteConfig: {
      linuxFxVersion: 'node|14-lts'
    }
  }
}
```

```bicep
resource StorageAccount 'Microsoft.Storage/storageAccounts@2021-06-01' = {
  name: 'funcstor${uniqueString(resourceGroup().id)}'
  location: resourceGroup().location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
}

resource AppServicePlan 'Microsoft.Web/serverfarms@2020-06-01' = {
  name: 'funchostingplan'
  location: resourceGroup().location
  kind: 'linux'
  sku: {
    tier: 'Dynamic'
    name: 'Y1'
  }
  properties: {
    reserved: true
  }
}
```

```bicep
 resource StorageAccount 'Microsoft.Storage/storageAccounts@2021-06-01' = {
  name: 'flowstor${uniqueString(resourceGroup().id)}'
  location: resourceGroup().location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
}

resource AppServicePlan 'Microsoft.Web/serverfarms@2020-06-01' = {
  name: 'flowhostingplan'
  location: resourceGroup().location
  sku: {
    tier: 'WorkflowStandard'
    name: 'WS1'
  }
  properties: {
    reserved: true
  }
}
```

```bicep
resource CosmosAccount 'Microsoft.DocumentDB/databaseAccounts@2021-06-15' = {
  name: 'sidney${uniqueString(resourceGroup().id)}'
  location: 'eastus'
  properties: {
    databaseAccountOfferType: 'Standard'
    locations: [
      {
        locationName: 'eastus'
      }
    ]
  }
}

resource CosmosDatabase 'Microsoft.DocumentDB/databaseAccounts/sqlDatabases@2021-06-15' = {
  parent: CosmosAccount
  name: 'cosmicworks'
  properties: {
    resource: {
      id: 'cosmicworks'
    }
  }
}

resource CosmosContainer 'Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers@2021-06-15' = {
  parent: CosmosDatabase
  name: 'products'
  properties: {
    options: {
      throughput: 400
    }
    resource: {
      id: 'products'
      partitionKey: {
        paths: [
          '/categoryId'
        ]
      }      
    }
  }
}
```
