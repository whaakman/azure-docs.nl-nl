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
ms.openlocfilehash: e55058d6b1f76b4afcb847b946df85d5ab69971b
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55985402"
---
# <a name="enabling-azure-ultra-ssds"></a>Enabling Azure Ultra SSDs

Azure Ultra SSD biedt hoge doorvoer, een hoge IOPS en consistente lage latentie disk-opslag voor Azure IaaS VM's. Deze nieuwe aanbieding biedt boven aan de prestaties van de regel op het niveau van de dezelfde beschikbaarheid als onze bestaande schijven-aanbiedingen. Extra voordelen van Ultra SSD omvatten de mogelijkheid om de prestaties van de schijf, samen met uw workloads zonder dat uw virtuele machines opnieuw moet worden dynamisch te wijzigen. Ultra SSD is geschikt voor gegevensintensieve workloads, zoals SAP HANA, databases van de bovenste laag en transactie-zware workloads.

Op dit moment Ultra SSD's zijn beschikbaar als preview en moet u [inschrijven](https://aka.ms/UltraSSDPreviewSignUp) in de Preview-versie om ze toegang te krijgen.

Na goedkeuring, voert u een van de volgende opdrachten om te bepalen welke zone in VS-Oost 2 uw Ultra SSD te implementeren:

PowerShell: `Get-AzComputeResourceSku | where {$_.ResourceType -eq "disks" -and $_.Name -eq "UltraSSD_LRS" }`

CLI: `az vm list-skus --resource-type disks --query “[?name==’UltraSSD_LRS’]”`

Het antwoord is vergelijkbaar met het onderstaande formulier, waarbij X staat voor de Zone moet worden gebruikt voor het implementeren van in VS-Oost 2. X kan 1, 2 of 3 zijn.

|ResourceType  |Name  |Locatie  |Zones  |Beperking  |Mogelijkheid  |Value  |
|---------|---------|---------|---------|---------|---------|---------|
|Schijven     |UltraSSD_LRS         |eastus2         |X         |         |         |         |

Als er geen reactie van de opdracht is, betekent dit dat uw registratie voor de functie is nog steeds in behandeling of niet goedgekeurd nog.

Nu dat u weet welke zone om in te implementeren, volgt u de implementatiestappen in dit artikel om op te halen van uw eerste virtuele machines geïmplementeerd met Ultra SSD-schijven.

## <a name="deploying-an-ultra-ssd"></a>Implementatie van een Ultra SSD

Bepaal eerst de VM-grootte te implementeren. Als onderdeel van deze Preview-versie, worden alleen DsV3 en de EsV3-VM-families ondersteund. Raadpleeg de tweede tabel op deze [blog](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/) voor meer informatie over deze VM-grootten.
Ook verwijzen naar het voorbeeld [een virtuele machine maken met meerdere Ultra SSD-schijven](https://aka.ms/UltraSSDTemplate), laat zien hoe u een virtuele machine maken met meerdere Ultra SSD-schijven.

Hieronder vindt u informatie nieuwe/gewijzigd Resource Manager wijzigingen in de sjabloon: **apiVersion** voor `Microsoft.Compute/virtualMachines` en `Microsoft.Compute/Disks` moet worden ingesteld als `2018-06-01` (of hoger).

Schijf Sku UltraSSD_LRS, schijfcapaciteit, IOPS en doorvoer in MBps die moet maken van een Ultra SSD-schijf opgeven. Hieronder volgt een voorbeeld waarin maakt u een schijf met 1024 GiB (GiB = 2 ^ 30 Bytes), maar liefst 80.000 IOPS en 1200 MBps (MBps = 10 ^ 6 Bytes per seconde):

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

Een aanvullende mogelijkheden toevoegen aan de eigenschappen van de virtuele machine om aan te geven de Ultra SSD ingeschakeld (Raadpleeg de [voorbeeld](https://aka.ms/UltraSSDTemplate) voor de volledige Resource Manager-sjabloon):

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

## <a name="additional-ultra-ssd-scenarios"></a>Aanvullende Ultra SSD-scenario 's

- Tijdens het maken van virtuele machine kunnen Ultra SSD's worden impliciet gemaakt ook. Deze schijven ontvangt echter standaard de waarde voor IOPS (500) en doorvoer (8 MiB/s).
- Aanvullende Ultra SSD's kan worden gekoppeld aan Ultra SSD compatibele virtuele machines.
- Ultra SSD biedt ondersteuning voor aanpassen van de kenmerken van de prestaties van schijf (IOPS en doorvoer) tijdens runtime zonder dat de schijf loskoppelen van de virtuele machine. Zodra een schijfbewerking voor de grootte van prestaties is verleend op een schijf, het kan een uur duren voor de wijziging daadwerkelijk pas van kracht.
- Groei van de schijfcapaciteit is vereist voor een virtuele machine ongedaan worden gemaakt.

## <a name="next-steps"></a>Volgende stappen

Als u wilt het proberen van het nieuwe schijftype en nog niet hebt aangemeld voor de Preview-versie nog [aanvragen van toegang via deze enquête](https://aka.ms/UltraSSDPreviewSignUp).
