---
title: 'Azure Active Directory B2C: Verwijzen naar - frameworks vertrouwen | Microsoft Docs'
description: Een onderwerp over Azure Active Directory B2C aangepast beleid en de identiteit ervaring Framework
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: mtillman
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: joroja
ms.openlocfilehash: 6dd9039520ec9bfadb358262dad7ebcdb67f6344
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="define-trust-frameworks-with-azure-ad-b2c-identity-experience-framework"></a>Vertrouwensrelatie Frameworks met Azure AD B2C identiteit ervaring Framework definiëren

Azure Active Directory B2C (Azure AD B2C) aangepaste beleidsregels die gebruikmaken van het Framework Identity-ervaring bieden uw organisatie, met een gecentraliseerde service. Deze service vermindert de complexiteit van identiteitsfederatie in een grote community van belang. De complexiteit is verlaagd tot een enkele vertrouwensrelatie en de uitwisseling van een enkele metagegevens.

Azure AD B2C aangepaste beleidsregels waarmee de identiteit ervaring Framework waarmee u kunt de volgende vragen beantwoorden:

- Wat zijn de juridische, beveiliging, privacy en beleid voor gegevensbeveiliging die moeten worden gehouden aan?
- Wie zijn de contactpersonen en wat zijn de processen voor toegevoegd als een erkende deelnemer?
- Ze bieden die de erkende informatie identiteitsproviders (ook wel bekend als ' claimproviders') en wat u doen?
- Wie zijn de erkende relying party's (en desgewenst wat ze hoeven)?
- Wat zijn de technische 'op de kabel' interoperabiliteitsvereisten voor deelnemers?
- Wat zijn de operationele 'runtime' regels die moeten worden afgedwongen voor het uitwisselen van gegevens van digitale identiteit?

Alle deze om vragen te beantwoorden, maken Azure AD B2C aangepaste beleidsregels die gebruikmaken van het gebruik van identiteit ervaring Framework de vertrouwen Framework (beginnen met TF). Laten we eens deze construct en wat biedt.

## <a name="understand-the-trust-framework-and-federation-management-foundation"></a>De foundation Framework vertrouwen en Federatie management begrijpen

Het Framework vertrouwen is een geschreven specificatie van de identiteit, beveiliging, privacy en beleid voor gegevensbeveiliging die deelneemt aan een community van belang moeten voldoen.

Federatieve identiteiten biedt een basis voor het bereiken van een eindgebruiker identiteitscontrole op Internet schaal. Door over te dragen identiteitsbeheer aan derden, één digitale identiteit voor een eindgebruiker opnieuw kan worden gebruikt met meerdere relying party's.  

Identiteitscontrole vereist dat de id-providers (IdPs) en de kenmerk-providers (AtPs) aan specifieke beveiliging, privacy, en operationele beleidsregels en procedures voldoen.  Als ze kunnen niet direct controles uitvoeren, moet relying party's (RPs) ontwikkelen vertrouwensrelaties met de IdPs en AtPs ze mee wilt werken.  

Wanneer het aantal gebruikers en providers van digitale identiteitsgegevens groeit, is het moeilijk om door te gaan pairwise beheer van deze vertrouwensrelaties of zelfs de pairwise uitwisseling van de technische metagegevens die zijn voor de verbinding met het netwerk vereist.  Federatieve hubs hebben bereikt alleen beperkt succes van deze problemen op te lossen.

### <a name="what-a-trust-framework-specification-defines"></a>Definieert wat een specificatie van het Framework vertrouwen
TFs zijn de linchpins van het model Open identiteit Exchange (OIX) vertrouwen Framework, waarbij elke community van belang is onderworpen aan een bepaalde TF-specificatie. Dergelijke een TF-specificatie worden gedefinieerd:

- **De beveiliging en privacy metrische gegevens voor de community van belang bij de definitie van:**
    - De beveiligingsniveaus (LOA) die aangeboden/vereist door de deelnemers worden. bijvoorbeeld: een geordende reeks classificaties van de authenticiteit van digitale identiteitsgegevens vertrouwen.
    - De beveiligingsniveaus (LOP) die aangeboden/vereist door de deelnemers worden. bijvoorbeeld: een geordende reeks vertrouwen classificaties voor de bescherming van gegevens van de digitale identiteit die wordt verwerkt door de deelnemers aan de community van belang.

- **De beschrijving van de digitale identiteit informatie die is aangeboden/vereist door deelnemers**.

