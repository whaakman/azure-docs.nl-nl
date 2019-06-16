---
title: Verbinding maken met virtuele Azure-netwerken van Azure Logic Apps via een integratie van service-omgeving (ISE)
description: Maken van een integratie van service-omgeving (ISE) zodat logic apps en integratieaccounts toegang krijgen tot Azure-netwerken (VNETs), behoud van persoonlijke en gescheiden van openbare of 'global' Azure
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 05/20/2019
ms.openlocfilehash: bd1f06c93a75673f86f0c52f78cad8a60f7a1a1e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65961446"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-by-using-an-integration-service-environment-ise"></a>Verbinding maken met virtuele Azure-netwerken van Azure Logic Apps met behulp van een integratie van service-omgeving (ISE)

Voor scenario's waar uw logic apps en de integratieaccounts toegang hebben tot moeten een [virtueel Azure-netwerk](../virtual-network/virtual-networks-overview.md), maak een [ *integratieserviceomgeving* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). Een ISE is een privé- en geïsoleerde omgeving die gebruikmaakt van opslagruimte en andere bronnen die zijn geïsoleerd van de openbare of 'global' Logic Apps-service. Dankzij deze scheiding vermindert ook eventuele gevolgen die andere Azure-tenants op de prestaties van uw apps hebben kunnen. Is voor uw ISE *geïnjecteerd* in met uw Azure-netwerk, die vervolgens implementeert u de Logic Apps-service in uw virtuele netwerk. Wanneer u een logische app of integratie-account maakt, selecteert u deze ISE als hun locatie. Uw logische app of integratie-account kan vervolgens rechtstreeks toegang tot resources, zoals virtuele machines (VM's), servers, systemen en services in uw virtuele netwerk.

