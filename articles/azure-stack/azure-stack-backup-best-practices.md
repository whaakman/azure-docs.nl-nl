---
title: Infrastructuur voor Backup-Service aanbevolen procedures voor Azure Stack | Microsoft Docs
description: Als u implementeert en Azure Stack in uw datacenter beheert om te beperken gegevens verloren gaan als er een onherstelbare fout, kunt u set met aanbevolen procedures volgen.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 221FDE40-3EF8-4F54-A075-0C4D66EECE1A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2019
ms.author: jeffgilb
ms.reviewer: hectorl
ms.lastreviewed: 02/08/2019
ms.openlocfilehash: d2568a4dfc4fefe9628fc63dcc0526b0876fde00
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55993874"
---
# <a name="infrastructure-backup-service-best-practices"></a>Aanbevolen procedures voor infrastructuur voor Backup-Service

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Als u implementeert en Azure Stack in uw datacenter beheert om te beperken verlies van gegevens in het geval van een onherstelbare fout, kunt u aanbevolen procedures volgen.

U moet de aanbevolen procedures bekijken met een regelmatig interval om te controleren of de installatie nog steeds in acht genomen wanneer wijzigingen worden aangebracht in de stroom van de bewerking. Moet u problemen tegenkomt tijdens het implementeren van deze aanbevolen procedures, neem contact op met Microsoft Support voor hulp.

## <a name="configuration-best-practices"></a>Aanbevolen procedures voor configuratie

### <a name="deployment"></a>Implementatie

Infrastructuur voor back-up inschakelen na de implementatie van elke Azure Stack-Cloud. U kunt back-ups van een client/server met toegang op de operator management API-eindpunt met behulp van PowerShell voor Azure Stack plannen.

### <a name="networking"></a>Netwerken

De Universal Naming Convention (UNC)-tekenreeks voor het pad moet een volledig gekwalificeerde domeinnaam (FQDN) gebruiken. IP-adres is mogelijk als naamomzetting niet mogelijk is. Een UNC-tekenreeks geeft de locatie van bronnen zoals gedeelde bestanden of apparaten.

### <a name="encryption"></a>Versleuteling

#### <a name="version-1901-and-newer"></a>Versie 1901 en nieuwer

Het versleutelingscertificaat dat wordt gebruikt voor het versleutelen van back-upgegevens die wordt geëxporteerd naar een externe opslag. Het certificaat kan een zelfondertekend certificaat zijn, omdat het certificaat alleen gebruikt wordt voor het transport van sleutels. New-SelfSignedCertificate Raadpleeg voor meer informatie over het maken van een certificaat.  
De sleutel moet worden opgeslagen op een veilige locatie (bijvoorbeeld algemene Azure Key Vault-certificaat). Het CER-indeling van het certificaat wordt gebruikt om gegevens te versleutelen. Het PFX-indeling moet worden gebruikt tijdens de implementatie van de recovery cloud van Azure Stack voor het ontsleutelen van back-upgegevens.

![Het certificaat op een veilige locatie opgeslagen.](media/azure-stack-backup/azure-stack-backup-encryption-store-cert.png)

#### <a name="1811-and-older"></a>1811 en ouder

De versleutelingssleutel wordt gebruikt voor het versleutelen van back-upgegevens die wordt geëxporteerd naar een externe opslag. De sleutel wordt gegenereerd als onderdeel van [back-up inschakelen voor Azure Stack met PowerShell](azure-stack-backup-enable-backup-powershell.md).

De sleutel moet worden opgeslagen op een veilige locatie (bijvoorbeeld algemene Azure Key Vault-geheim). Deze sleutel moet worden gebruikt tijdens het opnieuw implementeren van Azure Stack. 

![De sleutel een veilige locatie opgeslagen.](media/azure-stack-backup/azure-stack-backup-encryption2.png)

## <a name="operational-best-practices"></a>Aanbevolen operationele procedures

