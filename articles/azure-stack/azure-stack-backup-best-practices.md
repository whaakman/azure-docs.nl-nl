---
title: Infrastructuur Backup-Service aanbevolen procedures voor het Azure-Stack | Microsoft Docs
description: Wanneer u implementeren en beheren van Azure-Stack in uw datacenter te verminderen gegevens verloren gaan als er een onherstelbare fout kunt u de set met aanbevolen procedures volgen.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 221FDE40-3EF8-4F54-A075-0C4D66EECE1A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/27/2017
ms.author: mabrigg
ms.openlocfilehash: b9438f3bab92f40a5c79ce7b7a572195c182be45
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="infrastructure-backup-service-best-practices"></a>Aanbevolen procedures voor infrastructuur-Backup-Service

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Wanneer u implementeren en beheren van Azure-Stack in uw datacenter om te beperken van gegevensverlies in geval van een onherstelbare fout kunt u de aanbevolen procedures volgen.

U wordt aangeraden de aanbevolen procedures met een regelmatig interval om te controleren of de installatie nog steeds in acht genomen wanneer wijzigingen worden aangebracht in de stroom van de bewerking. Moet er problemen optreden tijdens de uitvoering van deze best practices, neem contact op met Microsoft Support voor hulp.

## <a name="configuration-best-practices"></a>Aanbevolen procedures voor configuratie

### <a name="deployment"></a>Implementatie

Infrastructuur voor back-up inschakelen na implementatie van elke Azure-Cloud-Stack. Gebruik AzureStack-hulpprogramma's die u kunt back-ups plannen vanaf elke client/server met toegang tot het eindpunt van de operator management-API.

### <a name="networking"></a>Netwerken

De tekenreeks Universal Naming Convention (UNC) voor het pad moet een volledig gekwalificeerde domeinnaam (FQDN) gebruiken. IP-adres is mogelijk als naamomzetting niet mogelijk is. Een UNC-tekenreeks bevat de locatie van resources, zoals gedeelde bestanden of apparaten.

### <a name="encryption"></a>Versleuteling

De versleutelingssleutel wordt gebruikt voor het versleutelen van back-upgegevens die wordt geëxporteerd naar een externe opslag. De sleutel kan worden gegenereerd met AzureStack-hulpprogramma's. 

![AzureStack-hulpprogramma 's](media\azure-stack-backup\azure-stack-backup-encryption1.png)

De sleutel moet worden opgeslagen op een veilige locatie (bijvoorbeeld: openbare Azure Key Vault geheim). Deze sleutel moet worden gebruikt tijdens het opnieuw distribueren van Azure-Stack. 

![De sleutel op een veilige locatie opgeslagen.](media\azure-stack-backup\azure-stack-backup-encryption2.png)

## <a name="operational-best-practices"></a>Aanbevolen operationele procedures

### <a name="backups"></a>Back-ups

 - Infrastructuurbeheerder back-up moet worden geactiveerd op verzoek. U wordt aangeraden aan back-up ten minste twee keer per dag.
 - Back-uptaken uitgevoerd terwijl het systeem wordt uitgevoerd, zodat er geen uitvaltijd voor de ervaringen of de gebruikerstoepassingen. Verwacht dat de back-uptaken op 20-40 minuten duren voordat een oplossing die redelijkerwijs wordt belast.
 - Met de OEM opgegeven instructie, moeten handmatig back-netwerkswitches en de hardware-lifecycle-host (HLH) worden opgeslagen op dezelfde back-share waarop de back-Controller infrastructuur winkels besturingselement vlak voor back-upgegevens. U kunt de switch en HLH configuraties opslaan in de map regio. Als u meerdere exemplaren van de Azure-Stack in dezelfde regio hebt, kunt u overwegen een id voor elke configuratie die deel uitmaakt van een schaaleenheid.

### <a name="folder-names"></a>Mapnamen

 - MASBACKUP map wordt automatisch gemaakt door de infrastructuur. Dit is een share door Microsoft beheerd. U kunt shares op hetzelfde niveau als MASBACKUP maken. Het is niet raadzaam maken van mappen of van opslaggegevens binnen MASBACKUP die geen Azure-Stack maakt. 
 -  Gebruiker FQDN en regio in de mapnaam te onderscheiden van de back-upgegevens uit andere clouds. De volledig gekwalificeerde domeinnaam (FQDN) van uw Azure-Stack-implementatie en de eindpunten is de combinatie van de parameter regio en de externe Domain Name-parameter. Zie voor meer informatie [Stack Azure datacenter integratie - DNS-](azure-stack-integrate-dns.md).

Bijvoorbeeld, is de back-upshare AzSBackups gehost op fileserver01.contoso.com. In deze bestandsshare worden een map per Azure-Stack-implementatie met behulp van de externe domeinnaam en een submap met de regionaam. 

FQDN-naam: contoso.com  
Regio: nyc


    \\fileserver01.contoso.com\AzSBackups
    \\fileserver01.contoso.com\AzSBackups\contoso.com
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\MASBackup

MASBackup map is waar de back-upgegevens worden opgeslagen in Azure-Stack. U moet deze map niet gebruiken voor het opslaan van uw eigen gegevens. OEM moet deze map niet gebruiken voor het opslaan van een back-upgegevens ofwel. 

Is het raadzaam voor het opslaan van back-upgegevens voor hun onderdelen onder de map regio. Elke netwerkswitches host van de levenscyclus van hardware (HLH), en kunnen worden opgeslagen in een eigen submap. Bijvoorbeeld:

    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\HLH
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\Switches
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\DeploymentData
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\Registration

### <a name="monitoring"></a>Bewaking

De volgende waarschuwingen worden ondersteund door het systeem:

| Waarschuwing                                                   | Beschrijving                                                                                     | Herstel                                                                                                                                |
|---------------------------------------------------------|-------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Back-up is mislukt, omdat de bestandsshare capaciteit | Bestandsshare heeft onvoldoende capaciteit en back-controller kan back-upbestanden naar de locatie niet exporteren. | Voeg meer opslagcapaciteit toe en probeer opnieuw een back-up. Verwijder de bestaande back-ups (vanaf oudste eerst) om ruimte vrij.                    |
| Back-up is mislukt vanwege problemen met de netwerkconnectiviteit.             | Netwerk tussen Azure-Stack en het bestand share problemen ondervindt.                          | Los het netwerkprobleem en probeer het back-up opnieuw.                                                                                            |
| Back-up is mislukt vanwege een fout in het pad                | Het pad naar bestandsshare kan niet worden omgezet                                                          | Wijs de share vanaf een andere computer om te controleren of dat de share is toegankelijk. Mogelijk moet u het pad bijwerken als deze niet langer geldig.       |
| Back-up is mislukt vanwege een verificatiefout               | Er zijn mogelijk een probleem met de referenties of een netwerkprobleem die van invloed is op verificatie.    | Wijs de share vanaf een andere computer om te controleren of dat de share is toegankelijk. Mogelijk moet u referenties bijwerken als ze niet langer geldig zijn. |
| Back-up is mislukt vanwege een algemene fout                    | De mislukte aanvragen kan worden veroorzaakt door een tijdelijk probleem. Probeer opnieuw een back-up.                    | oproep-ondersteuning                                                                                                                               |

## <a name="next-steps"></a>Volgende stappen

 - Raadpleeg het referentiemateriaal voor de [infrastructuur Backup-Service](azure-stack-backup-reference.md).  
 - Schakel de [infrastructuur Backup-Service](azure-stack-backup-enable-backup-console.md).