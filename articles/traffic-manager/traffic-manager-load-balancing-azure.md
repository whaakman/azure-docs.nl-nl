---
title: Met load balancing-services in Azure | Microsoft Docs
description: 'Deze zelfstudie leert u hoe u voor het maken van een scenario met behulp van de Azure load balancing-portfolio: Traffic Manager, Application Gateway en Load Balancer.'
services: traffic-manager
documentationcenter: ''
author: liumichelle
manager: dkays
editor: ''
ms.assetid: f89be3be-a16f-4d47-bcae-db2ab72ade17
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/27/2016
ms.author: limichel
ms.openlocfilehash: a6f7a690cac5718216636d3191f348c71bcfb5d6
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55734348"
---
# <a name="using-load-balancing-services-in-azure"></a>Met behulp van load balancing-services in Azure

## <a name="introduction"></a>Inleiding

Microsoft Azure biedt verschillende services voor het beheren van hoe netwerkverkeer wordt verdeeld en taakverdeling. U kunt deze services afzonderlijk gebruiken of het combineren van hun methoden, afhankelijk van uw behoeften, om de optimale oplossing te bouwen.

In deze zelfstudie, we eerst definiëren van een klant use case te zien hoe deze kan worden gemaakt krachtiger en goed presterende met behulp van de volgende Azure load balancing-portfolio: Traffic Manager, Application Gateway en Load Balancer. We vervolgens bieden stapsgewijze instructies voor het maken van een implementatie die is geografisch redundant, verdeelt het verkeer naar VM's en helpt u bij het beheren van verschillende soorten aanvragen.

Op het niveau van een conceptuele speelt elk van deze services een afzonderlijke rol in de hiërarchie-taakverdeling.

* **Traffic Manager** biedt globale DNS wordt de taakverdeling. Deze kijkt naar binnenkomende aanvragen voor DNS- en reageert met een gezond eindpunt, in overeenstemming met het beleid voor het routeren dat de klant is geselecteerd. Opties voor methoden zijn:
  * Prestaties van routering voor het verzenden van de aanvrager naar het dichtstbijzijnde eindpunt in termen van latentie.
  * Prioriteit routering om al het verkeer naar een eindpunt dat met andere eindpunten als back-up.
  * Gewogen round robin routering, die verdeelt het verkeer op basis van het gewicht dat is toegewezen aan elk eindpunt.
  * Routering om het verkeer naar de toepassingseindpunten van uw te distribueren op basis van Geografie op basis van geografische locatie van de gebruiker.
  * Subnet op basis van routering om het verkeer naar de toepassingseindpunten van uw te distribueren op basis van het subnet (IP-adresbereik) van de gebruiker.
  * Meerdere waarden die routering kunt u IP-adressen van meer dan één toepassingseindpunten in één DNS-antwoord verzenden.

  De client maakt rechtstreeks verbinding met het eindpunt dat is geretourneerd door Traffic Manager. Met Azure Traffic Manager detecteert wanneer een eindpunt niet in orde is en vervolgens de clients omgeleid naar een ander exemplaar van de in orde. Raadpleeg [documentatie over Azure Traffic Manager](traffic-manager-overview.md) voor meer informatie over de service.
* **Application Gateway** application delivery controller (ADC) biedt as a service, biedt verschillende Layer 7 load balancing mogelijkheden voor uw toepassing. Hiermee kunnen klanten de productiviteit van webfarms optimaliseren door het offloaden van CPU-intensieve SSL-beëindiging aan de toepassingsgateway. Andere Routeringsmogelijkheden voor laag 7 bevatten round-robin-distributie van inkomend verkeer, cookies gebaseerde sessieaffiniteit, URL-pad gebaseerde routering en de mogelijkheid voor het hosten van meerdere websites achter één application gateway. Application Gateway kan worden geconfigureerd als een Internet gerichte gateway, een gateway alleen interne of een combinatie van beide. Application Gateway wordt volledig door Azure beheerde, schaalbare en maximaal beschikbaar. Het biedt een uitgebreide verzameling diagnostische gegevens en functies voor logboekregistratie voor betere beheersbaarheid.
* **Load Balancer** is een integraal onderdeel van de Azure SDN-stack, leveren van hoge prestaties en lage latentie Layer 4 load balancing services voor alle UDP en TCP-protocollen. Het beheert binnenkomende en uitgaande verbindingen. U kunt openbare en interne taakverdeling eindpunten configureren en definieer regels voor binnenkomende verbindingen naar back-end-pool bestemmingen toewijzen met behulp van TCP- en HTTP-statustesten opties voor het beheren van de beschikbaarheid van de service.

