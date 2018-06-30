---
title: Herstel na noodgevallen met Azure DNS- en Traffic Manager | Microsoft Docs
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
ms.openlocfilehash: d608378f9b3ff3179f9e37ef13f88c65a645d018
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37112983"
---
# <a name="disaster-recovery-using-azure-dns-and-traffic-manager"></a>Herstel na noodgevallen met Azure DNS en Traffic Manager

Herstel na noodgevallen artikel gaat over het herstellen van een ernstige verlies aan functionaliteit van de toepassing. Als u wilt ervoor kiest een noodherstel, eigenaren van zakelijke en technologische moeten eerst bepalen het niveau van functionaliteit die nodig is tijdens een ramp, zoals - niet beschikbaar of gedeeltelijk beschikbaar via gereduceerde of later beschikbaar of volledig beschikbaar.
De meeste zakelijke klanten kiest een architectuur met meerdere regio voor tolerantie tegen een toepassing of het niveau failover-infrastructuur. Klanten kunnen verschillende manieren in de aanvraag voor certificaatvernieuwing voor failover en hoge beschikbaarheid via redundante architectuur kiezen. Hier volgen enkele van de populaire methoden:

- **Actief / passief met koude stand-by**: In deze failoveroplossing de virtuele machines en andere apparaten die wordt uitgevoerd in de stand-by-regio zijn niet actief totdat er dan behoefte voor failover. De productie-omgeving wordt echter gerepliceerd in de vorm van back-ups en de VM-installatiekopieën en het Resource Manager-sjablonen in een andere regio. Dit mechanisme failover is rendabele, maar duurt het langer om uit te voeren van een volledige failover.
 
    ![Actief/passief-met koude stand-by](./media/disaster-recovery-dns-traffic-manager/active-passive-with-cold-standby.png)
    
    *Afbeelding - actief/passief-met koude stand-by noodherstelconfiguratie*

- **Actief/passief-met proef licht**: In deze failoveroplossing de stand-by-omgeving is ingesteld met een minimale configuratie. De instelling heeft alleen de benodigde services ondersteunen alleen een minimale en kritieke set toepassingen die worden uitgevoerd. In de oorspronkelijke indeling kan dit scenario alleen minimale functies uitvoeren, maar kunt omhoog schalen en extra services bulksgewijs van de belasting van de productie te nemen als er een failover optreedt kan worden gemaakt.
    
    ![Actief/passief-met proef licht](./media/disaster-recovery-dns-traffic-manager/active-passive-with-pilot-light.png)
    
    *Afbeelding: Actief/passief-met noodherstelconfiguratie proef licht*

- **Actief/passief-met warme stand-by**: In deze failoveroplossing de stand-by regio vooraf Verwarm en gereed is voor de base belasting, automatisch schalen is ingeschakeld en alle exemplaren actief zijn. Deze oplossing wordt niet aangepast om de volledige productie-belasting te maar functioneert, en alle services actief zijn. Deze oplossing is een uitgebreide versie van de test licht aanpak.
    
    ![Actief/passief-met warme stand-by](./media/disaster-recovery-dns-traffic-manager/active-passive-with-warm-standby.png)
    
    *Afbeelding: Actief/passief-met warme stand-by noodherstelconfiguratie*
    
Zie voor meer informatie over failover en hoge beschikbaarheid, [herstel na noodgevallen voor Azure-toepassingen](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications).


## <a name="planning-your-disaster-recovery-architecture"></a>Planning van uw disaster recovery-architectuur

Er zijn twee technische aspecten voor het instellen van uw disaster recovery-architectuur:
-  Met behulp van een mechanisme deployment exemplaren, gegevens en configuraties repliceren tussen de primaire en stand-by-omgevingen. Dit type herstel na noodgevallen kan worden gedaan systeemeigen via Azure Site-Recovery via Microsoft Azure-partner toestellen/services zoals Veritas of NetApp. 
- De ontwikkeling van een oplossing voor het omleiden van netwerk of web verkeer van de primaire site naar de stand-by-site. Dit type herstel na noodgevallen kan worden bereikt via Azure DNS, Azure verkeer Manager(DNS) of van derden globale netwerktaakverdelers.

