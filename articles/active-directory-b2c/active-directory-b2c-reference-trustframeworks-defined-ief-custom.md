---
title: Referentie - frameworks vertrouwen in Azure Active Directory B2C | Microsoft Docs
description: Een onderwerp over Azure Active Directory B2C aangepast beleid en de Identiteitservaring-Framework.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 4eee1f2c9ca37227ca2cea0ef5e2ff25f6fad828
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443406"
---
# <a name="define-trust-frameworks-with-azure-ad-b2c-identity-experience-framework"></a>Vertrouwensrelatie Frameworks met Azure AD B2C Identity-Ervaringsframework definiëren

Azure Active Directory B2C (Azure AD B2C) aangepaste beleidsregels die gebruikmaken van de Identity-Ervaringsframework voorziet uw organisatie met een gecentraliseerde service. Deze service vermindert de complexiteit van identiteitsfederatie in een grote gebruikerscommunity van belang zijn. De complexiteit is verlaagd tot een enkele vertrouwensrelatie en de uitwisseling van een enkele metagegevens.

Azure AD B2C aangepaste beleidsregels die gebruikmaken van de Identiteitservaring-Framework waarmee u kunt de volgende vragen beantwoorden:

- Wat zijn de juridische, beveiliging, privacy en beleidsregels voor gegevensbescherming die moeten worden gehouden aan?
- Wie zijn de contactpersonen en wat zijn de processen voor een erkende deelnemer steeds?
- Ze bieden die de erkende id-gegevens-providers (ook wel bekend als ' claimproviders'), en doen?
- Wie zijn de erkende Relying Party's (en eventueel wat ze hoeven)?
- Wat zijn de technische 'op de kabel"interoperabiliteitsvereisten voor deelnemers aan?
- Wat zijn de operationele "runtime" regels die moeten worden afgedwongen voor het uitwisselen van gegevens van digitale identiteit?

Als u wilt alle deze vragen te beantwoorden, een aangepast Azure AD B2C-beleid die gebruikmaken van het gebruik Identity-Ervaringsframework de "vertrouwen Framework" (TF) maken. Laten we eens v tomto konstruktoru SE en het pakket bevat.

## <a name="understand-the-trust-framework-and-federation-management-foundation"></a>Inzicht in de basis van de management Framework vertrouwen en Federatie

Het Framework vertrouwen is een schriftelijke specificatie van de identiteit, beveiliging, privacy en beleidsregels voor gegevensbescherming waarop deelnemers in een community van belang zijn, moeten voldoen.

Federatieve identiteiten biedt een basis voor het bereiken van eindgebruikers identiteitscontrole op internetschaal. Door over te dragen identiteitsbeheer aan derden, één digitale identiteit voor een eindgebruiker opnieuw kan worden gebruikt met meerdere Relying Party's.  

Identiteitscontrole is vereist dat de id-providers (IdPs) en de kenmerk-providers (AtPs) aan specifieke beveiliging, privacy, en operationele beleidsregels en procedures voldoen.  Als ze niet direct controles uitvoeren, moet Relying Party's (RPs) ontwikkelen vertrouwensrelaties met de id-providers en ze ervoor kiezen om te werken met AtPs.  

Als het aantal gebruikers en informatie over digitale id-providers groeit, is het moeilijk zijn om door te gaan pairwise beheer van deze vertrouwensrelaties of zelfs de pairwise uitwisseling van de metagegevens van de technische die zijn voor verbinding met het netwerk vereist.  Federation-hubs hebben alleen beperkt succes bij het oplossen van deze problemen bereikt.

### <a name="what-a-trust-framework-specification-defines"></a>Definieert wat een specificatie van het Framework vertrouwen
TFs zijn de linchpins van het Open identiteit Exchange (OIX) vertrouwen Framework-model, waarbij elke community van belang is onderworpen aan een bepaalde TF-specificatie. Dergelijke een TF-specificatie worden gedefinieerd:

- **De beveiliging en privacy metrische gegevens voor de definitie van de Gemeenschap van belang:**
    - Het niveau van zekerheid (LOA) die aangeboden/vereist voor deelnemers zijn. bijvoorbeeld: een geordende reeks vertrouwen classificaties voor de authenticiteit van de gegevens van digitale identiteit.
    - De beveiligingsniveaus (LOP) die aangeboden/vereist voor deelnemers zijn. bijvoorbeeld: een geordende reeks vertrouwen classificaties voor de beveiliging van gegevens van de digitale identiteit die wordt verwerkt door de deelnemers aan de community van belang zijn.

- **De beschrijving van de digitale identiteitsinformatie die is aangeboden/vereist door deelnemers**.

