---
title: Overzicht van de architectuur - Azure Active Directory | Microsoft Docs
description: Leer wat een Azure Active Directory-tenant is en het beheren van Azure met behulp van Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 08/23/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4185f4ad83d083ab1a15208361ee7b44e118c68e
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56168303"
---
# <a name="what-is-the-azure-active-directory-architecture"></a>Wat is de Azure Active Directory-architectuur?
Met Azure AD (Azure Active Directory) kunt u veilig de toegang tot Azure-services en -resources beheren voor uw gebruikers. Azure AD omvat een volledige suite met mogelijkheden voor identiteitsbeheer. Zie [Wat is Azure Active Directory?](active-directory-whatis.md) voor meer informatie over de functies van Azure AD.

Met Azure AD kunt u gebruikers en groepen maken en beheren, en machtigingen inschakelen om toegang tot bedrijfsresources te verlenen of te weigeren. Zie [De grondbeginselen van Azure-identiteitsbeheer](active-directory-whatis.md) voor meer informatie over identiteitsbeheer.

## <a name="azure-ad-architecture"></a>Azure AD-architectuur
Geografisch verspreide architectuur van Azure AD combineert uitgebreide controle, geautomatiseerde omleidingen, failover en herstel mogelijkheden, die het hele bedrijf beschikbaarheid en prestaties aan klanten leveren.

In dit artikel worden de volgende elementen van de architectuur besproken:
 *  Servicearchitectuurontwerp
 *  Gebruiksgemak 
 *  Continue beschikbaarheid
 *  Datacenters

### <a name="service-architecture-design"></a>Servicearchitectuurontwerp
De meest voorkomende manier om te bouwen, een toegankelijk en kan worden gebruikt en gegevensrijk systeem is tot en met onafhankelijke bouwstenen of schaaleenheden. Voor de Azure AD-gegevenslaag, eenheden heten *partities*. 

De gegevenslaag heeft meerdere front-end-services die mogelijkheden bieden voor lezen/schrijven. Het volgende diagram laat zien hoe de onderdelen van een partitie met één map worden geleverd in de gehele geografisch gedistribueerde datacenters. 

  ![Partities met één map](./media/active-directory-architecture/active-directory-architecture.png)

De onderdelen van Azure AD-architectuur omvatten een primaire replica en secundaire replica's.

**Primaire replica**

De *primaire replica* ontvangt alle *schrijfbewerkingen* voor de partitie waarbij deze hoort. Alle schrijfbewerkingen worden onmiddellijk gerepliceerd naar een secundaire replica in een ander datacenter, voordat de aanroeper een melding van slagen ontvangt. Op deze manier wordt de geografisch redundante duurzaamheid van schrijfbewerkingen verzekerd.

**Secundaire replica's**

Alle *leesbewerkingen* in een map worden afgehandeld vanuit *secundaire replica's*, die zich in datacenters op verschillende fysieke locaties bevinden. Er zijn veel secundaire replica's, omdat gegevens asynchroon worden gerepliceerd. Leesbewerkingen in een map, zoals verificatieaanvragen, worden afgehandeld vanuit datacenters die zich dicht bij klanten. De secundaire replica's zijn verantwoordelijk voor de schaalbaarheid van leesbewerkingen.

### <a name="scalability"></a>Schaalbaarheid

Schaalbaarheid is de mogelijkheid van een service om uit te breiden en zo te voldoen aan groeiende prestatievereisten. Schaalbaarheid van schrijfbewerkingen wordt bereikt door de gegevens te partitioneren. Schaalbaarheid van leesbewerkingen wordt bereikt door gegevens uit één partitie te repliceren naar meerdere secundaire replica's over de hele wereld.

Aanvragen van directory-toepassingen worden doorgestuurd naar het datacenter waar ze zich fysiek die het dichtst bij. Schrijfbewerkingen worden transparant omgeleid naar de primaire replica voor lees-/schrijfconsistentie. Secundaire replica's breiden de schaal van partities aanzienlijk uit, omdat in de mappen doorgaans leesbewerkingen worden afgehandeld.

