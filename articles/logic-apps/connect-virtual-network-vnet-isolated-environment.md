---
title: Verbinding maken met virtuele Azure-netwerken van Azure Logic Apps
description: Voor toegang tot Azure-netwerken van Azure Logic Apps, kunt u persoonlijke, exclusieve en geïsoleerd integration service-omgevingen die houden van logische apps maken en andere resources scheiden van openbare of 'global' Azure
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 09/25/2018
ms.openlocfilehash: 354c31014448b914b33d2bef5483efc78092f726
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47391918"
---
# <a name="create-isolated-environments-to-access-azure-virtual-networks-from-azure-logic-apps"></a>Geïsoleerde omgevingen voor toegang tot Azure-netwerken van Azure Logic Apps maken

> [!NOTE]
> Deze mogelijkheid is in *privépreview*. Om te vragen tot, [maken van uw aanvraag voor deelname aan hier](https://aka.ms/iseprivatepreview).

Voor integratie van scenario's waar uw logic apps en de integratieaccounts toegang hebben tot moeten een [virtueel Azure-netwerk](../virtual-network/virtual-networks-overview.md), kunt u een [ *integratieserviceomgeving* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) die is gekoppeld aan het virtuele netwerk en de Logic Apps-service in uw netwerk implementeert. Als u logic apps en integratieaccounts maakt, selecteert u deze ISE als hun locatie. Uw logic apps en de van integratieaccounts kunnen vervolgens rechtstreeks toegang tot resources, zoals virtuele machines (VM's), servers, systemen en services in uw virtuele netwerk. 

