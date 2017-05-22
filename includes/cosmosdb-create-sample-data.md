U kunt nu gegevens aan uw nieuwe verzameling toevoegen met behulp van Data Explorer.

1. De nieuwe database wordt in Data Explorer weergegeven in het deelvenster Verzamelingen. Vouw de database **Items** uit, vouw de verzameling **ToDoList** uit, klik op **Documenten** en klik vervolgens op **Nieuwe documenten**. 

   ![Nieuwe documenten maken in Data Explorer in Azure Portal](./media/cosmosdb-create-sample-data/azure-cosmosdb-data-explorer-emulator-new-document.png)
  
2. Voeg nu een aantal nieuwe documenten aan de verzameling toe met de volgende structuur, waarbij u in elk document een unieke waarde invult voor de id en de andere eigenschappen naar eigen inzicht wijzigt. De nieuwe documenten kunnen elke gewenste structuur hebben, omdat in Azure Cosmos DB uw gegevens geen schema krijgen opgelegd.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries."
     }
     ```

     U kunt nu query's in Data Explorer gebruiken om uw gegevens te halen. Data Explorer maakt standaard gebruik van SELECT * FROM c om alle documenten in de verzameling op te halen, maar u kunt dit wijzigen in SELECT * FROM c ORDER BY c.name ASC om alle documenten terug te zetten in alfabetische volgorde op basis van de naameigenschap. 
 
     U kunt Data Explorer ook gebruiken voor het maken van opgeslagen procedures, UDF's en triggers om bedrijfslogica aan de serverzijde uit te voeren. In Data Explorer wordt alle ingebouwde programmatische gegevenstoegang zichtbaar die beschikbaar is in de API's, maar biedt eenvoudige toegang tot uw gegevens in Azure Portal.