Directory-toepassingen maken verbinding met de dichtstbijzijnde datacenters. Deze verbinding verbetert de prestaties en uitschalen is mogelijk. Omdat een mappartitie meerdere secundaire replica's kan hebben, kunnen deze secundaire replica's dichter bij Directory-clients worden geplaatst. Alleen interne Directory-serviceonderdelen die veel schrijfbewerkingen afhandelen, zijn rechtstreeks gericht op de actieve primaire replica.

### <a name="continuous-availability"></a>Continue beschikbaarheid

Beschikbaarheid (of bedrijfstijd) definieert de mogelijkheid van een systeem om ononderbroken actief te zijn. De sleutel voor hoge beschikbaarheid van Azure AD is dat de services snel verkeer tussen meerdere geografisch verspreide datacenters verplaatsen kunnen. Elk datacenter is onafhankelijk. Dit maakt het gebruik van gedecorreleerde foutmodi mogelijk.

Ontwerp van Azure AD-partities is vereenvoudigd in vergelijking met de enterprise AD ontwerp met behulp van een single-master-ontwerp dat een primaire replica voor zorgvuldig Geregisseerd en deterministisch failoverproces omvat.

**Fouttolerantie**

Een systeem is beschikbaarder als het tolerant is voor fouten in hardware, software en het netwerk. Voor elke partitie in de directory bestaat een maximaal beschikbare hoofdreplica: De primaire replica. Op deze replica worden alleen schrijfbewerkingen naar de partitie uitgevoerd. Deze replica wordt voortdurend en nauwlettend gecontroleerd. Indien er een fout wordt gedetecteerd, kunnen schrijfbewerkingen onmiddellijk worden verplaatst naar een andere replica (die dan de nieuwe primaire replica wordt). Tijdens de failover kan er een verlies van schrijfbeschikbaarheid optreden. Dit duurt meestal maar 1-2 minuten. De leesbeschikbaarheid wordt gedurende deze tijd niet beïnvloed.

Leesbewerkingen (die vele malen vaker voorkomen dan schrijfbewerkingen) worden alleen opgeslagen in secundaire replica's. Aangezien secundaire replica's idempotent zijn, kan het verlies van een van de replica's in een bepaalde partitie eenvoudig worden gecompenseerd door de leesbewerkingen naar een andere replica te leiden. Meestal is dit dan een replica in hetzelfde datacenter.

**Duurzaamheid van gegevens**

Een schrijfbewerking wordt definitief doorgevoerd in ten minste twee datacenters vóórdat deze wordt bevestigd. Dit gebeurt door de schrijfbewerking eerst door te voeren op de primaire replica en vervolgens onmiddellijk naar ten minste één ander datacenter te repliceren. Met deze schrijfactie zorgt ervoor dat een eventueel onherstelbaar verlies van het datacenter die als host fungeert voor de primaire niet tot verlies van gegevens leidt.