## <a name="scenario"></a>Scenario

In dit voorbeeldscenario gebruiken we een eenvoudige website waarmee twee typen inhoud fungeert: afbeeldingen en dynamisch gerenderde webpagina's. De website moet geografisch redundant en deze moet de gebruikers van de dichtstbijzijnde (laagste latentie) dienen locatie toe. Ontwikkelaar van de toepassing heeft besloten dat URL's die overeenkomen met het patroon/afbeeldingen / * worden aangeleverd vanuit een specifieke pool van virtuele machines die verschillen van de rest van de webfarm.

Daarnaast moet de standaardgroep van de virtuele machine de dynamische inhoud om te communiceren met een back-end-database die wordt gehost op een cluster met hoge beschikbaarheid. De volledige implementatie is ingesteld via Azure Resource Manager.

Met Traffic Manager, Application Gateway en Load Balancer, kunt deze website om deze ontwerpdoelstellingen te realiseren:

* **Multi-geografische redundantie**: Als een regio uitvalt, stuurt Traffic Manager verkeer naadloos naar de dichtstbijzijnde regio zonder tussenkomst van de eigenaar van de toepassing.
* **Gereduceerde latentie**: Omdat de klant naar de dichtstbijzijnde regio automatisch om Traffic Manager te leiden, klantervaringen de lagere latentie bij het aanvragen van de inhoud van de webpagina.
* **Onafhankelijke schaalbaarheid**: Omdat de workload van de web-toepassing is gescheiden door het type inhoud, kunt de eigenaar van de toepassing de aanvraag werkbelastingen onafhankelijk van elkaar schalen. Application Gateway zorgt ervoor dat het verkeer wordt doorgestuurd naar de juiste groepen op basis van de opgegeven regels en de status van de toepassing.
* **Interne taakverdeling**: Omdat Load Balancer in het zicht van het cluster met hoge beschikbaarheid is, wordt alleen het eindpunt actief en in orde is voor een database wordt blootgesteld aan de toepassing. Een databasebeheerder kunt bovendien de werkbelasting optimaliseren door het distribueren van actieve en passieve replica's in het cluster onafhankelijk van de front-endtoepassing. Load Balancer verbindingen levert aan het cluster met hoge beschikbaarheid en zorgt ervoor dat alleen databases die in orde verbindingsaanvragen ontvangen.

Het volgende diagram toont de architectuur van dit scenario:

![Diagram van de loadbalancer-architectuur](./media/traffic-manager-load-balancing-azure/scenario-diagram.png)

> [!NOTE]
> In dit voorbeeld is slechts één van de vele mogelijke configuraties van de load balancing-services die Azure biedt. Traffic Manager, Application Gateway en Load Balancer kunnen worden gecombineerd en afgestemd op zelf de behoeften van uw load balancing. Bijvoorbeeld, als SSL-offload of Layer 7-verwerking niet nodig is, kan Load Balancer kan worden gebruikt in plaats van Application Gateway.

## <a name="setting-up-the-load-balancing-stack"></a>Instellen van de stack-taakverdeling

### <a name="step-1-create-a-traffic-manager-profile"></a>Stap 1: Een Traffic Manager-profiel maken

