---
title: Met gelijke taakverdeling van services in Azure | Microsoft Docs
description: 'Deze zelfstudie leert u hoe u een scenario te maken met behulp van de Azure-taakverdeling portfolio: Traffic Manager en Application Gateway Load Balancer.'
services: traffic-manager
documentationcenter: 
author: liumichelle
manager: vitinnan
editor: 
ms.assetid: f89be3be-a16f-4d47-bcae-db2ab72ade17
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/27/2016
ms.author: limichel
ms.openlocfilehash: ae9bd30b76786f94f0d836a39137da696fdb94a2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="using-load-balancing-services-in-azure"></a>Gebruik van taakverdeling services in Azure

## <a name="introduction"></a>Inleiding

Microsoft Azure biedt meerdere services voor het beheren van het netwerkverkeer wordt gedistribueerd en taakverdeling. U kunt deze services afzonderlijk gebruiken of het combineren van hun methoden, afhankelijk van uw behoeften, om de optimale oplossing te bouwen.

In deze zelfstudie we eerst een gebruiksvoorbeeld klant definiëren en hoe deze kan worden gemaakt krachtiger en weergegeven zodat met behulp van de volgende Azure load balancing portfolio: Traffic Manager en Application Gateway Load Balancer. Vervolgens bieden stapsgewijze instructies voor het maken van een implementatie die geografisch redundante is, wordt verkeer naar virtuele machines en kunt u verschillende soorten aanvragen beheren.

Elk van deze services speelt een afzonderlijke rol in de hiërarchie-taakverdeling op een conceptuele niveau.

* **Traffic Manager** biedt globale DNS taakverdeling. Het wordt gekeken naar binnenkomende aanvragen van de DNS- en reageert met een goede eindpunt, in overeenstemming met het beleid voor het routeren dat de klant heeft geselecteerd. Opties voor methoden zijn:
  * De prestaties van routering voor het verzenden van de aanvrager naar het dichtstbijzijnde eindpunt in termen van latentie.
  * Prioriteit routering alle verkeer naar een eindpunt met de andere eindpunten als back-up.
  * Gewogen round robin routering, die wordt verkeer op basis van het gewicht dat is toegewezen aan elk eindpunt.

  De client verbinding rechtstreeks naar dat eindpunt. Azure Traffic Manager detecteert wanneer een eindpunt beschadigd is en stuurt vervolgens door de clients naar een ander exemplaar van de orde. Raadpleeg [Azure Traffic Manager-documentatie](traffic-manager-overview.md) voor meer informatie over de service.
* **Toepassingsgateway** toepassing levering controller (ADC) biedt een service en biedt verschillende laag 7 taakverdeling mogelijkheden voor uw toepassing. Hiermee kunnen klanten web farm productiviteit optimaliseren door het offloaden van CPU-intensief SSL-beëindiging aan de toepassingsgateway. Andere mogelijkheden voor het doorsturen van laag 7 bevatten round-robin distributie van inkomend verkeer, sessie cookies gebaseerde affiniteit URL-pad gebaseerde routering en de mogelijkheid voor het hosten van meerdere websites achter een gateway één toepassing. Application Gateway kan worden geconfigureerd als een gateway internetgerichte, een gateway interne alleen-lezen of een combinatie van beide. Toepassingsgateway is volledig Azure beheerde, schaalbare en maximaal beschikbaar. Het biedt een uitgebreide verzameling diagnostische gegevens en functies voor logboekregistratie voor betere beheersbaarheid.
* **De Load Balancer** is een integraal onderdeel van de Azure SDN-stack bieden hoge prestaties, lage latentie laag 4-taakverdeling voor alle UDP en TCP-protocollen. Het beheert binnenkomende en uitgaande verbindingen. U kunt openbare en interne eindpunten voor netwerktaakverdeling configureren en definiëren regels voor binnenkomende verbindingen worden toegewezen aan de doelen van de back-end-pool met behulp van TCP- en HTTP-status probing opties voor het beheren van de beschikbaarheid van de service.

## <a name="scenario"></a>Scenario

