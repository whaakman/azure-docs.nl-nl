---
title: Naslaginformatie over Azure Stack-infrastructuur Backup-Service | Microsoft Docs
description: In dit artikel bevat referentiemateriaal dat beschikbaar is voor de Backup-Service van Azure Stack-infrastructuur.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: D6EC0224-97EA-446C-BC95-A3D32F668E2C
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: hectorl
ms.lastreviewed: 10/25/2018
ms.openlocfilehash: a7930ea86f7972a6e4abb939fb148d519ca924e9
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56416714"
---
# <a name="infrastructure-backup-service-reference"></a>Documentatie over Backup-Service-infrastructuur

## <a name="azure-backup-infrastructure"></a>Azure-back-upinfrastructuur

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Azure Stack bestaat uit vele services die deel uitmaken van de portal, Azure Resource Manager en beheer van infrastructuur optreden. De apparaat-achtige beheerervaring van Azure Stack is gericht op het verkorten van de complexiteit blootgesteld aan de beheerder van de oplossing.

Infrastructuur voor back-up is ontworpen om de complexiteit van de back-ups van eigen maken en herstellen van gegevens voor infrastructuurservices, ervoor te zorgen dat operators zich kunt richten op de oplossing voor beheer en onderhoud van een SLA voor gebruikers.

Exporteren van de back-upgegevens naar een externe bestandsshare is vereist om te voorkomen dat opslaan van back-ups op hetzelfde systeem. Vereisen van een externe bestandsshare, biedt de beheerder de flexibiliteit om te bepalen waar de gegevens op basis van bestaande bedrijf BC-/ DR-beleid wordt opgeslagen. 

### <a name="infrastructure-backup-components"></a>Back-up infrastructuuronderdelen

Back-up van de infrastructuur omvat de volgende onderdelen:

 - **Infrastructuur voor back-Controller**  
 De Controller van de back-up-infrastructuur met wordt geïnstantieerd en bevindt zich in elke Azure Stack-Cloud.
 - **Back-Resourceprovider**  
 De back-Resourceprovider (back-RP) bestaat uit de gebruiker-interface en toepassing program interfaces (API) s die basisfunctionaliteit voor back-up voor Azure Stack-infrastructuur.

#### <a name="infrastructure-backup-controller"></a>Infrastructuur voor back-Controller

De infrastructuur voor back-up-Controller is een Service Fabric service wordt gestart voor een Azure Stack-Cloud. Back-resources worden gemaakt op een regionaal niveau en vastleggen regiospecifiek-servicegegevens van AD, CA, Azure Resource Manager, CRP, SRP, NRP, KeyVault, RBAC. 

### <a name="backup-resource-provider"></a>Back-Resourceprovider

De back-Resourceprovider geeft de gebruikersinterface in de Azure Stack-portal voor de basisconfiguratie en lijst met back-resources. Operator kunt u de volgende bewerkingen uitvoeren in de gebruikersinterface:

 - Back-up inschakelen voor de eerste keer door te geven locatie voor de externe opslag, referenties en versleutelingssleutel
 - Weergave voltooid gemaakt back-bronnen en bronnen van de status wordt gemaakt
 - Wijzigen van de opslaglocatie waar de back-Controller back-upgegevens geplaatst
 - De referenties die back-up Controller gebruikt voor toegang tot externe opslaglocatie wijzigen
 - De versleutelingssleutel die back-up Controller gebruikt voor het versleutelen van back-ups wijzigen 


## <a name="backup-controller-requirements"></a>Vereisten voor de back-domeincontroller

Deze sectie beschrijft de belangrijke vereisten voor infrastructuur voor back-up. Het is raadzaam dat u de informatie voordat u back-up voor uw exemplaar van Azure Stack inschakelen, en vervolgens terug naar deze zo nodig tijdens de implementatie en latere bewerking verwijzen zorgvuldig te controleren.

