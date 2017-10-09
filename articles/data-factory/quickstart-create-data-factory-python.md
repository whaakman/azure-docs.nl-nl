---
title: Een Azure data factory maken met behulp van Python | Microsoft Docs
description: "Maak een Azure data factory om gegevens te kopiëren van een locatie in een Azure Blob Storage naar een andere locatie in dezelfde Blob Storage."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 09/19/2017
ms.author: jingwang
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 9c6f7c8e052aed2ab5aba2ee54065bf40e81a022
ms.contentlocale: nl-nl
ms.lasthandoff: 09/25/2017

---

# <a name="create-a-data-factory-and-pipeline-using-python"></a>Een data factory en pijplijn maken met behulp van Python
Azure Data Factory is een cloudgebaseerde gegevensintegratieservice waarmee u gegevensgestuurde werkstromen kunt maken in de cloud. Op deze manier kunt u de verplaatsing en transformatie van gegevens indelen en automatiseren. Met Azure Data Factory kunt u gegevensgestuurde werkstromen (ook wel pijplijnen genoemd) maken en plannen die gegevens uit verschillende gegevensopslagexemplaren kunnen opnemen en de gegevens kunnen verwerken/transformeren met behulp van rekenservices zoals Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics en Azure Machine Learning. Daarnaast kunt u de uitvoergegevens publiceren naar gegevensopslagexemplaren zoals Azure SQL Data Warehouse, zodat BI-toepassingen (business intelligence) ze kunnen gebruiken. 

In deze snelstartgids wordt beschreven hoe u Python kunt gebruiken om een Azure data factory te maken. Met de pijplijn in deze data factory worden gegevens gekopieerd van de ene map naar een andere map in een Azure Blob-opslag.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

* **Azure Storage-account**. U gebruikt de blob-opslag als gegevensopslag voor **bron** en **sink**. Als u geen Azure-opslagaccount hebt, raadpleegt u het artikel [Een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account) om een account te maken. 
* **Maak een toepassing in Azure Active Directory** aan de hand van [deze instructie](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application). Noteer de volgende waarden voor gebruik in latere stappen: **toepassings-id**, **verificatiesleutel** en **tenant-id**. Wijs de toepassing toe aan de rol '**Inzender**' door de instructies in hetzelfde artikel te volgen. 

### <a name="create-and-upload-an-input-file"></a>Een invoerbestand maken en uploaden

1. Start Kladblok. Kopieer de volgende tekst en sla deze op schijf op in het bestand **input.txt**.
    
    ```
    John|Doe
    Jane|Doe
    ```
2.  Gebruik hulpprogramma's zoals [Azure Opslagverkenner](http://storageexplorer.com/) om de container **adfv2tutorial** te maken en om het bestand input.txt te uploaden naar de container. 

