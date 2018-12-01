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
ms.date: 11/29/2018
ms.openlocfilehash: 798b50887bcfdf5b4298c37beb1b9eea8f9abdda
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52682194"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-through-an-integration-service-environment-ise"></a>Verbinding maken met virtuele Azure-netwerken van Azure Logic Apps via een integratie van service-omgeving (ISE)

> [!NOTE]
> Deze mogelijkheid is in *privépreview*. Om te vragen tot, [maken van uw aanvraag voor deelname aan hier](https://aka.ms/iseprivatepreview).

Voor scenario's waar uw logic apps en de integratieaccounts toegang hebben tot moeten een [virtueel Azure-netwerk](../virtual-network/virtual-networks-overview.md), maak een [ *integratieserviceomgeving* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). Een ISE is een privé- en geïsoleerde omgeving die gebruikmaakt van opslagruimte en andere resources bewaard scheiden van de openbare of *globale* Logic Apps-service. Dankzij deze scheiding vermindert ook eventuele gevolgen die andere Azure-tenants op de prestaties van uw apps hebben kunnen. Is voor uw ISE *geïnjecteerd* in met uw Azure-netwerk, die vervolgens implementeert u de Logic Apps-service in uw virtuele netwerk. Wanneer u een logische app of integratie-account maakt, selecteert u deze ISE als hun locatie. Uw logische app of integratie-account kan vervolgens rechtstreeks toegang tot resources, zoals virtuele machines (VM's), servers, systemen en services in uw virtuele netwerk. 