Azure AD onderhoudt een nul [Recovery Time Objective (RTO)](https://en.wikipedia.org/wiki/Recovery_time_objective) niet gegevensverlies tijdens failovers. Dit omvat:
-  Token-uitgifte en leesbewerkingen in een map
-  Zodat u slechts circa 5 minuten RTO voor directory-schrijfbewerkingen

### <a name="data-centers"></a>Datacenters

Azure AD-replica's worden opgeslagen in datacenters over de hele wereld. Zie [Azure-datacenters](https://azure.microsoft.com/overview/datacenters) voor meer informatie.

Azure AD werkt met verschillende datacenters met de volgende kenmerken:

 * Verificatie, Graph en andere AD-services bevinden zich achter de Gateway-service. De taakverdeling van deze services wordt via de gateway beheerd. Dit wordt automatische failover als een beschadigde servers worden gedetecteerd via transactionele tests. Op basis van deze tests wordt het verkeer dynamisch via de gateway naar de goede datacenters geleid.
 * Voor *leesbewerkingen* beschikt de map over secundaire replica's en bijbehorende front-end-services in een actief-actief-configuratie die in meerdere datacenters actief is. Mocht er een storing optreden die een heel datacenter treft, dan wordt het verkeer automatisch naar een ander datacenter geleid.
 *  Voor *schrijft*, de map primaire (hoofd) replica over verschillende datacenters worden overgebracht via geplande (nieuwe primaire wordt gesynchroniseerd naar de oude) of failoverprocedures. Duurzaamheid van gegevens wordt bereikt door een willekeurige doorvoering te repliceren naar minstens twee datacenters.

**Gegevensconsistentie**

Het model van de directory is een van de uiteindelijke consistenties. Een typisch probleem met verspreide asynchrone replicatiesystemen is dat de gegevens die worden geretourneerd van een "name" replica niet bijgewerkt zijn kan. 

Met behulp van een secundaire replica biedt Azure AD lees-/schrijfconsistentie voor toepassingen door schrijfbewerkingen naar de primaire replica te leiden en ze tegelijkertijd terug te halen naar de secundaire replica.

Voor toepassingsschrijfbewerkingen met behulp van de Graph-API of Azure AD wordt geen affiniteit onderhouden met een mapreplica voor lees-/schrijfconsistentie. De Azure AD Graph-service onderhoudt een logische sessie die affiniteit heeft met een secundaire replica die wordt gebruikt voor leesbewerkingen. Affiniteit wordt vastgelegd in een replicatoken dat met de Graph-service in cache wordt geplaatst met behulp van een gedistribueerde cache. Dit token wordt vervolgens gebruikt voor verdere bewerkingen in dezelfde logische sessie. 

 >[!NOTE]
 >Schrijfbewerkingen worden onmiddellijk gerepliceerd naar de secundaire replica waarop de leesbewerkingen van de logische sessie zijn weggeschreven.
 >

**Back-upbeveiliging**

De map implementeert voorlopig verwijderen, in plaats van definitief, voor gebruikers en tenants, wat eenvoudig herstel mogelijk maakt wanneer items per ongeluk worden verwijderd door een klant. Als uw tenantbeheerder per ongeluk gebruikers verwijdert, kunnen ze gemakkelijk ongedaan maken en de verwijderde gebruikers terugzetten. 

Met Azure AD worden dagelijkse back-ups van alle gegevens geïmplementeerd. Daarom kunnen gegevens bindend worden teruggezet in het geval van logische verwijderingen of beschadigingen. De gegevenslaag maakt gebruik van fouten corrigeren codes, zodat deze kan controleren op fouten en bepaalde typen schijffouten automatisch worden gecorrigeerd.

**Metrische gegevens en controles**

Voor het uitvoeren van een service met een hoge beschikbaarheid zijn uitstekende mogelijkheden voor metrische gegevens en controle vereist. Met Azure AD worden belangrijke metrische gegevens met betrekking tot de status van de service voortdurend geanalyseerd voor elk van de services. Er is ook doorlopende ontwikkeling en het afstemmen van metrische gegevens en het bewaking en waarschuwingen voor elk scenario, binnen elke Azure AD-service en tussen alle services.

Als een Azure AD-service niet werkt zoals verwacht, wordt onmiddellijk actie ondernomen om de functionaliteit zo snel mogelijk te herstellen. De belangrijkste metrische gegevens van Azure AD-sporen is hoe snel live site problemen kunnen worden gedetecteerd en verholpen voor klanten. We investeren veel in controle en waarschuwingen om de detectietijd (TTD-doel: < 5 minuten) te minimaliseren en in operationele paraatheid om de hersteltijd (TTM-doel: < 30 minuten) zo kort mogelijk te houden.

**Veilige bewerkingen**

Met behulp van operationele besturingselementen zoals multi-factor authentication (MFA) voor elke bewerking, evenals controle van alle bewerkingen. Bovendien met behulp van een just-in-time-elevationsysteem om benodigde tijdelijke toegang voor alle operationele taken op aanvraag regelmatig te verlenen. Zie [De vertrouwde cloud](https://azure.microsoft.com/support/trust-center) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
[Ontwikkelaarshandleiding voor Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)

