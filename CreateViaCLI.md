<h1>Please run this script the day before because it takes ~15 min to create the cluster and database!!!!</h1>

These steps are a modified version of what can be found at https://docs.microsoft.com/en-us/azure/data-explorer/create-cluster-database-cli.

1. [![Azure Cloud Shell](/images/hdi-launch-cloud-shell.png)](https://shell.azure.com)
1. Install extension to use the latest Kusto CLI version:

    ```azurecli-interactive
    az extension add -n kusto
    ```

1. Set the subscription where you want your cluster to be created. Replace `MyAzureSub` with the name of the Azure subscription that you want to use:

    ```azurecli-interactive
    az account set --subscription MyAzureSub
    ```
    ## Create the Azure Data Explorer cluster
1. Create the resource group for the testing:

    ```azurecli-interactive
    az group create --name 90PercentofKustoin75PercentofanHour --location eastus2
    ```

1. Create your cluster by using the following command:

    ```azurecli-interactive
    az kusto cluster create --cluster-name cxpdolkql<youralias> --sku name="Dev(No SLA)_Standard_E2a_v4" tier="Basic" --resource-group 90PercentofKustoin75PercentofanHour --location eastus2
    ```

   |**Setting** | **Suggested value** | **Field description**|
   |---|---|---|
   | name | *cxpdolkql<youralias>* | The desired name of your cluster.|
   | sku | *Dev(No SLA)_Standard_E2a_v4* | The SKU that will be used for your cluster. Parameters: *name* -  The SKU name. *tier* - The SKU tier. |
   | resource-group | *90PercentofKustoin75PercentofanHour* | The resource group name where the cluster will be created. |
   | location | *eastus2* | The location where the cluster will be created. |

    There are additional optional parameters that you can use, such as the capacity of the cluster.

1. Run the following command to check whether your cluster was successfully created:

    ```azurecli-interactive
    az kusto cluster show --cluster-name cxpdolkql<youralias> --resource-group 90PercentofKustoin75PercentofanHour
    ```

If the result contains `provisioningState` with the `Succeeded` value, then the cluster was successfully created.

## Create the database in the Azure Data Explorer cluster

1. Create your database by using the following command:

    ```azurecli-interactive
    az kusto database create --cluster-name cxpdolkql<youralias> --database-name cxpdolkql --resource-group 90PercentofKustoin75PercentofanHour --read-write-database soft-delete-period=P30D hot-cache-period=P10D location=eastus2
    ```

   |**Setting** | **Suggested value** | **Field description**|
   |---|---|---|
   | cluster-name | *cxpdolkql<youralias>* | The name of your cluster where the database will be created.|
   | database-name | *cxpdolkql* | The name of your database.|
   | resource-group | *90PercentofKustoin75PercentofanHour* | The resource group name where the cluster will be created. |
   | read-write-database | *P30D*, *P10D*, *eastus2* | The database type. Parameters: *soft-delete-period* - Signifies the amount of time the data will be kept available to query. See [retention policy](kusto/management/retentionpolicy.md) for more information. *hot-cache-period* - Signifies the amount of time the data will be kept in cache. See [cache policy](kusto/management/cachepolicy.md) for more information. *location* -The location where the database will be created. |

1. Run the following command to see the database that you created:

    ```azurecli-interactive
    az kusto database show --database-name cxpdolkql --resource-group 90PercentofKustoin75PercentofanHour --cluster-name cxpdolkql
    ```

You now have a cluster and a database.

## Clean up resources

* If you plan to follow our other articles, keep the resources you created.
* To clean up resources, delete the cluster. When you delete a cluster, it also deletes all the databases in it. Use the following command to delete your cluster:

    ```azurecli-interactive
    az kusto cluster delete --cluster-name cxpdolkql<youralias> --resource-group 90PercentofKustoin75PercentofanHour
    ```