1. Klik in de Azure-portal op **een resource maken** > **netwerken** > **Traffic Manager-profiel**  >   **Maak**.
2. Voer de volgende algemene informatie:

  * **Naam**: Geef uw Traffic Manager-profiel een DNS-voorvoegsel.
  * **Routeringsmethode**: Selecteer het beleid van de methode routering van verkeer. Zie voor meer informatie over de methoden [verkeersrouteringsmethoden voor Traffic Manager over](traffic-manager-routing-methods.md).
  * **Abonnement**: Selecteer het abonnement dat het profiel bevat.
  * **Resourcegroep**: Selecteer de resourcegroep die het profiel bevat. Een nieuwe of bestaande resourcegroep kan zijn.
  * **Locatie voor resourcegroep**: Traffic Manager-service is globaal en is niet gebonden aan een locatie. Echter, moet u een regio voor de groep waarin de metagegevens die zijn gekoppeld aan het profiel van Traffic Manager zich bevindt. Deze locatie heeft geen invloed op de runtime-beschikbaarheid van het profiel.

3. Klik op **maken** voor het genereren van Traffic Manager-profiel.

  ![Blade 'Traffic Manager maken'](./media/traffic-manager-load-balancing-azure/s1-create-tm-blade.png)

### <a name="step-2-create-the-application-gateways"></a>Stap 2: De Toepassingsgateways maken

1. Klik in de Azure-portal, in het linkerdeelvenster op **een resource maken** > **netwerken** > **Application Gateway**.
2. Voer de volgende algemene informatie over de application gateway:

  * **Naam**: De naam van de toepassingsgateway.
  * **SKU-grootte**: De grootte van de application gateway, die beschikbaar zijn als klein, normaal of groot.
  * **Aantal instanties**: Het aantal exemplaren, een waarde van 2 t/m 10.
  * **Resourcegroep**: De resourcegroep waarin de toepassingsgateway. Het kan een bestaande resourcegroep of nieuw zijn.
  * **Locatie**: De regio voor de toepassingsgateway, dit dezelfde locatie als de resourcegroep is. De locatie is belangrijk, omdat het virtuele netwerk en een openbaar IP-adres moet zich in dezelfde locatie als de gateway.
3. Klik op **OK**.
4. Definieert het virtuele netwerk, subnet, front-end-IP en configuraties van de listener voor de toepassingsgateway. In dit scenario wordt het front-end-IP-adres is **openbare**, waardoor ze worden toegevoegd als een eindpunt aan Traffic Manager-profiel later op.
5. Configureer de listener met een van de volgende opties:
    * Als u HTTP gebruikt, is er niets hoeft te configureren. Klik op **OK**.
    * Als u HTTPS gebruikt, verdere is configuratie vereist. Raadpleeg [een toepassingsgateway maken](../application-gateway/application-gateway-create-gateway-portal.md), vanaf stap 9. Wanneer u de configuratie hebt voltooid, klikt u op **OK**.

#### <a name="configure-url-routing-for-application-gateways"></a>URL-routering voor application gateways configureren

Als u een back-end-adrespool kiest, wordt een application gateway die geconfigureerd met een pad gebaseerde regel een padpatroon van de aanvraag-URL naast round-robin-distributie. In dit scenario wordt er een pad gebaseerde regel om een URL met worden toegevoegd ' /images/\*' aan de installatiekopie. Voor meer informatie over het configureren van URL-pad gebaseerde routering voor een toepassingsgateway verwijzen naar [maakt u een pad gebaseerde regel voor een toepassingsgateway](../application-gateway/application-gateway-create-url-route-portal.md).

![Application Gateway weblaag diagram](./media/traffic-manager-load-balancing-azure/web-tier-diagram.png)

1. Ga naar het exemplaar van de application gateway die u in de voorgaande sectie hebt gemaakt in de resourcegroep.
2. Onder **instellingen**, selecteer **back-endpools**, en selecteer vervolgens **toevoegen** om toe te voegen van de virtuele machines die u wilt koppelen aan de weblaag back-end-adresgroepen.
3. Voer de naam van de back-endpool en de IP-adressen van de machines die zich in de groep bevinden. In dit scenario wordt verbinding er twee back-endserverpools van virtuele machines.

  ![Application Gateway 'Back-endpool toevoegen'](./media/traffic-manager-load-balancing-azure/s2-appgw-add-bepool.png)

4. Onder **instellingen** van de toepassingsgateway, selecteer **regels**, en klik vervolgens op de **pad op basis van** knop kunt u een regel toevoegen.

  ![Application Gateway regels "Padgebaseerde" knop](./media/traffic-manager-load-balancing-azure/s2-appgw-add-pathrule.png)

