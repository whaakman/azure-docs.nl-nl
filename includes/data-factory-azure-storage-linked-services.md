### <a name="azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service
De **gekoppelde Azure Storage-service** kunt u een Azure storage-account koppelen aan een Azure data factory met behulp van de **accountsleutel**, waarmee de gegevensfactory met globale toegang tot de Azure Storage. De volgende tabel bevat een beschrijving voor JSON-elementen die specifiek zijn voor gekoppelde Azure Storage-service.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type |De eigenschap type moet worden ingesteld op: **AzureStorage** |Ja |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met Azure-opslag voor de eigenschap connectionString. |Ja |

Zie het volgende artikel voor stappen voor de accountsleutel kopie/weergeven voor een Azure Storage: [weergeven, kopiëren en opnieuw genereren opslag toegangssleutels](../articles/storage/common/storage-create-storage-account.md#manage-your-storage-account).

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
Een shared access signature (SAS) biedt gedelegeerde toegang tot bronnen in uw opslagaccount. Hiermee kunt u dat een client beperkte machtigingen voor objecten in uw storage-account gedurende een bepaalde tijd en met een opgegeven set machtigingen, zonder dat voor het delen van de toegangssleutels van uw account verlenen. De SAS is een URI die de gegevens die zijn vereist voor geverifieerde toegang tot een opslagresource, in de queryparameters omvat. Voor toegang tot de storage-resources met de SA's, moet de client alleen worden doorgegeven in de SA's aan de juiste constructor of methode. Zie voor gedetailleerde informatie over SAS [Shared Access Signatures: inzicht in het SAS-Model](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)

> [!IMPORTANT]
> Azure Data Factory nu ondersteunt alleen het **Service-SAS** maar geen Account-SAS. Zie [typen van handtekeningen voor gedeelde toegang](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) voor meer informatie over deze twee typen en bouwen. Let op de SAS-URL generable vanuit Azure-portal of Opslagverkenner is een SAS-Account, wat niet wordt ondersteund.
> 

De service Azure Storage SAS is gekoppeld, kunt u een Azure Storage-Account koppelen aan een Azure data factory met behulp van een Shared Access Signature (SAS). Het biedt de gegevensfactory beperkt/tijdsgebonden toegang tot alle/specifieke bronnen, (blobcontainer) / in de opslag. De volgende tabel bevat een beschrijving voor JSON-elementen die specifiek zijn voor Azure Storage SAS gekoppelde service. 

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type |De eigenschap type moet worden ingesteld op: **AzureStorageSas** |Ja |
| sasUri |Shared Access Signature URI voor de Azure Storage-resources zoals blob-container of tabel opgeven.  |Ja |

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

* Stel de juiste lezen/schrijven **machtigingen** op objecten op basis van hoe de gekoppelde service (lezen, schrijven, lezen/schrijven) wordt gebruikt in uw gegevensfactory.
* Stel **verlooptijdstip** op de juiste wijze. Zorg ervoor dat de toegang tot Azure Storage-objecten niet binnen een actieve periode van de pijplijn verloopt.
* URI moet worden gemaakt op de juiste container/blob of tabelniveau op basis van de behoeften. Een SAS-Uri met een Azure-blob kan de Data Factory-service voor toegang tot bepaalde blob. Een SAS-Uri naar een Azure blob-container kan de Data Factory-service om te doorlopen blobs in de container. Als u wilt toegang later meer/minder objecten bieden of bijwerken van de SAS-URI, moet u bij het bijwerken van de gekoppelde service met de nieuwe URI.   