De vereisten zijn onder andere:

  - **Softwarevereisten** – worden ondersteunde opslaglocaties en Richtlijn voor formaatbepaling beschreven. 
  - **Vereisten voor de** – netwerkvereisten voor verschillende opslaglocaties beschrijft.  

### <a name="software-requirements"></a>Softwarevereisten

#### <a name="supported-storage-locations"></a>Ondersteunde opslaglocaties

| Opslaglocatie                                                                 | Details                                                                                                                                                  |
|----------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| SMB-bestandsshare die wordt gehost op een opslagapparaat binnen de omgeving vertrouwd netwerk | SMB-share in hetzelfde datacenter waar Azure Stack is geïmplementeerd of in een ander datacenter. Meerdere exemplaren van Azure Stack kunnen gebruiken dezelfde bestandsshare. |
| SMB-bestandsshare op Azure                                                          | Momenteel niet ondersteund.                                                                                                                                 |
| Azure BLOB-opslag                                                            | Momenteel niet ondersteund.                                                                                                                                 |

#### <a name="supported-smb-versions"></a>Ondersteunde versies van SMB

| SMB | Versie |
|-----|---------|
| SMB | 3.x     |

#### <a name="storage-location-sizing"></a>Grootte van opslag-locatie 

Infrastructuur voor back-up Controller worden back-up van gegevens op aanvraag. De aanbeveling is het back-up van ten minste twee keer een dag en blijf aan de back-ups van maximaal zeven dagen. 

**1811 en meer**
| Omgeving schalen | Geschatte grootte van back-up | Totale hoeveelheid vereiste schijfruimte |
|-------------------|--------------------------|--------------------------------|
| 4-16 knooppunten        | 20 GB                    | 280 GB                        |
| ASDK              | 10 GB                    | 140 GB                        |

**Pre-1811**
| Omgeving schalen | Geschatte grootte van back-up | Totale hoeveelheid vereiste schijfruimte |
|-------------------|--------------------------|--------------------------------|
| 4-16 knooppunten, ASDK  | 10 GB                     | 140 GB                        |

### <a name="network-requirements"></a>Netwerkvereisten
| Opslaglocatie                                                                 | Details                                                                                                                                                                                 |
|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SMB-bestandsshare die wordt gehost op een opslagapparaat binnen de omgeving vertrouwd netwerk | Poort 445 is vereist als de Azure Stack-exemplaar bevindt zich in een omgeving met firewall. Infrastructuur voor back-up Controller initiëren een verbinding met de SMB-bestandsserver via poort 445. |
| Voor het gebruik van FQDN-naam van de bestandsserver, de naam moet kunnen worden omgezet vanaf de PEP             |                                                                                                                                                                                         |

> [!Note]  
> Er zijn geen poorten voor inkomend verkeer moeten worden geopend.

### <a name="encryption-requirements"></a>Vereisten voor gegevensversleuteling

