# Asynchronous Request-Reply pattern

Decouple backend processing from a frontend host, where backend processing needs to be asynchronous, but the frontend still needs a clear response. 

For more information about this pattern, see [Asynchronous Request-Reply pattern](https://docs.microsoft.com/azure/architecture/patterns/async-request-reply) on the Azure Architecture Center.

![Data flow of the async request-reply pattern](https://docs.microsoft.com/en-us/azure/architecture/patterns/_images/async-request-fn.png)

## Deploying the sample

### Prerequisites

- [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- [.NET Core SDK version 2.1](https://microsoft.com/net)
- [Azure Functions Core Tools](https://docs.microsoft.com/azure/azure-functions/functions-run-local#v2)

### Deploy the Azure resources

1. Clone or download this repo.

2. Navigate to the `async-request-reply` folder.

    ```bash
    cd async-request-reply/async-request-reply
    ```

3. Create a resource group.

    ```bash
    export RESOURCE_GROUP=<resource-group-name>
    export APP_NAME=<app-name> # 2-6 characters
    export LOCATION=<azure-region>

    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```

4. Deploy the template.

    ```bash
    az group deployment create \
    --resource-group $RESOURCE_GROUP \
    --template-file deploy.json \
    --parameters appName=$APP_NAME
    ```

5. Wait for the deployment to complete.

### Deploy the Functions app

1. Navigate to the `async-request-reply/src` folder.

    ```bash
    cd src
    ```

2. Deploy the app.

    ```bash
    func azure functionapp publish $(APP_NAME)-fn
    ```

### Send a request

```bash
curl -X POST "https://$(APP_NAME)-fn.azurewebsites.net/api/asyncprocessingworkacceptor" --header 'Content-Type: application/json' --header 'Accept: application/json' -k -i -d '{
   "id": "1234",
   "customername": "Contoso"
 }'
 ```

 > Note. The app uses the WEBSITE_HOSTNAME environment variable. This environment variable is set automatically by the Azure App Service runtime environment. For more information, see [Azure runtime environment](https.://github.com/projectkudu/kudu/wiki/Azure-runtime-environment)