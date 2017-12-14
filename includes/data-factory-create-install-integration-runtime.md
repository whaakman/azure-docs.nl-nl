## <a name="create-a-self-hosted-ir"></a>Een zelf-hostende IR maken

In deze sectie kunt u een zelf-hostende Integration Runtime maken en deze koppelen aan een on-premises computer met de SQL Server database. De zelf-hostende Integration Runtime is het onderdeel waarmee gegevens worden gekopieerd van SQL Server op uw computer naar Azure Blob Storage. 

1. Maak een variabele voor de naam van een Integration Runtime. Gebruik een unieke naam en noteer deze. U gaat deze verderop in de zelfstudie gebruiken. 

    ```powershell
   $integrationRuntimeName = "ADFTutorialIR"
    ```
1. Een zelf-hostende Integration Runtime maken. 

   ```powershell
   Set-AzureRmDataFactoryV2IntegrationRuntime -Name $integrationRuntimeName -Type SelfHosted -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName
   ```

   Hier volgt een voorbeeld van uitvoer:

   ```json
    Id                : /subscriptions/<subscription ID>/resourceGroups/ADFTutorialResourceGroup/providers/Microsoft.DataFactory/factories/onpremdf0914/integrationruntimes/myonpremirsp0914
    Type              : SelfHosted
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Name              : myonpremirsp0914
    Description       :
    ```
 

2. Voer de volgende opdracht uit om de status van de gemaakte zelf-hostende Integration Runtime op te halen. Controleer of de waarde van de **status** eigenschap is ingesteld op **NeedRegistration**. 

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntime -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Status
   ```

   Hier volgt een voorbeeld van uitvoer:

   ```json
   Nodes                     : {}
   CreateTime                : 9/14/2017 10:01:21 AM
   InternalChannelEncryption :
   Version                   :
   Capabilities              : {}
   ScheduledUpdateDate       :
   UpdateDelayOffset         :
   LocalTimeZoneOffset       :
   AutoUpdate                :
   ServiceUrls               : {eu.frontend.clouddatahub.net, *.servicebus.windows.net}
   ResourceGroupName         : <ResourceGroup name>
   DataFactoryName           : <DataFactory name>
   Name                      : <Integration Runtime name>
   State                     : NeedRegistration
   ```

3. Voer de volgende opdracht uit om **verificatiesleutels** op te halen voor het registreren van de zelf-hostende Integration Runtime met Data Factory-service in de cloud. 

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntimeKey -Name $integrationRuntimeName -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName | ConvertTo-Json
   ```

   Hier volgt een voorbeeld van uitvoer:

   ```json
   {
       "AuthKey1":  "IR@0000000000-0000-0000-0000-000000000000@xy0@xy@xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx=",
       "AuthKey2":  "IR@0000000000-0000-0000-0000-000000000000@xy0@xy@yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy="
   }
   ```    
4. Kopieer een van de sleutels (zonder de dubbele aanhalingstekens) om de zelf-hostende Integration Runtime te registeren die u tijdens de volgende stap op uw computer gaat installeren.  

## <a name="install-integration-runtime"></a>Integration Runtime installeren
1. Als u al de **Microsoft Integration Runtime** op uw computer heeft, verwijder deze dan met behulp van **Programma's toevoegen of verwijderen**. 
2. [Download](https://www.microsoft.com/download/details.aspx?id=39717) de zelf-hostende Integration Runtime op een lokale Windows-computer en voer de installatie uit. 
3. Klik bij **Welkom bij de installatiewizard van Microsoft Integration Runtime** op **Volgende**.  
4. Ga op de pagina **Gebruiksrechtovereenkomst** akkoord met de voorwaarden en de gebruiksrechtovereenkomst en klik op **Volgende**. 
5. Klik op de pagina **Doelmap** op **Volgende**. 
6. Klik bij **Gereed om Microsoft Integration Runtime te installeren** op **Installeren**. 
7. Als er een waarschuwingsbericht wordt weergegeven met de melding of de computer moet worden geconfigureerd om in de slaapstand of sluimerstand over te gaan als deze niet in gebruik is, klikt u op **OK**. 
8. Als het venster **Energiebeheer** wordt weergegeven, sluit u dit en schakelt u over naar het instellingenvenster. 
9. Klik op de pagina **De installatiewizard voor Microsoft Integration Runtime is voltooid**  op **Voltooien**.
10. Plak de sleutel die u in de vorige sectie hebt opgeslagen op de pagina **Integration Runtime (zelf-hostend) registreren** en klik op **Registreren**. 

   ![Integratieruntime registreren](media/data-factory-create-install-integration-runtime/register-integration-runtime.png)
2. U ziet het volgende bericht wanneer de zelf-hostende Integration Runtime is geregistreerd:

   ![Registratie is voltooid](media/data-factory-create-install-integration-runtime/registered-successfully.png)

3. Klik op de pagina **Nieuw knooppunt voor Integration Runtime (zelf-hostend)** op **Volgende**. 

    ![Pagina Nieuw knooppunt voor Integration Runtime](media/data-factory-create-install-integration-runtime/new-integration-runtime-node-page.png)
4. Bij **Intranetcommunicatiekanaal** klikt u op **Overslaan**. U kunt een TLS/SSL-certificaat selecteren voor het beveiligen van de communicatie tussen knooppunten in een Integration Runtime-omgeving met meerdere knooppunten. 

    ![Pagina Intranetcommunicatiekanaal](media/data-factory-create-install-integration-runtime/intranet-communication-channel-page.png)
5. Op de pagina **Integration Runtime (zelf-hostend) registeren** klikt u op **Configuration Manager starten**. 
6. Wanneer het knooppunt is verbonden met de cloudservice, ziet u de volgende pagina:

   ![Knooppunt is verbonden](media/data-factory-create-install-integration-runtime/node-is-connected.png)
7. Test nu de verbinding met uw SQL Server-database.

    ![Tabblad Diagnostische gegevens](media/data-factory-create-install-integration-runtime/config-manager-diagnostics-tab.png)   

    - Schakel in het venster **Configuratiebeheer** over naar het tabblad **Diagnostische gegevens**.
    - Selecteer **SqlServer** als het **type gegevensbron**.
    - Voer de naam van de **server** in.
    - Voer de naam van de **database** in. 
    - Selecteer de **verificatiemethode**. 
    - Voer de **gebruikersnaam** in. 
    - Voer het **wachtwoord** voor de gebruikersnaam in.
    - Klik op **Test** om te controleren of Integration Runtime verbinding kan maken met de SQL Server. U ziet een groen vinkje als het gelukt is om verbinding te maken. Anders wordt er een foutbericht weergegeven. Los eventuele problemen op en zorg ervoor dat de Integration Runtime verbinding met uw SQL Server kan maken.    

    > [!NOTE]
    > Noteer deze waarden (verificatietype, server, database, gebruiker, wachtwoord). U hebt deze waarden verderop in deze zelfstudie nodig. 
    
