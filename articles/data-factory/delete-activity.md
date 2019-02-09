---
title: Verwijderen van activiteit in Azure Data Factory | Microsoft Docs
description: Informatie over het verwijderen van bestanden in verschillende winkels van bestand met de activiteit verwijderen in Azure Data Factory.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: douglasl
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/10/2019
ms.openlocfilehash: 4d574c0ceef01ed3c292b77b5523b8423d34fe05
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55967342"
---
# <a name="delete-activity-in-azure-data-factory"></a>Activiteit in Azure Data Factory verwijderen

U kunt de activiteit verwijderen in Azure Data Factory gebruiken om bestanden te verwijderen of mappen uit de on-premises opslag worden opgeslagen of in de cloud storage winkels. Gebruik deze activiteit op te schonen of voor het archiveren van bestanden wanneer ze niet langer nodig zijn.

> [!WARNING]
> Verwijderde bestanden of mappen kunnen niet worden hersteld. Wees voorzichtig bij het gebruik van de activiteit verwijderen te verwijderen van bestanden of mappen.

## <a name="best-practices"></a>Aanbevolen procedures

Hier volgen enkele aanbevelingen voor het gebruik van de activiteit verwijderen:

-   Back-up van uw bestanden voordat deze worden verwijderd met de activiteit verwijderen als u wilt herstellen in de toekomst.

-   Zorg ervoor dat Data Factory schrijfmachtigingen voor de mappen of bestanden verwijderen uit de store opslag heeft.

-   Zorg ervoor dat u bent niet verwijderen van bestanden die op hetzelfde moment worden geschreven. 

-   Als u verwijderen van bestanden of mappen van een on-premises systeem wilt, zorg er dan voor dat u gebruikmaakt van een zelf-hostende integratieruntime met een versie die groter zijn dan 3.13.

## <a name="supported-data-stores"></a>Ondersteunde gegevensarchieven

### <a name="azure-data-stores"></a>Azure-gegevensopslag

-   [Azure Blob Storage](connector-azure-blob-storage.md)
-   [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md)
-   [Azure Data Lake Storage Gen2 (preview)](connector-azure-data-lake-storage.md)

### <a name="file-system-data-stores"></a>Systeem gegevens worden opgeslagen

-   [Bestandssysteem](connector-file-system.md)
-   [FTP](connector-ftp.md)
-   [HDFS](connector-hdfs.md)

## <a name="syntax"></a>Syntaxis

```json
{
    "name": "DeleteActivity",
    "type": "Delete",
    "typeProperties": {
        "dataset": {
            "referenceName": "<dataset name to be deleted>",
            "type": "DatasetReference"
        },
        "recursive": true/false,
        "maxConcurrentConnections": <number>,
        "enableLogging": true/false,
        "logStorageSettings": {
            "linkedServiceName": {
                "referenceName": "<name of linked service>",
                "type": "LinkedServiceReference"
            },
            "path": "<path to save log file>"
        }
    }
}
```

