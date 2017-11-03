---
title: Toegang tot on-premises gegevensbronnen voor Azure Logic Apps | Microsoft Docs
description: De lokale data gateway instellen, zodat u toegang hebt tot gegevensbronnen on-premises vanuit logic apps
keywords: toegang tot gegevens, op de lokale, gegevensoverdracht, versleuteling en gegevensbronnen
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 6cb4449d-e6b8-4c35-9862-15110ae73e6a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/14/2017
ms.author: LADocs; millopis; estfan
ms.openlocfilehash: f385d832deed2eaf8ea21eb75d62944cbbf3d13d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="connect-to-data-sources-on-premises-from-logic-apps-with-on-premises-data-gateway"></a>Verbinding maken met gegevensbronnen on-premises vanuit logic apps met lokale gegevensgateway

Instellen voor toegang tot gegevensbronnen on-premises van uw logische apps moet een lokale gegevensgateway die logische apps met ondersteunde connectors kunnen gebruiken. De gateway fungeert als een brug waarmee snelle gegevensoverdracht en -versleuteling tussen gegevensbronnen on-premises en uw logische apps. De gateway stuurt gegevens van lokale bronnen op gecodeerde kanalen via de Azure Service Bus. Al het verkeer afkomstig is als beveiligde uitgaand verkeer van de gateway-agent. Meer informatie over [de werking van de gegevensgateway](logic-apps-gateway-install.md#gateway-cloud-service). 

De gateway ondersteunt verbindingen met deze gegevensbronnen on-premises:

*   BizTalk Server 2016
*   DB2  
*   Bestandssysteem
*   Informix
*   MQ
*   MySQL
*   Oracle Database
*   PostgreSQL
*   SAP-toepassingsserver 
*   SAP-berichtenserver
*   SharePoint
*   SQL Server
*   Teradata

Deze stappen laten zien hoe de gegevens op de lokale gateway instellen om te werken met uw logische apps. Zie voor meer informatie over ondersteunde connectors [Connectors voor Azure Logic Apps](../connectors/apis-list.md). 

Zie voor informatie over het gebruik van de gateway met andere services, deze artikelen:

*   [Microsoft Power BI lokale gegevensgateway](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [Azure Analysis Services op locatie gegevensgateway](../analysis-services/analysis-services-gateway.md)
*   [Microsoft-Flow lokale gegevensgateway](https://flow.microsoft.com/documentation/gateway-manage/)
*   [Microsoft PowerApps lokale gegevensgateway](https://powerapps.microsoft.com/tutorials/gateway-management/)

## <a name="requirements"></a>Vereisten

* U moet al hebben [data gateway geïnstalleerd op een lokale computer](logic-apps-gateway-install.md).

* Wanneer u zich bij de Azure-portal aanmelden, hebt u dezelfde werk of schoolaccount dat is gebruikt om te gebruiken [installeren van de lokale data gateway](logic-apps-gateway-install.md#requirements). Uw account aanmelden moet ook beschikken over een Azure-abonnement moet worden gebruikt wanneer u een gateway-resource in de Azure-portal voor uw gateway-installatie maken.

* De gateway-installatie kan niet al door een Azure-gateway-resource wordt geclaimd. U kunt de gateway-installatie slechts aan één Azure-gateway resource koppelen. Claim gebeurt wanneer u de gateway-resource maken, zodat de installatie niet beschikbaar voor andere bronnen is.

* De lokale data gateway wordt uitgevoerd als een Windows-service en is ingesteld om te gebruiken `NT SERVICE\PBIEgwService` aanmeldingsreferenties voor het Windows-service. Maken en onderhouden van de gateway-resource in de Azure-portal, de [Windows-serviceaccount](../logic-apps/logic-apps-gateway-install.md) moet er ten minste **Inzender** machtigingen. 

  > [!NOTE]
  > Het Windows-service-account verschilt van het account dat wordt gebruikt voor het verbinden met on-premises gegevens bronnen, en van de Azure werk- of schoolaccount gebruikt voor aanmelding bij cloud-services.

## <a name="set-up-the-data-gateway-connection"></a>De data gateway-verbinding instellen

### <a name="1-install-the-on-premises-data-gateway"></a>1. De on-premises gegevensgateway installeren

Als u nog niet gedaan hebt, volgt u de [stappen voor het installeren van de lokale data gateway](logic-apps-gateway-install.md). Voordat u met de andere stappen doorgaat, zorg er dan voor dat u de data gateway geïnstalleerd op een lokale computer.

<a name="create-gateway-resource"></a>

### <a name="2-create-an-azure-resource-for-the-on-premises-data-gateway"></a>2. Een Azure-resource voor de lokale data gateway maken

Nadat u de gateway op een lokale computer installeert, maakt u uw data gateway als een resource in Azure. Uw gateway-resource koppelt in deze stap ook aan uw Azure-abonnement.

1. Meld u aan bij [Azure Portal](https://portal.azure.com "Azure Portal"). Zorg ervoor dat voor het gebruik van hetzelfde Azure werk of school e-mailadres gebruikt voor het installeren van de gateway.

2. Kies in het Azure hoofdmenu **nieuw** > **Enterprise Integration** > **On-premises gegevensgateway** als volgt te werk:

   !['On-premises data gateway' vinden](./media/logic-apps-gateway-connection/find-on-premises-data-gateway.png)

3. Op de **verbinding-gateway maken** pagina, vindt u deze informatie om de bron van uw data gateway te maken:

    * **Naam**: Voer een naam voor uw gateway-resource. 

    * **Abonnement**: Selecteer de Azure-abonnement wilt koppelen aan uw gateway-resource. 
    Dit abonnement moet hetzelfde abonnement als uw logische app.
   
      Het standaardabonnement is gebaseerd op het Azure-account dat u gebruikt voor aanmelden.

    * **Resourcegroep**: een resourcegroep maken of een bestaande resourcegroep selecteren voor het implementeren van uw gateway-resource. 
    Resourcegroepen kunnen u gerelateerde Azure activa beheren als een verzameling.

    * **Locatie**: Azure beperkt deze locatie bij dezelfde regio die is geselecteerd voor de gateway-cloudservice tijdens [gateway-installatie](logic-apps-gateway-install.md). 

      > [!NOTE]
      > Zorg ervoor dat de locatie van de resource gateway overeenkomt met de gateway cloud service-locatie. De gateway-installatie mogelijk anders niet weergegeven in de lijst Geïnstalleerde gateways voor selectie in de volgende stap.
      > 
      > U kunt verschillende regio's gebruiken voor uw gateway-resource en voor uw logische app.

    * **De naam van de installatie**: als uw gateway-installatie niet is geselecteerd, selecteert u de gateway die u eerder hebt geïnstalleerd. 

    Als u wilt de bron van de gateway toevoegen aan uw Azure-dashboard, kies **vastmaken aan dashboard**. 
    Als u bent klaar, kiest u **maken**.

    Bijvoorbeeld:

    ![Geef details op uw on-premises gegevensgateway maken](./media/logic-apps-gateway-connection/createblade.png)

    Als u wilt vinden of weergeven van uw data gateway op elk gewenst moment, vanuit het Azure hoofdmenu, gaat u naar **meer Services** > **Enterprise Integration** > **gegevensgateways On-premises** .

    ![Ga naar 'Meer services', 'Enterprise Integration', 'On-premises gegevensgateways'](./media/logic-apps-gateway-connection/find-on-premises-data-gateway-enterprise-integration.png)

<a name="connect-logic-app-gateway"></a>

### <a name="3-connect-your-logic-app-to-the-on-premises-data-gateway"></a>3. Uw logische app verbinden met de lokale data gateway

Nu dat u hebt uw data gateway resource gemaakt en die uw Azure-abonnement is gekoppeld aan deze resource, maak een verbinding tussen uw logische app en de data gateway.

> [!NOTE]
> De locatie van uw gateway verbinding moet aanwezig zijn in dezelfde regio bevinden als uw logische app, maar u kunt een data gateway die voorkomt in een andere regio.

1. In de Azure portal maken of openen van uw logische app in Logic App-ontwerper.

2. Toevoegen van een connector die ondersteuning biedt voor lokale verbindingen, zoals SQL Server.

3. Na de aangegeven volgorde en selecteer **verbinden via lokale gegevensgateway**, Geef een unieke verbindingsnaam en de vereiste gegevens in en selecteert u de gateway-resource voor gegevens die u wilt gebruiken. Als u bent klaar, kiest u **maken**.

   > [!TIP]
   > Een unieke verbindingsnaam kunt u gemakkelijk herkennen die verbinding later, vooral wanneer u meerdere verbindingen maken. Indien van toepassing, moet u ook de gekwalificeerde domeinnaam voor uw gebruikersnaam bevatten. 

   ![Verbinding maken tussen logic app en data gateway](./media/logic-apps-gateway-connection/blankconnection.png)

Gefeliciteerd, uw gatewayverbinding is nu gereed voor uw logische app te gebruiken.

## <a name="edit-your-gateway-connection-settings"></a>De instellingen voor de gateway-verbinding bewerken

Nadat u een gatewayverbinding voor uw logische app maakt, kunt u later de instellingen voor die specifieke verbinding bijwerken.

1. De gatewayverbinding vinden:

   * Klik in het menu logic app onder **ontwikkelingsprogramma's**, selecteer **API verbindingen**. 
   
     De **API verbindingen** deelvenster ziet u alle API-verbindingen die zijn gekoppeld aan uw logische app, inclusief gatewayverbindingen.

     ![Ga naar uw logische app, selecteert u 'API verbindingen'](./media/logic-apps-gateway-connection/logic-app-find-api-connections.png)

   * Of Ga naar in het Azure hoofdmenu **meer Services** > **Web en mobiel** > **API verbindingen** voor alle API-verbindingen, met inbegrip van gateway verbindingen die gekoppeld aan uw Azure-abonnement zijn. 

   * Of op het Azure hoofdmenu, gaat u naar **alle resources** voor alle API-verbindingen, inclusief gatewayverbindingen die gekoppeld aan uw Azure-abonnement zijn.

2. Selecteer de gatewayverbinding die u wilt weergeven of bewerken en kies **API bewerken verbinding**.

   > [!TIP]
   > Als u de updates worden niet doorgevoerd, probeert u [stoppen en opnieuw starten van de gateway Windows-service](./logic-apps-gateway-install.md#restart-gateway).

<a name="change-delete-gateway-resource"></a>
## <a name="switch-or-delete-your-on-premises-data-gateway-resource"></a>Schakelt u of uw lokale gegevens gateway bron verwijderen

Als u wilt maken van een andere gateway-resource, uw gateway koppelen aan een andere resource of verwijder de gateway-resource, kunt u de bron van de gateway verwijderen zonder gevolgen voor de installatie van de gateway. 

1. Ga naar in het Azure hoofdmenu **alle resources**. 
2. Zoek en selecteer uw data gateway-resource.
3. Kies **On-premises Data Gateway**, en kies op de werkbalk resource **verwijderen**.

<a name="faq"></a>
## <a name="frequently-asked-questions"></a>Veelgestelde vragen

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

## <a name="next-steps"></a>Volgende stappen

* [Uw logische apps beveiligen](./logic-apps-securing-a-logic-app.md)
* [Algemene voorbeelden en scenario's voor logic apps](./logic-apps-examples-and-scenarios.md)
