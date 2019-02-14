---
title: Beveiliging - Azure Cosmos DB-database
description: Meer informatie over hoe Azure Cosmos DB biedt beveiliging en gegevens Databasebeveiliging voor uw gegevens.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/15/2017
ms.author: rimman
ms.openlocfilehash: 3d05da5f62a076dc168bef029cd0babc3946ee6b
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56243141"
---
# <a name="security-in-azure-cosmos-db---overview"></a>Beveiliging in Azure Cosmos DB - overzicht

In dit artikel wordt beschreven aanbevolen procedures voor database-beveiliging en belangrijke functies die worden aangeboden door Azure Cosmos DB om te voorkomen, detecteren en reageren op inbreuken op de database.
 
## <a name="whats-new-in-azure-cosmos-db-security"></a>Wat is er nieuw in Azure Cosmos DB-beveiliging?

Versleuteling-at-rest is nu beschikbaar voor documenten en back-ups die zijn opgeslagen in Azure Cosmos DB in alle Azure-regio's. Versleuteling-at-rest wordt automatisch toegepast voor zowel nieuwe als bestaande klanten in deze regio's. Er is niet nodig voor het configureren van alles zijn: en u krijgt de dezelfde geweldige latentie, doorvoer, beschikbaarheid en functionaliteit als voordat u met het voordeel van de wetenschap dat uw gegevens is veilig en beveiligen met versleuteling-at-rest.

## <a name="how-do-i-secure-my-database"></a>Hoe beveilig ik mijn database? 

