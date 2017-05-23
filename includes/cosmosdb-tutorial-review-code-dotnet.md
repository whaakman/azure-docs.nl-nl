Laten we eens kijken wat er precies gebeurt in de app. Open het bestand DocumentDBRepository.cs en u zult zien dat deze regels code de DocumentDB-resources maken. 

* De DocumentClient wordt geïnitialiseerd.

    ```csharp
    client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);`
    ```

* Er wordt een nieuwe database gemaakt.

    ```csharp
    await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
    ```

* Er wordt een nieuwe verzameling gemaakt.

    ```csharp
    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(DatabaseId),
        new DocumentCollection { Id = CollectionId },
        new RequestOptions { OfferThroughput = 1000 });
    ```
