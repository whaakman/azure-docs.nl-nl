---
title: Naslaginformatie over Azure Backup-Service van Stack infrastructuur | Microsoft Docs
description: Dit artikel bevat referentiemateriaal voor de back-up-Service van Azure Stack-infrastructuur.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: D6EC0224-97EA-446C-BC95-A3D32F668E2C
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: 4e6e0a52b2c55239e38757223f54e5e94dc98c42
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="infrastructure-backup-service-reference"></a>Verwijzing naar de back-up-Service van de infrastructuur

## <a name="azure-backup-infrastructure"></a>Azure-back-infrastructuur

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Azure Stack bestaat uit veel services die bestaan uit de portal, Azure Resource Manager en beheer van de infrastructuur optreden. De apparaat-achtige beheerervaring van Azure-Stack is gericht op het verminderen van de complexiteit blootgesteld aan de operator van de oplossing.

Infrastructuur back-up is ontworpen om eigen de complexiteit van een back-up maken en herstellen van gegevens voor infrastructuurservices gezorgd operators kan zich concentreren op de oplossing beheren en onderhouden van een SLA voor gebruikers.

Exporteren van de back-upgegevens naar een externe share is vereist om te voorkomen dat opslaan van back-ups op hetzelfde systeem. Vereisen van een externe share, biedt de beheerder de flexibiliteit om te bepalen waar de gegevens op basis van bestaande BC-/ DR bedrijfsbeleid wordt opgeslagen. 

### <a name="infrastructure-backup-components"></a>Onderdelen van de back-up-infrastructuur

Back-up van de infrastructuur omvat de volgende onderdelen:

 - **Infrastructuur voor back-Controller**  
 De Controller infrastructuur-back-up is gemaakt met en bevindt zich op elke Azure-Cloud-Stack.
 - **Back-Resourceprovider**  
 De back-Resourceprovider (back-up RP) bestaat uit de gebruiker interface- en programma-interfaces (API) s blootstellen van back-up basisfunctionaliteit voor Azure-Stack-infrastructuur.

#### <a name="infrastructure-backup-controller"></a>Infrastructuur voor back-Controller

De infrastructuur back-up-Controller is een Service Fabric service opgehaald geïnstantieerd voor een Azure-Cloud-Stack. Back-resources worden gemaakt in een regionale niveau en vastleggen regiospecifieke service van AD, CA, Azure Resource Manager, CRP, SRP, NRP, KeyVault, RBAC. 

### <a name="backup-resource-provider"></a>Back-Resourceprovider

De back-Resourceprovider geeft de gebruikersinterface in de Stack van Azure-portal voor basisconfiguratie en overzicht van back-resources. Operator kunt u de volgende bewerkingen uitvoeren in de gebruikersinterface:

 - Back-up voor het eerst door te geven locatie voor de externe opslag, referenties en versleutelingssleutel inschakelen
 - Weergave voltooid gemaakt back-bronnen en bronnen van de status onder maken
 - Wijzigen van de opslaglocatie waar back-Controller back-upgegevens geplaatst
 - De referenties die back-up Controller gebruikt voor toegang tot externe opslaglocatie wijzigen
 - De versleutelingssleutel die back-up Controller gebruikt voor het versleutelen van back-ups wijzigen 


## <a name="backup-controller-requirements"></a>Vereisten voor de back-domeincontroller

Deze sectie beschrijft de belangrijke vereisten voor infrastructuur back-up. Het is raadzaam dat u de informatie voordat u back-up voor uw Azure-Stack-exemplaar inschakelen en vervolgens naar deze zo nodig tijdens de implementatie en de volgende bewerking terugverwijzen zorgvuldig te controleren.

De vereisten zijn:

  - **Softwarevereisten** – beschrijft ondersteunde opslaglocaties en Richtlijn. 
  - **Vereisten voor de** – netwerkvereisten voor de verschillende opslaglocaties beschrijft.  

### <a name="software-requirements"></a>Softwarevereisten

#### <a name="supported-storage-locations"></a>Ondersteunde opslaglocaties