### <a name="backups"></a>Back-ups

 - Back-uptaken uitgevoerd terwijl het systeem wordt uitgevoerd, zodat er geen downtime naar de ervaringen voor of toepassingen is. Verwacht dat de back-uptaken 20-40 minuten voor een oplossing die is redelijk belast.
 - Met behulp van OEM-instructies, moeten handmatig back-netwerkswitches en de hardware-lifecycle-host (HLH) worden opgeslagen op de dezelfde back-upshare waar de infrastructuur voor back-Controller winkels besturingselement vlak voor back-upgegevens. Houd rekening met opslaan van de switch en HLH configuraties in de regio-map. Als u meerdere exemplaren van Azure Stack in dezelfde regio hebt, kunt u overwegen een id voor elke configuratie die deel uitmaakt van een schaaleenheid.

### <a name="folder-names"></a>Namen van mappen

 - MASBACKUP map wordt automatisch gemaakt door de infrastructuur. Dit is een beheerd door Microsoft share. U kunt bestandsshares maken op hetzelfde niveau als MASBACKUP. Het is niet raadzaam om het maken van mappen of opslag van gegevens binnen MASBACKUP die Azure Stack geen maakt. 
 -  Gebruiker FQDN-naam en -regio's in de mapnaam van uw te onderscheiden van de back-upgegevens van verschillende clouds. De volledig gekwalificeerde domeinnaam (FQDN) van uw Azure Stack-implementatie en eindpunten is de combinatie van de parameters regio en het externe domeinnaam. Zie voor meer informatie, [integratie van Azure Stack datacenter - DNS-](azure-stack-integrate-dns.md).

Bijvoorbeeld, is de back-upshare die worden gehost op fileserver01.contoso.com AzSBackups. In deze bestandsshare worden een map per Azure Stack-implementatie met behulp van de naam van het externe domein en een submap die gebruikmaakt van de regionaam van de. 

FQDN-naam: contoso.com  
Regio: nyc


    \\fileserver01.contoso.com\AzSBackups
    \\fileserver01.contoso.com\AzSBackups\contoso.com
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\MASBackup

MASBackup map is waar de back-upgegevens worden opgeslagen in Azure Stack. U moet deze map niet gebruiken voor het opslaan van uw eigen gegevens. OEM moet deze map niet gebruiken voor het opslaan van een back-upgegevens ofwel. 

OEM's wordt aangeraden voor het opslaan van back-upgegevens van de bijbehorende onderdelen onder de map regio. Elke netwerkswitches, host van de levenscyclus van hardware (HLH), enzovoort, kunnen worden opgeslagen in een eigen submap. Bijvoorbeeld:

    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\HLH
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\Switches
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\DeploymentData
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\Registration

### <a name="monitoring"></a>Bewaking

De volgende waarschuwingen worden ondersteund door het systeem:

| Waarschuwing                                                   | Description                                                                                     | Herstel                                                                                                                                |
|---------------------------------------------------------|-------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Back-up is mislukt omdat de bestandsshare beperkt is | De bestandsshare is beperkt en back-controller kan back-upbestanden naar de locatie niet exporteren. | Voeg meer opslagcapaciteit toe en probeer opnieuw een back-up. Bestaande back-ups (vanaf van de oudste eerst) verwijderen om ruimte vrij te maken.                    |
| Back-up is mislukt vanwege problemen met de netwerkverbinding.             | Netwerk tussen de Azure Stack en de share problemen ondervindt.                          | De netwerkprobleem op te lossen en probeer het back-up opnieuw.                                                                                            |
| Back-up is mislukt vanwege een fout in het pad                | Het pad naar bestandsshare kan niet worden omgezet                                                          | De share op een andere computer om te controleren of dat de share toegankelijk is toegewezen. Mogelijk moet u het pad bijwerken als deze niet meer geldig is.       |
| Back-up is mislukt vanwege een verificatieprobleem               | Er zijn mogelijk een probleem met de referenties of een netwerkprobleem gevolgen heeft voor verificatie.    | De share op een andere computer om te controleren of dat de share toegankelijk is toegewezen. Mogelijk moet u referenties bijwerken als ze niet langer geldig zijn. |
| Back-up is mislukt vanwege een algemene fout                    | De mislukte aanvraag kan worden veroorzaakt door een tijdelijk probleem. Probeer het opnieuw een back-up.                    | Contact opnemen met ondersteuning                                                                                                                               |

## <a name="next-steps"></a>Volgende stappen

Bekijk het referentiemateriaal voor de [infrastructuur Backup-Service](azure-stack-backup-reference.md)

Schakel de [infrastructuur Backup-Service](azure-stack-backup-enable-backup-console.md)
