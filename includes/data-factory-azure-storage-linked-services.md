### <a name="azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service
De **gekoppelde Azure Storage-service** kunt u een Azure storage-account koppelen aan een Azure data factory met behulp van de **accountsleutel**, waarmee u de data factory met wereldwijde toegang tot de Azure Storage. De volgende tabel bevat een beschrijving op voor JSON-elementen die specifiek zijn voor de gekoppelde Azure Storage-service.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type |De eigenschap type moet worden ingesteld op: **AzureStorage** |Ja |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met Azure storage voor de connectionString-eigenschap. |Ja |

Zie de volgende sectie voor stappen voor het weergeven/kopiëren de accountsleutel voor een Azure Storage: [toegangssleutels](../articles/storage/common/storage-account-manage.md#access-keys).

**Voorbeeld:**  

```json
{  
    "name": "StorageLinkedService",  
    "properties": {  
        "type": "AzureStorage",  
        "typeProperties": {  
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
        }  
    }  
}  
```

### <a name="azure-storage-sas-linked-service"></a>Sas van Azure Storage gekoppelde Service
Een shared access signature (SAS) biedt gedelegeerde toegang tot resources in uw opslagaccount. Hiermee kunt u een client beperkte machtigingen voor objecten in uw storage-account voor een bepaalde periode en met een opgegeven set machtigingen, zonder dat u hoeft voor het delen van de toegangssleutels van uw account verlenen. De SAS is een URI die in de queryparameters alle informatie die nodig zijn voor geverifieerde toegang tot een opslagresource omvat. Voor toegang tot de storage-resources met de SAS moet moet de client alleen worden doorgegeven in de SAS tot de juiste constructor of methode. Zie voor gedetailleerde informatie over SAS [Shared Access Signatures: inzicht in het SAS-Model](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)

> [!IMPORTANT]
> Azure Data Factory nu alleen ondersteunt **Service-SAS** , maar geen Account-SAS. Zie [typen van handtekeningen voor gedeelde toegang](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) voor meer informatie over deze twee typen en hoe u een van. Houd er rekening mee de SAS-URL generable vanuit Azure portal of Storage Explorer is een Account-SAS, wat niet wordt ondersteund.

> [!TIP]
> U kunt de onderstaande PowerShell-opdrachten voor het genereren van een SAS-Service voor uw opslagaccount (Vervang de plaatsaanduidingen en de benodigde machtiging verlenen) uitvoeren: `$context = New-AzureStorageContext -StorageAccountName <accountName> -StorageAccountKey <accountKey>`
> `New-AzureStorageContainerSASToken -Name <containerName> -Context $context -Permission rwdl -StartTime <startTime> -ExpiryTime <endTime> -FullUri`

De SAS van Azure Storage gekoppelde service kunt u een Azure Storage-Account koppelen aan een Azure data factory met behulp van een Shared Access Signature (SAS). Het biedt de data factory met beperkte/tijdelijke toegang tot alle of naar een specifiek resources (blob/container) in de opslag. De volgende tabel bevat een beschrijving op voor JSON-elementen die specifiek zijn voor Azure Storage SAS gekoppelde service. 

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type |De eigenschap type moet worden ingesteld op: **AzureStorageSas** |Ja |
| sasUri |Shared Access Signature URI voor de Azure Storage-resources, zoals blob, container of tabel opgeven.  |Ja |

**Voorbeeld:**

```json
{  
    "name": "StorageSasLinkedService",  
    "properties": {  
        "type": "AzureStorageSas",  
        "typeProperties": {  
            "sasUri": "<Specify SAS URI of the Azure Storage resource>"   
        }  
    }  
}  
```

Bij het maken van een **SAS-URI**, overweegt het volgende:  

* Instellen van de juiste lezen/schrijven **machtigingen** op objecten op basis van hoe de gekoppelde service (lezen, schrijven, lezen/schrijven) wordt gebruikt in uw data factory.
* Stel **verlooptijd** op de juiste wijze. Zorg ervoor dat de toegang tot Azure Storage-objecten niet binnen een actieve periode van de pijplijn verloopt.
* URI moet worden gemaakt op de juiste container/blob of het niveau van de tabel op basis van de behoeften. Een SAS-Uri met een Azure-blob kan de Data Factory-service voor toegang tot die specifieke blob. Een SAS-Uri naar een Azure blob-container kan de Data Factory-service u doorloopt de blobs in deze container. Als u wilt toegang later meer/minder objecten bieden of bijwerken van de SAS-URI, vergeet dan niet bijwerken van de gekoppelde service met de nieuwe URI.   

