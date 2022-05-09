## Now, we are going to ingest some data:

1. Open the URL for the web browswer UI for Data Explorer: 

https://dataexplorer.azure.com/clusters/cxpdolkql\<youralias\>.\<yourregion\> 
(be sure to use the cluster name and region you used when creating your instance)

1. Expand the tree in the left-hand pane so you can see your database:

![Expanded tree view of database browser](/images/TreeView.jpg "TreeView")

1. Make sure you have highlighted your database, then paste in the following command, and select **Run** to create a StormEvents table.

    ```Kusto
    .create table StormEvents (StartTime: datetime, EndTime: datetime, EpisodeId: int, EventId: int, State: string, EventType: string, InjuriesDirect: int, InjuriesIndirect: int, DeathsDirect: int, DeathsIndirect: int, DamageProperty: int, DamageCrops: int, Source: string, BeginLocation: string, EndLocation: string, BeginLat: real, BeginLon: real, EndLat: real, EndLon: real, EpisodeNarrative: string, EventNarrative: string, StormSummary: dynamic)
    ```

1. Paste in the following command, and select **Run** to ingest data into StormEvents table.

    ```Kusto
    .ingest into table StormEvents 'https://kustosamples.blob.core.windows.net/samplefiles/StormEvents.csv' with (ignoreFirstRecord=true)
    ```

1. After ingestion completes, paste in the following query and select **Run**.

    ```Kusto
    StormEvents
    | sort by StartTime desc
    | take 10
    ```

    The query returns the following results from the ingested sample data.

    ![snippet of results](https://docs.microsoft.com/en-us/azure/data-explorer/media/ingest-sample-data/query-results.png "Snippet of top 10 results")

---