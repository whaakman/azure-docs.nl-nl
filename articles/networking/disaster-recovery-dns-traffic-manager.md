---
title: Herstel na noodgevallen met Azure DNS en Traffic Manager | Microsoft Docs
description: Overzicht van de noodhersteloplossingen met behulp van Azure DNS en Traffic Manager.
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/08/2018
ms.author: kumud
ms.openlocfilehash: ec252c1f45e5c27f17b725f6ab68cc94f67897c4
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58120734"
---
# <a name="disaster-recovery-using-azure-dns-and-traffic-manager"></a>Herstel na noodgevallen met Azure DNS en Traffic Manager

Herstel na noodgevallen is gericht op het herstellen van een ernstig verlies van de functionaliteit van de toepassing. Als u wilt ervoor kiest een oplossing voor noodherstel, eigenaren van zakelijke en technologische moeten eerst bepalen het niveau van de functionaliteit die is vereist tijdens een noodgeval, zoals - niet beschikbaar, gedeeltelijk beschikbaar via de verminderde functionaliteit of vertraagde beschikbaarheid, of volledig beschikbaar.
De meeste zakelijke klanten kiezen een architectuur met meerdere regio's voor bescherming tegen een toepassing of het niveau failover-infrastructuur. Klanten kunnen verschillende benaderingen kiezen in de zoektocht om failover- en hoge beschikbaarheid via redundante architectuur te realiseren. Hier volgen enkele van de populaire methoden:

- **Actief-passief met koude stand-by**: In deze failoveroplossing zijn de VM's en andere apparaten die worden uitgevoerd in de stand-by-regio niet actief totdat er een nodig voor failover. De productie-omgeving wordt echter wel gerepliceerd in de vorm van back-ups, VM-installatiekopieën of Resource Manager-sjablonen, naar een andere regio. Dit mechanisme failover rendabel is, maar duurt het langer om uit te voeren een volledige failover.
 
    ![Actief/passief met koude stand-by](./media/disaster-recovery-dns-traffic-manager/active-passive-with-cold-standby.png)
    
    *Afbeelding - actief/passief met koude stand-by-noodherstelconfiguratie*

- **Actief/passief met Controlelampje**: In deze failoveroplossing de stand-by-omgeving instellen met een minimale configuratie. De instelling heeft alleen de benodigde services die ter ondersteuning van slechts een minimale en kritieke set toepassingen worden uitgevoerd. In hun oorspronkelijke vorm, kan in dit scenario alleen minimale functies uitvoeren, maar kunt omhoog schalen en extra services naar het grootste deel van de productiebelasting te nemen als er een failover optreedt produceren.
    
    ![Actief/passief met Controlelampje](./media/disaster-recovery-dns-traffic-manager/active-passive-with-pilot-light.png)
    
    *Afbeelding: Actief/passief met Controlelampje noodherstelconfiguratie*

- **Actief/passief met warme stand-by**: In deze failoveroplossing de stand-by-regio is vooraf verwarmde en gereed is voor de basis belasting, automatisch schalen is ingeschakeld en alle exemplaren actief zijn. Deze oplossing wordt niet geschaald als u wilt maken van de volledige productie-belasting, maar werkt en alle services zijn actief en werkend. Deze oplossing is een uitgebreide versie van het Controlelampje benadering.
    
    ![Actief/passief met warme stand-by](./media/disaster-recovery-dns-traffic-manager/active-passive-with-warm-standby.png)
    
    *Afbeelding: Actief/passief met warme stand-by-noodherstelconfiguratie*
    
Zie voor meer informatie over failover en hoge beschikbaarheid, [herstel na noodgevallen voor Azure-toepassingen](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications).


## <a name="planning-your-disaster-recovery-architecture"></a>Planning van uw disaster recovery-architectuur

