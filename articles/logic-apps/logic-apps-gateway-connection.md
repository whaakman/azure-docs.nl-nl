---
title: Toegang tot gegevensbronnen on-premises van Azure Logic Apps | Microsoft Docs
description: Verbinding maken met on-premises gegevens gegevensbronnen vanuit logische apps met het maken van een on-premises gegevensgateway
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: arthii, LADocs
ms.topic: article
ms.date: 10/01/2018
ms.openlocfilehash: 2b9e1c153c3fa9b17145eb6c3c8f3ed02e3bf40f
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/07/2019
ms.locfileid: "54064063"
---
# <a name="connect-to-on-premises-data-sources-from-azure-logic-apps"></a>Verbinding maken met on-premises gegevensbronnen vanuit Azure Logic Apps

Voor toegang tot gegevensbronnen on-premises van uw logische apps, maakt u een on-premises data gateway-resource in Azure portal. Uw logische apps kunnen vervolgens worden gebruikt de [on-premises connectors](../logic-apps/logic-apps-gateway-install.md#supported-connections). Dit artikel wordt beschreven hoe u uw Azure-gateway-resource maakt *nadat* u [downloaden en installeren van de gateway op uw lokale computer](../logic-apps/logic-apps-gateway-install.md). 

> [!TIP]
> Voor verbinding met virtuele Azure-netwerken, houd rekening met het maken van een [ *integratieserviceomgeving* ](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) in plaats daarvan. 

Zie de volgende artikelen voor meer informatie over het gebruik van de gateway met andere services:

* [Microsoft Power BI on-premises gegevensgateway](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
* [Microsoft Flow on-premises gegevensgateway](https://flow.microsoft.com/documentation/gateway-manage/)
* [Microsoft PowerApps on-premises gegevensgateway](https://powerapps.microsoft.com/tutorials/gateway-management/)
* [Azure Analysis Services on-premises gegevensgateway](../analysis-services/analysis-services-gateway.md)

## <a name="prerequisites"></a>Vereisten

* U hebt al [gedownload en de data gateway geïnstalleerd op een lokale computer](../logic-apps/logic-apps-gateway-install.md).

* De gatewayinstallatie van uw nog niet is gekoppeld aan een gateway-resource in Azure. U kunt de gatewayinstallatie koppelen slechts aan één gateway-resource, die wordt uitgevoerd wanneer u de gateway-resource maakt en selecteer de gatewayinstallatie. Deze koppeling wordt de installatie van de gateway niet beschikbaar voor andere bronnen.

* Wanneer u zich aanmeldt bij de Azure portal en de gateway-resource maakt, zorg ervoor dat u de dezelfde aanmeldingsaccount die eerder gebruikt voor het was [installeren van de on-premises gegevensgateway](../logic-apps/logic-apps-gateway-install.md#requirements) samen met dezelfde [Azure-abonnement ](https://docs.microsoft.com/azure/architecture/cloud-adoption-guide/adoption-intro/subscription-explainer) die is gebruikt voor het installeren van de gateway. Als u een Azure-abonnement nog geen <a href="https://azure.microsoft.com/free/" target="_blank">zich aanmelden voor een gratis Azure-account</a>.

* Maken en onderhouden van de gateway-resource in Azure portal, uw [Windows-serviceaccount](../logic-apps/logic-apps-gateway-install.md#windows-service-account) heeft ten minste **Inzender** machtigingen. De on-premises gegevensgateway wordt uitgevoerd als een Windows-service en is ingesteld voor het gebruik `NT SERVICE\PBIEgwService` voor de Windows-service aanmeldingsreferenties. 

  > [!NOTE]
  > De Windows-serviceaccount wijkt af van het account voor verbinding maken met on-premises gegevens gebruikt voor gegevensbronnen en werken met de Azure- of schoolaccount gebruikt om aan te melden bij cloudservices.

## <a name="download-and-install-gateway"></a>Gateway downloaden en installeren

Voordat u kunt doorgaan met de stappen in dit artikel, zorg er dan voor dat uw gateway is al geïnstalleerd op een lokale computer.
Als u niet hebt gedaan, volgt u de stappen voor het [downloaden en installeren van de on-premises gegevensgateway](../logic-apps/logic-apps-gateway-install.md). 

<a name="create-gateway-resource"></a>

## <a name="create-azure-resource-for-gateway"></a>Azure-resource voor gateway maken

Nadat u de gateway op een lokale computer installeert, kunt u vervolgens een Azure-resource maken voor uw gateway. Uw gateway-resource koppelt in deze stap ook aan uw Azure-abonnement.

1. Meld u aan bij <a href="https://portal.azure.com" target="_blank">Azure Portal</a>. Zorg ervoor dat u gebruikt hetzelfde Azure werk of school-e-mailadres gebruikt voor het installeren van de gateway.

2. Selecteer in het hoofdmenu van Azure **een resource maken** > 
**integratie** > **On-premises gegevensgateway**.

   ![Zoek 'On-premises gegevensgateway'](./media/logic-apps-gateway-connection/find-on-premises-data-gateway.png)

3. Op de **verbindingsgateway maken** pagina, typt u deze informatie voor uw gateway:

   | Eigenschap | Beschrijving | 
   |----------|-------------|
   | **Naam** | De naam voor uw gateway-resource | 
   | **Abonnement** | De naam van uw Azure-abonnement, moet hetzelfde abonnement als uw logische app. Het standaardabonnement is gebaseerd op het Azure-account waarmee u zich. | 
   | **Resourcegroep** | De naam voor de [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) voor het ordenen van verwante bronnen | 
   | **Locatie** | Azure beperkt deze locatie bij dezelfde regio die is geselecteerd voor de gateway-cloudservice tijdens [gatewayinstallatie](../logic-apps/logic-apps-gateway-install.md). <p>**Opmerking**: Zorg ervoor dat deze gateway Resourcelocatie komt overeen met de servicelocatie van de gateway-cloud. De gatewayinstallatie mogelijk anders niet weergegeven in de lijst met geïnstalleerde gateways voor u om te selecteren in de volgende stap. U kunt verschillende regio's gebruiken voor uw gateway en voor uw logische app. | 
   | **De Installatienaam van de** | Als de gatewayinstallatie is niet nog is geselecteerd, selecteert u de gateway die u eerder hebt geïnstalleerd. | 
   | | | 

   Hier volgt een voorbeeld:

   ![Geef details voor het maken van uw on-premises gegevensgateway](./media/logic-apps-gateway-connection/createblade.png)

4. Selecteer de gateway-resource toevoegen aan uw Azure-dashboard, **vastmaken aan dashboard**. Wanneer u klaar bent, kiest u **Maken**.

   Als u wilt vinden of weergeven van uw gateway op elk gewenst moment, vanuit het hoofdmenu van Azure, selecteer **alle services**. 
   Voer in het zoekvak 'on-premises gegevensgateways' en selecteer vervolgens **On-premises gegevensgateways**.

   ![Zoek 'On-premises gegevensgateways"](./media/logic-apps-gateway-connection/find-on-premises-data-gateway-enterprise-integration.png)

<a name="connect-logic-app-gateway"></a>

## <a name="connect-to-on-premises-data"></a>Verbinding maken met on-premises gegevens

Nadat u uw gateway-resource maakt en uw Azure-abonnement aan deze bron koppelen, kunt u nu een verbinding maken tussen uw logische app en uw on-premises gegevensbron met behulp van de gateway.

1. In de Azure-portal maken of uw logische app in Logic App Designer openen.

2. Toevoegen van een connector die ondersteuning biedt voor on-premises verbindingen, bijvoorbeeld **SQL Server**.

3. Stel nu de verbinding:

   1. Selecteer **verbinding maken via een on-premises gegevensgateway**. 

   2. Voor **Gateways**, selecteert u de gateway-resource die u eerder hebt gemaakt. 

      Hoewel de locatie van uw gateway verbinding moet aanwezig zijn in dezelfde regio als uw logische app, kunt u een gateway selecteren in een andere regio.

   3. Geef een unieke verbindingsnaam en de vereiste gegevens. 

      Naam van de unieke verbinding kunt u eenvoudig identificeren die verbinding later, met name wanneer u meerdere verbindingen maken. Indien van toepassing, moet u ook de gekwalificeerde domein voor uw gebruikersnaam bevatten.
   
      Hier volgt een voorbeeld:

      ![Verbinding maken tussen logische Apps en gegevens-gateway](./media/logic-apps-gateway-connection/blankconnection.png)

   4. Wanneer u klaar bent, kiest u **Maken**. 

De gatewayverbinding is nu gereed voor uw logische app te gebruiken.

## <a name="edit-connection"></a>Verbinding bewerken

Nadat u een gateway-verbinding voor uw logische app gemaakt, wilt u mogelijk de instellingen voor die specifieke verbinding later bijwerken.

1. De gatewayverbinding vinden:

   * Alle API-verbindingen voor uw logische app, onder in het menu van uw logische app vinden **ontwikkeltools**, selecteer **API-verbindingen**. 
   
     ![Ga naar uw logische app, selecteert u "API-verbindingen"](./media/logic-apps-gateway-connection/logic-app-find-api-connections.png)

   * Zoek alle API-verbindingen die zijn gekoppeld aan uw Azure-abonnement: 

     * Ga in het hoofdmenu van Azure naar **alle services** > **Web** > **API-verbindingen**. 
     * Of, in het hoofdmenu van Azure, gaat u naar **alle resources**.

2. Selecteer de gatewayverbinding die u wilt en kies vervolgens **bewerken API-verbinding**.

   > [!TIP]
   > Als de updates worden niet doorgevoerd, probeert u [stoppen en opnieuw starten van de Windows-gatewayservice](./logic-apps-gateway-install.md#restart-gateway).

<a name="change-delete-gateway-resource"></a>

## <a name="delete-gateway-resource"></a>Gateway-resource verwijderen

Als u wilt maken van een andere gateway-resource, uw gateway koppelen aan een andere resource of verwijder de gateway-resource, kunt u de gateway-resource verwijderen zonder gevolgen voor de gatewayinstallatie. 

1. Ga in het hoofdmenu van Azure naar **alle resources**. 

2. Zoek en selecteer de gateway-resource.

3. Als u nog niet is geselecteerd in het menu van de resource gateway selecteert **On-premises Data Gateway**. 

4. Kies op de werkbalk resource **verwijderen**.

<a name="faq"></a>

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* [Uw logische apps beveiligen](./logic-apps-securing-a-logic-app.md)
* [Algemene voorbeelden en scenario's voor logische apps](./logic-apps-examples-and-scenarios.md)
