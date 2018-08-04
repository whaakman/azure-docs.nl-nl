---
title: Azure Batch-taken uitvoeren end-to-end met behulp van sjablonen | Microsoft Docs
description: Batch-pools, jobs en taken maken met sjabloonbestanden en de Azure CLI.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: na
ms.topic: article
ms.workload: big-compute
ms.date: 08/02/2018
ms.author: danlep
ms.openlocfilehash: 50ed5a6b57c3c994f636db5cc975ad1908e50c7d
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39493430"
---
# <a name="use-azure-batch-cli-templates-and-file-transfer"></a>Azure Batch CLI sjablonen en -bestandsoverdracht gebruiken

Met behulp van een Azure Batch-extensie voor de Azure CLI, is het mogelijk om uit te voeren van Batch-taken zonder code te schrijven.

Maken en gebruiken van JSON-sjabloon-bestanden met de Azure CLI voor Batch-pools, jobs en taken maken. Gebruik CLI-extensie-opdrachten eenvoudig jobinvoerbestanden te uploaden naar het opslagaccount die is gekoppeld aan het Batch-account en downloadtaak uitvoerbestanden.

## <a name="overview"></a>Overzicht

Een uitbreiding van de Azure CLI kunt Batch gebruikt end-to-end worden door gebruikers die geen ontwikkelaars. Met alleen CLI-opdrachten, kunt u een pool maakt, invoergegevens uploaden, taken en de bijbehorende taken maken en de resulterende uitvoergegevens downloaden. Er is geen extra programmacode vereist. De CLI-opdrachten rechtstreeks worden uitgevoerd of integreren in scripts.

