---
title: Azure faciliteiten, lokale en fysieke beveiliging | Microsoft Docs
description: Het artikel beschrijft de Azure-datacenters, met inbegrip van de fysieke infrastructuur, beveiliging en compliance-aanbiedingen.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 69a54640ae3b4e71d7782712ad1764babab7104a
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39170674"
---
# <a name="azure-facilities-premises-and-physical-security"></a>Azure faciliteiten, lokale en fysieke beveiliging
Azure bestaat uit een [wereldwijd gedistribueerde datacenter-infrastructuur](https://azure.microsoft.com/global-infrastructure/), ondersteunen duizenden online services en spanning van meer dan 100 streng beveiligde faciliteiten over de hele wereld.

De infrastructuur is ontworpen om toepassingen dichter bij gebruikers over de hele wereld, gegevenslocatie te behouden en biedt uitgebreide nalevings- en opslagopties voor klanten. Azure beschikt over 52 regio's over de hele wereld, en is beschikbaar in 140 landen.

Een regio is een set datacenters die is verbonden via een zeer grote en robuuste netwerk. Het netwerk bevat de distributie van inhoud, taakverdeling, redundantie en versleuteling standaard. Met meer wereldwijde regio's dan enige andere cloudprovider biedt Azure u de flexibiliteit om toepassingen te implementeren waar u ze nodig hebt.

Azure-regio's zijn geordend in geografieën. Een Azure-geografie zorgt ervoor dat de vereisten binnen geografische grenzen met betrekking tot de gegevenslocatie, soevereiniteit, naleving en tolerantie in acht worden genomen.

Geografieën bieden klanten met specifieke behoeften met betrekking tot de gegevenslocatie en naleving de mogelijkheid om hun gegevens en toepassingen in de buurt te houden. Verschillende geografische gebieden zijn fouttolerante tegen volledig regio fout, dankzij de verbinding met de netwerkinfrastructuur toegewezen, hoge capaciteit.

Beschikbaarheidszones zijn fysiek gescheiden locaties binnen een Azure-regio. Elke beschikbaarheidszone bestaat uit een of meer datacenters die zijn uitgerust met onafhankelijke voeding, koeling en netwerken. U kunt met beschikbaarheidszones missiekritieke toepassingen uitvoeren met hoge beschikbaarheid en lage latentiereplicatie.

De volgende afbeelding ziet u hoe de globale Azure-infrastructuur paren regio en beschikbaarheid zones binnen dezelfde gegevenslocatiegrens voor hoge beschikbaarheid, herstel na noodgevallen en back-up.

![Diagram waarin gegevenslocatiegrens][1]

Geografisch gedistribueerde datacenters kunt Microsoft moet dicht bij klanten, Verminder de netwerklatentie en toestaan voor geografisch redundante back-up en failover.

## <a name="physical-security"></a>Fysieke beveiliging
Microsoft ontwerpt, bouwt en werkt datacenters op een manier die strikt beheert de fysieke toegang tot de gebieden waar uw gegevens worden opgeslagen. Microsoft begrijpt het belang om uw gegevens te beveiligen en is toegewezen aan het beveiligen van de datacenters die uw gegevens bevatten. We hebben een gehele afdeling bij Microsoft besteed aan het ontwerpen, bouwen en de fysieke faciliteiten ondersteuning van Azure. Dit team wordt geïnvesteerd in het onderhouden van fysieke beveiliging van status-of-the-art.

Microsoft hanteert een gelaagde benadering van fysieke beveiliging, vermindert het risico van niet-gemachtigde gebruikers die fysieke toegang tot gegevens en de resources in het datacenter. Datacenters beheerd door Microsoft zijn uitgebreide beveiligingslagen: toegang tot de goedkeuring in van de faciliteit perimeternetwerk, in van het gebouw perimeternetwerk, binnen het gebouw en op de verdieping datacenter. Lagen van fysieke beveiliging zijn:

- **Aanvraag voor toegang tot en goedkeuring.** U moet toegang te krijgen voordat die binnenkomen in het datacenter op te vragen. Je moet een geldige zakelijke reden opgeven wanneer ze voor uw bezoek, zoals naleving of controledoeleinden. Alle aanvragen worden goedgekeurd op basis van de noodzaak voor toegang door werknemers van Microsoft. De basis van een nodig voor toegang tot wordt voorkomen dat het aantal personen die nodig zijn voor het voltooien van een taak in de datacenters voor de minimumwaarde. Nadat u Microsoft toestemming verleent, een persoon heeft alleen toegang tot het discrete gebied van het datacenter nodig, op basis van de goedgekeurde zakelijke reden. Machtigingen zijn beperkt tot een bepaalde periode, en vervolgens verlopen.

- **Perimeter van opslagruimte.** Wanneer u bij een datacenter aankomt, bent u vereist een goed gedefinieerde toegangspunt doorlopen. Hoog fences staal en concreet omvatten doorgaans elke inch van het perimeternetwerk. Er zijn camera's rond de datacentra, met een bewaking van hun video's te allen tijde beveiligingsteam.

- **Building ingang.** De datacenter-ingang is bemand met professionele security officers die hebben ondergaan strenge controles voor training en achtergrond. Deze security officers ook regelmatig patrouilleren van het datacenter en de video's van de camera's binnen het datacenter te allen tijde bewaken.

- **In het gebouw.** Nadat u het gebouw invoert, moet u verificatie met twee factoren met biometrie om door te gaan met het doorlopen van het datacenter doorgeven. Als uw identiteit is gevalideerd, kunt u alleen het gedeelte van het datacenter waar u toegang tot hebt goedgekeurd. U kunt er blijven alleen voor de duur van de tijd die is goedgekeurd.

- **Datacenter floor.** U mag alleen naar de verdieping dat u hebt goedgekeurd om in te voeren. U bent verplicht om door te geven van een volledige hoofdtekst metal detectie screening. Om het risico van niet-geautoriseerde gegevens binnenkomen of het datacenter zonder onze kennis verlaten, kunnen alleen goedgekeurde apparaten in de verdieping datacenter mogelijk maken. Daarnaast worden video camera's monitor de front- en achterkant van elke server rek. Wanneer u de verdieping datacenter afsluit, moet opnieuw u doorgeven via volledige hoofdtekst metal detectie screening. Als u wilt laten het datacenter, bent u verplicht om door te geven via een scan voor extra beveiliging.

Microsoft vereist dat bezoekers badges bij het verlaten van een Microsoft-faciliteit afstand.

## <a name="physical-security-reviews"></a>Beoordelingen van fysieke beveiliging
We voeren periodiek, beoordelingen van fysieke beveiliging van de opslagruimten, om ervoor te zorgen de datacenters correct adres Azure voor beveiliging voldoen. Het datacenter hosting provider personeel bieden Azure service management. Personeel kunnen niet aanmelden bij Azure-systemen en geen fysieke toegang hebt tot de Azure CO-locaties ruimte en kooien.

## <a name="data-bearing-devices"></a>Gegevens die apparaten
Microsoft maakt gebruik van best practice-procedures en een om oplossing die is [NIST 800-88 compatibel](https://csrc.nist.gov/publications/detail/sp/800-88/archive/2006-09-01). Voor harde schijven kunnen niet worden gewist, gebruiken we een vernietiging proces dat wordt het vernietigd en het herstel van gegevens wordt weergegeven. Dit proces vernietiging kan uiteenspringen bestaat, versnipperen, pulverize of verbranding zijn. We bepalen de middelen van verwijdering op basis van het assettype. We bewaren records van de vernietiging.  

## <a name="equipment-disposal"></a>Verwijderen van apparatuur
Bij van een systeem einde van de levenscyclus Volg operationele medewerkers van Microsoft strenge gegevensverwerking en hardware verwijdering procedures om te bevestigen dat uw gegevens met hardware is niet beschikbaar gesteld voor niet-vertrouwde partijen. Gebruiken we een benadering veilig wissen voor harde schijven die dit ondersteunen. Voor harde schijven kunnen niet worden gewist, gebruiken we een vernietiging proces dat het station vernietigd en het herstel van gegevens wordt weergegeven. Dit proces vernietiging kan uiteenspringen bestaat, versnipperen, pulverize of verbranding zijn. We bepalen de middelen van verwijdering op basis van het assettype. We bewaren records van de vernietiging. Alle Azure-services gebruik goedgekeurde mediaservices voor opslag en verwijdering.

## <a name="compliance"></a>Naleving
We ontwerpen en beheren van de Azure-infrastructuur om te voldoen aan een groot scala internationale en bedrijfsspecifieke nalevingsstandaarden, zoals ISO 27001, HIPAA, FedRAMP, SOC 1 en SOC 2. Er is ook te voldoen aan landspecifieke standaarden, waaronder IRAP voor Australië, Verenigd Koninkrijk G-Cloud en MTCS voor Singapore. Grondige externe audits, zoals die worden uitgevoerd door het British Standards Institute, controleert u of voldoet aan de strikte maatregelen voor die deze standaarden verplichten.

Zie voor een volledige lijst van standaarden voor compliance dat Azure voldoet aan de [Compliance-aanbiedingen](https://www.microsoft.com/trustcenter/compliance/complianceofferings). 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over wat Microsoft doet om te helpen beveiligen van de Azure-infrastructuur:

- [Beschikbaarheid van Azure-infrastructuur](azure-infrastructure-availability.md)
- [Onderdelen van het systeem Azure informatie en grenzen](azure-infrastructure-components.md)
- [Architectuur van Azure-netwerk](azure-infrastructure-network.md)
- [Azure productienetwerk](azure-production-network.md)
- [Azure SQL Database-beveiligingsfuncties](azure-infrastructure-sql.md)
- [-Azure-productiebewerkingen en beheer](azure-infrastructure-operations.md)
- [Azure-infrastructuur bewaken](azure-infrastructure-monitoring.md)
- [Integriteit van de Azure-infrastructuur](azure-infrastructure-integrity.md)
- [Azure-klant-gegevensbeveiliging](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-physical-security/data-residency-boundary.png
