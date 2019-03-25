---
title: Verbinding maken met virtuele Azure-netwerken van Azure Logic Apps via een integratie van service-omgeving (ISE)
description: Maken van een integratie van service-omgeving (ISE) zodat logic apps en integratieaccounts toegang krijgen tot Azure-netwerken (VNETs), behoud van persoonlijke en gescheiden van openbare of 'global' Azure
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 03/12/2019
ms.openlocfilehash: 2f84c48092581a313ff7bead7a862221e0fe4eee
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58400924"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-by-using-an-integration-service-environment-ise"></a>Verbinding maken met virtuele Azure-netwerken van Azure Logic Apps met behulp van een integratie van service-omgeving (ISE)

> [!NOTE]
> Deze mogelijkheid is in [ *openbare preview*](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Voor scenario's waar uw logic apps en de integratieaccounts toegang hebben tot moeten een [virtueel Azure-netwerk](../virtual-network/virtual-networks-overview.md), maak een [ *integratieserviceomgeving* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). Een ISE is een privé- en geïsoleerde omgeving die gebruikmaakt van opslagruimte en andere bronnen die zijn geïsoleerd van de openbare of 'global' Logic Apps-service. Dankzij deze scheiding vermindert ook eventuele gevolgen die andere Azure-tenants op de prestaties van uw apps hebben kunnen. Is voor uw ISE *geïnjecteerd* in met uw Azure-netwerk, die vervolgens implementeert u de Logic Apps-service in uw virtuele netwerk. Wanneer u een logische app of integratie-account maakt, selecteert u deze ISE als hun locatie. Uw logische app of integratie-account kan vervolgens rechtstreeks toegang tot resources, zoals virtuele machines (VM's), servers, systemen en services in uw virtuele netwerk.