![Integratie van service-omgeving selecteren](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

In dit artikel laat zien hoe deze taken uit te voeren:

* Zorg ervoor dat alle vereiste poorten in een virtueel netwerk zijn geopend zodat verkeer kan worden verzonden via de integratie van service-omgeving (ISE) via de subnetten in dit virtuele netwerk.

* Maak de integratie van service-omgeving (ISE).

* Maak een logische app die kan worden uitgevoerd in de ISE.

* Maak een integratieaccount voor uw logische apps in uw ISE.

Zie voor meer informatie over de integratie van service-omgevingen, [toegang tot resources van Azure Virtual Network van Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>.

  > [!IMPORTANT]
  > Logic apps, ingebouwde triggers, ingebouwde acties en connectors die worden uitgevoerd in de ISE-gebruik een prijsstelling verschilt van de prijsstelling op basis van gebruik. Zie voor meer informatie, [prijzen voor Logic Apps](../logic-apps/logic-apps-pricing.md).

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

## <a name="check-network-ports"></a>Controleer de netwerk-poorten

Wanneer u een integratie van service-omgeving (ISE) met een virtueel netwerk gebruiken, is een veelvoorkomend probleem voor setup heeft een of meer geblokkeerde poorten. De connectors die u gebruikt voor het maken van verbindingen tussen uw ISE en het doelsysteem hebben mogelijk ook hun eigen Poortvereisten. Bijvoorbeeld, als u met een FTP-systeem kunnen communiceren met behulp van de FTP-connector, zorg ervoor dat de poort die u op dat de FTP-systeem, zoals poort 21 voor het verzenden van opdrachten, beschikbaar is.

Voor het beheren van het verkeer via subnetten van het virtuele netwerk waarin u ISE implementeren, kunt u instellen [netwerkbeveiligingsgroepen](../virtual-network/security-overview.md) door [netwerkverkeer filteren tussen subnetten](../virtual-network/tutorial-filter-network-traffic.md). Uw ISE moet echter specifieke poorten zijn geopend op het virtuele netwerk dat gebruikmaakt van netwerkbeveiligingsgroepen hebben. Op die manier uw ISE blijft toegankelijk is en correct kan werken, zodat je geen toegang tot uw ISE verliest. Anders, als alle vereiste poorten beschikbaar zijn, uw ISE werkt niet meer.

Deze tabellen beschrijven de poorten in het virtuele netwerk dat gebruikmaakt van uw ISE en waar deze poorten ophalen gebruikt. De [Resource Manager-servicetags](../virtual-network/security-overview.md#service-tags) vertegenwoordigt een groep IP-adresvoorvoegsels die helpen bij het minimaliseren van complexiteit bij het maken van beveiligingsregels.

> [!IMPORTANT]
> Voor de interne communicatie binnen de subnetten van ISE vereist is dat u alle poorten binnen deze subnetten openen.

| Doel | Direction | Poorten | Bronservicetag | Doelservicetag | Opmerkingen |
|---------|-----------|-------|--------------------|-------------------------|-------|
| Communicatie van Azure Logic Apps | Uitgaande | 80 & 443 | VirtualNetwork | Internet | De poort, is afhankelijk van de externe service waarmee de Logic Apps-service communiceert |
| Azure Active Directory | Uitgaande | 80 & 443 | VirtualNetwork | AzureActiveDirectory | |
| Afhankelijk van Azure Storage | Uitgaande | 80 & 443 | VirtualNetwork | Opslag | |
| Intersubnet communicatie | Inkomende en uitgaande | 80 & 443 | VirtualNetwork | VirtualNetwork | Voor de communicatie tussen subnetten |
| Communicatie met Azure Logic Apps | Inkomend | 443 | Internet  | VirtualNetwork | Het IP-adres voor de computer of service die een aanvraag als trigger of een webhook die deel uitmaakt van uw logische app aanroepen. Sluiten of te blokkeren van deze poort wordt voorkomen dat HTTP-aanroepen naar logic apps met triggers voor aanvragen.  |
| Logische app uitvoeringsgeschiedenis | Inkomend | 443 | Internet  | VirtualNetwork | Geschiedenis van de uitvoering van het IP-adres voor de computer van waaruit u de logische app weergeven. Hoewel sluiten of te blokkeren van deze poort wordt niet dat u de uitvoeringsgeschiedenis bekijken voorkomen, kunt u de invoer niet weergeven en uitvoer voor elke stap in die de geschiedenis uitvoeren. |
| Verbindingsbeheer | Uitgaande | 443 | VirtualNetwork  | Internet | |
| Diagnostische logboeken en metrische gegevens publiceren | Uitgaande | 443 | VirtualNetwork  | AzureMonitor | |
| Communicatie van Azure Traffic Manager | Inkomend | 443 | AzureTrafficManager | VirtualNetwork | |
| Ontwerper van logische Apps - dynamische eigenschappen | Inkomend | 454 | Internet  | VirtualNetwork | Aanvragen afkomstig zijn van de Logic Apps [toegang hebben tot eindpunt inkomende IP-adressen in die regio](../logic-apps/logic-apps-limits-and-config.md#inbound). |
| Service Management-App-afhankelijkheid | Inkomend | 454 & 455 | AppServiceManagement | VirtualNetwork | |
| Connector-implementatie | Inkomend | 454 & 3443 | Internet  | VirtualNetwork | Dit is nodig voor het implementeren en bijwerken van connectors. Sluiten of te blokkeren van deze poort zorgt ervoor dat de ISE-implementaties mislukken en voorkomt u dat updates van de connector of oplossingen. |
| Azure SQL-afhankelijkheid | Uitgaande | 1433 | VirtualNetwork | SQL |
| Azure Resource Health | Uitgaande | 1886 | VirtualNetwork | Internet | Voor het publiceren van de integriteitsstatus van de op Resource Health |
| API Management - beheereindpunt | Inkomend | 3443 | APIManagement  | VirtualNetwork | |
| Afhankelijkheid van logboek naar Event Hub-beleid en de monitoring agent | Uitgaande | 5672 | VirtualNetwork  | EventHub | |
| Toegang tot Azure Cache voor instanties van Redis tussen Rolinstanties | Inkomend <br>Uitgaande | 6379-6383 | VirtualNetwork  | VirtualNetwork | Ook voor ISE voor gebruik met Azure Cache voor Redis, moet u openen deze [binnenkomende en uitgaande poorten die worden beschreven in de Cache van Azure voor veelgestelde vragen over Redis](../azure-cache-for-redis/cache-how-to-premium-vnet.md#outbound-port-requirements). |
| Azure Load Balancer | Inkomend | * | AzureLoadBalancer | VirtualNetwork |  |
||||||

<a name="create-environment"></a>

## <a name="create-your-ise"></a>Uw ISE maken

Volg deze stappen voor het maken van de integratie van service-omgeving (ISE):

1. In de [Azure-portal](https://portal.azure.com), selecteer in het hoofdmenu van Azure **een resource maken**.
Typ 'integratieserviceomgeving' als filter in het zoekvak.

   ![Nieuwe resource maken](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. Kies in het deelvenster van het maken van Integratieserviceomgeving **maken**.

   ![Kies 'Maken'](./media/connect-virtual-network-vnet-isolated-environment/create-integration-service-environment.png)

1. Geef de volgende gegevens voor uw omgeving, en kies vervolgens **revisie + maken**, bijvoorbeeld:

   ![Geef details van de omgeving](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Eigenschap | Vereist | Value | Description |
   |----------|----------|-------|-------------|
   | **Abonnement** | Ja | <*Azure-subscription-name*> | Het Azure-abonnement moet worden gebruikt voor uw omgeving |
   | **Resourcegroep** | Ja | <*Azure-resource-group-name*> | De Azure-resourcegroep waar u om uw omgeving te maken |
   | **Naam van integratieserviceomgeving** | Ja | <*naam van omgeving*> | De naam te geven van uw omgeving |
   | **Location** | Ja | <*Azure-datacenter-region*> | De Azure-datacenter-regio waar u om uw omgeving te implementeren |
   | **Extra capaciteit** | Ja | 0 tot en met 10 | Het aantal eenheden van de extra verwerking moet worden gebruikt voor deze resource ISE. Zie voor informatie over het toevoegen van capaciteit na het maken van [toevoegen ISE capaciteit](#add-capacity). |
   | **Virtueel netwerk** | Ja | <*Azure-virtual-network-name*> | De Azure-netwerk waarin u invoeren van uw omgeving wilt, zodat logic apps in die omgeving krijgen uw virtuele netwerk tot toegang. Als u een netwerk, geen [maakt u eerst een Azure virtual network](../virtual-network/quick-create-portal.md). <p>**Belangrijke**: U kunt *alleen* deze injectie uitvoeren bij het maken van uw ISE. |
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
      * **Adresbereik (CIDR-blok)** : Bereik van uw subnet in het virtuele netwerk en in CIDR-indeling

      ![Subnetgegevens van het toevoegen](./media/connect-virtual-network-vnet-isolated-environment/subnet-details.png)

   1. Als u klaar bent, kiest u **Done**.

   1. Herhaal deze stappen voor drie meer subnetten.

      > [!NOTE]
      > Als de subnetten die u probeert te maken, niet geldig, worden de Azure-portal ziet u een bericht, maar uw voortgang niet blokkeert.

1. Nadat Azure uw ISE-gegevens met succes worden gevalideerd, kiest u **maken**, bijvoorbeeld:

   ![Nadat de validatie is geslaagd, kiest u 'Maken'](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure begint met het implementeren van uw omgeving, maar dit proces *mogelijk* duurt maximaal twee uur voordat u klaar bent. 
   Om te controleren of de implementatiestatus op de werkbalk van Azure, kies het meldingspictogram opent u het deelvenster meldingen.

   ![De implementatiestatus van de controleren](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Als de implementatie is voltooid, ziet u Azure deze melding:

   ![Implementatie is voltooid](./media/connect-virtual-network-vnet-isolated-environment/deployment-success.png)

   Anders gaat u als volgt de instructies voor Azure portal voor problemen met implementatie oplossen.

   > [!NOTE]
   > Als implementatie mislukt of als u uw ISE, verwijdert Azure kan een uur duren voordat uw subnetten zijn vrijgegeven. Deze vertraging betekent dat betekent dat die u mogelijk moet worden gewacht voordat opnieuw gebruiken van deze subnetten in een andere ISE. 
   >
   > Als u uw virtuele netwerk verwijdert, wordt in het algemeen duurt maximaal twee uur voordat het vrijgeven van de subnetten van het Azure, maar met deze bewerking kan het langer duren. 
   > Bij het verwijderen van virtuele netwerken, zorg ervoor dat er nog steeds geen resources zijn aangesloten. Zie [virtueel netwerk verwijderen](../virtual-network/manage-virtual-network.md#delete-a-virtual-network).

1. Als u uw omgeving, kiest u **naar de resource gaan** als Azure niet automatisch naar uw omgeving gaat na de implementatie is voltooid.  

Zie voor meer informatie over het maken van subnets, [toevoegen van een virtueel netwerksubnet](../virtual-network/virtual-network-manage-subnet.md).

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-app---ise"></a>Logische app - ISE maken

Voor het maken van logische apps die worden uitgevoerd in de integratie van service-omgeving (ISE), [uw logische apps maken op de gebruikelijke manier](../logic-apps/quickstart-create-first-logic-app-workflow.md) , behalve tijdens het instellen van de **locatie** eigenschap, selecteert u uw ISE uit de  **Integratieserviceomgevingen** sectie, bijvoorbeeld:

  ![Integratie van service-omgeving selecteren](./media/connect-virtual-network-vnet-isolated-environment/create-logic-app-with-integration-service-environment.png)

Voor verschillen in hoe triggers en acties werken en hoe ze bent met het label wanneer u een ISE vergeleken met de globale Logic Apps-service, Zie [geïsoleerd ten opzichte van globale in het overzicht van ISE](connect-virtual-network-vnet-isolated-environment-overview.md#difference).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-account---ise"></a>Integratieaccount - ISE maken

Als u wilt een integratieaccount gebruiken met logic apps in een integratieserviceomgeving (ISE), dat integratieaccount moet gebruiken de *dezelfde omgeving* als de logic apps. Logische apps in een ISE kunnen verwijzen naar alleen integratieaccounts in dezelfde ISE.

Maken van een integratieaccount dat gebruikmaakt van een ISE [uw integratie-account maken in de gebruikelijke manier](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) , behalve tijdens het instellen van de **locatie** eigenschap, selecteert u uw ISE uit de **integratie Service-omgevingen** sectie, bijvoorbeeld:

![Integratie van service-omgeving selecteren](./media/connect-virtual-network-vnet-isolated-environment/create-integration-account-with-integration-service-environment.png)

<a name="add-capacity"></a>

## <a name="add-ise-capacity"></a>ISE-capaciteit toevoegen

De basiseenheid voor uw ISE-capaciteit, is opgelost, zodat als u meer doorvoer nodig hebt, kunt u meer schaaleenheden toevoegen. U kunt automatisch schalen op basis van metrische gegevens voor prestaties of op basis van een aantal extra verwerkingseenheden. Als u automatisch schalen op basis van metrische gegevens, kunt u kiezen uit verschillende criteria en geef de voorwaarden van de drempelwaarde voor het voldoen aan deze criteria.

1. In de Azure-portal zoeken uw ISE.

1. Als u wilt controleren van metrische gegevens over gebruik en prestaties voor uw ISE, in het hoofdmenu van uw ISE, selecteer **overzicht**.

   ![Gebruik voor ISE weergeven](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-usage.png)

1. Voor het instellen van automatisch schalen, onder **instellingen**, selecteer **uitschalen**. Op de **configureren** tabblad **automatisch schalen inschakelen**.

   ![Automatisch schalen inschakelen](./media/connect-virtual-network-vnet-isolated-environment/scale-out.png)

1. Voor **naam van de instelling voor automatisch schalen**, Geef een naam op voor de instelling.

1. In de **standaard** sectie, kiest u **schalen op basis van een metrische waarde** of **schalen naar een specifiek aantal instanties**.

   * Als u ervoor op basis van het exemplaar kiest, voert u het aantal verwerkingseenheden tussen 0 en 10 liggen.

   * Als u ervoor op basis van metrische gegevens kiest, volgt u deze stappen:

     1. In de **regels** sectie **toevoegen van een regel**.

     1. Op de **schaalregel** deelvenster, instellen van uw criteria en de actie moet worden uitgevoerd wanneer de regel wordt geactiveerd.

     1. Wanneer u klaar bent, kiest u **toevoegen**.

1. Wanneer u klaar met de instellingen voor automatisch schalen bent, moet u de wijzigingen opslaan.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)
* Meer informatie over [integratie van virtuele netwerken voor Azure-services](../virtual-network/virtual-network-for-azure-services.md)