In dit voorbeeldscenario gebruiken we een eenvoudige website die twee typen inhoud fungeert: installatiekopieën en dynamisch gerenderde webpagina's. De website moet geografisch redundante en deze moet de gebruikers van de dichtstbijzijnde (laagste latentie) dienen locatie toe. De ontwikkelaar van toepassingen heeft besloten die alle URL's die overeenkomen met het patroon/installatiekopieën / * worden geleverd vanuit een specifieke groep van virtuele machines die afwijken van de rest van de webfarm.

Bovendien moet de standaardgroep VM van de dynamische inhoud Neem contact op met een back-end-database die wordt gehost op een cluster met hoge beschikbaarheid. De volledige implementatie is ingesteld via Azure Resource Manager.

Met Traffic Manager, Application Gateway en Load Balancer, kunt deze website om deze ontwerpdoelstellingen te realiseren:

* **Multi-geografische redundantie**: als één regio uitvalt, Traffic Manager verkeer gerouteerd naadloos naar de dichtstbijzijnde regio zonder tussenkomst van de eigenaar van de toepassing.
* **Kortere wachttijd**: omdat Traffic Manager automatisch de klant naar de dichtstbijzijnde regio wordt verwezen, de klant lagere latentie optreedt bij het aanvragen van de inhoud van de webpagina.
* **Onafhankelijke schaalbaarheid**: omdat de web-toepassing werklast wordt gescheiden door het type inhoud, de aanvraag-werkbelastingen onafhankelijk van elkaar kan worden geschaald door de eigenaar van de toepassing. Toepassingsgateway zorgt ervoor dat het verkeer wordt doorgestuurd naar de juiste groepen op basis van de opgegeven regels en de status van de toepassing.
* **Interne load balancing**: omdat de Load Balancer is voor het cluster met hoge beschikbaarheid, alleen de actief en in goede orde endpoint voor een database wordt blootgesteld aan de toepassing. Bovendien kan een databasebeheerder de werkbelasting optimaliseren door het distribueren van actieve en passieve replica's in het cluster onafhankelijk van de front-toepassing. Load Balancer verbindingen levert aan het cluster met hoge beschikbaarheid en zorgt ervoor dat alleen orde databases verbindingsaanvragen ontvangen.

Het volgende diagram toont de architectuur van dit scenario:

![Diagram van de taakverdelings-architectuur](./media/traffic-manager-load-balancing-azure/scenario-diagram.png)

> [!NOTE]
> In dit voorbeeld is alleen een van de vele mogelijke configuraties van de taakverdelings-services die Azure biedt. Traffic Manager en Application Gateway Load Balancer kunnen worden gecombineerd en overeenkomende zelf uw behoeften load balancing. Bijvoorbeeld, als SSL-offload of laag 7 verwerking niet nodig is, kan Load Balancer kan worden gebruikt in plaats van Application Gateway.

## <a name="setting-up-the-load-balancing-stack"></a>Instellen van de stack-taakverdeling

### <a name="step-1-create-a-traffic-manager-profile"></a>Stap 1: Een Traffic Manager-profiel maken

1. Klik in de Azure-portal op **nieuw**, en zoek vervolgens de marketplace voor 'Traffic Manager-profiel'.
2. Op de **maken Traffic Manager-profiel** blade, voer de volgende basisinformatie:

  * **Naam**: naam van uw Traffic Manager-profiel een DNS-voorvoegsel geven.
  * **Routeringsmethode voor**: Selecteer het beleid voor verkeersroutering methode. Zie voor meer informatie over de methoden [verkeersrouteringsmethoden voor Traffic Manager over](traffic-manager-routing-methods.md).
  * **Abonnement**: Selecteer het abonnement met het profiel.
  * **Resourcegroep**: Selecteer de resourcegroep met het profiel. Het kan zijn dat een nieuwe of bestaande resourcegroep.
  * **Locatie voor resourcegroep**: Traffic Manager-service is globale en niet naar een locatie gebonden. U moet echter opgeven dat een regio voor de groep waar de metagegevens gekoppeld aan het Traffic Manager-profiel is opgeslagen. Deze locatie heeft geen invloed op de runtime-beschikbaarheid van het profiel.

