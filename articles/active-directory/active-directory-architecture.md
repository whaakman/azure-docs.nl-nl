---
title: Azure Active Directory-architectuur begrijpen | Microsoft Docs
description: Hierin wordt uitgelegd wat een Azure AD-tenant is en hoe u Azure beheert via Azure Active Directory
services: active-directory
documentationcenter: 
author: MarkusVi
writer: v-lorisc
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/02/2017
ms.author: markvi
ms.openlocfilehash: 50dad848cfbdab7f5b1fff0fcec3b5f754e6ae74
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="understand-azure-active-directory-architecture"></a>Azure Active Directory-architectuur begrijpen
Met Azure AD (Azure Active Directory) kunt u veilig de toegang tot Azure-services en -resources beheren voor uw gebruikers. Azure AD omvat een volledige suite met mogelijkheden voor identiteitsbeheer. Zie [Wat is Azure Active Directory?](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-whatis) voor meer informatie over de functies van Azure AD.

Met Azure AD kunt u gebruikers en groepen maken en beheren, en machtigingen inschakelen om toegang tot bedrijfsresources te verlenen of te weigeren. Zie [The fundamentals of Azure identity management](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals-identity) (De grondbeginselen van Azure-identiteitsbeheer) voor meer informatie over identiteitsbeheer.

## <a name="azure-ad-architecture"></a>Azure AD-architectuur
De geografisch verspreide architectuur van Azure AD combineert uitgebreide controle, geautomatiseerde omleidingen, failover en herstelmogelijkheden. Hierdoor kunnen we onze klanten beschikbaarheid en prestaties op bedrijfsniveau bieden.

In dit artikel worden de volgende elementen van de architectuur besproken:
 *  Servicearchitectuurontwerp
 *  Schaalbaarheid 
 *  Continue beschikbaarheid
 *  Datacenters

### <a name="service-architecture-design"></a>Servicearchitectuurontwerp
De meest voorkomende manier om een schaalbaar, maximaal beschikbaar en gegevensrijk systeem te bouwen, is met onafhankelijke bouwstenen of schaaleenheden voor de Azure AD-gegevenslaag. Schaaleenheden worden *partities* genoemd. 

De gegevenslaag heeft meerdere front-end-services die mogelijkheden bieden voor lezen/schrijven. In het onderstaande diagram ziet u hoe de onderdelen van een partitie met één map worden verdeeld over geografisch verspreide datacenters. 

  ![Partities met één map](./media/active-directory-architecture/active-directory-architecture.png)

De onderdelen van Azure AD-architectuur omvatten een primaire replica en secundaire replica's.

**Primaire replica**

De *primaire replica* ontvangt alle *schrijfbewerkingen* voor de partitie waarbij deze hoort. Alle schrijfbewerkingen worden onmiddellijk gerepliceerd naar een secundaire replica in een ander datacenter, voordat de aanroeper een melding van slagen ontvangt. Op deze manier wordt de geografisch redundante duurzaamheid van schrijfbewerkingen verzekerd.

**Secundaire replica's**

Alle *leesbewerkingen* in een map worden afgehandeld vanuit *secundaire replica's*, die zich in datacenters op verschillende fysieke locaties bevinden. Er zijn veel secundaire replica's, omdat gegevens asynchroon worden gerepliceerd. Leesbewerkingen in een map, zoals verificatieaanvragen, worden afgehandeld vanuit datacenters in de buurt van onze klanten. De secundaire replica's zijn verantwoordelijk voor de schaalbaarheid van leesbewerkingen.

### <a name="scalability"></a>Schaalbaarheid

Schaalbaarheid is de mogelijkheid van een service om uit te breiden en zo te voldoen aan groeiende prestatievereisten. Schaalbaarheid van schrijfbewerkingen wordt bereikt door de gegevens te partitioneren. Schaalbaarheid van leesbewerkingen wordt bereikt door gegevens uit één partitie te repliceren naar meerdere secundaire replica's over de hele wereld.

Aanvragen van Directory-toepassingen worden meestal omgeleid naar het datacenter waar ze zich fysiek het dichtst bij bevinden. Schrijfbewerkingen worden transparant omgeleid naar de primaire replica voor lees-/schrijfconsistentie. Secundaire replica's breiden de schaal van partities aanzienlijk uit, omdat in de mappen doorgaans leesbewerkingen worden afgehandeld.