![Integratie van service-omgeving selecteren](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

Uw ISE is een privé- en geïsoleerde omgeving die gebruikmaakt van toegewezen opslag en andere resources die afzonderlijk bestaan uit het publiek of *globale* Logic Apps-service. Dankzij deze scheiding helpt ook bij elke die andere Azure-tenants op de prestaties van uw apps hebben kunnen verminderd. 

In dit artikel laat zien hoe deze taken uit te voeren:

* Instellen van machtigingen in uw Azure-netwerk, zodat de persoonlijke Logic Apps-exemplaar toegang heeft tot uw virtuele netwerk.

* Maak de integratie van service-omgeving (ISE). 

* Maak een logische app die kan worden uitgevoerd in de ISE. 

* Maak een integratieaccount voor uw logische apps in uw ISE.

Zie voor meer informatie over de integratie van service-omgevingen, [toegang tot resources van Azure Virtual Network van geïsoleerde Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* Als u niet een virtueel Azure-netwerk hebt, krijgt u informatie over het [maken van een Azure virtual network](../virtual-network/quick-create-portal.md). 

  > [!IMPORTANT]
  > Als u niet een virtueel Azure-netwerk nodig hebt voor het maken van uw omgeving, kunt u *alleen* een virtueel netwerk selecteert als peer van uw omgeving, wanneer u die omgeving maakt. 

* Uw logische apps direct toegang geven tot uw Azure-netwerk, [instellen van machtigingen voor toegangsbeheer op basis van rollen (RBAC)](#vnet-access) , zodat de Logic Apps-service beschikt over de machtigingen voor toegang tot uw virtuele netwerk. 

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="vnet-access"></a>

## <a name="set-virtual-network-permissions"></a>Virtueel netwerkmachtigingen instellen

Wanneer u de integratie van service-omgeving maakt, kunt u een Azure-netwerk als een *peer* voor uw omgeving. Echter, kunt u alleen in deze stap uitvoeren of *peering*, bij het maken van uw omgeving. Deze relatie kiest, kan de Logic Apps-service die rechtstreeks verbinding maken met resources in dit virtuele netwerk en uw omgeving toegang tot deze bronnen. 

Voordat u het virtuele netwerk selecteren kunt, moet u machtigingen voor toegangsbeheer op basis van rollen (RBAC) instellen in uw virtuele netwerk. Als u wilt deze taak hebt voltooid, moet u specifieke rollen toewijzen aan de service Azure Logic Apps.

1. In de [Azure-portal](https://portal.azure.com), zoek en selecteer het virtuele netwerk. 

1. Selecteer in het menu van het virtuele netwerk **toegangsbeheer (IAM)**. 

1. Onder **Access Control**, selecteer **roltoewijzing** als nog niet is geselecteerd. Op de **roltoewijzing** werkbalk, kiest u **toevoegen**. 

   ![Roltoewijzingen toevoegen](./media/connect-virtual-network-vnet-isolated-environment/set-up-role-based-access-control-vnet.png)

1. Op de **machtigingen toevoegen** deelvenster, instellen van elke rol in deze tabel voor de service Azure Logic Apps. Zorg ervoor dat u kiest **opslaan** nadat u klaar bent met elke rol:

   | Rol | Toegang toewijzen aan | Selecteer | 
   |------|------------------|--------|
   | **Inzender voor netwerken** | **Azure AD-gebruiker, groep of toepassing** | Voer **Azure Logic Apps**. Nadat de ledenlijst wordt weergegeven, selecteert u dezelfde waarde. <p>**Tip**: als u deze service niet vinden, voert u de app-ID van de Logic Apps-service: `7cd684f4-8a78-49b0-91ec-6a35d38739ba` | 
   | **Inzender voor klassieke** | **Azure AD-gebruiker, groep of toepassing** | Voer **Azure Logic Apps**. Nadat de ledenlijst wordt weergegeven, selecteert u dezelfde waarde. <p>**Tip**: als u deze service niet vinden, voert u de app-ID van de Logic Apps-service: `7cd684f4-8a78-49b0-91ec-6a35d38739ba` | 
   |||| 

   Bijvoorbeeld:

   ![Machtigingen toevoegen](./media/connect-virtual-network-vnet-isolated-environment/add-contributor-roles.png)

   Zie voor meer informatie over de rolmachtigingen die vereist zijn voor de peering, de [machtigingen sectie in maken, wijzigen of verwijderen van een peering op virtueel netwerk](../virtual-network/virtual-network-manage-peering.md#permissions). 

Als uw virtuele netwerk is verbonden via Azure ExpressRoute, Azure Point-to-Site VPN of Azure Site-naar-Site VPN, doorgaan met de volgende sectie, zodat u de vereiste gatewaysubnet kunt toevoegen. Anders gaat u door met [maken van uw omgeving](#create-environment).

<a name="add-gateway-subnet"></a>

## <a name="add-gateway-subnet-for-virtual-networks-with-expressroute-or-vpns"></a>Gatewaysubnet voor virtuele netwerken met ExpressRoute of VPN-verbindingen toevoegen

Nadat u de vorige stappen hebt voltooid, zodat de integratie van service-omgeving (ISE) toegang tot een Azure-netwerk dat is verbonden via [Azure ExpressRoute](../expressroute/expressroute-introduction.md), [Azure punt-naar-Site VPN](../vpn-gateway/point-to-site-about.md), of [Azure Site-naar-Site VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), moet u ook toevoegen een [ *gatewaysubnet* ](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsub) met het virtuele netwerk:

1. In de [Azure-portal](https://portal.azure.com), zoek en selecteer het virtuele netwerk. Selecteer in het menu van het virtuele netwerk **subnetten**, en kies vervolgens **gatewaysubnet** > **OK**.

   ![Gatewaysubnet toevoegen](./media/connect-virtual-network-vnet-isolated-environment/add-gateway-subnet.png)

1. Maak nu een [ *routetabel*](../virtual-network/manage-route-table.md), die u moet koppelen aan het gatewaysubnet dat u eerder hebt gemaakt.

   1. Selecteer in het hoofdmenu van Azure **een resource maken** > 
    **netwerken** > **routetabel**.

      ![Routetabel maken](./media/connect-virtual-network-vnet-isolated-environment/create-route-table.png)

   1. Informatie over de routetabel, zoals de naam van uw Azure-abonnement te gebruiken, Azure-resourcegroep en locatie opgeven. Zorg ervoor dat de **doorgifte van BGP-route** eigenschap is ingesteld op **ingeschakeld**, en kies vervolgens **maken**.

      ![Geef details van de route-table](./media/connect-virtual-network-vnet-isolated-environment/enter-route-table-information.png)

   1. Selecteer in het menu van de tabel route **subnetten**, en kies vervolgens **koppelen**. 

      ![Routetabel verbinden met subnet](./media/connect-virtual-network-vnet-isolated-environment/associate-route-table.png)

   1. Selecteer **virtueel netwerk**, en selecteer vervolgens het virtuele netwerk.
   
   1. Selecteer **Subnet**, en selecteer vervolgens het eerder gemaakte gatewaysubnet.

   1. Wanneer u klaar bent, kiest u **OK**.

1. Als u een punt-naar-site-VPN hebt, deze stappen te voltooien:

   1. In Azure, zoek en selecteer een resource van uw virtuele netwerkgateway.

   1. Selecteer in het menu van de gateway **punt-naar-site-configuratie**. 
   en kies vervolgens **VPN-client downloaden** zodat u beschikt over de configuratie van de meest recente VPN-client.

      ![Laatste VPN-client downloaden](./media/connect-virtual-network-vnet-isolated-environment/download-vpn-client.png)

U bent nu klaar met het instellen van een gateway-subnet voor virtuele netwerken die gebruikmaken van ExpressRoute, punt-naar-site VPN-verbindingen of site-naar-site VPN-verbindingen. Volg de volgende stappen uit om door te gaan met het maken van de integratie van service-omgeving.

<a name="create-environment"></a>

## <a name="create-your-ise"></a>Uw ISE maken

Volg deze stappen voor het maken van de integratie van service-omgeving (ISE):

1. In de [Azure-portal](https://portal.azure.com), selecteer in het hoofdmenu van Azure **een resource maken**.

   ![Nieuwe resource maken](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. Typ 'integratieserviceomgeving' als filter in het zoekvak.
Selecteer in de lijst met resultaten **Integratieserviceomgeving (preview)**, en kies vervolgens **maken**.

   ![Selecteer 'Integratieserviceomgeving'](./media/connect-virtual-network-vnet-isolated-environment/select-integration-service-environment.png)

   ![Kies 'Maken'](./media/connect-virtual-network-vnet-isolated-environment/create-integration-service-environment.png)

1. Geef de volgende gegevens voor uw omgeving:

   ![Geef details van de omgeving](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Eigenschap | Vereist | Waarde | Beschrijving |
   |----------|----------|-------|-------------|
   | **Naam** | Ja | <*naam van omgeving*> | De naam te geven van uw omgeving | 
   | **Abonnement** | Ja | <*Azure-abonnement-naam*> | Het Azure-abonnement moet worden gebruikt voor uw omgeving | 
   | **Resourcegroep** | Ja | <*Azure-resource-group-name*> | De Azure-resourcegroep waar u om uw omgeving te maken |
   | **Locatie** | Ja | <*Azure-datacenter-regio*> | De Azure-datacenter-regio waar u informatie over uw omgeving op te slaan |
   | **Gelijkwaardige VNET** | Nee | <*Azure-VNET-naam*> | Het Azure-netwerk om te koppelen aan uw omgeving bevinden als een *peer* , zodat logic apps in die omgeving krijgen uw virtuele netwerk tot toegang. Voordat u deze relatie maken kunt, zorg ervoor dat u al [instellen van op rollen gebaseerd toegangsbeheer in uw virtuele netwerk voor Azure Logic Apps](#vnet-access). <p>**Belangrijke**: hoewel een virtueel netwerk is niet vereist, kunt u een virtueel netwerk selecteren *alleen* bij het maken van uw omgeving. | 
   | **De naam van de peering** | Ja, met een geselecteerde virtuele netwerk | <*peering-naam*> | De naam te geven van de peer-relatie | 
   | **VNET-IP-bereik** | Ja, met een geselecteerde virtuele netwerk | <*IP-adresbereik*> | Het IP-adresbereik moet worden gebruikt voor het maken van resources in uw omgeving. Dit bereik moet gebruiken de [notatie (Classless Inter-Domain Routing)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing), bijvoorbeeld 10.0.0.1/16, en vereist een klasse B-adresruimte. Het bereik mag niet bestaan in de adresruimte voor het virtuele netwerk is geselecteerd in de **gelijkwaardige VNET** eigenschap, noch binnen een andere persoonlijke IP-adressen wanneer de peernetwerk is verbonden, via peering of gateways. <p><p>**Belangrijke**: U *kan niet worden gewijzigd* dit adresbereik nadat u uw omgeving hebt gemaakt. |
   |||||
   
1. Wanneer u klaar bent, kiest u **Maken**. 

   Azure begint met het implementeren van uw omgeving, maar dit proces kan duren *maximaal twee uur* voordat u klaar bent. 
   Om te controleren of de implementatiestatus op de werkbalk van Azure, kies het meldingspictogram opent u het deelvenster meldingen.

   ![De implementatiestatus van de controleren](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Wanneer de implementatie is voltooid, ziet u Azure deze melding:

   ![Implementatie voltooid](./media/connect-virtual-network-vnet-isolated-environment/deployment-success.png)

1. Als u uw omgeving, kiest u **naar de resource gaan** als Azure niet automatisch naar uw omgeving gaat na de implementatie is voltooid.  

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-app---ise"></a>Logische app - ISE maken

Voor het maken van logische apps die de integratie van service-omgeving (ISE) gebruiken, gaat u als volgt de gebruikelijke stappen in [over het maken van een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md) , maar met deze verschillen en overwegingen: 

* Wanneer u uw logische app, maakt de **locatie** eigenschappenlijsten uw ISEs onder **integratieserviceomgevingen** samen met beschikbare regio's. Selecteer uw ISE, in plaats van een regio, bijvoorbeeld:

  ![Integratie van service-omgeving selecteren](./media/connect-virtual-network-vnet-isolated-environment/create-logic-app-with-integration-service-environment.png)

* U kunt de dezelfde dient te worden, zoals de HTTP-trigger of actie, die worden uitgevoerd in de ISE hetzelfde als de bovenliggende logische app gebruiken. Connectors met de **ISE** labelen ook uitvoeren in de ISE hetzelfde als de bovenliggende logische app. Connectors zonder de **ISE** label uitgevoerd in de globale Logic Apps-service.

  ![ISE-connectors selecteren](./media/connect-virtual-network-vnet-isolated-environment/select-ise-connectors.png)

* Als u uw ISE met een Azure-netwerk als een peer hebt ingesteld, kunnen de logische apps in uw ISE rechtstreeks toegang tot resources in dit virtuele netwerk. Voor on-premises systemen in een virtueel netwerk dat gekoppeld aan een ISE, logische apps rechtstreeks toegang tot deze systemen met behulp van deze items: 

  * ISE-connector voor dat systeem, bijvoorbeeld SQL Server

  * HTTP-actie 

  * Aangepaste connector

  Voor on-premises systemen die zich niet in een virtueel netwerk of geen ISE connectors, kunt u nog steeds verbinding maken nadat u [instellen en gebruiken van de on-premises gegevensgateway](../logic-apps/logic-apps-gateway-install.md).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-account---ise"></a>Integratieaccount - ISE maken

Voor het gebruik van een integratieaccount met logic apps in een integratieserviceomgeving (ISE), dat integratieaccount moet gebruiken de *dezelfde omgeving* als de logic apps. Logische apps in een ISE kunnen verwijzen naar alleen integratieaccounts in dezelfde ISE. 

Voor het maken van een integratieaccount dat gebruikmaakt van een ISE, gaat u als volgt de gebruikelijke stappen in [over het maken van integratieaccounts](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) , behalve voor de **locatie** eigenschap, die nu een lijst met uw ISEs onder  **Integratieserviceomgevingen** samen met beschikbare regio's. Selecteer uw ISE, in plaats van een regio, bijvoorbeeld:

![Integratie van service-omgeving selecteren](./media/connect-virtual-network-vnet-isolated-environment/create-integration-account-with-integration-service-environment.png)

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">Azure Logic Apps forum</a> (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de <a href="http://aka.ms/logicapps-wish" target="_blank">website voor feedback van Logic Apps-gebruikers</a>.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)
* Meer informatie over [integratie van virtuele netwerken voor Azure-services](../virtual-network/virtual-network-for-azure-services.md)
