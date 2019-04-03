---
title: Beveiligen van uw netwerkresources in Azure Security Center | Microsoft Docs
description: In dit document adressen aanbevelingen in Azure Security Center die u helpen beveiligen van uw Azure-netwerkresources en blijven in overeenstemming met beveiligingsbeleid.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 96c55a02-afd6-478b-9c1f-039528f3dea0
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/02/2019
ms.author: monhaber
ms.openlocfilehash: cca1962e5146300cc376fab4bcb1bf0876acec6c
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58863148"
---
# <a name="protect-your-network-resources-in-azure-security-center"></a>Beveilig uw netwerkresources in Azure Security Center
Azure Security Center analyseert voortdurend de beveiligingsstatus van uw Azure-resources voor best practices voor netwerkbeveiliging. Wanneer Security Center potentiële beveiligingsproblemen worden geïdentificeerd, worden er aanbevelingen die u bij het proces begeleiden van het configureren van de benodigde besturingselementen om te versterken en beschermen van uw resources.

In dit artikel biedt aanbevelingen die betrekking hebben op uw Azure-resources vanuit beveiligingsoogpunt netwerk. Netwerken center aanbevelingen om de volgende generation-firewalls, Netwerkbeveiligingsgroepen en regels voor te soepele binnenkomend verkeer van JIT-VM-toegang. Zie voor een lijst met aanbevelingen voor netwerken en herstelacties, [aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md).