- **De technische beleidsregels voor productie en het verbruik van informatie van de digitale identiteit en dus voor het meten LOA en LOP. Deze beleidsregels voor geschreven bevatten doorgaans de volgende categorieën van beleid:**
    - Identiteit taalprogramma beleidsregels, bijvoorbeeld: *hoe sterk is van een persoon identiteitsgegevens goedgekeurd?*
    - Beveiligingsbeleid, bijvoorbeeld: *hoe sterk zijn informatie integriteit en vertrouwelijkheid beveiligd?*
    - Privacybeleid, bijvoorbeeld: *welke besturingselement van een gebruiker heeft via persoonsgegevens persoonsgegevens (PII)*?
    - Overlevingsvermogen beleidsregels, bijvoorbeeld: *wanneer een provider niet langer bewerkingen, hoe gaat continuïteit en beveiliging van de functie PII?*

- **De technische profielen voor productie en het verbruik van informatie van de digitale identiteit. Deze profielen bevatten:**
    - Bereik interfaces waarvoor digitale identiteits-informatie beschikbaar op een opgegeven LOA is.
    - Technische vereisten voor de kabel interoperabiliteit.

- **De beschrijvingen van de verschillende functies die deelneemt aan de community kunnen uitvoeren en de kwalificatie die nodig zijn om te voldoen aan deze rollen.**

Een TF-specificatie bepaalt dus hoe de gegevens van identiteit worden uitgewisseld tussen de deelnemers van de community van belang: relying party's, identiteit en -kenmerk providers en controleprogramma's voor kenmerk.

Een opgegeven TF zijn een of meerdere documenten die fungeren als een verwijzing voor het beheer van de community van belang dat de bevestiging regelt en het verbruik van de gegevens van de digitale identiteit binnen de community. Het is een gedocumenteerde set beleidsregels en procedures die zijn ontworpen om een vertrouwensrelatie in de digitale identiteiten die worden gebruikt voor online transacties tussen leden van een community van belang.  

Met andere woorden, definieert een TF-specificatie de regels voor het maken van een ecosysteem levensvatbaar federatieve identiteiten voor een community.

Er is momenteel wijdverbreid overeenkomst op het voordeel van deze manier. Er is geen twijfel bestaat dat vertrouwensrelatie framework specificaties vergemakkelijken de ontwikkeling van de digitale identiteit ecosystemen met controleerbare beveiliging, betrouwbaarheid en privacy kenmerken, wat betekent dat ze opnieuw kunnen worden gebruikt tussen meerdere community's van belang.

Voor die reden, Azure AD B2C aangepaste beleidsregels die gebruikmaken van de identiteit ervaring Framework maakt gebruik van de specificatie als basis voor de weergave gegevens voor een TF interoperabiliteit mogelijk te maken.  

Azure AD B2C aangepaste beleidsregels die gebruikmaken van het kader van de gebruikerservaring identiteit vertegenwoordigen een TF-specificatie als een combinatie van menselijke en automatisch leesbare gegevens. Sommige secties van dit model (meestal secties die meer gericht is op governance zijn) worden weergegeven als verwijzingen naar de gepubliceerde beveiliging en privacy documentatie over beleid samen met de bijbehorende procedures (indien aanwezig). Andere secties worden de metagegevens en runtime configuratieregels die operationele automation vergemakkelijken in detail beschreven.

## <a name="understand-trust-framework-policies"></a>Framework vertrouwen beleid begrijpen

In termen van toepassing bestaat de specificatie TF uit een set van beleidsregels waarmee u volledige controle over het gedrag van de identiteit en ervaringen.  Azure AD B2C aangepaste beleidsregels die gebruikmaken van het Framework Identity-ervaring kunnen u om te schrijven en maak uw eigen TF via dergelijk declaratieve beleid kunt configureren en definiëren:

- De verwijzing naar document of verwijzingen die het ecosysteem van federatieve identiteiten van de community die is gekoppeld aan de TF definiëren. Ze zijn koppelingen naar de TF-documentatie. De regels (vooraf gedefinieerde) operationele 'runtime' of de gebruiker trajecten automatiseren en/of de exchange- en informatie over het gebruik van de claims bepalen. Deze gebruiker trajecten zijn gekoppeld aan een LOA (en een LOP). Een beleid kan daarom gebruiker trajecten met verschillende LOAs (en LOPs) hebben.

- De identiteits- en -providers, of de claimproviders in de community van belang en de technische profielen die ze samen met de LOA/LOP erkenning van (out of band) dat is gekoppeld aan deze ondersteunen.

- De integratie met controleprogramma's voor de kenmerk- of claimproviders.

- De relying party's in de community (door Deductie).

- De metagegevens voor het tot stand brengen van communicatie tussen de deelnemers netwerken. Deze metagegevens, samen met de technische profielen worden gebruikt tijdens een transactie leegmaken 'op de kabel' interoperabiliteit tussen de relying party's en andere deelnemers aan de community.

- De conversie van het protocol indien van toepassing (bijvoorbeeld SAML, OAuth2, WS-Federation en OpenID Connect).

- De verificatievereisten.

- Meervoudige orchestration indien van toepassing.