In dit artikel is beperkt tot benaderingen via netwerk- en omleiding van verkeer. Zie voor instructies voor het instellen van Azure Site Recovery [Azure Site Recovery-documentatie](https://docs.microsoft.com/azure/site-recovery/).
DNS is een van de meest efficiënte mechanismen voor het netwerkverkeer omleiden omdat DNS vaak globale en buiten het datacenter is en is geïsoleerd van regionale of beschikbaarheid zone (AZ) niveau fouten. Een failover op basis van een DNS-mechanisme kunnen worden gebruikt en in Azure, twee DNS-services, dezelfde op enige wijze - Azure DNS (gezaghebbende DNS) en Azure Traffic Manager (smart DNS-verkeersroutering) kunnen uitvoeren. 

Het is belangrijk te weten van enkele concepten in DNS die grote schaal wordt gebruikt voor het bespreken van de oplossingen die zijn opgegeven in dit artikel:
- **DNS A-Record** – A-Records zijn verwijzingen die een domein naar een IPv4-adres verwijzen. 
- **De naam van CNAME- of Canonical** -dit recordtype wordt gebruikt om te verwijzen naar een andere DNS-record. CNAME reageert niet met een IP-adres, maar in plaats daarvan de pointer naar de record met het IP-adres. 
- **Routering gewogen** – een kunt kiezen om te koppelen van een gewicht voor service-eindpunten en distribueert u vervolgens het verkeer op basis van de gewichten toegewezen. Deze methode voor het doorsturen is een van de vier traffic routing mechanismen beschikbaar binnen het Traffic Manager. Zie voor meer informatie [routeringsmethode gewogen](../traffic-manager/traffic-manager-routing-methods.md#weighted).
- **Prioriteit routering** – prioriteit routering is gebaseerd op statuscontroles van eindpunten. Standaard Azure Traffic manager verzendt alle verkeer van het eindpunt van de hoogste prioriteit en bij een fout of noodgeval Traffic Manager het verkeer gerouteerd naar de secundaire eindpunt. Zie voor meer informatie [routeringsmethode voor prioriteit](../traffic-manager/traffic-manager-routing-methods.md#priority).

## <a name="manual-failover-using-azure-dns"></a>Handmatige failover met behulp van Azure DNS
De Azure DNS handmatige failover-oplossing voor herstel na noodgevallen gebruikt de standaard DNS-mechanisme failover naar de back-site. De handmatige optie via Azure DNS werkt het beste wanneer gebruikt in combinatie met de koude stand-by of de proef licht aanpak. 

![Handmatige failover met behulp van Azure DNS](./media/disaster-recovery-dns-traffic-manager/manual-failover-using-dns.png)

*Afbeelding - handmatige failover met behulp van Azure DNS*

De veronderstellingen voor de oplossing zijn:
-   Primaire en secundaire eindpunten hebben statische IP-adressen die niet vaak veranderen. Stel voor de primaire site het IP-adres is 100.168.124.44 en het IP-adres voor de secundaire site is 100.168.124.43.
-   Er bestaat een Azure DNS-zone voor de primaire en secundaire site. Spreek voor de primaire site het eindpunt is prod.contoso.com en voor de back-upsite dr.contoso.com. Er bestaat ook een DNS-record voor de hoofdtoepassing www.contoso.com genoemd.   
-   De TTL is op of onder de RTO SLA ingesteld in de organisatie. Als bijvoorbeeld een onderneming stelt de RTO van het antwoord van de toepassing na noodgevallen 60 minuten worden dan de TTL-waarde moet lager zijn dan 60 minuten, bij voorkeur de laagste hoe beter. U kunt Azure DNS instellen voor handmatige failover als volgt:
1. Een DNS-zone maken
2. DNS-zone-records maken
3. CNAME-record bijwerken

### <a name="step-1-create-a-dns"></a>Stap 1: Een DNS-server maken
Maak een DNS-zone (bijvoorbeeld www.contoso.com) zoals hieronder wordt weergegeven:

![Een DNS-zone maken in Azure](./media/disaster-recovery-dns-traffic-manager/create-dns-zone.png)

*Afbeelding: een DNS-zone maken in Azure*

### <a name="step-2-create-dns-zone-records"></a>Stap 2: DNS-zone-records maken

Maak drie records (bijvoorbeeld - www.contoso.com, prod.contoso.com en dr.consoto.com) zoals u hierna ziet in deze zone.

![DNS-zone-records maken](./media/disaster-recovery-dns-traffic-manager/create-dns-zone-records.png)

*Afbeelding - records van DNS-zone maken in Azure*

In dit scenario, site, www.contoso.com heeft een TTL-waarde van 30 minuten, die lager is dan de vermelde RTO goed en naar de productie site prod.contoso.com wijst. Deze configuratie is tijdens de normale zakelijke activiteiten. De TTL van prod.contoso.com en dr.contoso.com is ingesteld op 300 seconden of 5 minuten. Kunt u een Azure-service zoals Azure-Monitor of Azure App Insights controleren of een partner bewakingsoplossingen zoals Dynatrace, u kunt zelfs thuis geteeld oplossingen die kunnen bewaken of toepassing of virtuele infrastructuur niveau fouten detecteren.

### <a name="step-3-update-the-cname-record"></a>Stap 3: De CNAME-record bijwerken

Zodra de storing wordt gedetecteerd, wijzigt u de recordwaarde om te verwijzen naar dr.contoso.com, zoals hieronder wordt weergegeven:
       
![CNAME-record bijwerken](./media/disaster-recovery-dns-traffic-manager/update-cname-record.png)

*Afbeelding - Update voor de CNAME-record in Azure*

Binnen 30 minuten, waarin de meeste resolvers het zonebestand in de cache wordt vernieuwd, wordt een query voor www.contoso.com worden omgeleid naar dr.contoso.com.
U kunt ook de volgende opdracht in de Azure CLI de CNAME-waarde te wijzigen uitvoeren:
 ```azurecli
   az network dns record-set cname set-record \
   --resource-group 123 \
   --zone-name contoso.com \
   --record-set-name www \
   --cname dr.contoso.com
```
Deze stap kan worden uitgevoerd, handmatig of via automation. Dit kan handmatig worden gedaan via de console of door de Azure CLI. De Azure SDK en de API kan worden gebruikt voor het automatiseren van de update CNAME zodat er geen actie te ondernemen. Automatisering kan worden ingebouwd via de Azure functions of binnen een toepassing voor de bewaking van derden of zelfs van on-premises.

### <a name="how-manual-failover-works-using-azure-dns"></a>Hoe handmatige failover werkt met behulp van Azure DNS
Omdat de DNS-server buiten de failover of noodgeval zone, is het geïsoleerd tegen uitvaltijd. Dit kan kan de gebruiker aan het ontwerp van een eenvoudige failover-scenario dat betaalbaar en werkt alle de tijd ervan uitgaande dat de operator netwerkverbinding tijdens na noodgevallen en de spiegelen. Als de oplossing vastgelegd in een script is, klikt u vervolgens een moet ervoor zorgen dat de server of de service die het script wordt uitgevoerd op basis van het probleem die invloed hebben op de productieomgeving moet worden geïsoleerd. Houd er ook rekening de lage TTL-waarde die is ingesteld op basis van de zone, zodat geen resolver overal ter wereld het eindpunt in de cache opgeslagen lang houdt en klanten toegang de site binnen de RTO tot. Voor een koude stand-by en test licht, omdat sommige prewarming en andere administratieve activiteiten mogelijk – geeft een ook voldoende tijd voordat het spiegelen.

## <a name="automatic-failover-using-azure-traffic-manager"></a>Automatische failover met Azure Traffic Manager
Wanneer u complexe architecturen en meerdere sets van resources staat uit te voeren dezelfde functie hebt, kunt u Azure Traffic Manager (op basis van DNS) configureren om te controleren van de status van uw resources en het verkeer van de resource niet in orde aan de orde sturen de bron. In het volgende voorbeeld wordt een volledige implementatie hebben door zowel de primaire regio als de secundaire regio. Deze implementatie omvat het cloud-services en een gesynchroniseerde database. 

![Automatische failover met Azure Traffic Manager](./media/disaster-recovery-dns-traffic-manager/automatic-failover-using-traffic-manager.png)

*Afbeelding - automatische failover met Azure Traffic Manager*

Alleen de primaire regio verwerkt echter actief netwerkaanvragen van de gebruikers. De secundaire regio actief alleen wanneer de primaire regio optreedt in een service wordt onderbroken. In dat geval worden alle nieuwe netwerkaanvragen routeren naar de secundaire regio. Sinds de back-up van de database is bijna onmiddellijk, de netwerktaakverdelers hebben IP-adressen die kan worden health dit selectievakje inschakelt, en de exemplaren zijn altijd up-to-date en wordt uitgevoerd, deze topologie bevat een optie voor gaat voor een laag RTO en failover zonder handmatige tussenkomst. De regio secundaire failover moet gereed om te gaan live onmiddellijk na een storing van de primaire regio.
Dit scenario is ideaal voor het gebruik van Azure Traffic Manager met ingebouwde tests voor diverse typen van statuscontroles, met inbegrip van http / https en TCP. Met Azure Traffic manager heeft ook een regel-engine die kan worden geconfigureerd voor failover wanneer er een fout optreedt, zoals hieronder wordt beschreven. De volgende oplossing met Traffic Manager laten we eens:
- De klant heeft het gebied #1 eindpunt prod.contoso.com wel met een statisch IP-adres als 100.168.124.44 en een bekend als dr.contoso.com met een statisch IP-adres als 100.168.124.43 regio #2-eindpunt. 
-   Elk van deze omgevingen wordt fronted via een openbare gerichte eigenschap als een load balancer. De load balancer kan worden geconfigureerd als u een eindpunt op basis van een DNS- of een volledig gekwalificeerde domeinnaam (FQDN) zoals hierboven.
-   Alle exemplaren in regio 2 zijn in bijna realtime replicatie met gebied 1. Bovendien de installatiekopieën van de machine zijn bijgewerkt en alle gegevens van de software of configuratieserver is hersteld en overeenstemming gebied 1 zijn.  
-   Automatisch schalen vooraf is vooraf geconfigureerd. 

De stappen voor het configureren van de failover met Azure Traffic Manager zijn als volgt:
1. Maak een nieuw Azure Traffic Manager-profiel
2. Eindpunten in de Traffic Manager-profiel maken
3. Status controleren en failover-configuratie instellen

### <a name="step-1-create-a-new-azure-traffic-manager-profile"></a>Stap 1: Maak een nieuw Azure Traffic Manager-profiel
Een nieuwe Azure Traffic manager-profiel maken met de naam contoso123 en selecteer de routeringsmethode prioriteit. Als u een bestaande resourcegroep die u koppelen, wilt kunt u een bestaande resourcegroep hebt, anders wordt een nieuwe resourcegroep te maken.

![Traffic Manager-profiel maken](./media/disaster-recovery-dns-traffic-manager/create-traffic-manager-profile.png)
*afbeelding: een Traffic Manager-profiel maken*

### <a name="step-2-create-endpoints-within-the-traffic-manager-profile"></a>Stap 2: Eindpunten in de Traffic Manager-profiel maken

In deze stap maakt u eindpunten die naar de productie en noodherstelsites verwijzen. Hier kiest u de **Type** als een extern eindpunt, maar als de bron wordt gehost in Azure, kunt u **Azure-eindpunt** ook. Als u ervoor kiest **Azure-eindpunt**, selecteer vervolgens een **Doelresource** dat ofwel een **App Service** of een **openbare IP-adres** die is toegewezen door Azure. De prioriteit is ingesteld als **1** omdat deze de primaire regio 1-service.
Op dezelfde manier de disaster recovery eindpunten maken binnen Traffic Manager ook.

![Disaster recovery eindpunten maken](./media/disaster-recovery-dns-traffic-manager/create-disaster-recovery-endpoint.png)

*Afbeelding - disaster recovery eindpunten maken*

### <a name="step-3-set-up-health-check-and-failover-configuration"></a>Stap 3: Configuratie voor het controleren en failover van status instellen

In deze stap stelt u de TTL DNS op 10 seconden die wordt herkend door het meeste internetgerichte recursieve resolvers. Deze configuratie betekent dat er geen DNS-resolver cache worden opgeslagen door de gegevens voor meer dan 10 seconden. Voor de instellingen van de monitor eindpunt het pad is bij de huidige ingesteld / of hoofdmap, maar u kunt de eindpuntinstellingen evalueren van een pad, bijvoorbeeld prod.contoso.com/index aanpassen. In het voorbeeld hieronder toont de **https** als Zoek protocol. U kunt echter **http** of **tcp** ook. De keuze van het protocol is afhankelijk van de toepassing afsluiten. Het Zoek interval is ingesteld op 10 seconden, waarmee snel zoeken en de nieuwe poging is ingesteld op 3. Als gevolg hiervan wordt het Traffic Manager failover naar het tweede eindpunt als drie opeenvolgende intervallen registreert een fout. De volgende formule definieert de totale tijd voor een automatische failover: tijd voor failover = TTL + probeer * zoeken interval en in dit geval wordt de waarde is 10 + 3 * 10 = 40 seconden (Max).
Als de nieuwe poging is ingesteld op 1 en TTL-waarde is ingesteld op 10 sec vervolgens de tijd voor failover 10 + 1 * 10 = 20 seconden. De nieuwe pogingen ingesteld op een waarde groter is dan **1** kans op failovers vanwege fout-positieven of een secundaire netwerk blips elimineren. 


![Statuscontrole instellen](./media/disaster-recovery-dns-traffic-manager/set-up-health-check.png)

*Afbeelding - status controleren en failover-configuratie instellen*

### <a name="how-automatic-failover-works-using-traffic-manager"></a>Hoe automatische failover werkt met Traffic Manager

Tijdens een ramp, het primaire eindpunt wordt aangeduid en de status gewijzigd in **gedegradeerd** en het herstel na noodgevallen site blijft **Online**. Traffic Manager verzendt standaard al het verkeer naar de primaire (hoogste prioriteit)-eindpunt. Als het primaire eindpunt wordt gedegradeerd weergegeven, Traffic Manager het verkeer naar het tweede eindpunt gestuurd, zolang deze goed blijft. Een heeft de optie voor het configureren van meer eindpunten binnen Traffic Manager dat kunnen fungeren als failover voor extra eindpunten of, als taakverdelers delen de werklast tussen de eindpunten.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md).
- Meer informatie over [Azure DNS](../dns/dns-overview.md).