## <a name="install-the-python-package"></a>Het Python-pakket installeren
1. Open een terminal of opdrachtprompt met beheerdersbevoegdheden.  
2. Voer de volgende opdracht uit om het Python-pakket voor Data Factory te installeren:

    ```
    pip install azure-mgmt-datafactory
    ```

    De [Python-SDK voor Data Factory](https://github.com/Azure/azure-sdk-for-python) ondersteunt Python 2.7, 3.3, 3.4, 3.5 en 3.6.
## <a name="create-a-data-factory-client"></a>Een data factory-client maken

1. Maak een bestand met de naam **datafactory.py**. Voeg de volgende instructies toe om verwijzingen naar naamruimten toe te voegen.
    
    ```python
    from azure.common.credentials import ServicePrincipalCredentials
    from msrestazure.azure_cloud import Cloud, CloudEndpoints, CloudSuffixes
    from azure.mgmt.resource import ResourceManagementClient
    from azure.mgmt.datafactory import DataFactoryManagementClient
    from azure.mgmt.datafactory.models import *
    import json
    import time    
    ```
2. Voeg de volgende functies voor het afdrukken van informatie toe. 

    ```python
    def print_item(group):
      """Print an Azure object instance."""
      print("\tName: {}".format(group.name))
      print("\tId: {}".format(group.id))
      if hasattr(group, 'location'):
          print("\tLocation: {}".format(group.location))
      if hasattr(group, 'tags'):
          print("\tTags: {}".format(group.tags))
      if hasattr(group, 'properties'):
          print_properties(group.properties)
    
    def print_properties(props):
      """Print a ResourceGroup properties instance."""
      if props and hasattr(props, 'provisioning_state') and props.provisioning_state:
          print("\tProperties:")
          print("\t\tProvisioning State: {}".format(props.provisioning_state))
      print("\n\n")    
    ```
3. Voeg de volgende code toe aan de methode **Main** om een instantie van de klasse DataFactoryManagementClient te maken. U gebruikt dit object om de data factory, een gekoppelde service, gegevenssets en een pijplijn te maken. U kunt dit object ook gebruiken om de details van de pijplijnuitvoering te controleren.

    ```python   
    subscription_id = '<your subscription ID where the factory resides>'
    credentials = ServicePrincipalCredentials(
            client_id='<yourClientId>',
            secret='<YourPassword>',
            tenant='<YourTenandId>'
    )
    resource_client = ResourceManagementClient(credentials, subscription_id)
    adf_client = DataFactoryManagementClient(credentials, subscription_id)
    
    rg_params = {'location':'eastus'}
    df_params = {'location':'eastus'}
    rg_name = '<Your Resource Group Name>'
    df_name = '<Your Data Factory Name>'
    ```

## <a name="create-a-data-factory"></a>Een data factory maken

Voeg de volgende code toe aan de methode **Main** om een **data factory** te maken. 

```python
#Create Resource Group
resource_client.resource_groups.create_or_update(rg_name, rg_params)

#Create Data Factory
df_resource = Factory(location='eastus')
df = adf_client.factories.create_or_update(rg_name, df_name, df_resource)
print_item(df)
while df.provisioning_state != 'Succeeded':
    df = adf_client.factories.get(rg_name, df_name)
    time.sleep(1)
print_item(adf_client.factories.get(rg_name, df_name))
```

## <a name="create-a-linked-service"></a>Een gekoppelde service maken

Voeg de volgende code toe aan de methode **Main** om een **gekoppelde Azure Storage-service** te maken.

U maakt gekoppelde services in een gegevensfactory om uw gegevensarchieven en compute-services aan de gegevensfactory te koppelen. In deze snelstartgids hoeft u maar één gekoppelde Azure Storage-service te maken die in het voorbeeld wordt gebruikt als zowel de bron voor het kopiëren en als de sinkopslag, met de naam 'AzureStorageLinkedService'.

```python
#Create Storage Linked Service
ls_name = 'storageLinkedService'

#Replace Storage String with your credentials
storage_string = SecureString('DefaultEndpointsProtocol=https;AccountName=<replace>;AccountKey=<replace>')

ls_azure_storage = AzureStorageLinkedService(connection_string=storage_string)
ls = adf_client.linked_services.create_or_update(rg_name, df_name, ls_name, ls_azure_storage)
print_item(ls)
```
## <a name="create-datasets"></a>Gegevenssets maken
In deze sectie maakt u twee gegevenssets: één voor de bron en de andere voor de sink.

### <a name="create-a-dataset-for-source-azure-blob"></a>Een gegevensset maken voor de brongegevens in Azure Blob
Voeg de volgende code toe aan de methode Main om een Azure blob-gegevensset te maken. Zie het [artikel over Azure Blob-connectoren](connector-azure-blob-storage.md#dataset-properties) voor meer informatie over de eigenschappen van een Azure Blob-gegevensset. 

U definieert een gegevensset die de brongegevens in Azure Blob vertegenwoordigt. Deze Blob-gegevensset verwijst naar de gekoppelde Azure Storage-service die u in de vorige stap hebt gemaakt.

```python
#Create Dataset Input
ds_name = 'ds_in'
ds_ls = LinkedServiceReference(ls_name)
blob_path= 'adfv2tutorial/'
blob_filename = 'input.txt'
ds_azure_blob= AzureBlobDataset(ds_ls, folder_path=blob_path, file_name = blob_filename)
ds = adf_client.datasets.create_or_update(rg_name, df_name, ds_name, ds_azure_blob)
print_item(ds)
```

### <a name="create-a-dataset-for-sink-azure-blob"></a>Een gegevensset maken voor de sinkgegevens in Azure Blob
Voeg de volgende code toe aan de methode Main om een Azure blob-gegevensset te maken. Zie het [artikel over Azure Blob-connectoren](connector-azure-blob-storage.md#dataset-properties) voor meer informatie over de eigenschappen van een Azure Blob-gegevensset. 

U definieert een gegevensset die de brongegevens in Azure Blob vertegenwoordigt. Deze Blob-gegevensset verwijst naar de gekoppelde Azure Storage-service die u in de vorige stap hebt gemaakt.

```python
#Create Dataset Output
dsOut_name = 'ds_out'
output_blobpath = 'output/'
dsOut_azure_blob = AzureBlobDataset(ds_ls, folder_path=output_blobpath)
dsOut = adf_client.datasets.create_or_update(rg_name, df_name, dsOut_name, dsOut_azure_blob)
print_item(dsOut)
```

## <a name="create-a-pipeline"></a>Een pijplijn maken

Voeg de volgende code toe aan de methode **Main** om **een pijplijn met een kopieeractiviteit te maken**.

```python
#Create 1st activity: Copy Activity
act_name =  'copyBlobtoBlob'
blob_source = BlobSource()
blob_sink = BlobSink()
dsin_ref = DatasetReference(ds_name)
dsOut_ref = DatasetReference(dsOut_name)
copy_activity = CopyActivity(act_name,inputs=[dsin_ref], outputs=[dsOut_ref], source=blob_source, sink=blob_sink)

#Create Pipeline
p_name =  'copyPipeline'
params_for_pipeline = {}
p_obj = PipelineResource(activities=[copy_activity], parameters=params_for_pipeline)
p = adf_client.pipelines.create_or_update(rg_name, df_name, p_name, p_obj)
print_item(p)
```


## <a name="create-a-pipeline-run"></a>Een pijplijnuitvoering maken

Voeg de volgende code toe aan de methode **Main** om een **pijplijnuitvoering te activeren**.

```python
#Create Pipeline Run
print(adf_client.pipelines.create_run(rg_name, df_name, p_name,
    {
    }
))
```

## <a name="run-the-code"></a>De code uitvoeren
Bouw en start de toepassing en controleer vervolgens de uitvoering van de pijplijn.

In de console wordt de voortgang weergegeven van het maken van een data factory, een gekoppelde service, gegevenssets, pijplijn en pijplijnuitvoering. Wacht totdat u details ziet van de uitvoering van de kopieeractiviteit, waaronder de omvang van de gelezen/weggeschreven gegevens. Gebruik vervolgens hulpprogramma's als [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) om te controleren of de blob(s) is/zijn gekopieerd van het 'inputBlobPath' naar het 'outputBlobPath' zoals u hebt opgegeven in de variabelen.


## <a name="clean-up-resources"></a>Resources opschonen
Als u de data factory programmatisch wilt verwijderen, voegt u de volgende regels code toe aan het programma: 

```csharp
adf_client.data_factories.delete(rg_name, df_name)
```

## <a name="next-steps"></a>Volgende stappen
Met de pijplijn in dit voorbeeld worden gegevens gekopieerd van de ene locatie naar een andere locatie in een Azure Blob-opslag. Doorloop de [zelfstudies](tutorial-copy-data-dot-net.md) voor meer informatie over het gebruiken van Data Factory in andere scenario's. 