- **De technische beleidsregels voor productie en het verbruik van gegevens van digitale identiteit, en dus voor het meten van LOA en LOP. Deze schriftelijke beleidsregels zijn doorgaans de volgende categorieën van het beleid:**
    - Identiteit taalprogramma beleid, bijvoorbeeld: *hoe sterk is van een persoon identiteitsgegevens gecontroleerd?*
    - Beveiligingsbeleid, bijvoorbeeld: *hoe sterk zijn informatie integriteit en vertrouwelijkheid beveiligd?*
    - Privacybeleid, bijvoorbeeld: *welke besturingselement een gebruiker hebben over persoonlijk identificeerbare informatie (PII)*?
    - Overlevingsvermogen beleid, bijvoorbeeld: *als een provider niet langer operations, hoe gaat de continuïteit en bescherming van persoonsgegevens functie?*

- **De technische profielen voor productie en het verbruik van gegevens van digitale identiteit. Deze profielen bevatten:**
    - Bereik interfaces waarvoor digitale identiteitsinformatie beschikbaar op een opgegeven LOA is.
    - Technische vereisten voor de wire-compatibiliteit.

- **De beschrijvingen van de verschillende rollen die deelneemt aan de community kunnen uitvoeren en de kwalificatie die nodig zijn om te voldoen aan deze rollen.**

Een specificatie TF bepaalt dus hoe identiteitsgegevens worden uitgewisseld tussen de deelnemers van de community van belang: Relying Party's, identiteit en -kenmerk providers en controleprogramma's voor kenmerk.

Een TF-specificatie is een of meerdere documenten, die dienen als referentie voor het beheer van de community van belang dat de verklaring regelt en het verbruik van de gegevens van de digitale identiteit binnen de community. Er is een gedocumenteerde set beleidsregels en procedures die zijn ontworpen voor vertrouwen in de digitale identiteiten die worden gebruikt voor online transacties tussen de leden van een community van belang zijn.  

Met andere woorden, definieert een specificatie TF de regels voor het maken van een ecosysteem van bruikbare federatieve identiteiten voor een community.

Er is momenteel wijdverbreid overeenkomst op het voordeel van een dergelijke aanpak. Er is geen twijfel bestaat dat vertrouwensrelatie framework specificaties vergemakkelijken de ontwikkeling van digitale identiteit ecosystemen met controleerbare beveiliging, assurance en privacy kenmerken, wat betekent dat ze kunnen worden hergebruikt binnen meerdere community's van belang zijn.

Voor die om die reden worden aangepast Azure AD B2C-beleid die gebruikmaken van de Identity-Ervaringsframework maakt gebruik van de specificatie als basis voor de weergave van de gegevens voor een TF om interoperabiliteit mogelijk te maken.  

Azure AD B2C aangepaste beleidsregels die gebruikmaken van de Identity-Ervaringsframework vertegenwoordigen een specificatie TF als een combinatie van menselijke en door machine leesbare gegevens. Bepaalde secties van dit model (meestal secties die meer gericht is op governance zijn) worden weergegeven als verwijzingen naar de gepubliceerde beveiliging en privacy documentatie over beleid samen met de gerelateerde procedures (indien aanwezig). Andere secties worden de metagegevens en runtime configuratieregels die operationele automation vergemakkelijken in detail beschreven.

## <a name="understand-trust-framework-policies"></a>Framework vertrouwen beleid begrijpen

Wat betreft implementatie, wordt de specificatie TF bestaat uit een verzameling van beleidsregels waarmee u volledige controle over het gedrag van de identiteit en ervaringen.  Azure AD B2C aangepaste beleidsregels die gebruikmaken van de Identity-Ervaringsframework kunnen u voor het ontwerpen en maken van uw eigen TF via dergelijk declaratieve beleid die u kunt definiëren en configureren:

- De verwijzing naar document of verwijzingen die het ecosysteem federatieve identiteit van de community die is gekoppeld aan de TF definiëren. Ze vindt u koppelingen naar de TF-documentatie. De regels (vooraf gedefinieerde) operationele "runtime', of de gebruiker reizen automatiseren en/of beheren van de exchange en het gebruik van de claims. Deze gebruiker reizen zijn gekoppeld aan een LOA (en een LOP). Een beleid kan daarom gebruiker reizen met uiteenlopende LOAs (en LOPs) hebben.

- De identiteits- en -providers, of de claims-providers, in de community van belang en de technische profielen die ze samen met de LOA/LOP erkenning van (out-of-band) dat is gekoppeld aan deze ondersteunen.

- De integratie met kenmerk controleprogramma's of claims-providers.

- De Relying Party's in de community (door Deductie).

- De metagegevens voor het tot stand brengen van netwerkcommunicatie tussen deelnemers. Deze metagegevens, samen met de technische profielen worden gebruikt tijdens een transactie leegmaken 'op de kabel"interoperabiliteit tussen de relying party's en andere deelnemers aan de community.

- De conversie van het protocol indien van toepassing (bijvoorbeeld, SAML, OAuth2, WS-Federation en OpenID Connect).