5. De regel configureren door de volgende informatie te verstrekken.

   Basisinstellingen voor:

   + **Naam**: De beschrijvende naam van de regel die toegankelijk is in de portal.
   + **Listener**: De listener die wordt gebruikt voor de regel.
   + **Standaard back-endpool**: De back-end-adrespool met de standaardregel moet worden gebruikt.
   + **HTTP-instellingen standaard**: De HTTP-instellingen met de standaardregel moet worden gebruikt.

   Op pad gebaseerde regels:

   + **Naam**: De beschrijvende naam van het pad gebaseerde regel.
   + **Paden**: De padregel dat wordt gebruikt voor het doorsturen van verkeer.
   + **Back-Endpool**: De back-end-adrespool met deze regel moet worden gebruikt.
   + **HTTP-instelling**: De HTTP-instellingen moet worden gebruikt met deze regel.

   > [!IMPORTANT]
   > Paden: Geldige paden moeten beginnen met '/'. Het jokerteken '\*' mag alleen aan het einde. Geldige voorbeelden worden/xyz, / XYZ\*, of /xyz/\*.

   ![Application Gateway "Toevoegen op pad gebaseerde regel"-blade](./media/traffic-manager-load-balancing-azure/s2-appgw-pathrule-blade.png)

### <a name="step-3-add-application-gateways-to-the-traffic-manager-endpoints"></a>Stap 3: Toepassingsgateways met de Traffic Manager-eindpunten toevoegen

In dit scenario, is Traffic Manager verbonden met Toepassingsgateways (zoals geconfigureerd in de voorgaande stappen) die zich in verschillende regio's bevinden. Nu dat de Toepassingsgateways zijn geconfigureerd, wordt de volgende stap is deze verbinding te maken met uw Traffic Manager-profiel.

1. Open uw Traffic Manager-profiel. Om dit te doen, zoekt u in de resourcegroep of zoeken naar de naam van het Traffic Manager-profiel uit **alle Resources**.
2. Selecteer in het linkerdeelvenster **eindpunten**, en klik vervolgens op **toevoegen** een eindpunt toevoegen.

  ![Traffic Manager-eindpunten "toevoegen" knop](./media/traffic-manager-load-balancing-azure/s3-tm-add-endpoint.png)

3. Een eindpunt maken door te voeren van de volgende informatie:

  * **Type**: Selecteer het type van het eindpunt te verdelen. Selecteer in dit scenario **Azure-eindpunt** omdat er wordt verbinding met de application gateway-instanties die eerder zijn geconfigureerd.
  * **Naam**: Voer de naam van het eindpunt.
  * **Doelresourcetype**: Selecteer **openbaar IP-adres** en klik vervolgens onder **Doelresource**, selecteert u het openbare IP-adres van de toepassingsgateway die eerder is geconfigureerd.

   ![Traffic Manager '-eindpunt toevoegen'](./media/traffic-manager-load-balancing-azure/s3-tm-add-endpoint-blade.png)

4. Nu u uw instellingen testen kunt door deze te openen met de DNS-server van uw Traffic Manager-profiel (in dit voorbeeld: TrafficManagerScenario.trafficmanager.net). U kunt aanvragen verzenden, openen of uitvallen van VM's en webservers die zijn gemaakt in verschillende regio's en de instellingen van het Traffic Manager-profiel voor het testen van uw instellingen wijzigen.

### <a name="step-4-create-a-load-balancer"></a>Stap 4: Een load balancer maken

In dit scenario verdeelt Load Balancer het verbindingen van de weblaag naar de databases in een cluster met hoge beschikbaarheid.

Als uw databasecluster met hoge beschikbaarheid-van SQL Server AlwaysOn gebruikmaakt, raadpleegt u [configureren van een of meer altijd op beschikbaarheid van groep Listeners](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) voor stapsgewijze instructies.

Zie voor meer informatie over het configureren van een interne load balancer [een interne load balancer maken in Azure portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md).