> [!NOTE]
> De **netwerken** pagina kunt u dieper ingaan op de status van uw Azure-resource vanuit het netwerkperspectief van een. De netwerk-kaart en besturingselementen voor adaptieve netwerk zijn beschikbaar voor alleen de standaardlaag van Azure Security Center. [Als u de gratis laag, kunt u op de knop **verouderde netwerken weergeven** en netwerkaanbevelingen resource ontvangen](#legacy-networking).
>

De **netwerken** blade bevat een overzicht van de secties u diep kunt duiken, voor meer informatie over de status van uw netwerkresources:

- Netwerk-kaart (alleen in de Azure Security Center Standard laag)
- Aanbevelingen voor beveiliging van netwerken.
- Verouderde **netwerken** blade (de vorige netwerken blade) 
 
![Deelvenster voor netwerken](./media/security-center-network-recommendations/networking-pane.png)

## <a name="network-map"></a>Netwerktoewijzingen
De interactieve kaart biedt een grafische weergave beveiliging overlays, waardoor u aanbevelingen en inzichten voor het beperken van de netwerkresources van uw. U kunt de netwerktopologie van uw Azure-workloads, verbindingen tussen uw virtuele machines en subnetten en de mogelijkheid van de kaart is Inzoomen op specifieke resources en de aanbevelingen voor deze resources met behulp van de kaart bekijken.

Hiermee opent u de netwerktoewijzing:

1. Selecteer in Security Center onder Resource beveiliging hygiëne **netwerken**.
2. Onder **Netwerkoverzicht** klikt u op **topologie Zie**.
 
De standaardweergave van de topologie-kaart wordt weergegeven:
- Abonnementen die u hebt geselecteerd in Azure. De kaart biedt ondersteuning voor meerdere abonnementen.
- Virtuele machines, subnetten en VNets van het type Resource Manager-resource (in klassieke Azure-resources worden niet ondersteund)
- Gekoppelde VNets
- Alleen de resources met een [netwerk aanbevelingen](security-center-recommendations.md) met een hoge of gemiddelde urgentie  
- Resources op Internet gerichte
- De kaart is geoptimaliseerd voor de abonnementen die u hebt geselecteerd in Azure. Als u uw selectie wijzigt, de kaart wordt opnieuw berekend en opnieuw geoptimaliseerd op basis van de nieuwe instellingen.  

![Netwerken topologiekaart weer](./media/security-center-network-recommendations/network-map-info.png)

## <a name="understanding-the-network-map"></a>Informatie over de netwerk-kaart

De netwerk-kaart ziet u uw Azure-resources in een **topologie** weergeven en een **verkeer** weergeven.

### <a name="the-topology-view"></a>De Topologieweergave

In de **topologie** weergave van de kaart netwerken, kunt u de volgende inzichten bekijken over uw netwerkresources:
- In de binnenste cirkel, ziet u alle vnet's binnen de geselecteerde abonnementen, de volgende cirkel alle subnetten is, de buitenste cirkel is alle virtuele machines.
- De lijnen tussen de bronnen in de kaart zodat u weet welke resources zijn gekoppeld aan elkaar, en de structuur van uw Azure-netwerk. 
- De ernst-indicatoren gebruiken om snel een overzicht van die resources open aanbevelingen van Security Center hebben.
- U kunt klikken op een van de resources wilt inzoomen op deze en bekijk de details van die resource en de aanbevelingen rechtstreeks en in de context van het netwerk toewijzen.  
- Als er te veel dat resources die worden weergegeven op de kaart, haar eigen algoritme op slimme maakt gebruik van Azure Security Center uw resources, markeren van de resources die in de meest kritieke status, en hebben de meeste hoge urgentie aanbevelingen van het cluster. 

Omdat de toewijzing interactief en dynamische is, elk knooppunt geklikt is en de weergave kunt wijzigen op basis van de filters:

1. U kunt wijzigen wat u ziet op de kaart netwerk met behulp van de filters aan de bovenkant. U kunt zich richten de kaart op basis van:
   -  **Beveiligingsstatus van de resource**: U kunt de kaart op basis van ernst (hoog, Gemiddeld, laag) van uw Azure-resources kunt filteren.
   - **Aanbevelingen**: U kunt selecteren welke resources worden weergegeven op basis van welke aanbevelingen actief op deze resources zijn. U kunt bijvoorbeeld alleen bronnen waarvoor Security Center raadt dat u Netwerkbeveiligingsgroepen inschakelen weergeven.
   - **Zones netwerk**: Standaard de kaart toont alleen gerichte resources op Internet, kunt u ook interne virtuele machines.
 
2. U kunt klikken op **opnieuw** in de linkerbovenhoek op elk gewenst moment om te retourneren van de kaart op de standaardstatus heeft.

Om in te zoomen naar een resource:
1. Wanneer u een specifieke bron op de kaart selecteert, in het rechterdeelvenster wordt geopend en biedt u algemene informatie over de resource, verbonden beveiligingsoplossingen indien aanwezig, en de aanbevelingen die relevant zijn voor de resource. Het is hetzelfde type gedrag voor elk type resource dat u selecteert. 
2. Wanneer u de muisaanwijzer op een knooppunt in de kaart, vindt u algemene informatie over de resource, met inbegrip van abonnement, resourcetype en resourcegroep.
3. Gebruik de koppeling om te zoomen in de knopinfo en de kaart beter in te richten op die specifieke knooppunt. 
4. Als u wilt de toewijzing van een specifiek knooppunt beter in te richten, uitzoomen.

### <a name="the-traffic-view"></a>De weergave verkeer

De **verkeer** weergave biedt u een overzicht van alle mogelijke verkeer tussen uw resources. Dit biedt u met een visueel overzicht van alle regels die u hebt geconfigureerd die definieert welke resources kunnen communiceren met wie. Hiermee kunt u zien van de bestaande configuratie van de netwerkbeveiligingsgroepen, evenals mogelijke risicovolle configuraties binnen uw workloads snel kunt identificeren.

### <a name="uncover-unwanted-connections"></a>Ontdek ongewenste verbindingen

De sterkte van deze weergave wordt de mogelijkheid om weer te geven u deze toegestane verbindingen samen met de kwetsbaarheden die aanwezig zijn, zodat u kunt deze doorsnede van de gegevens uit te voeren van de beveiliging die nodig zijn voor uw resources. 

Bijvoorbeeld, u detecteert mogelijk twee machines dat u niet mee kunnen communiceren, zodat u beter kan worden geïsoleerd de werkbelastingen en subnetten.

### <a name="investigate-resources"></a>Resources onderzoeken

Om in te zoomen naar een resource:
1. Wanneer u een specifieke bron op de kaart selecteert, in het rechterdeelvenster wordt geopend en biedt u algemene informatie over de resource, verbonden beveiligingsoplossingen indien aanwezig, en de aanbevelingen die relevant zijn voor de resource. Het is hetzelfde type gedrag voor elk type resource dat u selecteert. 
2. Klik op **verkeer** om te zien van de lijst met mogelijke binnenkomende en uitgaande verkeer op de resource - dit is een uitgebreide lijst die kan communiceren met de resource en op welke deze kan communiceren met en via welke protocollen en poorten. Bijvoorbeeld, wanneer u selecteert een virtuele machine, alle virtuele machines die deze met communiceren kan worden weergegeven, en wanneer u een subnet selecteert, alle subnetten die met communiceren kan worden weergegeven.

**Deze gegevens is gebaseerd op de analyse van de Network Security Groups, evenals geavanceerde machine learning-algoritmen die meerdere regels voor meer informatie over hun spoorwegkruisingen en interacties te analyseren.** 

![Netwerkverkeer kaart](./media/security-center-network-recommendations/network-map-traffic.png)

## Verouderde netwerken <a name ="legacy-networking"></a>

Als u geen Security Center Standard-laag, is deze sectie wordt uitgelegd hoe u kunt gratis aanbevelingen voor netwerken weergeven.

Klik op toegang tot deze gegevens op de blade netwerken **verouderde netwerken weergeven**. 

![Verouderde netwerken](./media/security-center-network-recommendations/legacy-networking.png)

### <a name="internet-facing-endpoints-section"></a>Sectie Internetgerichte eindpunten
In de **internetgerichte eindpunten** sectie, kunt u de virtuele machines die momenteel zijn geconfigureerd met een Internetgericht eindpunt en de status zien.

Deze tabel bevat de eindpuntnaam, het internetgerichte IP-adres en de huidige status van de ernst van de netwerkbeveiligingsgroep en de NGFW aanbevelingen. De tabel is gesorteerd op ernst.

### <a name="networking-topology-section"></a>Sectie Netwerktopologie
De **netwerktopologie** sectie is een hiërarchische weergave van de resources.

Deze tabel is gesorteerd (virtuele machines en subnetten) op ernst.

In deze Topologieweergave geeft het eerste niveau Vnets. Weergegeven als de tweede bevat subnetten en het derde niveau wordt weergegeven voor de virtuele machines die deel uitmaken van deze subnetten. In de rechterkolom bevindt, geeft de huidige status van de netwerk-groep-aanbevelingen voor beveiliging voor deze resources.

Het derde niveau wordt weergegeven in virtuele machines, vergelijkbaar met wat eerder is beschreven. U kunt klikken op elke bron uit voor meer informatie of het benodigde beveiligingsbeheer of de configuratie van toepassing.

## <a name="network-recommendations"></a>Aanbevelingen voor netwerken

|Resourcetype|Beveiligingsscore|Aanbeveling|Description|
|----|----|----|----|
|Machine|40|Netwerkbeveiligingsgroepen op virtuele machines inschakelen|Netwerkbeveiligingsgroepen voor het beheren van toegang tot het netwerk van uw virtuele machines inschakelen.|
|Subnet|35|Netwerkbeveiligingsgroepen op subnetten inschakelen |Netwerkbeveiligingsgroepen voor het beheren van toegang tot het netwerk van de resources die zijn geïmplementeerd in uw subnetten inschakelen.|
|Machine|30|Just-In-Time-netwerktoegangsbeheer toepassen|Just-in-time VM toegangsbeheer vergrendelen permanent toegang tot geselecteerde poorten van toepassing en kunnen gemachtigde gebruikers om ze te openen via het mechanisme voor dezelfde en voor een beperkte hoeveelheid tijd wordt opgelost.|
|Machine|20|Toegang via een internetgericht eindpunt beperken|De netwerkbeveiligingsgroepen van uw virtuele machines voor internetgerichte door het beperken van de toegang van uw bestaande toestaan beter beschermen regels.|
|Machine|10|Een firewall van de volgende generatie toevoegen|Een volgende generatie Firewall (NGFW)-oplossing voor het beveiligen van uw virtuele machines van de internetgerichte toevoegen.|
|Machine|5|Verkeer routeren via een gateway netwerkfirewall alleen|Om de implementatie van de volgende generatie firewall-oplossing hebt voltooid, moet het verkeer naar uw beveiligde virtuele machines voor internetgerichte alleen via de volgende generatie firewall-oplossing worden gerouteerd.|
|VNet|5|DDoS-bescherming standaard inschakelen|Toepassingen met openbare IP-adressen in deze virtuele netwerken zijn niet beveiligd met de DDOS protection-service standard. Het wordt aanbevolen om het te beperken van protocol aanvallen en breng netwerk inschakelen.|
|Machine|10|Een firewall van de volgende generatie toevoegen|dd een volgende generatie Firewall (NGFW)-oplossing om uw virtuele machines van de internetgerichte beter te beschermen.|
|Machine|5|Verkeer routeren via een gateway netwerkfirewall alleen|Om de implementatie van de volgende generatie firewall-oplossing hebt voltooid, moet het verkeer naar uw beveiligde virtuele machines voor internetgerichte alleen via de volgende generatie firewall-oplossing worden gerouteerd.|
VNET|5|DDoS-bescherming standaard inschakelen|Toepassingen met openbare IP-adressen in deze virtuele netwerken zijn niet beveiligd met de DDOS protection-service standard. Het wordt aanbevolen om het te beperken van protocol aanvallen en breng netwerk inschakelen.|
## <a name="see-also"></a>Zie ook
Zie de volgende onderwerpen voor meer informatie over aanbevelingen die betrekking hebben op andere typen Azure-resources:

* [Beveiligen van uw virtuele machines in Azure Security Center](security-center-virtual-machine-recommendations.md)
* [Beveiligen van uw toepassingen in Azure Security Center](security-center-application-recommendations.md)
* [Beveiligen van uw Azure SQL-service in Azure Security Center](security-center-sql-service-recommendations.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](tutorial-security-policy.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