Vanaf 1901 biedt de infrastructuur voor back-up-service gebruikt een certificaat met een openbare sleutel (. CER) voor het versleutelen van back-upgegevens en een certificaat met de persoonlijke sleutel (. (PFX) voor het ontsleutelen van back-upgegevens tijdens het cloudherstel.   
 - Het certificaat wordt gebruikt voor het transport van sleutels en wordt niet gebruikt om beveiligde geverifieerde communicatie tot stand brengen. Daarom kan het certificaat een zelf-ondertekend certificaat zijn. Azure Stack hoeft niet te controleren of root of trust voor dit certificaat, zodat externe toegang tot internet niet vereist is.
 
De zelf-ondertekend certificaat bestaat uit twee delen, één met de openbare sleutel en één met de persoonlijke sleutel:
 - Back-gegevens versleutelen: Het certificaat met de openbare sleutel (geëxporteerd naar. CER-bestand) wordt gebruikt voor het versleutelen van back-upgegevens
 - Back-upgegevens ontsleutelen: Het certificaat met de persoonlijke sleutel (geëxporteerd naar. PFX-bestand) wordt gebruikt voor het ontsleutelen van back-upgegevens

Het certificaat met de openbare sleutel (. CER) wordt niet beheerd door interne geheime draaien. Om terug te draaien het certificaat, moet u een nieuw zelfondertekend certificaat maken en bijwerken van back-upinstellingen met het nieuwe bestand (. CER).  
 - Alle bestaande back-ups blijven versleuteld met behulp van de vorige openbare sleutel. Nieuwe back-ups wordt de nieuwe openbare sleutel gebruikt. 
 
Het certificaat dat wordt gebruikt tijdens het cloudherstel met de persoonlijke sleutel (. (PFX) worden niet opgeslagen door Azure Stack uit veiligheidsoverwegingen. Dit bestand moet expliciet worden opgegeven tijdens het cloudherstel.  

**Achterwaarts compatibiliteitsmodus** vanaf 1901 biedt ondersteuning voor de sleutel van de versleuteling is afgeschaft en wordt verwijderd in een toekomstige release. Als u van 1811 met de back-up is al ingeschakeld met behulp van een versleutelingssleutel gemaakt bijgewerkt, blijft Azure Stack gebruiken van de versleutelingssleutel. Achterwaarts wordt compatibiliteitsmodus ondersteund voor ten minste 3 release. Na deze periode wordt is een certificaat vereist. 
 * Bijwerken van de versleutelingssleutel naar certificaten is een enkele bewerking.  
 * Alle bestaande back-ups blijven versleuteld met behulp van de versleutelingssleutel. Nieuwe back-ups wordt het certificaat te gebruiken. 

## <a name="infrastructure-backup-limits"></a>Infrastructuur voor back-limieten

Rekening houden deze limieten bij het plannen, implementeren en uitvoeren van uw Microsoft Azure Stack-instanties. De volgende tabel beschrijft deze limieten.

### <a name="infrastructure-backup-limits"></a>Limieten voor back-up van infrastructuur
| Limiet-id                                                 | Limiet        | Opmerkingen                                                                                                                                    |
|------------------------------------------------------------------|--------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Type back-up                                                      | Alleen volledige    | Infrastructuur voor back-up-Controller ondersteunt alleen volledige back-ups. Incrementele back-ups worden niet ondersteund.                                          |
| Geplande back-ups                                                | Geplande en handmatige  | Back-up-controller ondersteunt geplande en on-demand back-ups                                                                                 |
| Maximum aantal gelijktijdige back-uptaken                                   | 1            | Slechts één actieve back-uptaak wordt per exemplaar van de back-domeincontroller ondersteund.                                                                  |
| Configuratie van de netwerkswitch                                     | Niet in bereik | De beheerder moet back-up van configuratie van de netwerkswitch met behulp van OEM-hulpprogramma's. Raadpleeg de documentatie voor Azure Stack is geleverd door de OEM-leverancier. |
| Hardware levenscyclus van Host                                          | Niet in bereik | De beheerder moet back-up Hardware Lifecycle Host met behulp van OEM-hulpprogramma's. Raadpleeg de documentatie voor Azure Stack is geleverd door de OEM-leverancier.      |
| Maximum aantal bestandsshares                                    | 1            | Slechts één bestandsshare kan worden gebruikt voor het opslaan van back-upgegevens                                                                                        |
| Back-up van App Services, functies, SQL, mysql resource provider-gegevens | Niet in bereik | Raadpleeg richtlijnen gepubliceerd voor het implementeren en beheren-de toegevoegde waarde van RPs die zijn gemaakt door Microsoft.                                                  |
| Resourceproviders voor back-up van derden                              | Niet in bereik | Raadpleeg richtlijnen gepubliceerd voor het implementeren en beheren-de toegevoegde waarde van RPs die zijn gemaakt door externe leveranciers.                                          |

## <a name="next-steps"></a>Volgende stappen

 - Zie voor meer informatie over de infrastructuur voor Backup-Service, [back-up en gegevensherstel voor Azure Stack met de infrastructuur voor Backup-Service](azure-stack-backup-infrastructure-backup.md).