Directory-toepassingen maken verbinding met de dichtstbijzijnde datacenters. Hierdoor worden de prestaties verbeterd, wat uitschalen mogelijk maakt. Omdat een mappartitie meerdere secundaire replica's kan hebben, kunnen deze secundaire replica's dichter bij Directory-clients worden geplaatst. Alleen interne Directory-serviceonderdelen die veel schrijfbewerkingen afhandelen, zijn rechtstreeks gericht op de actieve primaire replica.

### <a name="continuous-availability"></a>Continue beschikbaarheid

Beschikbaarheid (of bedrijfstijd) definieert de mogelijkheid van een systeem om ononderbroken actief te zijn. De sleutel tot de hoge beschikbaarheid van Azure AD is dat onze services snel verkeer kunnen verplaatsen tussen meerdere geografisch verspreide datacenters. Elk datacenter is onafhankelijk. Dit maakt het gebruik van gedecorreleerde foutmodi mogelijk.

Het ontwerp van Azure AD-partities is vereenvoudigd in vergelijking met het AD-ontwerp voor bedrijven, wat essentieel is voor het opschalen van het systeem. We maken gebruik van een single-master-ontwerp dat een zorgvuldig geregisseerd en deterministisch failoverproces omvat voor de primaire replica.

**Fouttolerantie**

Een systeem is beschikbaarder als het tolerant is voor fouten in hardware, software en het netwerk. Voor elke mappartitie bestaat een maximaal beschikbare hoofdreplica: de primaire replica. Op deze replica worden alleen schrijfbewerkingen naar de partitie uitgevoerd. Deze replica wordt voortdurend en nauwlettend gecontroleerd. Indien er een fout wordt gedetecteerd, kunnen schrijfbewerkingen onmiddellijk worden verplaatst naar een andere replica (die dan de nieuwe primaire replica wordt). Tijdens de failover kan er een verlies van schrijfbeschikbaarheid optreden. Dit duurt meestal maar 1-2 minuten. De leesbeschikbaarheid wordt gedurende deze tijd niet beïnvloed.

Leesbewerkingen (die vele malen vaker voorkomen dan schrijfbewerkingen) worden alleen opgeslagen in secundaire replica's. Aangezien secundaire replica's idempotent zijn, kan het verlies van een van de replica's in een bepaalde partitie eenvoudig worden gecompenseerd door de leesbewerkingen naar een andere replica te leiden. Meestal is dit dan een replica in hetzelfde datacenter.

**Duurzaamheid van gegevens**

Een schrijfbewerking wordt definitief doorgevoerd in ten minste twee datacenters vóórdat deze wordt bevestigd. Dit gebeurt door de schrijfbewerking eerst door te voeren op de primaire replica en vervolgens onmiddellijk naar ten minste één ander datacenter te repliceren. Dit zorgt ervoor dat een eventueel onherstelbaar verlies van het datacenter dat voor de primaire replica als host fungeert, niet resulteert in gegevensverlies.