![Integratie van service-omgeving selecteren](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

In dit artikel laat zien hoe deze taken uit te voeren:

* Poorten instellen in uw Azure-netwerk, zodat verkeer kan worden verzonden via de integratie van service-omgeving (ISE) via subnetten in het virtuele netwerk.

* Maak de integratie van service-omgeving (ISE).

* Maak een logische app die kan worden uitgevoerd in de ISE.

* Maak een integratieaccount voor uw logische apps in uw ISE.

Zie voor meer informatie over de integratie van service-omgevingen, [toegang tot resources van Azure Virtual Network van Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>.

  > [!IMPORTANT]
  > Logic apps, ingebouwde acties en connectors die worden uitgevoerd in de ISE-gebruik een andere prijsschema, niet de verbruik gebaseerde prijsstelling. Zie voor meer informatie, [prijzen voor Logic Apps](../logic-apps/logic-apps-pricing.md).

* Een [virtueel Azure-netwerk](../virtual-network/virtual-networks-overview.md). Als u een virtueel netwerk hebt, krijgt u informatie over het [maken van een Azure virtual network](../virtual-network/quick-create-portal.md). 

  * Het virtuele netwerk moet vier *leeg* subnetten voor het implementeren en het maken van resources in uw ISE. U kunt deze subnetten vooraf maken of kunt u wachten tot het maken van uw ISE waarin u de subnetten op hetzelfde moment kunt maken. Meer informatie over [subnetvereisten](#create-subnet). 
  
    > [!NOTE]
    > Als u [ExpressRoute](../expressroute/expressroute-introduction.md), waarmee u een particuliere verbinding met Microsoft cloud-services, moet u [een routetabel maken](../virtual-network/manage-route-table.md) waarvoor het volgende routeren en die tabel koppelen aan elk subnet dat wordt gebruikt door uw ISE:
    > 
    > **Naam**: <*route-naam*><br>
    > **Adresvoorvoegsel**: 0.0.0.0/0<br>
    > **Volgende hop**: Internet

  * Zorg ervoor dat uw virtuele netwerk [maakt deze poorten beschikbaar](#ports) , zodat uw ISE correct werkt en toegankelijk blijft.

* Als u wilt gebruiken van aangepaste DNS-servers voor uw Azure-netwerk, [die servers instellen door de volgende stappen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) voordat u uw ISE op uw virtuele netwerk implementeert. Anders wordt hebt telkens wanneer u uw DNS-server, u ook opnieuw opstarten van uw ISE, dit is een functie die beschikbaar is in openbare preview van ISE.

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="ports"></a>

## <a name="set-up-network-ports"></a>Instellen van netwerk-poorten

Voor het goed werkt en blijven toegankelijk is, moet de integratie van service-omgeving (ISE) specifieke poorten zijn beschikbaar in het virtuele netwerk. Anders als een van deze poorten niet beschikbaar zijn, u mogelijk geen toegang meer voor uw ISE, te werken. Wanneer u een ISE in een virtueel netwerk gebruikt, is een veelvoorkomend probleem voor setup heeft een of meer geblokkeerde poorten. Voor verbindingen tussen uw ISE en het doelsysteem, de connector die u mogelijk ook een eigen port requirements for Windows. Bijvoorbeeld, als u met een FTP-systeem kunnen communiceren met behulp van de FTP-connector, zorg ervoor dat de poort die u op dat de FTP-systeem, zoals poort 21 voor het verzenden van opdrachten, beschikbaar is.

Voor het beheren van het verkeer tussen subnetten van het virtuele netwerk waarin u uw ISE implementeren, kunt u instellen [netwerkbeveiligingsgroepen](../virtual-network/security-overview.md) voor deze subnetten door [netwerkverkeer filteren tussen subnetten](../virtual-network/tutorial-filter-network-traffic.md). Deze tabellen beschrijven de poorten in het virtuele netwerk dat gebruikmaakt van uw ISE en waar deze poorten ophalen gebruikt. De [servicetag](../virtual-network/security-overview.md#service-tags) vertegenwoordigt een groep IP-adresvoorvoegsels die helpen bij het minimaliseren van complexiteit bij het maken van beveiligingsregels.

> [!IMPORTANT]
> Voor de interne communicatie binnen de subnetten van ISE vereist is dat u alle poorten binnen deze subnetten openen.

| Doel | Richting | Poorten | Bronservicetag | Doelservicetag | Opmerkingen |
|---------|-----------|-------|--------------------|-------------------------|-------|
| Communicatie van Azure Logic Apps | Uitgaand | 80 & 443 | VIRTUAL_NETWORK | INTERNET | De poort, is afhankelijk van de externe service waarmee de Logic Apps-service communiceert |
| Azure Active Directory | Uitgaand | 80 & 443 | VIRTUAL_NETWORK | AzureActiveDirectory | |
| Afhankelijk van Azure Storage | Uitgaand | 80 & 443 | VIRTUAL_NETWORK | Opslag | |
| Intersubnet communicatie | Inkomende en uitgaande | 80 & 443 | VIRTUAL_NETWORK | VIRTUAL_NETWORK | Voor de communicatie tussen subnetten |
| Communicatie met Azure Logic Apps | Inkomend | 443 | INTERNET  | VIRTUAL_NETWORK | Het IP-adres voor de computer of service die een aanvraag als trigger of een webhook die deel uitmaakt van uw logische app aanroepen. Sluiten of te blokkeren van deze poort wordt voorkomen dat HTTP-aanroepen naar logic apps met triggers voor aanvragen.  |
| Logische app uitvoeringsgeschiedenis | Inkomend | 443 | INTERNET  | VIRTUAL_NETWORK | Geschiedenis van de uitvoering van het IP-adres voor de computer van waaruit u de logische app weergeven. Hoewel sluiten of te blokkeren van deze poort wordt niet dat u de uitvoeringsgeschiedenis bekijken voorkomen, kunt u de invoer niet weergeven en uitvoer voor elke stap in die de geschiedenis uitvoeren. |
| Verbindingsbeheer | Uitgaand | 443 | VIRTUAL_NETWORK  | INTERNET | |
| Diagnostische logboeken en metrische gegevens publiceren | Uitgaand | 443 | VIRTUAL_NETWORK  | AzureMonitor | |
| Ontwerper van logische Apps - dynamische eigenschappen | Inkomend | 454 | INTERNET  | VIRTUAL_NETWORK | Aanvragen afkomstig zijn van de Logic Apps [toegang hebben tot eindpunt inkomende IP-adressen in die regio](../logic-apps/logic-apps-limits-and-config.md#inbound). |
| Service Management-App-afhankelijkheid | Inkomend | 454 & 455 | AppServiceManagement | VIRTUAL_NETWORK | |
| Connector-implementatie | Inkomend | 454 & 3443 | INTERNET  | VIRTUAL_NETWORK | Dit is nodig voor het implementeren en bijwerken van connectors. Sluiten of te blokkeren van deze poort zorgt ervoor dat de ISE-implementaties mislukken en voorkomt u dat updates van de connector of oplossingen. |
| Azure SQL-afhankelijkheid | Uitgaand | 1433 | VIRTUAL_NETWORK | SQL |
| Azure Resource Health | Uitgaand | 1886 | VIRTUAL_NETWORK | INTERNET | Voor het publiceren van de integriteitsstatus van de op Resource Health |
| API Management - beheereindpunt | Inkomend | 3443 | APIManagement  | VIRTUAL_NETWORK | |
| Afhankelijkheid van logboek naar Event Hub-beleid en de monitoring agent | Uitgaand | 5672 | VIRTUAL_NETWORK  | EventHub | |
| Toegang tot Azure Cache voor instanties van Redis tussen Rolinstanties | Inkomend <br>Uitgaand | 6379-6383 | VIRTUAL_NETWORK  | VIRTUAL_NETWORK | Ook voor ISE voor gebruik met Azure Cache voor Redis, moet u openen deze [binnenkomende en uitgaande poorten die worden beschreven in de Cache van Azure voor veelgestelde vragen over Redis](../azure-cache-for-redis/cache-how-to-premium-vnet.md#outbound-port-requirements). |
| Azure Load Balancer | Inkomend | * | AZURE_LOAD_BALANCER | VIRTUAL_NETWORK |  |
||||||

<a name="create-environment"></a>

## <a name="create-your-ise"></a>Uw ISE maken

Volg deze stappen voor het maken van de integratie van service-omgeving (ISE):

1. In de [Azure-portal](https://portal.azure.com), selecteer in het hoofdmenu van Azure **een resource maken**.

   ![Nieuwe resource maken](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. Typ 'integratieserviceomgeving' als filter in het zoekvak.
Selecteer in de lijst met resultaten **Integratieserviceomgeving (preview)**, en kies vervolgens **maken**.

   ![Selecteer 'Integratieserviceomgeving'](./media/connect-virtual-network-vnet-isolated-environment/select-integration-service-environment.png)

   ![Kies 'Maken'](./media/connect-virtual-network-vnet-isolated-environment/create-integration-service-environment.png)

1. Geef de volgende gegevens voor uw omgeving, en kies vervolgens **revisie + maken**, bijvoorbeeld:

   ![Geef details van de omgeving](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Eigenschap | Vereist | Value | Description |
   |----------|----------|-------|-------------|
   | **Abonnement** | Ja | <*Azure-subscription-name*> | Het Azure-abonnement moet worden gebruikt voor uw omgeving |
   | **Resourcegroep** | Ja | <*Azure-resource-group-name*> | De Azure-resourcegroep waar u om uw omgeving te maken |
   | **Naam van integratieserviceomgeving** | Ja | <*naam van omgeving*> | De naam te geven van uw omgeving |
   | **Locatie** | Ja | <*Azure-datacenter-region*> | De Azure-datacenter-regio waar u om uw omgeving te implementeren |
   | **Extra capaciteit** | Ja | 0, 1, 2, 3 | Het aantal verwerkingseenheden moet worden gebruikt voor deze resource ISE. Zie voor informatie over het toevoegen van capaciteit na het maken van [toevoegen van capaciteit](#add-capacity). |
   | **Virtueel netwerk** | Ja | <*Azure-virtual-network-name*> | De Azure-netwerk waarin u invoeren van uw omgeving wilt, zodat logic apps in die omgeving krijgen uw virtuele netwerk tot toegang. Als u een netwerk hebt, kunt u een maken hier. <p>**Belangrijke**: U kunt *alleen* deze injectie uitvoeren bij het maken van uw ISE. Echter, voordat u deze relatie maken kunt, zorg ervoor dat u al [instellen van op rollen gebaseerd toegangsbeheer in uw virtuele netwerk voor Azure Logic Apps](#vnet-access). |
   | **Subnets** | Ja | <*subnet-resource-list*> | Een ISE vereist is vier *leeg* subnetten voor het maken van resources in uw omgeving. Elk subnet maken [Volg de stappen onder deze tabel](#create-subnet).  |
   |||||

   <a name="create-subnet"></a>

   **Subnet maken**

   Voor het maken van resources in uw omgeving, moet uw ISE vier *leeg* subnetten die niet worden gedelegeerd naar elke service. 
   U *kan geen* deze subnetadressen wijzigen nadat u uw omgeving hebt gemaakt. Elk subnet moet voldoen aan deze criteria voldoen:

   * Heeft een naam die begint met een letter of onderstrepingsteken en heeft deze tekens: `<`, `>`, `%`, `&`, `\\`, `?`, `/`

   * Maakt gebruik van de [notatie (Classless Inter-Domain Routing)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) en de adresruimte van een klasse B.

   * Maakt gebruik van ten minste een `/27` in het adres ruimte omdat elk subnet 32 adressen als moet de *minimale*. Bijvoorbeeld:

     * `10.0.0.0/27` 32 adressen heeft, omdat 2<sup>(32-27)</sup> 2<sup>5</sup> of 32.

     * `10.0.0.0/24` 256-adressen heeft omdat 2<sup>(32-24)</sup> 2<sup>8</sup> of 256.

     * `10.0.0.0/28` alleen 16 adressen heeft en is te klein omdat 2<sup>(32-28)</sup> 2<sup>4</sup> of 16.

     Zie voor meer informatie over het berekenen van adressen, [IPv4 CIDR-blokken](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#IPv4_CIDR_blocks).

   * Als u [ExpressRoute](../expressroute/expressroute-introduction.md), moet u [een routetabel maken](../virtual-network/manage-route-table.md) waarvoor het volgende routeren en die tabel koppelen aan elk subnet dat wordt gebruikt door uw ISE:

     **Naam**: <*route-naam*><br>
     **Adresvoorvoegsel**: 0.0.0.0/0<br>
     **Volgende hop**: Internet

   1. Onder de **subnetten** Kies **beheren subnetconfiguratie**.

      ![Subnetconfiguratie beheren](./media/connect-virtual-network-vnet-isolated-environment/manage-subnet.png)

   1. Op de **subnetten** deelvenster Kies **Subnet**.

      ![Subnet toevoegen](./media/connect-virtual-network-vnet-isolated-environment/add-subnet.png)

   1. Op de **subnet toevoegen** deelvenster deze informatie verstrekken.

      * **Naam**: De naam voor uw subnet
      * **Adresbereik (CIDR-blok)**: Bereik van uw subnet in het virtuele netwerk en in CIDR-indeling

      ![Subnetgegevens van het toevoegen](./media/connect-virtual-network-vnet-isolated-environment/subnet-details.png)

   1. Als u klaar bent, kiest u **Done**.

   1. Herhaal deze stappen voor drie meer subnetten.

1. Nadat Azure uw ISE-gegevens met succes worden gevalideerd, kiest u **maken**, bijvoorbeeld:

   ![Nadat de validatie is geslaagd, kiest u 'Maken'](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure begint met het implementeren van uw omgeving, maar dit proces *mogelijk* duurt maximaal twee uur voordat u klaar bent. 
   Om te controleren of de implementatiestatus op de werkbalk van Azure, kies het meldingspictogram opent u het deelvenster meldingen.

   ![De implementatiestatus van de controleren](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Als de implementatie is voltooid, ziet u Azure deze melding:

   ![Implementatie is voltooid](./media/connect-virtual-network-vnet-isolated-environment/deployment-success.png)

   > [!NOTE]
   > Als implementatie mislukt of als u uw ISE, Azure verwijderen *mogelijk* duren een uur voordat de subnetten zijn vrijgegeven. U moet dus gewacht voordat opnieuw gebruiken van deze subnetten in een andere ISE.

1. Als u uw omgeving, kiest u **naar de resource gaan** als Azure niet automatisch naar uw omgeving gaat na de implementatie is voltooid.  

<a name="add-capacity"></a>

### <a name="add-capacity"></a>Toevoegen van capaciteit

De basiseenheid voor uw ISE-capaciteit, is opgelost, zodat als u meer doorvoer nodig hebt, kunt u meer schaaleenheden toevoegen. U kunt automatisch schalen op basis van metrische gegevens voor prestaties of op basis van een aantal verwerkingseenheden. Als u automatisch schalen op basis van metrische gegevens, kunt u kiezen uit verschillende criteria en geef de voorwaarden van de drempelwaarde voor het voldoen aan deze criteria.

1. In de Azure-portal zoeken uw ISE.

1. Als u wilt weergeven van metrische gegevens voor prestaties voor uw ISE, in het hoofdmenu van uw ISE, kies **overzicht**.

1. Voor het instellen van automatisch schalen, onder **instellingen**, selecteer **uitschalen**. Op de **configureren** tabblad **automatisch schalen inschakelen**.

1. In de **standaard** sectie, kiest u **schalen op basis van een metrische waarde** of **schalen naar een specifiek aantal instanties**.

1. Als u ervoor op basis van het exemplaar kiest, voert u het aantal verwerkingseenheden tussen 0 en 3 liggen. Anders wordt voor follow-up op basis van metrische gegevens als volgt:

   1. In de **standaard** sectie **toevoegen van een regel**.

   1. Op de **schaalregel** deelvenster, instellen van uw criteria en de actie moet worden uitgevoerd wanneer de regel wordt geactiveerd.

   1. Wanneer u klaar bent, kiest u **toevoegen**.

1. Wanneer u klaar bent, moet u uw wijzigingen op te slaan.

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-app---ise"></a>Logische app - ISE maken

Voor het maken van logische apps die de integratie van service-omgeving (ISE) gebruiken, volgt u de stappen in [over het maken van een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md) maar met deze verschillen: 

* Bij het maken van uw logische app, onder de **locatie** eigenschap, selecteert u uw ISE uit de **integratieserviceomgevingen** sectie, bijvoorbeeld:

  ![Integratie van service-omgeving selecteren](./media/connect-virtual-network-vnet-isolated-environment/create-logic-app-with-integration-service-environment.png)

* U kunt de dezelfde ingebouwde triggers en acties, zoals HTTP, die worden uitgevoerd in de dezelfde ISE als uw logische app gebruiken. Connectors met de **ISE** labelen ook uitvoeren in de dezelfde ISE als uw logische app. Connectors zonder de **ISE** label uitgevoerd in de globale Logic Apps-service.

  ![ISE-connectors selecteren](./media/connect-virtual-network-vnet-isolated-environment/select-ise-connectors.png)

* Nadat u uw ISE in een Azure-netwerk invoeren, kunnen de logische apps in uw ISE rechtstreeks toegang tot resources in dit virtuele netwerk. Injecteer een ISE voor on-premises systemen die zijn verbonden met een virtueel netwerk, in dat netwerk, zodat uw logische apps rechtstreeks toegang deze systemen tot met behulp van deze items: 

  * ISE-connector voor dat systeem, bijvoorbeeld SQL Server
  
  * HTTP-actie 
  
  * Aangepaste connector

  Voor on-premises systemen die zich niet in een virtueel netwerk of geen ISE-connectors, eerst [instellen van de on-premises gegevensgateway](../logic-apps/logic-apps-gateway-install.md).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-account---ise"></a>Integratieaccount - ISE maken

Voor het gebruik van een integratieaccount met logic apps in een integratieserviceomgeving (ISE), dat integratieaccount moet gebruiken de *dezelfde omgeving* als de logic apps. Logische apps in een ISE kunnen verwijzen naar alleen integratieaccounts in dezelfde ISE. 

Volg de stappen in voor het maken van een integratieaccount dat gebruikmaakt van een ISE, [over het maken van integratieaccounts](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) , behalve voor de **locatie** eigenschap waar de **integratieserviceomgevingen**  sectie wordt nu weergegeven. In plaats daarvan selecteert u uw ISE, in plaats van een regio, bijvoorbeeld:

![Integratie van service-omgeving selecteren](./media/connect-virtual-network-vnet-isolated-environment/create-integration-account-with-integration-service-environment.png)

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">Azure Logic Apps forum</a> (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de <a href="https://aka.ms/logicapps-wish" target="_blank">website voor feedback van Logic Apps-gebruikers</a>.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)
* Meer informatie over [integratie van virtuele netwerken voor Azure-services](../virtual-network/virtual-network-for-azure-services.md)