Batch-sjablonen zijn gebaseerd op de [bestaande Batch support in de Azure CLI](batch-cli-get-started.md#json-files-for-resource-creation) voor JSON-bestanden eigenschapswaarden opgeven bij het maken van pools, jobs, taken en andere items. Batch-sjablonen toevoegen de volgende mogelijkheden:

-   Parameters kunnen worden gedefinieerd. Wanneer de sjabloon wordt gebruikt, worden alleen de parameterwaarden die zijn opgegeven om te maken van het item met andere waarden van het item eigenschap opgegeven in de hoofdtekst van de sjabloon. Een gebruiker die Batch begrijpt en de toepassingen die worden uitgevoerd door de Batch kunnen sjablonen maken, toepassingen, taak en eigenschapswaarden van de taak op te geven. Een gebruiker minder bekend bent met Batch en/of de toepassingen moet alleen de waarden voor de gedefinieerde parameters op te geven.

-   Taak taak factory's maken van een of meer taken die zijn gekoppeld aan een taak, het vermijden van de noodzaak van veel taakdefinities moet worden gemaakt en verzenden van taken aanzienlijk te vereenvoudigen.


Taken doorgaans invoergegevensbestanden gebruiken en gegevensbestanden uitvoer produceren. Een storage-account is gekoppeld, standaard, met elk Batch-account. Overdracht van bestanden naar en van dit storage-account met behulp van de CLI met geen codering en geen storage-referenties.

Bijvoorbeeld, [ffmpeg](http://ffmpeg.org/) is een populaire toepassing die audio en video-bestanden worden verwerkt. Hier volgen de stappen met de Azure Batch CLI om aan te roepen ffmpeg te transcoderen videobestanden bronbestanden naar verschillende resoluties.

-   Een pool-sjabloon maken. De gebruiker die het maken van de sjabloon kent over het aanroepen van de toepassing ffmpeg en de vereisten; ze geven het juiste besturingssysteem, de VM grootte, hoe ffmpeg is geïnstalleerd (van een toepassingspakket of met behulp van Pakketbeheer, bijvoorbeeld) en andere eigenschapswaarden van toepassingen. Parameters worden gemaakt wanneer de sjabloon wordt gebruikt, alleen de groeps-ID en het aantal virtuele machines moeten worden opgegeven.

-   Maak een taaksjabloon. De gebruiker die het maken van de sjabloon weet hoe ffmpeg moet worden aangeroepen met transcoderen bron video naar een andere resolutie en Hiermee geeft u op de opdrachtregel van de taak; ook weten dat er een map met de bronbestanden video met een taak die vereist is per bestand voor invoer is.

-   Een eindgebruiker met een set te transcoderen videobestanden maakt eerst een groep van toepassingen met behulp van de sjabloon van toepassingen op te geven alleen de groeps-ID en het aantal virtuele machines die zijn vereist. Ze kunnen vervolgens de bronbestanden voor het transcoderen uploaden. Een taak kan vervolgens worden verzonden met behulp van de taaksjabloon, op te geven alleen de groeps-ID en de locatie van de bronbestanden geüpload. De Batch-taak is gemaakt, met één taak per bestand voor invoer wordt gegenereerd. Ten slotte kunnen de uitvoerbestanden transcoderen worden gedownload.

## <a name="installation"></a>Installatie

De Azure Batch CLI-extensie, eerst installeren [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli), of de Azure CLI uitvoert [Azure Cloud Shell](../cloud-shell/overview.md).

Installeer de nieuwste versie van de Batch-extensie met de volgende Azure CLI-opdracht:

```azurecli
az extension add --name azure-batch-cli-extensions
```

Zie voor meer informatie over de Batch-CLI-extensie en aanvullende installatie-opties, de [GitHub-opslagplaats](https://github.com/Azure/azure-batch-cli-extensions).


De CLI-extensie om functies te gebruiken, moet u een Azure Batch-account en voor de opdrachten die overdracht van bestanden en naar opslag, een gekoppelde storage-account.

Als u wilt aanmelden bij een Batch-account met de Azure CLI, Zie [Batch-resources beheren met Azure CLI](batch-cli-get-started.md).

## <a name="templates"></a>Sjablonen

Azure Batch-sjablonen zijn vergelijkbaar met Azure Resource Manager-sjablonen, in de functionaliteit en syntaxis. Deze zijn JSON-bestanden die de itemnamen van de eigenschap en waarden bevatten, maar Voeg de volgende belangrijkste concepten:

-   **Parameters**

    -   Toestaan dat eigenschapswaarden worden opgegeven in een sectie hoofdtekst met alleen de parameterwaarden die zijn dat u hoeft te worden opgegeven wanneer de sjabloon wordt gebruikt. Bijvoorbeeld, kan de volledige definitie voor een groep worden geplaatst in de hoofdtekst en slechts één parameter gedefinieerd voor de groeps-id; Daarom moet alleen een tekenreeks in de pool-ID worden opgegeven voor het maken van een groep.
        
    -   De hoofdtekst van de sjabloon kan worden gemaakt door iemand met kennis van de Batch- en de toepassingen die moeten worden uitgevoerd door Batch; alleen de waarden voor de auteur gedefinieerde parameters moeten worden opgegeven wanneer de sjabloon wordt gebruikt. Een gebruiker zonder de uitgebreide Batch en/of de kennis van de toepassing kan daarom de sjablonen gebruiken.

-   **Variabelen**

    -   Toestaan van eenvoudige of complexe parameterwaarden worden opgegeven op één plek en op een of meer plaatsen in de hoofdtekst van de sjabloon gebruikt. Variabelen kunnen vereenvoudigen en Reduceer de grootte van de sjabloon, evenals meer onderhouden maken door een locatie om eigenschappen te wijzigen.

-   **Een hoger niveau constructies**

    -   Sommige op een hoger niveau constructies zijn beschikbaar in de sjabloon die nog niet beschikbaar in de Batch-API's. De fabrieksinstellingen van een taak kan bijvoorbeeld worden gedefinieerd in een sjabloon die wordt gemaakt van meerdere taken voor de taak, met behulp van een gemeenschappelijke taakdefinitie. Deze gegevens te voorkomen dat de moeten code voor het dynamisch maken meerdere JSON-bestanden, zoals een bestand per taak, evenals scriptbestanden maken voor het installeren van toepassingen via een pakketbeheerder.

    -   Op een bepaald moment mogelijk deze constructies toegevoegd aan de Batch-service en beschikbaar is in de Batch-API's, gebruikersinterfaces, enzovoort.

### <a name="pool-templates"></a>Pool-sjablonen

Sjablonen van de groep van toepassingen ondersteunen de standaardsjabloon mogelijkheden van de parameters en variabelen. Ze ondersteunen ook het volgende op een hoger niveau om voor te bereiden:

-   **Pakket met verwijzingen**

    -   Eventueel kunt software moeten worden gekopieerd naar de pool met behulp van Pakketbeheer. De package manager en de pakket-ID worden opgegeven. U voorkomen door op te geven van een of meer pakketten, het maken van een script dat opgehaald van de vereiste pakketten, installeren van het script en het script is uitgevoerd op elk knooppunt in de pool.

Hier volgt een voorbeeld van een sjabloon dat een pool van virtuele Linux-machines met ffmpeg geïnstalleerd maakt. Als u wilt gebruiken, geef alleen een tekenreeks in de pool-ID en het aantal virtuele machines in de groep:

```json
{
    "parameters": {
        "nodeCount": {
            "type": "int",
            "metadata": {
                "description": "The number of pool nodes"
            }
        },
        "poolId": {
            "type": "string",
            "metadata": {
                "description": "The pool ID "
            }
        }
    },
    "pool": {
        "type": "Microsoft.Batch/batchAccounts/pools",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "[parameters('poolId')]",
            "virtualMachineConfiguration": {
                "imageReference": {
                    "publisher": "Canonical",
                    "offer": "UbuntuServer",
                    "sku": "16.04-LTS",
                    "version": "latest"
                },
                "nodeAgentSKUId": "batch.node.ubuntu 16.04"
            },
            "vmSize": "STANDARD_D3_V2",
            "targetDedicatedNodes": "[parameters('nodeCount')]",
            "enableAutoScale": false,
            "maxTasksPerNode": 1,
            "packageReferences": [
                {
                    "type": "aptPackage",
                    "id": "ffmpeg"
                }
            ]
        }
    }
}
```

Als de naam van het sjabloonbestand _pool ffmpeg.json_, klikt u vervolgens de sjabloon als volgt starten:

```azurecli
az batch pool create --template pool-ffmpeg.json
```

De CLI vraagt u waarden opgeven voor de `poolId` en `nodeCount` parameters. U kunt ook de parameters in een JSON-bestand opgeven. Bijvoorbeeld:

```json
{
  "poolId": {
    "value": "mypool"
  },
  "nodeCount": {
    "value": 2
  }
}
```

Als de naam van de parameters-JSON-bestand *pool-parameters.json*, klikt u vervolgens de sjabloon als volgt starten:

```azurecli
az batch pool create --template pool-ffmpeg.json --parameters pool-parameters.json
```

### <a name="job-templates"></a>Taaksjablonen

Taaksjablonen ondersteuning voor de sjabloon voor standard-mogelijkheden van parameters en variabelen. Ze ondersteunen ook het volgende op een hoger niveau om voor te bereiden:

-   **Taak factory**

    -   Maakt meerdere taken voor een taak van de definitie van een taak. Drie typen van taak factory worden ondersteund: parametrische zwaaihoek, taak per bestand, en de verzameling.

Hier volgt een voorbeeld van een sjabloon die u een taak aan video MP4-bestanden transcoderen met ffmpeg op een van twee lagere resoluties maakt. Hiermee maakt u één taak per video-bronbestand. Zie [groepen bestands- en bestandsoverdracht](#file-groups-and-file-transfer) voor meer informatie over bestandsgroepen voor taak invoer en uitvoer.

```json
{
    "parameters": {
        "poolId": {
            "type": "string",
            "metadata": {
                "description": "The name of Azure Batch pool which runs the job"
            }
        },
        "jobId": {
            "type": "string",
            "metadata": {
                "description": "The name of Azure Batch job"
            }
        },
        "resolution": {
            "type": "string",
            "defaultValue": "428x240",
            "allowedValues": [
                "428x240",
                "854x480"
            ],
            "metadata": {
                "description": "Target video resolution"
            }
        }
    },
    "job": {
        "type": "Microsoft.Batch/batchAccounts/jobs",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "[parameters('jobId')]",
            "constraints": {
                "maxWallClockTime": "PT5H",
                "maxTaskRetryCount": 1
            },
            "poolInfo": {
                "poolId": "[parameters('poolId')]"
            },
            "taskFactory": {
                "type": "taskPerFile",
                "source": { 
                    "fileGroup": "ffmpeg-input"
                },
                "repeatTask": {
                    "commandLine": "ffmpeg -i {fileName} -y -s [parameters('resolution')] -strict -2 {fileNameWithoutExtension}_[parameters('resolution')].mp4",
                    "resourceFiles": [
                        {
                            "blobSource": "{url}",
                            "filePath": "{fileName}"
                        }
                    ],
                    "outputFiles": [
                        {
                            "filePattern": "{fileNameWithoutExtension}_[parameters('resolution')].mp4",
                            "destination": {
                                "autoStorage": {
                                    "path": "{fileNameWithoutExtension}_[parameters('resolution')].mp4",
                                    "fileGroup": "ffmpeg-output"
                                }
                            },
                            "uploadOptions": {
                                "uploadCondition": "TaskSuccess"
                            }
                        }
                    ]
                }
            },
            "onAllTasksComplete": "terminatejob"
        }
    }
}
```

Als de naam van het sjabloonbestand _taak ffmpeg.json_, klikt u vervolgens de sjabloon als volgt starten:

```azurecli
az batch job create --template job-ffmpeg.json
```

Als vóór de CLI vraagt u waarden opgeven voor de parameters. U kunt ook de parameters in een JSON-bestand opgeven.

### <a name="use-templates-in-batch-explorer"></a>Sjablonen in de Batch Explorer gebruiken

U kunt een Batch-CLI-sjabloon voor het uploaden de [Batch Explorer](https://github.com/Azure/BatchExplorer) bureaubladtoepassing (voorheen aangeroepen BatchLabs) om een Batch-pool of de taak te maken. U kunt ook kiezen uit vooraf gedefinieerde sjablonen voor de pool en -taak in de Batch Explorer-galerie.

Een sjabloon uploaden:

1. Selecteer in Batch Explorer **galerie** > **lokale sjablonen**.

2. Selecteer, of slepen en neerzetten, een lokale groep of een sjabloon.

3. Selecteer **Gebruik deze sjabloon**, en volg de aanwijzingen op.

## <a name="file-groups-and-file-transfer"></a>Bestandsgroepen en bestandsoverdracht

De meeste jobs en taken vereisen invoerbestanden en uitvoerbestanden produceren. Normaal gesproken worden invoerbestanden en uitvoerbestanden overgedragen van de client naar het knooppunt of in het knooppunt naar de client. De Azure Batch CLI-extensie isoleert opslag bestandsoverdracht en maakt gebruik van het opslagaccount dat u aan elk Batch-account koppelen kunt.

Een groep is gelijk aan een container die in de Azure storage-account is gemaakt. De bestandsgroep mogelijk submappen.

De Batch-CLI-extensie bevat opdrachten voor het uploaden van bestanden van de client naar een opgegeven groep en bestanden uit de groep opgegeven bestand downloaden naar een client.

```azurecli
az batch file upload --local-path c:\source_videos\*.mp4 
    --file-group ffmpeg-input

az batch file download --file-group ffmpeg-output --local-path
    c:\output_lowres_videos
```

Groep van toepassingen en -taak met sjablonen kunt bestanden die zijn opgeslagen in de bestandsgroepen worden opgegeven voor het kopiëren naar de pool- of uitschakelen knooppunten in de pool naar een groep. Bijvoorbeeld, in de taak sjabloon eerder hebt opgegeven, de bestandsgroep *ffmpeg-invoer* is opgegeven voor de factory taak als de locatie van de video bronbestanden gekopieerd naar het knooppunt waar deze wordt omgezet. De bestandsgroep *ffmpeg-uitvoer* is de locatie waar de uitvoerbestanden transcoderen worden gekopieerd van het knooppunt elke taak uit te voeren.

## <a name="summary"></a>Overzicht

Ondersteuning voor het overbrengen sjabloon en de bestandsnaam op dit moment alleen voor de Azure CLI zijn toegevoegd. Het doel is om uit te breiden de doelgroep die u kunt Batch gebruiken om gebruikers die niet nodig hebben voor het ontwikkelen van code met behulp van de Batch-API's, zoals het wetenschappers en IT-gebruikers. Zonder te coderen, kunnen gebruikers met kennis van Azure, Batch en de toepassingen die moeten worden uitgevoerd door Batch sjablonen voor het maken van toepassingen en -taak maken. Gebruikers zonder uitgebreide kennis van de Batch- en de toepassingen en de sjabloonparameters kunnen de sjablonen gebruiken.

Probeer uit de Batch-extensie voor de Azure CLI en levert u geen feedback of suggesties, hetzij in de opmerkingen voor dit artikel of via de [Batch Community opslagplaats](https://github.com/Azure/Batch).

## <a name="next-steps"></a>Volgende stappen

- Gedetailleerde installatie en het gebruik documentatie, voorbeelden en broncode zijn beschikbaar in de [GitHub-opslagplaats Azure](https://github.com/Azure/azure-batch-cli-extensions).

- Meer informatie over het gebruik van [Batch Explorer](https://github.com/Azure/BatchExplorer) maken en beheren van Batch-resources.