1. Klik in de Azure-portal, in het linkerdeelvenster op **een resource maken** > **netwerken** > **Load balancer**.
2. Kies een naam voor de load balancer.
3. Stel de **Type** naar **intern**, en kies het juiste virtuele netwerk en subnet voor de load balancer bevinden zich in.
4. Onder **IP-adrestoewijzing**, selecteert u **dynamische** of **statische**.
5. Onder **resourcegroep**, kiest u de resourcegroep voor de load balancer.
6. Onder **locatie**, kiest u de juiste regio voor de load balancer.
7. Klik op **maken** voor het genereren van de load balancer.

#### <a name="connect-a-back-end-database-tier-to-the-load-balancer"></a>Verbinding maken met een back-end-database-laag van de load balancer

1. Vinden van de resourcegroep, de load balancer die in de vorige stappen is gemaakt.
2. Onder **instellingen**, klikt u op **back-endpools**, en klik vervolgens op **toevoegen** om toe te voegen een back-end-groep.

  ![Load Balancer 'Back-endpool toevoegen'](./media/traffic-manager-load-balancing-azure/s4-ilb-add-bepool.png)

3. Voer de naam van de back-endpool.
4. Afzonderlijke computers of een beschikbaarheidsset om de back-endpool toevoegen.

#### <a name="configure-a-probe"></a>Een test configureren

1. In de load balancer, onder **instellingen**, selecteer **tests**, en klik vervolgens op **toevoegen** om toe te voegen een test.

 ![Load Balancer 'Test toevoegen'](./media/traffic-manager-load-balancing-azure/s4-ilb-add-probe.png)

2. Voer de naam voor de test.
3. Selecteer de **Protocol** voor de test. Voor een database, kunt u een TCP-test in plaats van een HTTP-test. Raadpleeg voor meer informatie over load balancer-tests, [begrijpen load balancer-testen](../load-balancer/load-balancer-custom-probe-overview.md).
4. Voer de **poort** van uw database moet worden gebruikt voor toegang tot de test.
5. Onder **Interval**, Geef op hoe vaak moet de toepassing test.
6. Onder **drempelwaarde voor onjuiste status**, geef het aantal doorlopend testfouten dat moet optreden voor de back-end virtuele machine om te worden als slecht beschouwd.
7. Klik op **OK** te maken van de test.

#### <a name="configure-the-load-balancing-rules"></a>De regels voor taakverdeling configureren

1. Onder **instellingen** van de load balancer, selecteer **Taakverdelingsregels**, en klik vervolgens op **toevoegen** om een regel te maken.
2. Voer de **naam** voor de regel voor taakverdeling.
3. Kies de **front-end-IP-adres** van de load balancer, **Protocol**, en **poort**.
4. Onder **back-endpoort**, geef de poort moet worden gebruikt in de back-endpool.
5. Selecteer de **back-endpool** en de **Probe** die zijn gemaakt in de vorige stappen om toe te passen van de regel.
6. Onder **sessiepersistentie**, kiezen hoe u de sessies om vast te leggen.
7. Onder **inactief time-outs**, geef het aantal minuten voordat een time-out voor inactiviteit.
8. Onder **zwevend IP**, selecteert u **uitgeschakelde** of **ingeschakeld**.
9. Klik op **OK** om de regel te maken.

### <a name="step-5-connect-web-tier-vms-to-the-load-balancer"></a>Stap 5: Weblaag VM's verbinden met de load balancer

Nu configureren we de IP-adres en de load balancer front-endpoort in de toepassingen die worden uitgevoerd op uw weblaag VM's voor elke databaseverbindingen. Deze configuratie is specifiek voor de toepassingen die worden uitgevoerd op deze virtuele machines. Raadpleeg de documentatie van de toepassing voor het configureren van de doel-IP-adres en poort. Ga voor het IP-adres van de front-end, in Azure portal, gaat u naar het front-end-IP-adresgroep op de **instellingen netwerkverdeler**.

![Load Balancer "Frontend-IP-adresgroep" navigatiedeelvenster](./media/traffic-manager-load-balancing-azure/s5-ilb-frontend-ippool.png)

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van Traffic Manager](traffic-manager-overview.md)
* [Overzicht van Application Gateway](../application-gateway/application-gateway-introduction.md)
* [Overzicht van Azure Load Balancer](../load-balancer/load-balancer-overview.md)