3. Klik op **maken** voor het genereren van Traffic Manager-profiel.

  ![Blade 'Traffic Manager maken'](./media/traffic-manager-load-balancing-azure/s1-create-tm-blade.png)

### <a name="step-2-create-the-application-gateways"></a>Stap 2: De Toepassingsgateways maken

1. Klik in de Azure-portal in het linkerdeelvenster op **nieuw** > **Networking** > **Application Gateway**.
2. Voer de volgende algemene informatie over de toepassingsgateway:

  * **Naam**: de naam van de toepassingsgateway.
  * **SKU-grootte**: de grootte van de toepassingsgateway, beschikbaar als een klein, middelgroot of groot.
  * **Aantal exemplaar**: het aantal exemplaren, een waarde van 2 tot en met 10.
  * **Resourcegroep**: de resourcegroep waarin u de toepassingsgateway. Het kan een bestaande resourcegroep of een nieuwe map zijn.
  * **Locatie**: de regio voor de toepassingsgateway, de locatie van de resourcegroep is. De locatie is belangrijk, omdat het virtuele netwerk en het openbare IP-adres moet zich in dezelfde locatie als de gateway.
3. Klik op **OK**.
4. Definieer het virtuele netwerk, subnet, front-end-IP- en listener-configuraties voor de toepassingsgateway. In dit scenario wordt het front-end-IP-adres is **openbare**, waardoor ze worden toegevoegd als een eindpunt aan Traffic Manager-profiel later op.
5. Configureer de listener met een van de volgende opties:
    * Als u HTTP gebruikt, is er niets om te configureren. Klik op **OK**.
    * Als u HTTPS gebruikt, verdere is configuratie vereist. Raadpleeg [een toepassingsgateway maken](../application-gateway/application-gateway-create-gateway-portal.md), te beginnen bij stap 9. Wanneer u de configuratie hebt voltooid, klikt u op **OK**.

#### <a name="configure-url-routing-for-application-gateways"></a>URL voor Toepassingsgateways routering configureren

Wanneer u een back-end-pool kiest, wordt een application gateway die geconfigureerd met een regel op basis van het pad naar een pad patroon van de aanvraag-URL naast round-robin distributie. In dit scenario we een regel op basis van het pad naar elke URL met rechtstreeks toevoegen ' /images/\*' aan de installatiekopie-servergroep. Voor meer informatie over het configureren van URL-pad gebaseerde routering voor een toepassingsgateway verwijzen naar [maakt u een regel op basis van het pad voor een toepassingsgateway](../application-gateway/application-gateway-create-url-route-portal.md).

![Application Gateway weblaag diagram](./media/traffic-manager-load-balancing-azure/web-tier-diagram.png)

1. Ga naar het exemplaar van de toepassingsgateway die u in de vorige sectie hebt gemaakt in de resourcegroep.
2. Onder **instellingen**, selecteer **back-endpools**, en selecteer vervolgens **toevoegen** om toe te voegen van de virtuele machines die u wilt koppelen aan de weblaag back-end-adresgroepen.
3. Op de **toevoegen van de back-endpool** blade, voer de naam van de back-end-adresgroep en de IP-adressen van de machines die zich in de groep bevinden. In dit scenario zijn er twee back-endserver pools van virtuele machines verbinding.

  ![Application Gateway 'Back-endpool toevoegen' blade](./media/traffic-manager-load-balancing-azure/s2-appgw-add-bepool.png)

