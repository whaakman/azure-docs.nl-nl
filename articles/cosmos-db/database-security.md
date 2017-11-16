---
title: Beveiliging - Azure DB die Cosmos-database | Microsoft Docs
description: Meer informatie over hoe Azure Cosmos DB biedt beveiliging en gegevens Databasebeveiliging voor uw gegevens.
keywords: nosql-database beveiliging, informatiebeveiliging, beveiliging van gegevens, versleuteling van de database, databasebeveiliging, beveiligingsbeleid, beveiliging testen
services: cosmos-db
author: mimig1
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: a02a6a82-3baf-405c-9355-7a00aaa1a816
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: mimig
ms.openlocfilehash: 2f0f6578b14b2fdd3807303eb94df077df92ba77
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="azure-cosmos-db-database-security"></a>Beveiliging van Azure DB Cosmos-database

Dit artikel wordt beschreven database best practices voor beveiliging en belangrijke functies die worden aangeboden door Azure Cosmos DB om te detecteren, voorkomen van en reageren op schendingen van de database.
 
## <a name="whats-new-in-azure-cosmos-db-security"></a>Wat is nieuw in Azure DB die Cosmos-beveiliging?

Codering in rust is nu beschikbaar voor documenten en back-ups opgeslagen in Azure Cosmos DB in alle Azure-regio's. Codering in rust wordt automatisch toegepast op nieuwe en bestaande klanten in deze regio's. Hoeft niet te configureren; u krijgt de dezelfde geweldige latentie, doorvoer, beschikbaarheid en functionaliteit als voordat u met het voordeel van uw gegevens weten is veilig en beveiligd met versleuteling in rust.

## <a name="how-do-i-secure-my-database"></a>Hoe worden mijn database kunt beveiligen? 