Er zijn twee technische aspecten voor het instellen van uw disaster recovery-architectuur:
-  Met behulp van een implementatiemechanisme voor het repliceren van instanties, gegevens en configuraties tussen primaire en stand-by-omgevingen. Dit type herstel na noodgevallen kan worden gedaan systeemeigen via Azure Site-Recovery via Microsoft Azure-partner apparaten/services, zoals Veritas of NetApp. 
- Het ontwikkelen van een oplossing voor het omleiden van netwerk/webverkeer van de primaire site naar de stand-by-site. Dit type herstel na noodgevallen kan worden bereikt via de Azure DNS, Azure verkeer Manager(DNS) of externe globale load balancers.

In dit artikel is beperkt tot methoden via netwerk- en Web omleiden van verkeer. Zie voor instructies voor het instellen van Azure Site Recovery [documentatie over Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/).
DNS is een van de meest efficiënte mechanismen voor het netwerkverkeer omleiden omdat DNS vaak globale en buiten het datacenter is en is geïsoleerd van beschikbaarheid of regionale zone (AZ) op fouten. Een failover op basis van een DNS-mechanisme kunnen worden gebruikt en in Azure, twee DNS-services moeten hetzelfde zijn op enige wijze - Azure DNS (gezaghebbende DNS) en Azure Traffic Manager (routering van slimme verkeer op basis van DNS) kunnen uitvoeren. 