- Een gedeelde schema voor de claims die beschikbaar zijn en toewijzingen aan deelnemers van de community van belang.

- Alle claimtransformaties, samen met de tot gegevens in deze context, ter ondersteuning van de exchange en het gebruik van de claims.

- De binding en versleuteling.

- De opslag van claims.

### <a name="understand-claims"></a>Claims begrijpen

> [!NOTE]
> We gezamenlijk verwijzen naar alle mogelijke typen identiteitsgegevens die kunnen worden uitgewisseld als 'claims': claims over de verificatiereferenties van de eindgebruiker, identiteit toegang, communicatie-apparaat, fysieke locatie persoonlijk id-kenmerken, enzovoort.  
>
> We gebruiken de term 'claims'--in plaats van 'kenmerken'--omdat online transacties deze artefacten niet feiten die rechtstreeks kunnen worden geverifieerd door de relying party. Ze zijn in plaats daarvan asserties of claims over feiten waarvoor de relying party voldoende vertrouwen om toe te kennen van de eindgebruiker aangevraagde transactie moet ontwikkelen.  
>
> We gebruiken ook de term 'claims' omdat Azure AD B2C aangepaste beleidsregels die gebruikmaken van de identiteit ervaring Framework zijn ontworpen voor het vereenvoudigen van de uitwisseling van alle soorten gegevens van de digitale identiteit op een consistente manier ongeacht of het onderliggende protocol voor de verificatie of kenmerk ophalen van de gebruiker gedefinieerd.  Op dezelfde manier gebruiken we de term 'claimproviders' voor het gezamenlijk verwijzen naar de id-providers, kenmerk providers en controleprogramma's voor kenmerk wanneer we niet wilt dat onderscheid maken tussen hun specifieke functies.   

Dus bepalen ze hoe de gegevens van identiteit worden uitgewisseld tussen een relying party, identiteit en -kenmerk providers en controleprogramma's voor kenmerk. Ze bepalen welke identiteit en kenmerk providers zijn vereist voor een relying party-verificatie. Ze moeten worden beschouwd als een taal domeinspecifieke (DSL), dat wil zeggen, de computertaal van een die speciaal is bedoeld voor een bepaalde toepassingsdomein met overname, *als* overzichten, polymorfisme.

Deze beleidsregels vormen het machineleesbare gedeelte van de constructie TF in Azure AD B2C aangepaste beleidsregels voor gebruik van het kader van de gebruikerservaring identiteit. Ze bevatten alle operationele informatie, zoals claimproviders metagegevens en technische profielen, claims schemadefinities claims transformatiefuncties en gebruiker trajecten die zijn ingevuld operationele orchestration en automatisering mogelijk te maken.  

Zij worden aangenomen dat *levende documenten* omdat er een goede kans is dat de inhoud ervan wordt gewijzigd via de tijd over de actieve deelnemers gedeclareerd in het beleid. Er is ook de kans dat de voorwaarden voor een deelnemer kan worden gewijzigd.  

Federatieve installatie en het onderhoud aanzienlijk zijn vereenvoudigd door relying party's van actieve vertrouwen en connectiviteit herconfiguraties afscherming zoals providers/controleprogramma's voor verschillende claims aan of verwijderd (de community dat wordt vertegenwoordigd door) uit de set beleidsregels.

Interoperabiliteit is een andere belangrijke uitdaging. Providers/controleprogramma's voor extra claims moeten worden geïntegreerd, omdat de relying party's zijn waarschijnlijk niet de benodigde protocollen ondersteund. Azure AD B2C aangepast beleid oplossen dit probleem door de ondersteuning van industriestandaard-protocollen en door het toepassen van specifieke gebruiker trajecten voor de omzetting van aanvragen bij de relying party's en providers van kenmerk niet hetzelfde protocol ondersteunen.  

Gebruiker trajecten omvatten protocol profielen en metagegevens die worden gebruikt voor het leegmaken van 'op de kabel' interoperabiliteit tussen de relying party en andere deelnemers. Er zijn ook operationele runtime-regels die worden toegepast op de identiteit informatie exchange aanvraag/antwoord-berichten voor het afdwingen van compatibiliteit met gepubliceerde beleidsregels als onderdeel van de TF-specificatie. Het idee van de gebruiker trajecten is essentieel voor het aanpassen van de gebruikerservaring. De casusstudies werpen ook licht over de werking van het systeem op het protocolniveau van het.

Op basis daarvan relying party-toepassingen en -portals kunnen, afhankelijk van hun context aanroepen van Azure AD B2C aangepaste beleidsregels die gebruikmaken van de identiteit ervaring Framework de naam van een specifiek beleid doorgeeft en nauwkeurig de uitwisseling van gedrag en gegevens ophalen ze willen zonder wirwar, fuss of risico.