Beveiliging van gegevens is een gedeelde verantwoordelijkheid tussen u, de klant en uw databaseprovider. Afhankelijk van de database-provider die u kiest, kan de hoeveelheid u blijven verantwoordelijkheid variëren. Als u een on-premises-oplossing kiest, moet u om alles uit de beveiliging van de eindpunt aan fysieke beveiliging van uw hardware - dit geen eenvoudige taak is te bieden. Als u een PaaS-cloud databaseprovider zoals Azure Cosmos DB kiest, wordt uw regio problematisch aanzienlijk verkleind. De volgende afbeelding van Microsoft geleend [gedeeld verantwoordelijkheden voor Cloud Computing](https://aka.ms/sharedresponsibility) witboek, ziet u hoe uw verantwoordelijkheid met een PaaS-provider, zoals Azure Cosmos DB afneemt.

![De verantwoordelijkheden van klant en database-provider](./media/database-security/nosql-database-security-responsibilities.png)

Het voorgaande diagram toont op hoog niveau cloud beveiligingsonderdelen, maar welke items moet u bang specifiek voor uw databaseoplossing? En hoe kunt u oplossingen met elkaar vergelijken? 

U wordt aangeraden de volgende controlelijst van vereisten op voor het vergelijken van databasesystemen:

- Netwerkbeveiliging en firewall-instellingen
- Verificatie van gebruikers en fijnmazige gebruikersbesturingselementen
- Mogelijkheid om gegevens voor regionale fouten in één keer te repliceren
- Mogelijkheid voor het uitvoeren van failovers uit één datacenter naar de andere
- Replicatie van de lokale gegevens binnen een datacentrum
- Automatische gegevensback-ups
- Herstellen van verwijderde gegevens vanuit back-ups
- Beveiligen en isoleren van gevoelige gegevens
- Bewaking voor aanvallen
- Reageren op aanvallen
- Mogelijkheid tot geo fence gegevens in overeenstemming zijn met data governance beperkingen
- Fysieke beveiliging van servers in beveiligde datacenters

Hoewel het lijkt misschien duidelijk, recente [grootschalige database schendingen](http://thehackernews.com/2017/01/mongodb-database-security.html) herinneren ons de eenvoudige maar kritiek belang van de volgende vereisten:
- Servers die actueel blijven hersteld
- HTTPS door standaard/SSL-versleuteling
- Beheerdersaccounts met sterke wachtwoorden

## <a name="how-does-azure-cosmos-db-secure-my-database"></a>Hoe Azure Cosmos DB mijn database beveiligen?

Bekijk opnieuw de voorgaande lijst - hoeveel van de beveiligingsvereisten Azure Cosmos DB biedt? Elke één een.

We gaan verdiepen in elk criterium in detail.

|Beveiligingsvereiste|Azure Cosmos-DB beveiliging benadering|
|---|---|---|
|Netwerkbeveiliging|Een IP-firewall gebruikt, is de eerste laag van beveiliging voor het beveiligen van uw database. Azure Cosmos DB ondersteunt beleid IP gebaseerd toegangsbeheer voor binnenkomende firewallondersteuning aangestuurd. De toegang op basis van IP-besturingselementen zijn vergelijkbaar met de firewallregels die wordt gebruikt door traditionele databasesystemen, maar ze zijn uitgevouwen zodat Azure DB die Cosmos-databaseaccount alleen toegankelijk vanuit een goedgekeurde reeks machines of cloudservices is. <br><br>Azure Cosmos DB kunt u een specifiek IP-adres (168.61.48.0), een IP-adresbereik (168.61.48.0/8) en combinatie van IP-adressen en -bereiken inschakelen. <br><br>Alle aanvragen die afkomstig zijn van computers buiten deze lijst met toegestane zijn geblokkeerd door Azure Cosmos DB. Aanvragen van goedgekeurde machines en vervolgens een cloud-services moeten het verificatieproces voor toegangsbeheer te krijgen tot de resources te voltooien.<br><br>Meer informatie [Azure DB die Cosmos-firewallondersteuning](firewall-support.md).|
|Autorisatie|Azure Cosmos DB waarbij hash op basis van een message authenticatiecode (HMAC) voor autorisatie. <br><br>Elke aanvraag wordt gehasht met behulp van de geheime sleutel en de volgende base-64 gecodeerde hash bij elke aanroep van Azure DB die Cosmos verzonden. Voor het valideren van de aanvraag de Azure DB die Cosmos-service gebruikt de juiste geheime sleutel en de eigenschappen voor het genereren van een hash en vervolgens de waarde met de structuur in de aanvraag worden vergeleken. Als de twee waarden overeenkomen, de bewerking met succes is geautoriseerd en de aanvraag wordt verwerkt, anders er is een Autorisatiefout en de aanvraag wordt geweigerd.<br><br>U kunt ofwel een [hoofdsleutel](secure-access-to-data.md#master-keys), of een [resource token](secure-access-to-data.md#resource-tokens) fijnmazig toegang tot een resource, zoals een document toe te staan.<br><br>Meer informatie [ongeoorloofde toegang tot resources in Azure Cosmos DB](secure-access-to-data.md).|
|Gebruikers en machtigingen|Met behulp van de [hoofdsleutel](#master-key) voor het account, kunt u Gebruikersbronnen en bronnen van de machtiging per database maken. Een [resource token](#resource-token) is gekoppeld aan een machtiging in een database en bepaalt of de gebruiker heeft toegang (lezen / schrijven, alleen-lezen, of geen toegang) tot een bron van de toepassing in de database. Toepassingsresources bevatten verzamelingen, documenten, bijlagen, opgeslagen procedures, triggers en UDF's. De resource-token wordt vervolgens gebruikt tijdens de verificatie te leveren of weigeren van toegang tot de resource.<br><br>Meer informatie [ongeoorloofde toegang tot resources in Azure Cosmos DB](secure-access-to-data.md).|
|Active directory-integratie (RBAC)| U kunt ook toegang tot het account van de database met behulp van toegangsbeheer (IAM) in de Azure portal opgeven, zoals weergegeven in de schermafbeelding na deze tabel. IAM biedt toegangsbeheer op basis van rollen en kan worden geïntegreerd met Active Directory. U kunt ingebouwde rollen of aangepaste rollen voor personen en groepen zoals weergegeven in de volgende afbeelding.|
|Globale replicatie|Azure Cosmos DB biedt klare globale distributie, waarmee u uw gegevens gerepliceerd naar een van de Azure wereldwijd datacenters met één klik op een knop. Globale replicatie kunt u globaal worden geschaald en lage latentie toegang tot uw gegevens over de hele wereld.<br><br>In de context van beveiliging, globale replicatie weet u zeker gegevensbeveiliging tegen regionale fouten.<br><br>Meer informatie [Distribueer gegevens globaal](distribute-data-globally.md).|
|Regionale failover|Als u uw gegevens in meer dan één datacenter zijn gerepliceerd, Azure Cosmos DB automatisch wordt getotaliseerd via uw bewerkingen moet een regionaal datacenter offline gaan. U kunt een geprioriteerde lijst met failover-regio's met behulp van de regio's waar uw gegevens worden gerepliceerd. <br><br>Meer informatie [regionale Failovers in Azure Cosmos DB](regional-failover.md).|
|Lokale replicatie|Zelfs binnen één datacenter, repliceert Azure Cosmos DB automatisch gegevens voor hoge beschikbaarheid zodat u de keuze van [consistentieniveaus](consistency-levels.md). Er wordt gegarandeerd dat een 99,99% [beschikbaarheids-SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db) voor alle accounts voor één regio en alle meerdere landen/regio-accounts met versoepeld consistentie en 99,999% beschikbaarheid voor alle accounts voor meerdere landen/regio-database lezen.|
|Geautomatiseerde online back-ups|Azure DB Cosmos-databases worden regelmatig een back-up gemaakt en opgeslagen in een archief georedundant. <br><br>Meer informatie [automatische online back-up en herstel met Azure Cosmos DB](online-backup-and-restore.md).|
|Gegevens herstellen die zijn verwijderd|De geautomatiseerde online back-ups kunnen worden gebruikt om gegevens die u mogelijk per ongeluk hebt verwijderd ongeveer 30 dagen na de gebeurtenis te herstellen. <br><br>Meer informatie [automatische online back-up en herstel met Azure Cosmos-DB](online-backup-and-restore.md)|
|Beveiligen en isoleren van gevoelige gegevens|Alle gegevens in de regio's die worden vermeld in [wat is er nieuw?](#whats-new) nu in rust versleuteld.<br><br>PII en andere vertrouwelijke gegevens kunnen worden geïsoleerd voor specifieke verzamelingen en alleen-lezen of alleen-lezen toegang kan worden beperkt tot specifieke gebruikers.|
|Monitor voor aanvallen|Met behulp van [logboekregistratie en activiteit controlelogboeken](logging.md), kunt u uw account voor normaal en abnormaal activiteit controleren. U kunt weergeven welke bewerkingen zijn uitgevoerd op uw resources die heeft de bewerking wordt gestart wanneer de bewerking is opgetreden, de status van de bewerking en nog veel meer zoals weergegeven in de schermafbeelding onder deze tabel.|
|Reageren op aanvallen|Zodra u hebt gemaakt voor de ondersteuning van Azure om te melden bij een aanval met wordt een 5-stap respons op incidenten-proces gestart. Het doel van de 5-proces is de normale werking beveiliging en werking zo snel mogelijk herstellen nadat er een probleem wordt gedetecteerd en onderzoek is gestart.<br><br>Meer informatie [reactie van Microsoft Azure-beveiliging in de Cloud](https://aka.ms/securityresponsepaper).|
|Geofencing|Azure Cosmos DB zorgt ervoor dat gegevens bestuur en naleving voor soevereine regio's (bijvoorbeeld Duitsland, China, ons Gov).|
|Beveiligde faciliteiten|Gegevens in Azure Cosmos-database is opgeslagen op SSD's in Azure beveiligde datacenters.<br><br>Meer informatie [globale datacenters van Microsoft](https://www.microsoft.com/en-us/cloud-platform/global-datacenters)|
|HTTPS/SSL/TLS-versleuteling|Alle client-naar-service Azure Cosmos DB interacties zijn SSL/TLS 1.2 afgedwongen. Alle intra datacenter en cross datacenter replicatie wordt ook SSL/TLS 1.2 afgedwongen.|
|Versleuteling 'at rest'|Alle gegevens die zijn opgeslagen in Azure Cosmos DB in rust versleuteld. Meer informatie [Azure DB die Cosmos-versleuteling in rust](.\database-encryption-at-rest.md)|
|Patches servers|Als de database van een beheerde hoeven Azure Cosmos DB te beheren en de patch-servers, die automatisch voor u heeft gedaan.|
|Beheerdersaccounts met sterke wachtwoorden|Het is moeilijk om te geloven zelfs moet worden vermeld in deze vereiste, maar in tegenstelling tot enkele van onze concurrenten, is het onmogelijk om een Administrator-account zonder wachtwoord hebben in Azure Cosmos DB.<br><br> Beveiliging via SSL en HMAC geheime gebaseerde verificatie is standaard uitbreidbaar standaard.|
|Beveiligings- en data protection certificeringen|Azure Cosmos DB heeft [ISO 27001](https://www.microsoft.com/en-us/TrustCenter/Compliance/ISO-IEC-27001), [Europese Model componenten (EUMC)](https://www.microsoft.com/en-us/TrustCenter/Compliance/EU-Model-Clauses), en [HIPAA](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA) certificeringen. Extra certificaten worden uitgevoerd.|

De volgende schermafbeelding ziet u Active directory-integratie (RBAC) met behulp van toegangsbeheer (IAM) in de Azure portal: ![toegangsbeheer (IAM) in de Azure portal - beveiliging van de database te demonstreren](./media/database-security/nosql-database-security-identity-access-management-iam-rbac.png)

De volgende schermafbeelding ziet u hoe u logboekregistratie en activiteit van controlelogboeken voor het bewaken van uw account kunt gebruiken: ![logboeken van de activiteit voor Azure Cosmos-DB](./media/database-security/nosql-database-security-application-logging.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over hoofdsleutels en resource tokens [ongeoorloofde toegang tot gegevens van Azure DB die Cosmos](secure-access-to-data.md).

Zie voor meer informatie over logboekregistratie, [Diagnostische logboekregistratie van Azure DB die Cosmos](logging.md).

Zie voor meer informatie over Microsoft certificeringen [Azure Vertrouwenscentrum](https://azure.microsoft.com/support/trust-center/).