## <a name="type-properties"></a>Type-eigenschappen

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| Gegevensset | Bevat de referentie van de gegevensset om te bepalen welke bestanden of map die wordt verwijderd | Ja |
| recursieve | Geeft aan of de bestanden verwijderd recursief uit de submappen of alleen voor de opgegeven map zijn.  | Nee. De standaardwaarde is `false`. |
| maxConcurrentConnections | Het nummer van de verbindingen verbinding maken met opslag store gelijktijdig voor het verwijderen van map of bestanden.   |  Nee. De standaardwaarde is `1`. |
| EnableLogging | Geeft aan of u wilt vastleggen van de namen van mappen of bestanden die zijn verwijderd. Indien waar, moet u een opslagaccount om op te slaan van het logboekbestand verder opgeven zodat u het gedrag van de Delete-activiteit bijhouden kunt door te lezen van het logboekbestand. | Nee |
| logStorageSettings | Alleen van toepassing wanneer enablelogging = true.<br/><br/>Een groep met Opslageigenschappen die kunnen worden opgegeven waar u het bestand met de map of bestand namen die zijn verwijderd door de activiteit verwijderen. | Nee |
| linkedServiceName | Alleen van toepassing wanneer enablelogging = true.<br/><br/>De gekoppelde service van [Azure Storage](connector-azure-blob-storage.md#linked-service-properties) of [Azure Data Lake Store](connector-azure-data-lake-store.md#linked-service-properties) voor het opslaan van het logboekbestand dat bevat het bestand of map namen die zijn verwijderd door de activiteit verwijderen. | Nee |
| pad | Alleen van toepassing wanneer enablelogging = true.<br/><br/>Het pad naar het logboekbestand opslaan in uw storage-account. Als u een pad opgeeft, wordt in de service een container voor u gemaakt. | Nee |

## <a name="monitoring"></a>Bewaking

Er zijn twee manieren waarop u kunt zien en controleren van de resultaten van de activiteit verwijderen: 
-   Uit de uitvoer van de activiteit verwijderen.
-   In het logboekbestand.

### <a name="sample-output-of-the-delete-activity"></a>Voorbeeld van de uitvoer van de activiteit verwijderen

```json
{ 
  "isWildcardUsed": false, 
  "wildcard": null,
  "type": "AzureBlobStorage",
  "recursive": true,
  "maxConcurrentConnections": 10,
  "filesDeleted": 1,
  "logPath": "https://sample.blob.core.windows.net/mycontainer/5c698705-a6e2-40bf-911e-e0a927de3f07/5c698705-a6e2-40bf-911e-e0a927de3f07.json",
  "effectiveIntegrationRuntime": "MyAzureIR (West Central US)",
  "executionDuration": 650
}
```

### <a name="sample-log-file-of-the-delete-activity"></a>Logboekbestand van de activiteit verwijderen

```json
{
  "customerInput": {
    "type": "AzureBlob",
    "fileName": "",
    "folderPath": "folder/filename_to_be_deleted",
    "recursive": false,
    "enableFileFilter": false
  },
  "deletedFileList": [
    "folder/filename_to_be_deleted"
  ],
  "deletedFolderList": null,
  "error":"the reason why files are failed to be deleted"
}
```

## <a name="examples-of-using-the-delete-activity"></a>Voorbeelden van het gebruik van de activiteit verwijderen

### <a name="delete-specific-folders-or-files"></a>Specifieke mappen of bestanden verwijderen

De store heeft de volgende mapstructuur:

Hoofdmap /<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8.txt

U gebruikt nu de verwijderen map of bestanden verwijderen door de combinatie van een andere eigenschapswaarde van de gegevensset en de activiteit verwijderen:

| folderPath (van de gegevensset) | Bestandsnaam (van de gegevensset) | recursieve (van de activiteit verwijderen) | Uitvoer |
|:--- |:--- |:--- |:--- |
| Root/ Folder_A_2 | NULL | False | Hoofdmap /<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>4.txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>5.csv</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8.txt |
| Root/ Folder_A_2 | NULL | True | Hoofdmap /<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;<strike>Folder_A_2/</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>4.txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>5.csv</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>Folder_B_1/</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>6.txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>7.csv</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>Folder_B_2/</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>8.txt</strike> |
| Root/ Folder_A_2 | *.txt | False | Hoofdmap /<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>4.txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8.txt |
| Root/ Folder_A_2 | *.txt | True | Hoofdmap /<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.txt<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;Folder_A_2/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>4.txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_1/<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strike>6.txt</strike><br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Folder_B_2/ |

### <a name="periodically-clean-up-the-time-partitioned-folder-or-files"></a>Periodiek opschonen van de tijd gepartitioneerd map of bestanden

U kunt een pijplijn voor het periodiek opschonen van de tijd gepartitioneerde map of bestanden maken.  Bijvoorbeeld, de mapstructuur is vergelijkbaar als: `/mycontainer/2018/12/14/*.csv`.  U kunt gebruikmaken van de systeemvariabele ADF van trigger voor schema om te identificeren welke map of bestanden moeten worden verwijderd in elke pijplijnuitvoering. 

#### <a name="sample-pipeline"></a>Voorbeeldpijplijn

```json
{
    "name": "cleanup_time_partitioned_folder",
    "properties": {
        "activities": [
            {
                "name": "DeleteOneFolder",
                "type": "Delete",
                "policy": {
                    "timeout": "7.00:00:00",
                    "retry": 0,
                    "retryIntervalInSeconds": 30,
                    "secureOutput": false,
                    "secureInput": false
                },
                "typeProperties": {
                    "dataset": {
                        "referenceName": "PartitionedFolder",
                        "type": "DatasetReference",
                        "parameters": {
                            "TriggerTime": {
                                "value": "@formatDateTime(pipeline().parameters.TriggerTime, 'yyyy/MM/dd')",
                                "type": "Expression"
                            }
                        }
                    },
                    "recursive": true,
                    "logStorageSettings": {
                        "linkedServiceName": {
                            "referenceName": "BloblinkedService",
                            "type": "LinkedServiceReference"
                        },
                        "path": "mycontainer/log"
                    },
                    "enableLogging": true
                }
            }
        ],
        "parameters": {
            "TriggerTime": {
                "type": "String"
            }
        }
    },
    "type": "Microsoft.DataFactory/factories/pipelines"
}
```

#### <a name="sample-dataset"></a>Voorbeeldgegevensset

```json
{
    "name": "PartitionedFolder",
    "properties": {
        "linkedServiceName": {
            "referenceName": "BloblinkedService",
            "type": "LinkedServiceReference"
        },
        "parameters": {
            "TriggerTime": {
                "type": "String"
            }
        },
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": {
                "value": "@concat('mycontainer/',dataset().TriggerTime)",
                "type": "Expression"
            }
        }
    },
    "type": "Microsoft.DataFactory/factories/datasets"
}
```

#### <a name="sample-trigger"></a>Voorbeeld van trigger

```json
{
    "name": "DailyTrigger",
    "properties": {
        "runtimeState": "Started",
        "pipelines": [
            {
                "pipelineReference": {
                    "referenceName": "cleanup_time_partitioned_folder",
                    "type": "PipelineReference"
                },
                "parameters": {
                    "TriggerTime": "@trigger().scheduledTime"
                }
            }
        ],
        "type": "ScheduleTrigger",
        "typeProperties": {
            "recurrence": {
                "frequency": "Day",
                "interval": 1,
                "startTime": "2018-12-13T00:00:00.000Z",
                "timeZone": "UTC",
                "schedule": {
                    "minutes": [
                        59
                    ],
                    "hours": [
                        23
                    ]
                }
            }
        }
    }
}
```

### <a name="clean-up-the-expired-files-that-were-last-modified-before-201811"></a>Opschonen van de verlopen bestanden die het laatst zijn gewijzigd voordat 2018.1.1

U kunt een pijplijn voor het opschonen van de oude of verlopen bestanden door gebruik te maken van het kenmerkfilter bestand maken: "LastModified' in de gegevensset.  

#### <a name="sample-pipeline"></a>Voorbeeldpijplijn

```json
{
    "name": "CleanupExpiredFiles",
    "properties": {
        "activities": [
            {
                "name": "DeleteFilebyLastModified",
                "type": "Delete",
                "policy": {
                    "timeout": "7.00:00:00",
                    "retry": 0,
                    "retryIntervalInSeconds": 30,
                    "secureOutput": false,
                    "secureInput": false
                },
                "typeProperties": {
                    "dataset": {
                        "referenceName": "BlobFilesLastModifiedBefore201811",
                        "type": "DatasetReference"
                    },
                    "recursive": true,
                    "logStorageSettings": {
                        "linkedServiceName": {
                            "referenceName": "BloblinkedService",
                            "type": "LinkedServiceReference"
                        },
                        "path": "mycontainer/log"
                    },
                    "enableLogging": true
                }
            }
        ]
    }
}
```

#### <a name="sample-dataset"></a>Voorbeeldgegevensset

```json
{
    "name": "BlobFilesLastModifiedBefore201811",
    "properties": {
        "linkedServiceName": {
            "referenceName": "BloblinkedService",
            "type": "LinkedServiceReference"
        },
        "type": "AzureBlob",
        "typeProperties": {
            "fileName": "",
            "folderPath": "mycontainer",
            "modifiedDatetimeEnd": "2018-01-01T00:00:00.000Z"
        }
    }
}
```

### <a name="move-files-by-chaining-the-copy-activity-and-the-delete-activity"></a>Bestanden verplaatsen door het koppelen van de kopieeractiviteit en de activiteit verwijderen

U kunt een bestand verplaatst met behulp van een kopieeractiviteit om een bestand te kopiëren en vervolgens een de Delete-activiteit om een bestand in een pijplijn te verwijderen.  Als u wilt dat meerdere bestanden te verplaatsen, kunt u gebruikmaken van de activiteit GetMetadata + filteractiviteit + Foreach-activiteit + kopieeractiviteit + activiteit zoals in het volgende voorbeeld verwijderen:

> [!NOTE]
> Als u wilt verplaatsen van de hele map door te definiëren van een gegevensset met alleen een mappad en vervolgens met behulp van een kopieeractiviteit en een de Delete-activiteit om te verwijzen naar dezelfde gegevensset voor een map, moet u voorzichtig. Dit is omdat u om ervoor te zorgen dat er geen nieuwe bestanden die binnenkomen in de map worden tussen bewerking kopiëren en verwijderen van de bewerking.  Als er nieuwe bestanden die binnenkomen in de map op het moment wanneer de kopieeractiviteit zojuist de taak voor het kopiëren, maar de Delete-activiteit niet is stared zijn, is het mogelijk dat de activiteit verwijderen deze nieuwe binnenkomende-bestand dat niet is gekopieerd naar de destinati wordt verwijderd op het nog door het verwijderen van de hele map. 

#### <a name="sample-pipeline"></a>Voorbeeldpijplijn

```json
{
    "name": "MoveFiles",
    "properties": {
        "activities": [
            {
                "name": "GetFileList",
                "type": "GetMetadata",
                "typeProperties": {
                    "dataset": {
                        "referenceName": "OneSourceFolder",
                        "type": "DatasetReference"
                    },
                    "fieldList": [
                        "childItems"
                    ]
                }
            },
            {
                "name": "FilterFiles",
                "type": "Filter",
                "dependsOn": [
                    {
                        "activity": "GetFileList",
                        "dependencyConditions": [
                            "Succeeded"
                        ]
                    }
                ],
                "typeProperties": {
                    "items": {
                        "value": "@activity('GetFileList').output.childItems",
                        "type": "Expression"
                    },
                    "condition": {
                        "value": "@equals(item().type, 'File')",
                        "type": "Expression"
                    }
                }
            },
            {
                "name": "ForEachFile",
                "type": "ForEach",
                "dependsOn": [
                    {
                        "activity": "FilterFiles",
                        "dependencyConditions": [
                            "Succeeded"
                        ]
                    }
                ],
                "typeProperties": {
                    "items": {
                        "value": "@activity('FilterFiles').output.value",
                        "type": "Expression"
                    },
                    "batchCount": 20,
                    "activities": [
                        {
                            "name": "CopyAFile",
                            "type": "Copy",
                            "policy": {
                                "timeout": "7.00:00:00",
                                "retry": 0,
                                "retryIntervalInSeconds": 30,
                                "secureOutput": false,
                                "secureInput": false
                            },
                            "typeProperties": {
                                "source": {
                                    "type": "BlobSource",
                                    "recursive": false
                                },
                                "sink": {
                                    "type": "BlobSink"
                                },
                                "enableStaging": false,
                                "dataIntegrationUnits": 0
                            },
                            "inputs": [
                                {
                                    "referenceName": "OneSourceFile",
                                    "type": "DatasetReference",
                                    "parameters": {
                                        "path": "myFolder",
                                        "filename": {
                                            "value": "@item().name",
                                            "type": "Expression"
                                        }
                                    }
                                }
                            ],
                            "outputs": [
                                {
                                    "referenceName": "OneDestinationFile",
                                    "type": "DatasetReference",
                                    "parameters": {
                                        "DestinationFileName": {
                                            "value": "@item().name",
                                            "type": "Expression"
                                        }
                                    }
                                }
                            ]
                        },
                        {
                            "name": "DeleteAFile",
                            "type": "Delete",
                            "dependsOn": [
                                {
                                    "activity": "CopyAFile",
                                    "dependencyConditions": [
                                        "Succeeded"
                                    ]
                                }
                            ],
                            "policy": {
                                "timeout": "7.00:00:00",
                                "retry": 0,
                                "retryIntervalInSeconds": 30,
                                "secureOutput": false,
                                "secureInput": false
                            },
                            "typeProperties": {
                                "dataset": {
                                    "referenceName": "OneSourceFile",
                                    "type": "DatasetReference",
                                    "parameters": {
                                        "path": "myFolder",
                                        "filename": {
                                            "value": "@item().name",
                                            "type": "Expression"
                                        }
                                    }
                                },
                                "logStorageSettings": {
                                    "linkedServiceName": {
                                        "referenceName": "BloblinkedService",
                                        "type": "LinkedServiceReference"
                                    },
                                    "path": "Container/log"
                                },
                                "enableLogging": true
                            }
                        }
                    ]
                }
            }
        ]
    }
}
```

#### <a name="sample-datasets"></a>Voorbeeldgegevenssets

De gegevensset die wordt gebruikt door de activiteit GetMetadata opsommen van de lijst met bestanden.

```json
{
    "name": "OneSourceFolder",
    "properties": {
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "type": "AzureBlob",
        "typeProperties": {
            "fileName": "",
            "folderPath": "myFolder"
        }
    }
}
```

Gegevensset voor de gegevensbron die wordt gebruikt door de kopieeractiviteit en de Delete-activiteit.

```json
{
    "name": "OneSourceFile",
    "properties": {
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "parameters": {
            "path": {
                "type": "String"
            },
            "filename": {
                "type": "String"
            }
        },
        "type": "AzureBlob",
        "typeProperties": {
            "fileName": {
                "value": "@dataset().filename",
                "type": "Expression"
            },
            "folderPath": {
                "value": "@{dataset().path}",
                "type": "Expression"
            }
        }
    }
}
```

Gegevensset voor het Gegevensdoel van die worden gebruikt door de kopieeractiviteit.

```json
{
    "name": "OneDestinationFile",
    "properties": {
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "parameters": {
            "DestinationFileName": {
                "type": "String"
            }
        },
        "type": "AzureBlob",
        "typeProperties": {
            "fileName": {
                "value": "@dataset().DestinationFileName",
                "type": "Expression"
            },
            "folderPath": "mycontainer/dest"
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het kopiëren van bestanden in Azure Data Factory.

-   [De Kopieeractiviteit in Azure Data Factory](copy-activity-overview.md)

-   [Hulpprogramma voor kopiëren-gegevens in Azure Data Factory](copy-data-tool.md)
- 