- De verificatievereisten.

- De indeling van de meervoudige indien van toepassing.

- Een gedeelde schema voor de claims die beschikbaar zijn en toewijzingen aan deelnemers van de community van belang zijn.

- Alle claimtransformaties, samen met de tot mogelijk gegevens in deze context te accepteren van de exchange en het gebruik van de claims.

- De binding en versleuteling.

- De claims-opslag.

### <a name="understand-claims"></a>Informatie over claims

> [!NOTE]
> We gezamenlijk verwijzen naar alle mogelijke typen van identiteitsgegevens die kunnen worden uitgewisseld als 'claims': claims over van de eindgebruiker verificatiereferentie, identiteit gaan, communicatie-apparaat, fysieke locatie, persoonlijke id-kenmerken, enzovoort.  
>
> We gebruiken de term 'claims'--in plaats van "kenmerken"--omdat in online transacties, de gegevensartefacten van deze niet zijn ingesteld op feiten die rechtstreeks kunnen worden gecontroleerd door de relying party. Ze zijn in plaats van asserties of claims over feiten waarvoor de relying party voldoende vertrouwen voor het verlenen van de eindgebruiker aangevraagde transactie moet ontwikkelen.  
>
> We gebruiken ook de term 'claims' omdat Azure AD B2C aangepaste beleidsregels die gebruikmaken van de Identity-Ervaringsframework zijn ontworpen voor het vereenvoudigen van de uitwisseling van alle soorten gegevens van digitale identiteit in een consistente manier, ongeacht of het onderliggende protocol is voor verificatie of kenmerk ophalen van de gebruiker gedefinieerd.  Evenzo, gebruiken we de term "claimproviders" gezamenlijk id-providers, kenmerk providers en controleprogramma's voor kenmerk verwijzen als we niet willen onderscheid maken tussen hun specifieke functies.   

Dus bepalen ze hoe identiteitsgegevens worden uitgewisseld tussen een relying party, identiteit en -kenmerk providers en controleprogramma's voor kenmerk. Ze bepalen welke identiteit en kenmerk providers zijn vereist voor een relying party verificatie. Ze moeten worden beschouwd als een taal domeinspecifieke (DSL), dat wil zeggen, de computertaal van een die is speciaal bedoeld voor een bepaalde toepassingsdomein met overname *als* overzichten, polymorfisme.

Deze beleidsregels vormen het machineleesbare gedeelte van de constructie TF in Azure AD B2C aangepaste beleidsregels voor gebruik te maken van de Identiteitservaring-Framework. Ze bevatten alle operationele details, met inbegrip van claims-providers metagegevens en technische profielen, schemadefinities voor claims, claims transformatiefuncties en gebruiker reizen die worden ingevuld om operationele indeling en automatisering mogelijk te maken.  

Ze worden beschouwd *levensonderhoud documenten* omdat er is een goede kans is dat de inhoud ervan worden trouwens tijd over de actieve deelnemers gedeclareerd in het beleid. Er is ook de kans dat de voorwaarden en bepalingen voor een deelnemer kunnen worden gewijzigd.  

Federatieve installatie en het onderhoud sterk wordt vereenvoudigd door toepassing afscherming Relying Party's van actieve regels en connectiviteit wanneer andere claims-providers/controleprogramma's toevoegen of laat u (de community vertegenwoordigd door) de set met beleidsregels.

Interoperabiliteit is nog een aanzienlijke uitdaging om. Aanvullende claims providers/controleprogramma's moeten worden geïntegreerd, omdat de Relying Party's waarschijnlijk geen ondersteuning voor alle protocollen die nodig zijn. Azure AD B2C aangepast beleid wordt dit probleem oplossen door de ondersteuning van standaardprotocollen en door het toepassen van de specifieke gebruiker trajecten voor de omzetting van aanvragen als Relying Party's en leveranciers van het kenmerk niet hetzelfde protocol ondersteunen.  

Gebruikers reizen zijn protocol profielen en metagegevens die worden gebruikt voor het leegmaken van 'op de kabel"interoperabiliteit tussen de relying party's en andere deelnemers. Er zijn ook operationele runtime-regels die worden toegepast op identiteit exchange aanvraag/antwoord informatieberichten voor het afdwingen van naleving van gepubliceerde beleid als onderdeel van de TF-specificatie. Het idee van de gebruiker reizen is essentieel voor het aanpassen van de gebruikerservaring. Deze geworpen ook licht over de werking van het systeem op het protocolniveau van het.

Op basis daarvan kunnen, relying party toepassingen en -portals afhankelijk van hun context aangepast Azure AD B2C-beleid die gebruikmaken van de Identity-Ervaringsframework doorgeven van de naam van een specifiek beleid aanroepen en nauwkeurig de gedrag en informatie uitwisseling ophalen ze willen zonder wirwar, fuss of risico's.