Het is belangrijk te weten van enkele concepten in DNS die veel worden gebruikt voor de oplossingen die zijn opgegeven in dit artikel worden besproken:
- **DNS A-Record** – A-Records zijn aanwijzers die een domein naar een IPv4-adres verwijzen. 
- **Naam van CNAME- of Canonical** -dit type record wordt gebruikt om te verwijzen naar een andere DNS-record. CNAME reageert niet met een IP-adres, maar in plaats daarvan de muisaanwijzer op de record die het IP-adres bevat. 
- **Gewogen Routing** – één kunt kiezen om te koppelen van een gewicht naar service-eindpunten en distribueert u vervolgens het verkeer op basis van de toegewezen waarden. Deze methode is een van de vier traffic routing mechanismen beschikbaar in Traffic Manager. Zie voor meer informatie, [routeringsmethode gewogen](../traffic-manager/traffic-manager-routing-methods.md#weighted).
- **Prioriteit Routing** – prioriteit routering is gebaseerd op statuscontroles van eindpunten. Standaard Azure Traffic manager verzendt alle verkeer naar het prioriteitseindpunt van de hoogste en bij een fout of noodgeval, Traffic Manager het verkeer routeert naar het secundaire eindpunt. Zie voor meer informatie, [prioriteit routeringsmethode](../traffic-manager/traffic-manager-routing-methods.md#priority).

## <a name="manual-failover-using-azure-dns"></a>Handmatige failover met behulp van Azure DNS
De Azure DNS handmatige failover-oplossing voor herstel na noodgevallen maakt gebruik van de standaard DNS-mechanisme voor failover naar de back-site. De handmatige optie via Azure DNS werkt het beste bij gebruik in combinatie met de koude stand-by of het Controlelampje benadering. 

![Handmatige failover met behulp van Azure DNS](./media/disaster-recovery-dns-traffic-manager/manual-failover-using-dns.png)

*Afbeelding - handmatige failover met behulp van Azure DNS*

De aanbevelingen voor de oplossing zijn:
- Primaire en secundaire eindpunten hebben statische IP-adressen die niet vaak te wijzigen. Stel dat u voor de primaire site het IP-adres is 100.168.124.44 en het IP-adres voor de secundaire site is 100.168.124.43.
- Er bestaat een Azure DNS-zone voor de primaire en secundaire site. Stel dat voor de primaire site het eindpunt is prod.contoso.com en is voor de back-upsite dr.contoso.com. Een DNS-record voor de hoofdtoepassing bekend als www\.contoso.com ook bestaat.   
- De TTL is op of onder de RTO SLA instellen in de organisatie. Bijvoorbeeld, als een onderneming stelt de RTO van het antwoord van de toepassing na noodgevallen om de 60 minuten, worden dan de TTL-waarde moet lager zijn dan 60 minuten, bij voorkeur de laagste hoe beter. 
  U kunt Azure DNS instellen voor handmatige failover als volgt:
- Een DNS-zone maken
- DNS-zone-records maken
- CNAME-record bijwerken

### <a name="step-1-create-a-dns"></a>Stap 1: Een DNS-server maken
Een DNS-zone maken (bijvoorbeeld, www\.contoso.com), zoals hieronder weergegeven:

![Een DNS-zone maken in Azure](./media/disaster-recovery-dns-traffic-manager/create-dns-zone.png)

*Afbeelding: een DNS-zone maken in Azure*

### <a name="step-2-create-dns-zone-records"></a>Stap 2: DNS-zone-records maken

In deze zone drie records maken (bijvoorbeeld - www\.contoso.com, prod.contoso.com en dr.consoto.com) als hieronder weergeven.

![DNS-zone-records maken](./media/disaster-recovery-dns-traffic-manager/create-dns-zone-records.png)

*Afbeelding - records van DNS-zone maken in Azure*

In dit scenario, de site, de WWW-\.contoso.com heeft een TTL-waarde van 30 minuten, die goed onder het vermelde RTO, en verwijst naar de productie-site prod.contoso.com. Deze configuratie is tijdens de normale bedrijfsactiviteiten. De TTL van prod.contoso.com en dr.contoso.com is ingesteld op 300 seconden of 5 minuten. Kunt u een Azure-service zoals Azure Monitor of Azure App Insights te controleren of een partner bewakingsoplossingen zoals Dynatrace, u kunt zelfs home proces oplossingen die u kunnen controleren of toepassing of virtuele infrastructuur niveau fouten detecteren.

### <a name="step-3-update-the-cname-record"></a>Stap 3: De CNAME-record bijwerken

Zodra de storing wordt gedetecteerd, wijzigt u de recordwaarde om te verwijzen naar dr.contoso.com zoals hieronder wordt weergegeven:
       
![CNAME-record bijwerken](./media/disaster-recovery-dns-traffic-manager/update-cname-record.png)

*Afbeelding: bijwerken van het CNAME-record in Azure*

Binnen 30 minuten, gedurende welke de meeste resolvers het zonebestand in de cache wordt vernieuwd een query om toegang te www\.contoso.com wordt omgeleid naar dr.contoso.com.
U kunt ook de volgende Azure CLI-opdracht om de CNAME-waarde te wijzigen uitvoeren:
 ```azurecli
   az network dns record-set cname set-record \
   --resource-group 123 \
   --zone-name contoso.com \
   --record-set-name www \
   --cname dr.contoso.com
```
Deze stap kan worden uitgevoerd, handmatig of via automation. Deze kan handmatig worden gedaan via de console of door de Azure CLI. De Azure SDK en de API kunnen worden gebruikt om de CNAME-update automatisch zodat geen handmatige tussenkomst vereist is. Automation kan worden gebouwd via Azure functions of binnen een toepassing voor de bewaking van derden of zelfs van on-premises.

### <a name="how-manual-failover-works-using-azure-dns"></a>Hoe handmatige failover werkt met behulp van Azure DNS
Omdat de DNS-server buiten de zone van de failover of na noodgevallen, is het geïsoleerd tegen uitvaltijd. Dit kan kan de gebruiker voor het ontwerpen van een eenvoudige failover-scenario die betaalbaar en alle de tijd ervan uitgaande dat de operator heeft een netwerkverbinding tijdens noodgeval werkt en het spiegelen. Als de oplossing is in een script vastgelegd, klikt u vervolgens een moet ervoor zorgen dat de server of -service die het script is uitgevoerd op basis van het probleem die betrekking hebben op de productie-omgeving moet worden geïsoleerd. Houd er ook rekening met de laag TTL-waarde die is ingesteld op basis van de zone, zodat er is geen resolver over de hele wereld het eindpunt in de cache opgeslagen voor de lange blijft en klanten toegang de site binnen de RTO bepaalt tot. Voor een koude stand-by en het Controlelampje, omdat sommige prewarming en andere administratieve activiteit is mogelijk vereist: geeft een ook voldoende tijd voordat het spiegelen.

## <a name="automatic-failover-using-azure-traffic-manager"></a>Automatische failover met behulp van Azure Traffic Manager
Wanneer u complexe architecturen en meerdere sets met bronnen staat uit te voeren dezelfde functie hebt, kunt u Azure Traffic Manager (op basis van DNS) configureren om te controleren van de status van uw resources en het verkeer van de resource niet in orde zijn op de in orde de bron. Hebben een volledige implementatie in het volgende voorbeeld wordt zowel de primaire regio als de secundaire regio. Deze implementatie omvat cloudservices en een gesynchroniseerde database. 

![Automatische failover met behulp van Azure Traffic Manager](./media/disaster-recovery-dns-traffic-manager/automatic-failover-using-traffic-manager.png)

*Afbeelding - automatische failover met behulp van Azure Traffic Manager*

Alleen de primaire regio is echter actief verwerken van netwerkaanvragen van de gebruikers. De secundaire regio actief alleen wanneer de primaire regio optreedt in een service wordt onderbroken. In dat geval alle nieuwe netwerkaanvragen gerouteerd naar de secundaire regio. Sinds de back-up van de database is bijna onmiddellijk, zowel de load balancers zijn IP-adressen die de gezondheid van dit selectievakje inschakelt, kan worden en de exemplaren zijn altijd omhoog en uitgevoerd, deze topologie biedt een optie voor gaan voor een lage RTO en failover zonder handmatige tussenkomst. De failoverregio secundaire moet klaar zijn met go live onmiddellijk na het mislukken van de primaire regio.
In dit scenario is ideaal voor het gebruik van Azure Traffic Manager met ingebouwde tests voor verschillende soorten statuscontroles, waaronder http / https en TCP. Met Azure Traffic manager heeft ook een regel-engine die kan worden geconfigureerd voor failover wanneer er een fout optreedt, zoals hieronder wordt beschreven. Laten we eens de volgende oplossing met behulp van Traffic Manager:
- -Klant beschikt over het eindpunt van de regio 1 prod.contoso.com met een statisch IP-adres als 100.168.124.44 en een regio #2-eindpunt dr.contoso.com wel met een statisch IP-adres als 100.168.124.43 genoemd. 
-   Elk van deze omgevingen wordt fronted via een openbare gerichte eigenschap, zoals een load balancer. De load balancer kan worden geconfigureerd met een eindpunt op basis van een DNS- of een volledig gekwalificeerde domeinnaam (FQDN) zoals hierboven.
-   Alle exemplaren in de regio 2 zijn in bijna realtime replicatie met de regio 1. Bovendien de installatiekopieën voor virtuele machines zijn bijgewerkt, en alle software-/ configuratiegegevens is een patch uitgevoerd en zijn in overeenstemming met de regio 1.  
-   Automatisch schalen vooraf is vooraf geconfigureerd. 

De stappen voor het configureren van de failover met Azure Traffic Manager zijn als volgt:
1. Een nieuw Azure Traffic Manager-profiel maken
2. Eindpunten in Traffic Manager-profiel maken
3. Status controleren en failover-configuratie instellen

### <a name="step-1-create-a-new-azure-traffic-manager-profile"></a>Stap 1: Een nieuw Azure Traffic Manager-profiel maken
Een nieuwe Azure Traffic manager-profiel maken met de naam contoso123 en selecteer de routeringsmethode prioriteit. Als u een bestaande resourcegroep die u koppelen, wilt kunt u een bestaande resourcegroep hebt, anders wordt een nieuwe resourcegroep te maken.

![Traffic Manager-profiel maken](./media/disaster-recovery-dns-traffic-manager/create-traffic-manager-profile.png)
*afbeelding: een Traffic Manager-profiel maken*

### <a name="step-2-create-endpoints-within-the-traffic-manager-profile"></a>Stap 2: Eindpunten in Traffic Manager-profiel maken

In deze stap maakt u eindpunten die naar de productie- en disaster recovery sites verwijzen. Hier kiest u de **Type** als een extern eindpunt, maar als de resource wordt gehost in Azure, kunt u vervolgens kiezen **Azure-eindpunt** ook. Als u ervoor kiest **Azure-eindpunt**en selecteer vervolgens een **Doelresource** dat ofwel een **App Service** of een **openbaar IP-adres** die is toegewezen door Azure. De prioriteit is ingesteld als **1** omdat dit de primaire service voor de regio 1.
Op deze manier maakt u de disaster recovery-eindpunt in Traffic Manager ook.

![Disaster recovery-eindpunten maken](./media/disaster-recovery-dns-traffic-manager/create-disaster-recovery-endpoint.png)

*Afbeelding - disaster recovery-eindpunten maken*

### <a name="step-3-set-up-health-check-and-failover-configuration"></a>Stap 3: Status controleren en failover-configuratie instellen

In deze stap stelt u de DNS TTL op 10 seconden, die wordt herkend door de meeste internetgerichte recursieve resolvers. Deze configuratie betekent dat er geen DNS-resolver cache worden opgeslagen door de gegevens van meer dan 10 seconden. Voor de monitor eindpuntinstellingen het pad naar de huidige set op is / of de hoofdmap, maar u kunt de eindpuntinstellingen voor de evaluatie van een pad, bijvoorbeeld prod.contoso.com/index aanpassen. Het voorbeeld hieronder toont de **https** als het testinterval-protocol. U kunt echter **http** of **tcp** ook. De keuze van protocol is afhankelijk van de toepassing. Het testinterval interval is ingesteld op 10 seconden, waarmee snelle scannen en de nieuwe poging is ingesteld op 3. Als gevolg hiervan, Traffic Manager wordt er een failover naar het tweede eindpunt als drie opeenvolgende intervallen registreert een fout. De volgende formule definieert de totale tijd voor een automatische failover: Tijd voor failover = TTL-waarde en probeer het opnieuw * Probing interval en in dit geval wordt de waarde is 10 + 3 * 10 = 40 seconden (Max.).
Als de nieuwe poging is ingesteld op 1 en TTL-waarde is ingesteld op 10 seconden, klikt u vervolgens de tijd voor failover 10 + 1 * 10 = 20 seconden. De nieuwe poging ingesteld op een waarde groter is dan **1** te elimineren kans failovers vanwege fout-positieven of een kleine netwerkproblemen. 


![Statuscontrole instellen](./media/disaster-recovery-dns-traffic-manager/set-up-health-check.png)

*Afbeelding - status controleren en failover-configuratie instellen*

### <a name="how-automatic-failover-works-using-traffic-manager"></a>Werking van automatische failover werkt met Traffic Manager

Tijdens een noodgeval, het primaire eindpunt opgehaald voortgangstekst en de status gewijzigd in **gedegradeerd** en de site voor noodherstel blijft **Online**. Standaard verzendt Traffic Manager alle verkeer naar het primaire eindpunt van de (hoogste prioriteit). Als het primaire eindpunt wordt gedegradeerd weergegeven, stuurt Traffic Manager het verkeer naar het tweede eindpunt, zolang deze in orde blijft. Een heeft de optie voor het configureren van meer eindpunten in Traffic Manager, dat kunnen fungeren als failover extra eindpunten of, als netwerktaakverdelers delen van de belasting tussen de eindpunten.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md).
- Meer informatie over [Azure DNS](../dns/dns-overview.md).









