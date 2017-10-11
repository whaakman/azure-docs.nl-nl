## <a name="configure-your-application-to-access-azure-storage"></a>Uw toepassing configureren voor toegang tot Azure Storage
Er zijn twee manieren om te verifiëren van uw toepassing voor toegang tot opslagservices:

* Gedeelde sleutel: Gedeelde sleutel gebruiken alleen voor testdoeleinden
* Shared Access Signature (SAS): Gebruik SAS voor productietoepassingen

### <a name="shared-key"></a>Gedeelde sleutel
Verificatie met een gedeelde sleutel houdt in dat uw toepassing uw accountnaam en accountsleutel wordt gebruikt voor toegang tot opslagservices. Voor de doeleinden van snel waarin wordt getoond hoe deze bibliotheek gebruiken zullen we gebruikmaken van verificatie met gedeelde sleutel in deze aan de slag.

> [!WARNING] 
> **Alleen voor testdoeleinden verificatie met gedeelde sleutel gebruikt.** Uw accountnaam en accountsleutel, die volledige lees-/ schrijftoegang tot de gekoppelde Storage-account geven, worden gedistribueerd naar elke persoon die uw app kan worden gedownload. Dit is **niet** een goede oefenen als u het risico dat uw sleutel in gevaar komt door niet-vertrouwde clients.
> 
> 

Als u verificatie met gedeelde sleutel gebruikt, maakt u een [verbindingsreeks](../articles/storage/common/storage-configure-connection-string.md). De verbindingsreeks bestaat uit:  

* De **DefaultEndpointsProtocol** -kunt u HTTP of HTTPS. Gebruik van HTTPS wordt echter ten zeerste aanbevolen.
* De **accountnaam** -de naam van uw storage-account
* De **Accountsleutel** : op de [Azure Portal](https://portal.azure.com), navigeer naar uw storage-account en klik op de **sleutels** pictogram om deze informatie te vinden.
* (Optioneel) **EndpointSuffix** -dit wordt gebruikt voor storage-services in regio's met een ander eindpunt achtervoegsels, zoals Azure China of Azure Governance.

Hier volgt een voorbeeld van een verbindingsreeks met verificatie met gedeelde sleutel:

`"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here"`

### <a name="shared-access-signatures-sas"></a>Shared Access Signatures (SAS)
Voor een mobiele toepassing is de aanbevolen methode voor het verifiëren van een aanvraag door een client op de Azure Storage-service met behulp van een Shared Access Signature (SAS). SAS kunt u een clienttoegang verlenen tot een bron voor een opgegeven periode, met een opgegeven set machtigingen.
Als de eigenaar van het opslagaccount moet u een SAS genereren voor uw mobiele clients kunnen gebruiken. Als de SA's worden gegenereerd, wilt u waarschijnlijk een afzonderlijke service die de SAS naar uw clients worden gedistribueerd genereert schrijven. Voor testdoeleinden kunt u de [Microsoft Azure Storage Explorer](http://storageexplorer.com) of de [Azure Portal](https://portal.azure.com) om een SAS te genereren. Wanneer u de SAS maakt, kunt u de tijdsinterval waarover de SAS geldig is en de machtigingen die de SAS verleent aan de client.

Het volgende voorbeeld laat zien hoe de Microsoft Azure Storage Explorer gebruiken om een SAS te genereren.

1. Als u dat nog niet gedaan hebt, [Microsoft Azure Storage Explorer installeren](http://storageexplorer.com)
2. Maak verbinding met uw abonnement.
3. Klik op uw Storage-account en klik op het tabblad 'Acties' linksonder. Klik op 'Shared Access Signature ophalen' voor het genereren van een verbinding-tekenreeks voor uw SAS.
4. Hier volgt een voorbeeld van een SAS-verbindingsreeks dat verleent machtigingen lezen en op de service, de container en het objectniveau van het voor de blob-service van het opslagaccount schrijven.
   
   `"SharedAccessSignature=sv=2015-04-05&ss=b&srt=sco&sp=rw&se=2016-07-21T18%3A00%3A00Z&sig=3ABdLOJZosCp0o491T%2BqZGKIhafF1nlM3MzESDDD3Gg%3D;BlobEndpoint=https://youraccount.blob.core.windows.net"`

Zoals u ziet, wanneer u een SAS, bent u niet de sleutel van uw weergeeft in uw toepassing. U kunt meer informatie over SAS en aanbevolen procedures voor het gebruik van SAS door het uitchecken van [Shared Access Signatures: inzicht in het SAS-model](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md).