| Opslaglocatie                                                                 | Details                                                                                                                                                  |
|----------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| SMB-bestandsshare die wordt gehost op een opslagapparaat binnen de omgeving vertrouwd netwerk | SMB-share in hetzelfde datacenter waarop Azure Stack wordt geïmplementeerd of in een ander datacenter. Meerdere exemplaren van de Azure-Stack kunnen dezelfde bestandsshare gebruiken. |
| SMB-bestandsshare op Azure                                                          | Momenteel niet ondersteund.                                                                                                                                 |
| BLOB-opslag in Azure                                                            | Momenteel niet ondersteund.                                                                                                                                 |

#### <a name="supported-smb-versions"></a>Ondersteunde SMB-versies

| SMB | Versie |
|-----|---------|
| SMB | 3.x     |

#### <a name="storage-location-sizing"></a>Formaat van de locatie van opslag 

Infrastructuur voor back-up-Controller wordt back-up gegevens op verzoek. De aanbeveling is back-up ten minste twee keer een dag en het bewaren van back-ups van maximaal zeven dagen. 

| Schaal van de omgeving | Geschatte grootte van de back-up | Totale hoeveelheid vereiste schijfruimte |
|-------------------|--------------------------|--------------------------------|
| 4-12-knooppunten        | 10 GB                     | 140 GB                          |

### <a name="network-requirements"></a>Netwerkvereisten
| Opslaglocatie                                                                 | Details                                                                                                                                                                                 |
|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SMB-bestandsshare die wordt gehost op een opslagapparaat binnen de omgeving vertrouwd netwerk | Poort 445 is vereist als het exemplaar van Azure-Stack bevindt zich in een omgeving met firewall. Back-up infrastructuurbeheerder initieert een verbinding met de SMB-bestandsserver via poort 445. |
| Voor het gebruik van de FQDN-naam van de bestandsserver moet de naam worden omgezet vanaf de PEP             |                                                                                                                                                                                         |

> [!Note]  
> Er zijn geen poorten voor inkomend verkeer moeten worden geopend.


## <a name="infrastructure-backup-limits"></a>Back-limieten infrastructuur

U kunt deze limieten zoals plannen, implementeren en uw Microsoft Azure-Stack-exemplaren werkt. De volgende tabel beschrijft deze limieten.

### <a name="infrastructure-backup-limits"></a>Limieten voor infrastructuur-back-up
| Limiet-id                                                 | Limiet        | Opmerkingen                                                                                                                                    |
|------------------------------------------------------------------|--------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Type back-up                                                      | Alleen volledige    | Infrastructuur voor back-up-Controller ondersteunt alleen volledige back-ups. Incrementele back-ups worden niet ondersteund.                                          |
| Geplande back-ups                                                | Alleen handmatig  | Back-controller ondersteunt momenteel alleen back-ups op aanvraag                                                                                 |
| Maximum aantal gelijktijdige back-uptaken                                   | 1            | Slechts één actieve back-uptaak wordt per exemplaar van de back-Controller ondersteund.                                                                  |
| Netwerkconfiguratie van de switch                                     | Niet binnen het bereik | De beheerder moet back-up switch netwerkconfiguratie met de OEM-hulpprogramma's. Raadpleeg de documentatie voor Azure-Stack geleverd door de leverancier van elke OEM. |
| Hardware Lifecycle Host                                          | Niet binnen het bereik | De beheerder moet back-up Hardware Lifecycle-Host op met OEM-hulpprogramma's. Raadpleeg de documentatie voor Azure-Stack geleverd door de leverancier van elke OEM.      |
| Maximum aantal bestandsshares                                    | 1            | Slechts één bestandsshare kan worden gebruikt voor het opslaan van back-upgegevens                                                                                        |
| Back-App-Services, functie, SQL, mysql resource provider-gegevens | Niet binnen het bereik | Raadpleeg de richtlijnen voor het implementeren en beheren van toegevoegde waarde van-RPs gemaakt door Microsoft gepubliceerd.                                                  |
| Resourceproviders voor back-up van derden                              | Niet binnen het bereik | Raadpleeg de richtlijnen voor het implementeren en beheren van toegevoegde waarde van-RPs gemaakt door externe leveranciers gepubliceerd.                                          |

## <a name="next-steps"></a>Volgende stappen

 - Zie voor meer informatie over de infrastructuur Backup-Service, [back-up en gegevensherstel voor Azure-Stack met de infrastructuur Backup-Service](azure-stack-backup-infrastructure-backup.md).