4. Onder **instellingen** selecteren van de toepassingsgateway **regels**, en klik vervolgens op de **pad basis** knop een regel toevoegen.

  ![Application Gateway regels 'pad op basis van knop](./media/traffic-manager-load-balancing-azure/s2-appgw-add-pathrule.png)

5. Op de **toevoegen op basis van een pad regel** blade de regel configureren op basis van de volgende informatie.

   Basisinstellingen:

   + **Naam**: de beschrijvende naam van de regel die toegankelijk is in de portal.
   + **Listener**: de listener die wordt gebruikt voor de regel.
   + **Standaard back-endpool**: de back-end-pool moet worden gebruikt met de standaardregel.
   + **Standaardinstellingen voor HTTP-**: de HTTP-instellingen worden gebruikt met de standaardregel.

   Pad gebaseerde regels:

   + **Naam**: de beschrijvende naam van de regel op basis van het pad.
   + **Paden**: de padregel die wordt gebruikt voor het doorsturen van verkeer.
   + **Back-Endpool**: de back-end-pool moet worden gebruikt met deze regel.
   + **HTTP-instelling**: de HTTP-instellingen worden gebruikt met deze regel.

   > [!IMPORTANT]
   > Paden: Geldige paden moeten beginnen met '/'. Het jokerteken '\*' mag alleen aan het einde. Voorbeelden van geldige/xyz, / XYZ zijn\*, of /xyz/\*.

   ![Application Gateway 'Toevoegen op basis van een pad regel' blade](./media/traffic-manager-load-balancing-azure/s2-appgw-pathrule-blade.png)

### <a name="step-3-add-application-gateways-to-the-traffic-manager-endpoints"></a>Stap 3: Toepassingsgateways naar de Traffic Manager-eindpunten toevoegen

In dit scenario is Traffic Manager verbonden met Toepassingsgateways (zoals geconfigureerd in de voorgaande stappen) die zich in verschillende regio's bevinden. Nu de Toepassingsgateways zijn geconfigureerd, de volgende stap is om met uw Traffic Manager-profiel.

1. Open uw Traffic Manager-profiel. Zoek in de resourcegroep of zoeken naar de naam van het Traffic Manager-profiel uit om dit te doen **alle Resources**.
2. Selecteer in het linkerdeelvenster **eindpunten**, en klik vervolgens op **toevoegen** een eindpunt toevoegen.

  ![Traffic Manager-eindpunten "toevoegen" knop](./media/traffic-manager-load-balancing-azure/s3-tm-add-endpoint.png)

3. Op de **eindpunt toevoegen** blade een eindpunt maken door te voeren van de volgende informatie:

  * **Type**: Selecteer het type van het eindpunt te verdelen. Selecteer in dit scenario **Azure-eindpunt** omdat er wordt verbinding met de application gateway-exemplaren die eerder zijn geconfigureerd.
  * **Naam**: Voer de naam van het eindpunt.
  * **Resource doeltype**: Selecteer **openbaar IP-adres** en klik vervolgens onder **Doelresource**, selecteert u het openbare IP-adres van de toepassingsgateway die eerder is geconfigureerd.

   ![Traffic Manager-blade 'Eindpunt toevoegen'](./media/traffic-manager-load-balancing-azure/s3-tm-add-endpoint-blade.png)

4. Nu u uw instellingen testen kunt door deze te openen met de DNS-server van uw Traffic Manager-profiel (in dit voorbeeld: TrafficManagerScenario.trafficmanager.net). U kunt aanvragen verzenden, online zetten van of brengen VM's en de webservers die zijn gemaakt in verschillende regio's en de instellingen voor het Traffic Manager-profiel voor het testen van uw instellingen te wijzigen.

### <a name="step-4-create-a-load-balancer"></a>Stap 4: Een load balancer maken

In dit scenario distribueert de Load Balancer verbindingen vanaf de weblaag naar de databases binnen een cluster met hoge beschikbaarheid.

Als uw databasecluster met hoge beschikbaarheid-van SQL Server AlwaysOn gebruikmaakt, raadpleegt u [configureren van een of meer altijd op beschikbaarheid Beschikbaarheidsgroeplisteners](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) voor stapsgewijze instructies.

Zie voor meer informatie over het configureren van een interne load balancer [een interne load balancer maken in de Azure portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md).

1. Klik in de Azure-portal in het linkerdeelvenster op **nieuw** > **Networking** > **Load balancer**.
2. Op de **maken load balancer** blade, kies een naam voor de load balancer.
3. Stel de **Type** naar **intern**, en kies het juiste virtuele netwerk en subnet voor de load balancer zich bevinden.
4. Onder **IP-adrestoewijzing**, selecteert u **dynamische** of **statische**.
5. Onder **resourcegroep**, kiest u de resourcegroep voor de load balancer.
6. Onder **locatie**, kies de juiste regio voor de load balancer.
7. Klik op **maken** voor het genereren van de load balancer.

#### <a name="connect-a-back-end-database-tier-to-the-load-balancer"></a>Verbinding maken met een back-enddatabase laag aan de load balancer

1. Zoek de load balancer die is gemaakt in de vorige stappen van de resourcegroep.
2. Onder **instellingen**, klikt u op **back-endpools**, en klik vervolgens op **toevoegen** toevoegen van een back-end-pool.

  ![Load Balancer-blade 'Back-endpool toevoegen'](./media/traffic-manager-load-balancing-azure/s4-ilb-add-bepool.png)

3. Op de **toevoegen van de back-endpool** blade, voer de naam van de back-end-pool.
4. Afzonderlijke computers of een beschikbaarheidsset aan de groep back-end toevoegen.

#### <a name="configure-a-probe"></a>Een test configureren

1. In de load balancer, onder **instellingen**, selecteer **Probes**, en klik vervolgens op **toevoegen** toevoegen van een test.

 ![Load Balancer "Add-test" blade](./media/traffic-manager-load-balancing-azure/s4-ilb-add-probe.png)

2. Op de **toevoegen test** blade, geef de naam voor de test.
3. Selecteer de **Protocol** voor de test. Voor een database kunt u een TCP-controle in plaats van een HTTP-test. Raadpleeg voor meer informatie over de load balancer-tests, [Understand load balancer tests](../load-balancer/load-balancer-custom-probe-overview.md).
4. Voer de **poort** van uw database moet worden gebruikt voor toegang tot de test.
5. Onder **Interval**, opgeven hoe vaak moet worden test de toepassing.
6. Onder **drempelwaarde voor onjuiste status**, geef het aantal continue testfouten dat moet optreden voor de back-end-VM niet in orde beschouwd.
7. Klik op **OK** voor het maken van de test.

#### <a name="configure-the-load-balancing-rules"></a>De regels voor taakverdeling configureren

1. Onder **instellingen** van de load balancer, selecteer **Taakverdelingsregels**, en klik vervolgens op **toevoegen** om een regel te maken.
2. Op de **toevoegen taakverdelingsregel** blade, voer de **naam** voor de regel voor load balancing.
3. Kies de **Frontend-IP-adres** van de load balancer **Protocol**, en **poort**.
4. Onder **backendpoort**, geef de poort moet worden gebruikt in de groep back-end.
5. Selecteer de **back-endpool** en de **Probe** die zijn gemaakt in de vorige stappen in de regel wilt toepassen.
6. Onder **sessiepersistentie**, kiezen hoe u de sessies om vast te leggen.
7. Onder **inactief time-outs**, geef het aantal minuten voordat een time-out voor inactiviteit.
8. Onder **zwevend IP**, selecteert u **uitgeschakelde** of **ingeschakeld**.
9. Klik op **OK** om de regel te maken.

### <a name="step-5-connect-web-tier-vms-to-the-load-balancer"></a>Stap 5: Weblaag virtuele machines verbinding met de load balancer

Nu configureren we de IP-adres en de load balancer front-endpoort in de toepassingen die worden uitgevoerd op uw weblaag virtuele machines voor de databaseverbindingen. Deze configuratie is specifiek voor de toepassingen die worden uitgevoerd op deze virtuele machines. Raadpleeg de documentatie van de toepassing voor het configureren van de doel-IP-adres en poort. Het IP-adres van de front-end zoekt in de Azure-portal, gaat u naar de front-end-IP-adresgroep van de **instellingen netwerkverdeler** blade.

![Load Balancer 'Frontend-IP-adresgroep' navigatiedeelvenster](./media/traffic-manager-load-balancing-azure/s5-ilb-frontend-ippool.png)

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van Traffic Manager](traffic-manager-overview.md)
* [Overzicht van Application Gateway](../application-gateway/application-gateway-introduction.md)
* [Overzicht van Azure Load Balancer](../load-balancer/load-balancer-overview.md)
