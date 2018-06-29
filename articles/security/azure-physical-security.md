---
title: Azure opslagruimten, ruimten en fysieke beveiliging | Microsoft Docs
description: De Azure-datacenters, met inbegrip van de fysieke infrastructuur, beveiliging en naleving aanbiedingen beschreven.
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
ms.openlocfilehash: a6a9b1d6e12dabb09cde684c34481b4b3442c1b8
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102275"
---
# <a name="azure-facilities-premises-and-physical-security"></a>Azure opslagruimten, ruimten en fysieke beveiliging
De cloud van Microsoft bestaat uit een [globaal gedistribueerde datacenter-infrastructuur](https://azure.microsoft.com/global-infrastructure/) duizenden onlineservices ondersteunen en meer dan 100 maximaal spanning wereldwijde beveiligen.

De infrastructuur is ontworpen om aan te brengen dichter toepassingen voor gebruikers over de hele wereld, hand vestigingsplaats gegevens behouden en biedt uitgebreide opties voor de naleving en tolerantie voor klanten. Azure heeft 52 regio's overal ter wereld en is beschikbaar in 140 landen.

Een regio is een set van datacenters die onderling via een netwerk brede en robuuste verbonden. Het netwerk bevat de distributie van inhoud, taakverdeling redundantie en versleuteling standaard. Met meer wereldwijde regio's dan elke anderen cloudprovider, biedt Azure klanten de flexibiliteit om toepassingen te implementeren waar dat nodig is.

Azure-regio's zijn geordend in geografieën. Een Azure-geografie zorgt ervoor dat de vereisten binnen geografische grenzen met betrekking tot de gegevenslocatie, soevereiniteit, naleving en tolerantie in acht worden genomen.

Geografieën bieden klanten met specifieke behoeften met betrekking tot de gegevenslocatie en naleving de mogelijkheid om hun gegevens en toepassingen in de buurt te houden. Locaties zijn fouttolerante bestand zijn tegen voltooid regio fout via de verbinding met de speciale netwerkinfrastructuur van hoge capaciteit.

Beschikbaarheidszones zijn fysiek gescheiden locaties binnen een Azure-regio. Elke beschikbaarheidszone bestaat uit een of meer datacenters die zijn voorzien van een onafhankelijke stroomvoorziening, koeling en netwerk. Met beschikbaarheidszones kunnen klanten missiekritieke toepassingen uitvoeren met een hoge beschikbaarheid en een lage latentiereplicatie.

De volgende afbeelding ziet hoe de infrastructuur van Azure globale regio en beschikbaarheid Zones binnen dezelfde gegevens hand vestigingsplaats grens voor hoge beschikbaarheid, herstel na noodgevallen en back-up paren.

![Gegevens hand vestigingsplaats grens][1]

Een grote geografische gedistribueerde footprint van datacenters kan Microsoft dicht bij klanten om de netwerklatentie te verminderen en toe te staan voor geografisch redundante back-up en failover worden.

## <a name="physical-security"></a>Fysieke beveiliging
Microsoft ontwerpt, bouwt en werkt datacenters op een manier die strikt controleert de fysieke toegang tot de gebieden waarin gegevens van de klant is opgeslagen. Microsoft begrijpt het belang van het beveiligen van gegevens van de klant en zijn er alles aan om de datacenters met uw gegevens beveiligen. We hebben een hele deling bij Microsoft besteed aan het ontwerpen, bouwen en werken met de fysieke faciliteiten ondersteuning van Azure. Dit team is geïnvesteerd in het onderhouden van fysieke beveiliging van de nieuwste.

Microsoft hecht een gelaagde benadering aan fysieke beveiliging om de niet-geautoriseerde gebruikers fysieke toegang tot gegevens en de datacenterbronnen risico te verminderen. Datacenters beheerd door Microsoft zijn uitgebreide beveiligingslagen: toegang tot de goedkeuring in de faciliteit perimeternetwerk, in het gebouw perimeternetwerk, binnen het gebouw en op de verdieping datacenter. Lagen van fysieke beveiliging zijn:

- Aanvraag voor toegang tot en goedkeuring – moet u toegang te krijgen voordat dat binnenkomt bij het datacenter aanvragen. Je moet een geldige zakelijke reden voor uw bezoek, zoals naleving of controledoeleinden bieden. Alle aanvragen worden goedgekeurd toegang nodig hebt op basis van een Microsoft-werknemers. Een basis toegang nodig hebt wordt voorkomen dat het aantal personen die nodig zijn voor het voltooien van een taak in de datacentra in minimaal. Nadat de machtiging is verleend, heeft een afzonderlijk alleen toegang tot het discrete gebied van het datacenter op basis van de goedgekeurde zakelijke reden. Machtigingen zijn beperkt tot een bepaalde periode en verlopen na de toegestane periode.

- Faciliteit van perimeter - wanneer u bij een datacenter aankomt, bent u vereist een goed gedefinieerde toegangspunt doorlopen. Hoog fences bestaat staal en concreet omvatten doorgaans inch van het perimeternetwerk. Er zijn camera's rond de datacentra, met een beveiligingsteam bewaking van hun video's 24/7 en 365 dagen van het jaar.

- Professional-afdelingen die strenge controles voor trainings- en achtergrond hebben ondergaan on gebouw ingang - de ingang van het datacenter. Deze beveiliging managers patrouilleren ook regelmatig de datacenter terwijl ze ook de video's van de camera's binnen het datacenter 24/7- en 365 dagen per jaar controleren.

- Nadat u het gebouw, moet u in het gebouw - tweeledige verificatie met biometrie om door te gaan met het doorlopen van het datacenter doorgeven. Als uw identiteit is gevalideerd, kunt u het gedeelte van het datacenter die u hebt toegang tot goedgekeurd. U kunt er blijven alleen voor de duur van de tijd die is goedgekeurd.

- Datacenter floor – u zijn alleen toegestaan in de floor dat u bent goedgekeurd om in te voeren. U bent verplicht om door te geven van een volledige hoofdtekst metal detectie bestandsgroepen. Om het risico van niet-geautoriseerde gegevens binnenkomen of verlaten van het datacenter zonder onze weet, kunnen alleen apparaten die goedgekeurde hun manier maken in het datacenter floor. Bovendien rack videocamera monitor de voorgrond en terug van elke server. Volledige hoofdtekst metal detectie screenen wordt herhaald bij het afsluiten van het datacenter floor. Als u wilt laten het datacenter, bent u moet doorgeven aan een scan voor extra beveiliging.

Bezoekers zijn vereist voor de afstand badges bij het verlaten van een Microsoft-faciliteit.

## <a name="physical-security-reviews"></a>Beoordelingen van fysieke beveiliging
Beoordelingen van fysieke beveiliging van de faciliteiten worden regelmatig uitgevoerd om te controleren of de datacentra correct adres Microsoft Azure-beveiligingsvereisten. Het datacenter hosting provider personeel beschikbaar beheer van Microsoft Azure-service niet. Personeel hebben geen toegang tot Azure-systemen of fysieke toegang tot de Azure onderbrengen kamer- en kooien.

## <a name="data-bearing-devices"></a>Gegevens gevolgen apparaten
Microsoft gebruikt best practice procedures en een om oplossing die [NIST 800-compatibele 88](https://csrc.nist.gov/publications/detail/sp/800-88/archive/2006-09-01). Voor harde schijven kunnen niet worden gewist, wordt een vernietiging proces dat het vernietigt en geeft het herstel van gegevens onmogelijk gebruikt. Vernietiging proces kan worden uiteenspringen bestaat, versnipperen, pulverize of verbranding. De juiste wijze van verwijdering wordt bepaald door het activatype. Records van de vernietiging worden bewaard.  

## <a name="equipment-disposal"></a>Verwijderen van apparatuur
Microsoft Azure implementeert dit principe namens klanten. Bij een systeem einde van de levenscyclus Volg operationele medewerkers van Microsoft strengere gegevensverwerking en -procedures voor hardware-verwijdering te verzekeren dat hardware met gegevens van de klant is niet beschikbaar gesteld voor niet-vertrouwde partijen. Een benadering Secure wissen wordt gevolgd (via de firmware van de harde schijf) voor schijven die dit ondersteunen. Voor harde schijven kunnen niet worden gewist, wordt een vernietiging proces dat het vernietigt en geeft het herstel van gegevens onmogelijk gebruikt. Vernietiging proces kan worden uiteenspringen bestaat, versnipperen, pulverize of verbranding. De juiste wijze van verwijdering wordt bepaald door het activatype. Records van de vernietiging worden bewaard. Alle Microsoft Azure-services gebruiken een goedgekeurde mediaservices voor opslag en verwijdering.

## <a name="compliance"></a>Naleving
De Azure-infrastructuur is ontworpen en beheerd om te voldoen aan een uitgebreide reeks internationale en branchespecifieke naleving standaarden, zoals ISO 27001 HIPAA, FedRAMP, SOC 1 en SOC 2. Land-specifieke normen is voldaan, waaronder Australië IRAP, UK G-Cloud en Singapore MTCS. Grondige externe audits, zoals die van het British Standards Institute, bevestigen of Azure voldoet aan het strikte beveiligingsbeheer die deze standaarden voorschrijven.

Zie de [de offerings naleving](https://www.microsoft.com/trustcenter/compliance/complianceofferings) voor een volledige lijst met nalevingsstandaards aan door Azure gehouden.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Microsoft biedt voor het beveiligen van de Azure-infrastructuur:

- [Beschikbaarheid van de Azure-infrastructuur](azure-infrastructure-availability.md)
- [Onderdelen van het systeem Azure informatie en grenzen](azure-infrastructure-components.md)
- [Architectuur van de Azure-netwerk](azure-infrastructure-network.md)
- [Azure productienetwerk](azure-production-network.md)
- [Beveiligingsfuncties van Microsoft Azure SQL Database](azure-infrastructure-sql.md)
- [Azure productie-uitvoering en beheer](azure-infrastructure-operations.md)
- [Bewaking van Azure-infrastructuur](azure-infrastructure-monitoring.md)
- [Integriteit van de Azure-infrastructuur](azure-infrastructure-integrity.md)
- [Beveiliging van gegevens van de klant in Azure](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-physical-security/data-residency-boundary.png
