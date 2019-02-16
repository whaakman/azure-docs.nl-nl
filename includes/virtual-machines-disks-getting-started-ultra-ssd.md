---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/24/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 0369a7792f0d9c97aa3d943708dfcc07228effa2
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56330929"
---
# <a name="enabling-azure-ultra-disks"></a>Inschakelen van Azure ultra-schijven

Azure ultra-schijven bieden hoge doorvoer, hoge IOPS en consistente lage latentie schijfopslag voor Azure IaaS VM's. Deze nieuwe aanbieding biedt boven aan de prestaties van de regel op het niveau van de dezelfde beschikbaarheid als onze bestaande schijven-aanbiedingen. Extra voordelen van ultra schijven zijn de mogelijkheid om de prestaties van de schijf, samen met uw workloads zonder dat uw virtuele machines opnieuw moet worden dynamisch te wijzigen. Ultra-schijven zijn geschikt voor gegevensintensieve workloads, zoals SAP HANA, databases van de bovenste laag en transactie-zware workloads.

Op dit moment ultra schijven zijn in preview en moet u [inschrijven](https://aka.ms/UltraSSDPreviewSignUp) in de Preview-versie om ze toegang te krijgen.

Na goedkeuring, voert u een van de volgende opdrachten om te bepalen welke zone in VS-Oost 2 de ultra schijf om te implementeren:

PowerShell: `Get-AzComputeResourceSku | where {$_.ResourceType -eq "disks" -and $_.Name -eq "UltraSSD_LRS" }`

CLI: `az vm list-skus --resource-type disks --query “[?name==’UltraSSD_LRS’]”`

Het antwoord is vergelijkbaar met het onderstaande formulier, waarbij X staat voor de Zone moet worden gebruikt voor het implementeren van in VS-Oost 2. X kan 1, 2 of 3 zijn.

|ResourceType  |Name  |Locatie  |Zones  |Beperking  |Mogelijkheid  |Value  |
|---------|---------|---------|---------|---------|---------|---------|
|schijven     |UltraSSD_LRS         |eastus2         |X         |         |         |         |

Als er geen reactie van de opdracht is, betekent dit dat uw registratie voor de functie is nog steeds in behandeling of niet goedgekeurd nog.

Nu dat u weet welke zone om in te implementeren, volgt u de implementatiestappen in dit artikel om op te halen van uw eerste virtuele machines geïmplementeerd met ultra schijven.

## <a name="deploying-an-ultra-disk"></a>Implementatie van een ultra-schijf

Bepaal eerst de VM-grootte te implementeren. Als onderdeel van deze Preview-versie, worden alleen DsV3 en de EsV3-VM-families ondersteund. Raadpleeg de tweede tabel op deze [blog](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/) voor meer informatie over deze VM-grootten.
Ook verwijzen naar het voorbeeld [een virtuele machine maken met meerdere ultra schijven](https://aka.ms/UltraSSDTemplate), laat zien hoe u een virtuele machine maken met meerdere ultra schijven.

Hieronder vindt u informatie nieuwe/gewijzigd Resource Manager wijzigingen in de sjabloon: **apiVersion** voor `Microsoft.Compute/virtualMachines` en `Microsoft.Compute/Disks` moet worden ingesteld als `2018-06-01` (of hoger).

Schijf Sku UltraSSD_LRS, schijfcapaciteit, IOPS en doorvoer in MBps die moet maken van een ultra-schijf opgeven. Hieronder volgt een voorbeeld waarin maakt u een schijf met 1024 GiB (GiB = 2 ^ 30 Bytes), maar liefst 80.000 IOPS en 1200 MBps (MBps = 10 ^ 6 Bytes per seconde):

```json
"properties": {  
    "creationData": {  
    "createOption": "Empty"  
},  
"diskSizeGB": 1024,  
"diskIOPSReadWrite": 80000,  
"diskMBpsReadWrite": 1200,  
}
```

Een aanvullende mogelijkheden toevoegen aan de eigenschappen van de virtuele machine om aan te geven van de ultra ingeschakeld (Raadpleeg de [voorbeeld](https://aka.ms/UltraSSDTemplate) voor de volledige Resource Manager-sjabloon):

```json
{
    "apiVersion": "2018-06-01",
    "type": "Microsoft.Compute/virtualMachines",
    "properties": {
                    "hardwareProfile": {},
                    "additionalCapabilities" : {
                                    "ultraSSDEnabled" : "true"
                    },
                    "osProfile": {},
                    "storageProfile": {},
                    "networkProfile": {}
    }
}
```

Zodra de virtuele machine is ingericht, kunt u partitioneren en formatteren van de gegevensschijven en configureert deze voor uw workloads.

## <a name="additional-ultra-disk-scenarios"></a>Scenario's voor extra ultra-schijf

- Tijdens het maken van virtuele machine ultra schijven kunnen worden impliciet gemaakt ook. Deze schijven ontvangt echter standaard de waarde voor IOPS (500) en doorvoer (8 MiB/s).
- Extra ultra schijven kunnen worden gekoppeld aan compatibele virtuele machines.
- Ultra schijven ondersteuning voor aanpassen van de kenmerken van de prestaties van schijf (IOPS en doorvoer) tijdens runtime zonder dat de schijf loskoppelen van de virtuele machine. Zodra een schijfbewerking voor de grootte van prestaties is verleend op een schijf, het kan een uur duren voor de wijziging daadwerkelijk pas van kracht.
- Groei van de schijfcapaciteit is vereist voor een virtuele machine ongedaan worden gemaakt.

## <a name="next-steps"></a>Volgende stappen

Als u wilt het proberen van het nieuwe schijftype en nog niet hebt aangemeld voor de Preview-versie nog [aanvragen van toegang via deze enquête](https://aka.ms/UltraSSDPreviewSignUp).