In Azure AD is sprake van een [RTO (beoogde hersteltijd)](https://en.wikipedia.org/wiki/Recovery_time_objective) van nul voor het uitgeven van tokens en voor leesbewerkingen in mappen, en van slechts enkele minuten (ongeveer 5) voor schrijfbewerkingen in mappen. Er geldt ook een [RPO (herstelpuntdoel)](https://en.wikipedia.org/wiki/Recovery_point_objective) van nul en tijdens failovers gaan er geen gegevens verloren.

### <a name="data-centers"></a>Datacenters

Azure AD-replica's worden opgeslagen in datacenters over de hele wereld. Zie [Azure-datacenters](https://azure.microsoft.com/en-us/overview/datacenters) voor meer informatie.

Azure AD werkt met verschillende datacenters met de volgende kenmerken:

 * De gatewayservice omvat verificatie, Graph en andere AD-services. De taakverdeling van deze services wordt via de gateway beheerd. Als er via transactionele tests beschadigde servers worden gedetecteerd, wordt er automatisch een failover uitgevoerd. Op basis van deze tests wordt het verkeer dynamisch via de gateway naar de goede datacenters geleid.
 * Voor *leesbewerkingen* beschikt de map over secundaire replica's en bijbehorende front-end-services in een actief-actief-configuratie die in meerdere datacenters actief is. Mocht er een storing optreden die een heel datacenter treft, dan wordt het verkeer automatisch naar een ander datacenter geleid.
 *  Voor *schrijfbewerkingen* vindt voor de map failover plaats van de primaire (hoofd)replica naar verschillende datacenters, via geplande failoverprocedures (de nieuwe primaire replica wordt gesynchroniseerd naar de oude) of via failoverprocedures voor noodgevallen. Duurzaamheid van gegevens wordt bereikt door een willekeurige doorvoering te repliceren naar minstens twee datacenters.

**Gegevensconsistentie**

Het mapmodel wordt gekenmerkt door uiteindelijke consistentie. Een typisch probleem met verspreide asynchrone replicatiesystemen is dat de gegevens die van een bepaalde replica worden geretourneerd, mogelijk niet zijn bijgewerkt. 

Met behulp van een secundaire replica biedt Azure AD lees-/schrijfconsistentie voor toepassingen door schrijfbewerkingen naar de primaire replica te leiden en ze tegelijkertijd terug te halen naar de secundaire replica.

Voor toepassingsschrijfbewerkingen met behulp van de Graph-API of Azure AD wordt geen affiniteit onderhouden met een mapreplica voor lees-/schrijfconsistentie. De Azure AD Graph-service onderhoudt een logische sessie die affiniteit heeft met een secundaire replica die wordt gebruikt voor leesbewerkingen. Affiniteit wordt vastgelegd in een replicatoken dat met de Graph-service in cache wordt geplaatst met behulp van een gedistribueerde cache. Dit token wordt vervolgens gebruikt voor verdere bewerkingen in dezelfde logische sessie. 

 >[!NOTE]
 >Schrijfbewerkingen worden onmiddellijk gerepliceerd naar de secundaire replica waarop de leesbewerkingen van de logische sessie zijn weggeschreven.
 >

**Back-upbeveiliging**

De map implementeert voorlopig verwijderen, in plaats van definitief, voor gebruikers en tenants, wat eenvoudig herstel mogelijk maakt wanneer items per ongeluk worden verwijderd door een klant. Als uw tenantbeheerder per ongeluk gebruikers verwijdert, kunnen zij dit gemakkelijk ongedaan maken en de verwijderde gebruikers terugzetten. 

Met Azure AD worden dagelijkse back-ups van alle gegevens geïmplementeerd. Daarom kunnen gegevens bindend worden teruggezet in het geval van logische verwijderingen of beschadigingen. Voor onze gegevenslagen wordt gebruikgemaakt van codes voor het corrigeren van fouten, zodat er kan worden gecontroleerd op fouten en bepaalde typen schijffouten automatisch kunnen worden gecorrigeerd.

**Metrische gegevens en controles**

Voor het uitvoeren van een service met een hoge beschikbaarheid zijn uitstekende mogelijkheden voor metrische gegevens en controle vereist. Met Azure AD worden belangrijke metrische gegevens met betrekking tot de status van de service voortdurend geanalyseerd voor elk van de services. Metrische gegevens, controle en waarschuwingen worden doorlopend ontwikkeld en verfijnd voor elk scenario, zowel binnen de afzonderlijke Azure AD-service als op serviceoverstijgend niveau.

Als een Azure AD-service niet werkt zoals verwacht, wordt er onmiddellijk actie ondernomen om de functionaliteit zo snel mogelijk te herstellen. Van alle metrische gegevens die met Azure AD worden bijgehouden, is de snelheid waarmee problemen van klanten of problemen met live-sites worden gedetecteerd en opgelost, het belangrijkste. We investeren veel in controle en waarschuwingen om de detectietijd (TTD-doel: < 5 minuten) te minimaliseren en in operationele paraatheid om de hersteltijd (TTM-doel: < 30 minuten) zo kort mogelijk te houden.

**Veilige bewerkingen**

We gebruiken operationele besturingselementen zoals MFA (Multi-Factor Authentication), zowel voor elke bewerking afzonderlijk als om alle bewerkingen te controleren. Daarnaast gebruiken we een Just-in-time-elevationsysteem om de benodigde tijdelijke toegang te verlenen voor alle operationele taken op aanvraag die momenteel worden uitgevoerd. Zie [De vertrouwde cloud](https://azure.microsoft.com/en-us/support/trust-center) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
[Ontwikkelaarshandleiding voor Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-developers-guide)