Beveiliging van gegevens is een gedeelde verantwoordelijkheid tussen u, de klant en uw databaseprovider. Afhankelijk van de database-provider die u kiest, kan variëren van de hoeveelheid verantwoordelijkheid die u uitvoeren. Als u een on-premises oplossing kiest, moet u alles van eindpuntbescherming fysieke beveiliging van uw hardware - dit geen eenvoudige taak is opgeven. Als u ervoor geen databaseprovider van PaaS cloud zoals Azure Cosmos DB kiest, wordt het gebied van belang aanzienlijk kleiner. De volgende afbeelding is overgenomen uit Microsofts [gedeelde verantwoordelijkheden voor Cloud Computing](https://aka.ms/sharedresponsibility) whitepaper, ziet u hoe uw verantwoordelijkheid wordt verlaagd met een PaaS-provider, zoals Azure Cosmos DB.

![Klant- en database-provider verantwoordelijkheden](./media/database-security/nosql-database-security-responsibilities.png)

Het vorige diagram toont op hoog niveau cloud security-onderdelen, maar welke items moet u zorgen te maken over specifiek voor uw databaseoplossing? En hoe kunt u oplossingen met elkaar vergelijken? 

U wordt aangeraden de volgende controlelijst voor vereisten waarop u wilt vergelijken databasesystemen:

- Beveiliging van het netwerk en firewall-instellingen
- Verificatie via gebruikersnaam en goed korrelig gebruikersbesturingselementen
- Mogelijkheid om gegevens wereldwijd voor regionale storingen te repliceren
- Mogelijkheid om uit te voeren van failovers van één datacenter naar de andere
- Replicatie van lokale gegevens binnen een datacenter
- Automatische back-ups
- Het herstellen van verwijderde gegevens van back-ups
- Beveiligen en isoleren van de gevoelige gegevens
- Bewaking voor aanvallen
- Reageren op aanvallen
- Mogelijkheid tot geo-omheining gegevens om te voldoen aan de beperkingen voor het beheer van gegevens
- Fysieke beveiliging van servers in beveiligde datacenters
- Certificering

En hoewel het lijkt misschien duidelijk, recente [grootschalige database schendingen](https://thehackernews.com/2017/01/mongodb-database-security.html) herinneren dat we de eenvoudige maar kritiek belang van de volgende vereisten:
- Patch uitgevoerd voor servers die zijn bijgewerkt
- HTTPS door standaard/SSL-versleuteling
- Beheerdersaccounts met sterke wachtwoorden

## <a name="how-does-azure-cosmos-db-secure-my-database"></a>Hoe mijn database in Azure Cosmos DB beveiligen?

Laten we kijken weer de voorgaande lijst - hoeveel van de beveiligingsvereisten van de Azure Cosmos DB biedt? Elke enkel een.

Laten we even stilstaan bij elkaar in detail.

|Beveiligingsvereiste|Aanpak van Azure Cosmos-DB-beveiliging|
|---|---|---|
|Netwerkbeveiliging|Met behulp van een IP-firewall, is de eerste laag van beveiliging voor het beveiligen van uw database. Azure Cosmos DB ondersteunt basis van IP-gebaseerd toegangsbeheer voor firewallondersteuning van de inkomende-beleid. De besturingselementen voor toegang op basis van IP zijn vergelijkbaar met de firewall-regels die door traditionele databasesystemen gebruikt, maar ze zijn uitgevouwen, zodat een Azure Cosmos DB-databaseaccount alleen toegankelijk is vanaf een goedgekeurde set machines of cloudservices is. <br><br>Azure Cosmos DB kunt u mogelijk om een specifiek IP-adres (168.61.48.0), een IP-adresbereik (168.61.48.0/8) en combinaties van IP-adressen en -bereiken. <br><br>Alle aanvragen die afkomstig zijn van computers buiten deze toegestane lijst geblokkeerd door Azure Cosmos DB. Aanvragen van goedgekeurde machines en cloudservices vervolgens vult het verificatieproces uit als u wilt toegang krijgen tot de resources.<br><br>Meer informatie in [firewallondersteuning van Azure Cosmos DB](firewall-support.md).|
|Autorisatie|Azure Cosmos DB maakt gebruik van hash-gebaseerde bericht verificatiecode op te geven (HMAC) voor autorisatie. <br><br>Elke aanvraag wordt opgedeeld met behulp van de geheime sleutel en de volgende base-64 gecodeerde hash wordt verzonden met elke aanroep naar Azure Cosmos DB. Voor het valideren van de aanvraag, de Azure Cosmos DB-service maakt gebruik van de juiste geheime sleutel en de eigenschappen voor het genereren van een hash en vervolgens de waarde met de in de aanvraag worden vergeleken. Als de twee waarden overeenkomen met de bewerking is geautoriseerd en de aanvraag wordt verwerkt, anders wordt er is een Autorisatiefout en de aanvraag wordt afgewezen.<br><br>U kunt ofwel een [hoofdsleutel](secure-access-to-data.md#master-keys), of een [resourcetoken](secure-access-to-data.md#resource-tokens) fijnmazige toegang tot een resource, zoals een document toe te staan.<br><br>Meer informatie in [toegang tot Azure Cosmos DB-resources beveiligen](secure-access-to-data.md).|
|Gebruikers en machtigingen|Met behulp van de hoofdsleutel voor het account, kunt u Gebruikersresources en machtiging resources per database maken. Een resourcetoken is gekoppeld aan een machtiging in een database en bepaalt of de gebruiker heeft toegang (lezen / schrijven, alleen-lezen, of geen toegang) tot de bron van een toepassing in de database. Toepassingsresources omvatten container, documenten, bijlagen, opgeslagen procedures, triggers en UDF's. De resourcetoken wordt vervolgens gebruikt tijdens de verificatie om te geven of weigeren van toegang tot de resource.<br><br>Meer informatie in [toegang tot Azure Cosmos DB-resources beveiligen](secure-access-to-data.md).|
|Active directory-integratie (RBAC)| U kunt ook toegang bieden tot het account van de database met behulp van toegangsbeheer (IAM) in Azure portal, zoals wordt weergegeven in de schermopname onder deze tabel. IAM biedt op rollen gebaseerd toegangsbeheer en kan worden geïntegreerd met Active Directory. U kunt ingebouwde rollen of aangepaste rollen gebruiken voor individuen en groepen, zoals wordt weergegeven in de volgende afbeelding.|
|Globale replicatie|Azure Cosmos DB biedt kant en klare wereldwijde distributie, waarmee u uw gegevens gerepliceerd naar een van de world wide-datacenters van Azure met één klik op een knop. Globale replicatie kunt u wereldwijd schalen en lage latentie toegang bieden tot uw gegevens over de hele wereld.<br><br>In de context van beveiliging gegevensbeveiliging globale replicatie tegen regionale fouten.<br><br>Zie [Gegevens wereldwijd distribueren](distribute-data-globally.md) voor meer informatie.|
|Regionale failovers|Als u uw gegevens in meer dan één datacenter zijn gerepliceerd, Azure Cosmos DB automatisch wordt getotaliseerd via uw bewerkingen moet een regionaal datacenter offline gaan. U kunt een geprioriteerde lijst met failover-regio's met behulp van de regio's waarin uw gegevens worden gerepliceerd. <br><br>Meer informatie in [regionale Failovers in Azure Cosmos DB](high-availability.md).|
|Lokale replicatie|Zelfs binnen één datacentrum, Azure Cosmos DB automatisch worden gegevens gerepliceerd voor hoge beschikbaarheid, waarbij u de keuze van [consistentieniveaus](consistency-levels.md). Dit garandeert een 99,99% [beschikbaarheids-SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db) voor alle accounts voor één regio en alle accounts voor meerdere regio's met soepele consistentie en leesbeschikbaarheid van 99,999% beschikbaarheid voor alle databaseaccounts voor meerdere regio's.|
|Automatische online back-ups|Azure Cosmos DB-databases worden regelmatig een back-up gemaakt en opgeslagen in een winkel georedundant. <br><br>Meer informatie in [automatische online back-up en herstel met Azure Cosmos DB](online-backup-and-restore.md).|
|Terugzetten van verwijderde gegevens|De geautomatiseerde online back-ups kunnen worden gebruikt om gegevens die u hebt mogelijk per ongeluk verwijderd tot ongeveer 30 dagen na de gebeurtenis te herstellen. <br><br>Meer informatie in [automatische online back-up en herstel met Azure Cosmos DB](online-backup-and-restore.md)|
|Beveiligen en isoleren van de gevoelige gegevens|Alle gegevens in de regio's die worden vermeld in de wat is er nieuw? nu worden in rust versleuteld.<br><br>Persoonlijke gegevens en andere vertrouwelijke gegevens kunnen worden geïsoleerd in een specifieke container en lezen / schrijven, of alleen-lezen toegang kan worden beperkt tot specifieke gebruikers.|
|Monitor voor aanvallen|Met behulp van [logboekregistratie en activiteit auditlogboeken](logging.md), kunt u uw account voor normaal en abnormaal activiteit controleren. U kunt weergeven welke bewerkingen zijn uitgevoerd op uw resources, die de bewerking heeft gestart wanneer de bewerking is opgetreden, de status van de bewerking, en nog veel meer zoals weergegeven in de schermopname onder deze tabel.|
|Reageren op aanvallen|Nadat u hebt verbinding gemaakt met de ondersteuning van Azure om aan te melden bij een aanval, wordt een reactie op incidenten 5-stap-proces gestart. Het doel van de 5-proces is normaal servicebeveiliging en bewerkingen zo snel mogelijk herstellen nadat er een probleem is gedetecteerd en een onderzoek is gestart.<br><br>Meer informatie in [Microsoft Azure Security Response in de Cloud](https://aka.ms/securityresponsepaper).|
|Geofencing|Azure Cosmos DB garandeert gegevensbeheer voor soevereine regio's (bijvoorbeeld Duitsland, China, VS (overheid)).|
|Beveiligde faciliteiten|Gegevens in Azure Cosmos DB worden opgeslagen op SSD's in de beveiligde datacenters van Azure.<br><br>Meer informatie in [wereldwijde Microsoft-datacenters](https://www.microsoft.com/en-us/cloud-platform/global-datacenters)|
|HTTPS/SSL/TLS-versleuteling|Alle interacties voor de Azure Cosmos DB-client-naar-service zijn SSL/TLS 1.2 die geschikt. Alle intra datacenter en cross datacenter replicatie is ook SSL/TLS 1.2 afgedwongen.|
|Versleuteling 'at rest'|Alle gegevens die zijn opgeslagen in Azure Cosmos DB is versleuteld in rust. Meer informatie in [Azure Cosmos DB versleuteling-at-rest](./database-encryption-at-rest.md)|
|Gecorrigeerde servers|Als een beheerde database hoeft Azure Cosmos DB te beheren en vullen van de servers, die automatisch voor u heeft gedaan.|
|Beheerdersaccounts met sterke wachtwoorden|Het is moeilijk te geloven moeten we ook nog deze vereiste, maar in tegenstelling tot sommige van onze concurrenten, is het niet mogelijk om een Administrator-account zonder een wachtwoord in Azure Cosmos DB.<br><br> Beveiliging via SSL en HMAC geheime gebaseerde verificatie is standaard sparen.|
|Beveiliging en bescherming-certificeringen|Voor de meeste tot aan de gegevenslijst met van certificeringen, Zie de algemene [naleving van Azure site](https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings) en de meest recente [Azure conformiteitsdocument](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) met alle certificeringen (zoek Cosmos). Voor een meer gerichte leest u uitchecken van het bericht 25 April 2018 [Azure #CosmosDB: Veilige, particuliere en compatibele](https://azure.microsoft.com/blog/azure-cosmosdb-secure-private-compliant/) waaronder Soc's 1/2 Type 2, HITRUST, PCI DSS Level 1, ISO 27001, HIPAA, FedRAMP High en nog veel meer.

De volgende schermafbeelding ziet u Active directory-integratie (RBAC) met behulp van toegangsbeheer (IAM) in Azure portal: ![Toegangsbeheer (IAM) in Azure portal - beveiliging van de database aan te tonen](./media/database-security/nosql-database-security-identity-access-management-iam-rbac.png)

De volgende schermafbeelding ziet u hoe u logboekregistratie en activiteit van controlelogboeken voor het bewaken van uw account kunt gebruiken: ![Activiteitenlogboeken voor Azure Cosmos DB](./media/database-security/nosql-database-security-application-logging.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over hoofdsleutels en brontokens [toegang tot Azure Cosmos DB-gegevens beveiligen](secure-access-to-data.md).

Zie voor meer informatie over het vastleggen van controlegebeurtenissen [Diagnostische logboekregistratie van Azure Cosmos DB](logging.md).

Zie voor meer informatie over Microsoft-certificeringen [Azure Trust Center](https://azure.microsoft.com/support/trust-center/).