![Integratie van service-omgeving selecteren](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

In dit artikel laat zien hoe deze taken uit te voeren:

* Instellen van machtigingen in uw Azure-netwerk, zodat de persoonlijke Logic Apps-exemplaar toegang heeft tot uw virtuele netwerk.

* Maak de integratie van service-omgeving (ISE). 

* Maak een logische app die kan worden uitgevoerd in de ISE. 

* Maak een integratieaccount voor uw logische apps in uw ISE.

Zie voor meer informatie over de integratie van service-omgevingen, [toegang tot resources van Azure Virtual Network van Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* Een [virtueel Azure-netwerk](../virtual-network/virtual-networks-overview.md). Als u een virtueel netwerk hebt, krijgt u informatie over het [maken van een Azure virtual network](../virtual-network/quick-create-portal.md). 

* Uw logische apps direct toegang geven tot uw Azure-netwerk, [instellen van machtigingen voor toegangsbeheer op basis van rollen (RBAC)](#vnet-access) , zodat de Logic Apps-service beschikt over de machtigingen voor toegang tot uw virtuele netwerk. 

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="vnet-access"></a>

## <a name="set-virtual-network-permissions"></a>Virtueel netwerkmachtigingen instellen

Wanneer u een integratie van service-omgeving (ISE) maakt, u een Azure-netwerk selecteren in waar u *invoeren* uw omgeving. Echter, voordat u een virtueel netwerk voor het injecteren van uw omgeving selecteren kunt, moet u instellen machtigingen voor toegangsbeheer op basis van rollen (RBAC) in uw virtuele netwerk. Als u machtigingen instelt, wijst u deze specifieke rollen toe aan de service Azure Logic Apps:

1. In de [Azure-portal](https://portal.azure.com), zoek en selecteer het virtuele netwerk. 

1. Selecteer in het menu van het virtuele netwerk **toegangsbeheer (IAM)**. 

1. Onder **Access Control (IAM)**, kiest u **toevoegen**. 

   ![Functies toevoegen](./media/connect-virtual-network-vnet-isolated-environment/set-up-role-based-access-control-vnet.png)

1. Op de **roltoewijzing toevoegen** deelvenster, instellen van elke rol voor de service Azure Logic Apps, zoals is beschreven in de tabel in deze stap. Zorg ervoor dat u kiest **opslaan** nadat u klaar bent met elke rol.

   ![Roltoewijzing toevoegen](./media/connect-virtual-network-vnet-isolated-environment/add-contributor-roles.png)

   | Rol | Toegang toewijzen aan | Selecteer | 
   |------|------------------|--------|
   | **Inzender voor netwerken** | **Azure AD-gebruiker, groep of toepassing** | Voer **Azure Logic Apps**. Nadat de ledenlijst wordt weergegeven, selecteert u dezelfde waarde. <p>**Tip**: als u deze service niet vinden, voert u de app-ID van de Logic Apps-service: `7cd684f4-8a78-49b0-91ec-6a35d38739ba` | 
   | **Inzender voor klassieke** | **Azure AD-gebruiker, groep of toepassing** | Voer **Azure Logic Apps**. Nadat de ledenlijst wordt weergegeven, selecteert u dezelfde waarde. <p>**Tip**: als u deze service niet vinden, voert u de app-ID van de Logic Apps-service: `7cd684f4-8a78-49b0-91ec-6a35d38739ba` | 
   |||| 

Zie voor meer informatie, [machtigingen voor toegang tot het virtuele netwerk](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

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

   | Eigenschap | Vereist | Waarde | Beschrijving |
   |----------|----------|-------|-------------|
   | **Abonnement** | Ja | <*Azure-subscription-name*> | Het Azure-abonnement moet worden gebruikt voor uw omgeving | 
   | **Resourcegroep** | Ja | <*Azure-resource-group-name*> | De Azure-resourcegroep waar u om uw omgeving te maken |
   | **Naam van integratieserviceomgeving** | Ja | <*naam van omgeving*> | De naam te geven van uw omgeving | 
   | **Locatie** | Ja | <*Azure-datacenter-regio*> | De Azure-datacenter-regio waar u om uw omgeving te implementeren | 
   | **Capaciteit** | Ja | 0, 1, 2, 3 | Het aantal verwerkingseenheden moet worden gebruikt voor deze resource ISE | 
   | **Virtueel netwerk** | Ja | <*Azure--naam-virtueel netwerk*> | De Azure-netwerk waarin u invoeren van uw omgeving wilt, zodat logic apps in die omgeving krijgen uw virtuele netwerk tot toegang. Als u een netwerk hebt, kunt u een maken hier. <p>**Belangrijke**: U kunt *alleen* deze injectie uitvoeren bij het maken van uw ISE. Echter, voordat u deze relatie maken kunt, zorg ervoor dat u al [instellen van op rollen gebaseerd toegangsbeheer in uw virtuele netwerk voor Azure Logic Apps](#vnet-access). | 
   | **Subnets** | Ja | <*IP-adresbereik*> | Een ISE vereist is vier *leeg* subnetten, dit hoeft geen delegering voor elke service en worden gebruikt voor het maken van resources in uw omgeving. Elk subnet moet voldoen aan deze criteria voldoen: <p>-Hiermee wordt de [notatie (Classless Inter-Domain Routing)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing). <br>-Een klasse B-adresruimte vereist. <br>-Heeft een naam die niet met een getal of een afbreekstreepje begint. <br>-Bevat een `/27`, bijvoorbeeld elk subnet hier Hiermee geeft u een 32-bits-adresbereik: `10.0.0.0/27`, `10.0.0.32/27`, `10.0.0.64/27`, en `10.0.0.96/27`. <br>-Mag niet bestaan in hetzelfde adresbereik voor het geselecteerde virtuele netwerk en eventuele andere privé IP-adressen waarin het virtuele netwerk is verbonden. <br>-Moet niet leeg zijn. <p><p>**Belangrijke**: U *kan niet worden gewijzigd* deze IP-bereiken nadat u uw omgeving hebt gemaakt. |
   |||||

1. Nadat Azure uw ISE-gegevens met succes worden gevalideerd, kiest u **maken**, bijvoorbeeld:

   ![Nadat de validatie is geslaagd, kiest u 'Maken'](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure begint met het implementeren van uw omgeving, maar dit proces kan duren *maximaal twee uur* voordat u klaar bent. 
   Om te controleren of de implementatiestatus op de werkbalk van Azure, kies het meldingspictogram opent u het deelvenster meldingen.

   ![De implementatiestatus van de controleren](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Wanneer de implementatie is voltooid, ziet u Azure deze melding:

   ![Implementatie is voltooid](./media/connect-virtual-network-vnet-isolated-environment/deployment-success.png)

1. Als u uw omgeving, kiest u **naar de resource gaan** als Azure niet automatisch naar uw omgeving gaat na de implementatie is voltooid.  

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-app---ise"></a>Logische app - ISE maken

Voor het maken van logische apps die de integratie van service-omgeving (ISE) gebruiken, volgt u de stappen in [over het maken van een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md) maar met deze verschillen: 

* Wanneer u uw logische app maakt, selecteer uw ISE, in plaats van een Azure-regio in de **locatie** lijst uit de **integratieserviceomgevingen** sectie, bijvoorbeeld:

  ![Integratie van service-omgeving selecteren](./media/connect-virtual-network-vnet-isolated-environment/create-logic-app-with-integration-service-environment.png)

* U kunt de dezelfde ingebouwde triggers en acties, zoals HTTP, die worden uitgevoerd in de dezelfde ISE als uw logische app gebruiken. Connectors met de **ISE** labelen ook uitvoeren in de dezelfde ISE als uw logische app. Connectors zonder de **ISE** label uitgevoerd in de globale Logic Apps-service.

  ![ISE-connectors selecteren](./media/connect-virtual-network-vnet-isolated-environment/select-ise-connectors.png)

* Nadat u uw ISE in een Azure-netwerk invoeren, kunnen de logische apps in uw ISE rechtstreeks toegang tot resources in dit virtuele netwerk. Voor on-premises systemen in een virtueel netwerk dat gekoppeld aan een ISE, logische apps rechtstreeks toegang tot deze systemen met behulp van deze items: 

  * ISE-connector voor dat systeem, bijvoorbeeld SQL Server
  * HTTP-actie 
  * Aangepaste connector

  Voor on-premises systemen die zich niet in een virtueel netwerk of geen ISE-connectors, eerst [instellen en gebruiken van de on-premises gegevensgateway](../logic-apps/logic-apps-gateway-install.md).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-account---ise"></a>Integratieaccount - ISE maken

Voor het gebruik van een integratieaccount met logic apps in een integratieserviceomgeving (ISE), dat integratieaccount moet gebruiken de *dezelfde omgeving* als de logic apps. Logische apps in een ISE kunnen verwijzen naar alleen integratieaccounts in dezelfde ISE. 

Voor het maken van een integratieaccount dat gebruikmaakt van een ISE, gaat u als volgt de gebruikelijke stappen in [over het maken van integratieaccounts](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) , behalve voor de **locatie** eigenschap, die nu een lijst met uw ISEs onder  **Integratieserviceomgevingen** samen met beschikbare regio's. Selecteer uw ISE, in plaats van een regio, bijvoorbeeld:

![Integratie van service-omgeving selecteren](./media/connect-virtual-network-vnet-isolated-environment/create-integration-account-with-integration-service-environment.png)

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">Azure Logic Apps forum</a> (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de <a href="https://aka.ms/logicapps-wish" target="_blank">website voor feedback van Logic Apps-gebruikers</a>.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)
* Meer informatie over [integratie van virtuele netwerken voor Azure-services](../virtual-network/virtual-network-for-azure-services.md)
