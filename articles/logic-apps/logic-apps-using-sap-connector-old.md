---
title: Verbinding maken met SAP-systemen - Azure Logic Apps | Microsoft Docs
description: Over het openen en SAP-resources beheren met het automatiseren van werkstromen met Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 05/31/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, divswa, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 77d1e11c1400f9a3d6bb6bda8e935cd4d24a195e
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50230893"
---
# <a name="connect-to-sap-systems-from-azure-logic-apps"></a>Verbinding maken met SAP-systemen in Azure Logic Apps

> [!NOTE]
> Deze connector SAP worden binnenkort afgeschaft. We hebben uitgebracht nieuwe en geavanceerde SAP-connector en aanbevolen om te kiezen of verplaatsen naar de [nieuwe SAP-connector](./logic-apps-using-sap-connector.md).
>  

Dit artikel wordt beschreven hoe u kunt toegang tot de resources van uw SAP uit in een logische app met behulp van de SAP-toepassingsserver en SAP-berichtenserver connectors. Op die manier kunt u taken, processen en werkstromen die uw SAP-gegevens en resources beheren met het maken van logische apps automatiseren.

Dit voorbeeld wordt een logische app die u kunt activeren met een HTTP-aanvraag. De logische app verzendt een tussenliggende Document (IDoc) naar een SAP-server en retourneert een antwoord naar de aanvrager dat de logische app genoemd.
De huidige SAP-connectors acties, maar geen triggers hebben, zodat in dit voorbeeld wordt de [HTTP-aanvraag als trigger](../connectors/connectors-native-reqres.md) als de eerste stap in de logic app-werkstroom. Voor SAP-connector-specifieke technische informatie, Zie de referentie-artikelen: 

* <a href="https://docs.microsoft.com/connectors/sapapplicationserver/" target="blank">SAP toepassingsserver-connector</a>
* <a href="https://docs.microsoft.com/connectors/sapmessageserver/" target="blank">SAP berichtenserver-connector</a>

Als u een Azure-abonnement nog geen <a href="https://azure.microsoft.com/free/" target="_blank">zich aanmelden voor een gratis Azure-account</a>.

## <a name="prerequisites"></a>Vereisten

Als u wilt volgen, samen met dit artikel, moet u deze items:

* De logische app van waar u toegang tot uw SAP-systeem en een trigger die de werkstroom van uw logische app wordt gestart. De SAP-connectors bieden op dit moment alleen acties. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps](../logic-apps/logic-apps-overview.md) en [Snelstartgids: uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Uw <a href="https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server" target="_blank">SAP-toepassingsserver</a> of <a href="https://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm" target="_blank">SAP-berichtenserver</a>

* Download en installeer de meest recente [on-premises gegevensgateway](https://www.microsoft.com/download/details.aspx?id=53127) op een on-premises computer. Zorg ervoor dat u uw gateway in Azure portal hebt ingesteld, voordat u doorgaat. De gateway kunt u veilig toegang tot gegevens en resources zijn on-premises. Zie voor meer informatie, [installeren on-premises gegevensgateway voor Azure Logic Apps](../logic-apps/logic-apps-gateway-install.md).

* Download en installeer de meest recente SAP-clientbibliotheek, dat zich momenteel <a href="https://softwaredownloads.sap.com/file/0020000000086282018" target="_blank">SAP-Connector (NCo) 3.0.20.0 voor Microsoft .NET Framework 4.0 en Windows 64-bits (x64)</a>, op dezelfde computer als de on-premises gegevensgateway. Deze versie installeren of hoger om volgende redenen:

  * Eerdere versies van de SAP-NCo kunnen in een impasse worden geraakt wanneer meer dan één IDoc-berichten worden verzonden op hetzelfde moment. 
  Dit probleem blokkeert alle latere berichten die worden verzonden naar de bestemming voor SAP, waardoor de berichten time-out.

  * De on-premises gegevensgateway wordt uitgevoerd alleen op 64-bits systemen. 
  Anders treedt er een fout 'Ongeldige installatiekopie' omdat de data gateway host-service biedt geen ondersteuning voor 32-bits-assembly's.

  * Zowel de data gateway host-service en de SAP-Adapter van Microsoft .NET Framework 4.5 gebruiken De SAP-NCo voor .NET Framework 4.0 werkt met processen die gebruikmaken van de .NET runtime 4.0-4.7.1. 
  De SAP-NCo voor .NET Framework 2.0 werkt met processen die gebruikmaken van de .NET runtime 2.0-3.5 en niet meer werkt met de meest recente on-premises data gateway.

* Inhoud van het bericht die u uw SAP-server, bijvoorbeeld een voorbeeldbestand IDoc kunt verzenden. Deze inhoud moet zich in de XML-indeling en vermeld de naamruimte voor de SAP-actie die u wilt gebruiken.

<a name="add-trigger"></a>

## <a name="add-http-request-trigger"></a>HTTP-aanvraag als trigger toevoegen

In Azure Logic Apps, elke logische app moet beginnen met een [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts), die wordt geactiveerd wanneer een bepaalde gebeurtenis wordt uitgevoerd of wanneer een bepaalde voorwaarde wordt voldaan. Telkens wanneer de trigger wordt geactiveerd, de Logic Apps-engine een exemplaar van de logische app maakt en werkstroom van uw app wordt gestart.

In dit voorbeeld u een logische app maken met een eindpunt in Azure zodat u kunt verzenden *HTTP POST-aanvragen* aan uw logische app. Wanneer uw logische app deze HTTP-aanvragen ontvangt, wordt de trigger wordt geactiveerd en wordt uitgevoerd de volgende stap in uw werkstroom.

1. Maak een lege logische app, die de ontwerper van logische App wordt geopend in de Azure-portal. 

2. Typ 'http-aanvraag' als filter in het zoekvak. Selecteer in de lijst met triggers deze trigger: **aanvraag: wanneer een HTTP-aanvraag wordt ontvangen**

   ![HTTP-aanvraag trigger toevoegen](./media/logic-apps-using-sap-connector-old/add-trigger.png)

3. Nu uw logische app opslaan, zodat u een eindpunt-URL voor uw logische app kunt genereren.
Kies **Opslaan** op de werkbalk van de ontwerper. 

   Het eindpunt van de URL wordt nu weergegeven in de trigger, bijvoorbeeld:

   ![URL genereren voor het eindpunt](./media/logic-apps-using-sap-connector-old/generate-http-endpoint-url.png)

<a name="add-action"></a>

## <a name="add-sap-action"></a>SAP-actie toevoegen

In Azure Logic Apps, een [actie](../logic-apps/logic-apps-overview.md#logic-app-concepts) is een stap in uw werkstroom die volgt op een trigger of een andere actie. Als u dit nog niet hebt toegevoegd een trigger aan uw logische app nog en voer dit voorbeeld wilt [toevoegen van de trigger die in deze sectie beschreven](#add-trigger).

1. Kies in de Logic App Designer onder de trigger **nieuwe stap** > **een actie toevoegen**.

   ![Een actie toevoegen](./media/logic-apps-using-sap-connector-old/add-action.png) 

2. Typ 'sap-server' als filter in het zoekvak. Selecteer de actie voor uw SAP-server uit de lijst met acties: 

   * **SAP-toepassingsserver - verzenden naar SAP**
   * **SAP-berichtenserver - verzenden naar SAP**

   In dit voorbeeld wordt met deze actie: **SAP-toepassingsserver - verzenden naar SAP**

   ![Selecteer "SAP-toepassingsserver" of "SAP-berichtenserver"](media/logic-apps-using-sap-connector-old/select-sap-action.png)

3. Als u wordt gevraagd of u voor de verbindingsgegevens, maakt u nu uw SAP-verbinding. Anders als de verbinding al bestaat, gaat u door met de volgende stap, zodat u uw SAP-actie kunt instellen. 

   **On-premises SAP-verbinding maken**

   1. Voor **Gateways**, selecteer **verbinding maken via een on-premises gegevensgateway** zodat de eigenschappen van de on-premises-verbinding worden weergegeven.

   2. De verbindingsgegevens voor uw SAP-server opgeven. 
   Voor de **gateway** eigenschap, selecteert u de data gateway die u hebt gemaakt in de Azure-portal voor uw gatewayinstallatie bijvoorbeeld:

      **SAP-toepassingsserver**

      ![Serververbinding voor SAP-toepassing maken](./media/logic-apps-using-sap-connector-old/create-SAP-app-server-connection.png)  

      **SAP-berichtenserver**

      ![SAP bericht serververbinding maken](media/logic-apps-using-sap-connector-old/create-SAP-message-server-connection.png) 

   2. Wanneer u klaar bent, kiest u **Maken**.

      Logic Apps wordt ingesteld en test de verbinding, ervoor te zorgen dat de verbinding naar behoren werkt.

4. Nu zoeken en selecteer een actie van uw SAP-server. 

   1. In de **SAP actie** Kies het pictogram van de map. 
   Uit de lijst met mappen Zoek en selecteer de actie die u wilt gebruiken. 

      In dit voorbeeld wordt de **IDOC** categorie voor de actie IDoc. 

      ![Zoek en selecteer de actie IDoc](./media/logic-apps-using-sap-connector-old/SAP-app-server-find-action.png)

      Als u niet de actie die u wilt vinden, kunt u een pad, bijvoorbeeld handmatig invoeren:

      ![Pad naar IDoc actie handmatig opgeven](./media/logic-apps-using-sap-connector-old/SAP-app-server-manually-enter-action.png)

      Zie voor meer informatie over de werking van IDoc [bericht schema's voor IDOC-bewerkingen](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations)

   2. Klik in de **invoerbericht** vak zodat de lijst met dynamische inhoud wordt weergegeven. 
   In deze lijst onder **wanneer een HTTP-aanvraag wordt ontvangen**, selecteer de **hoofdtekst** veld. 

      Deze stap bevat de hoofdtekst van uw HTTP-aanvraag als trigger en verstuurt die uitvoer naar uw SAP-server.

      ![Veld 'Hoofdtekst' selecteren](./media/logic-apps-using-sap-connector-old/SAP-app-server-action-select-body.png)

      Wanneer u klaar bent, ziet uw SAP-actie eruit zoals in dit voorbeeld:

      ![Complete SAP-actie](./media/logic-apps-using-sap-connector-old/SAP-app-server-complete-action.png)

6. Sla uw logische app op. Kies **Opslaan** op de werkbalk van de ontwerper.

<a name="add-response"></a>

## <a name="add-http-response-action"></a>HTTP-antwoord actie toevoegen

Nu een reactie toevoegen aan uw logische app-werkstroom en de uitvoer van de SAP-actie bevatten. Op die manier kunnen uw logische app retourneert de resultaten van uw SAP-server naar de oorspronkelijke aanvrager. 

1. Kies in de Logic App Designer onder de actie SAP **nieuwe stap** > **een actie toevoegen**.

2. Typ 'response' als filter in het zoekvak. Selecteer in de lijst met acties met deze actie: **aanvraag - reactie**

3. Klik in de **hoofdtekst** vak zodat de lijst met dynamische inhoud wordt weergegeven. In deze lijst onder **verzenden naar SAP**, selecteer de **hoofdtekst** veld. 

   ![Complete SAP-actie](./media/logic-apps-using-sap-connector-old/select-sap-body-for-response-action.png)

4. Sla uw logische app op. 

## <a name="test-your-logic-app"></a>Uw logische app testen

1. Als uw logische app nog niet ingeschakeld, klikt u op het menu van uw logische app, kiest u **overzicht**. Kies op de werkbalk **inschakelen**. 

2. Kies op de werkbalk Logic App Designer **uitvoeren**. Deze stap kunt u handmatig uw logische app start.

3. Activeer uw logische app door een HTTP POST-aanvraag te verzenden naar de URL op uw HTTP-aanvraag als trigger en uw inhoud met uw aanvraag bericht opnemen. Voor het verzenden van de aanvraag, kunt u een hulpprogramma zoals [Postman](https://www.getpostman.com/apps). 

   In dit artikel stuurt de aanvraag een IDoc-bestand dat moet worden in XML-indeling en vermeld de naamruimte voor de SAP-actie die u gebruikt, bijvoorbeeld: 

   ``` xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <Send xmlns="http://Microsoft.LobServices.Sap/2007/03/Idoc/3/ORDERS05//620/Send">
      <idocData>
         <...>
      </idocData>
   </Send>
   ```

4. Nadat u de HTTP-aanvraag verzendt, wacht u totdat de reactie van uw logische app.

> [!NOTE]
> Uw logische app time-out mogelijk als alle stappen die nodig zijn voor het antwoord is niet voltooid binnen de [aanvraag time-outlimiet](./logic-apps-limits-and-config.md). Als dit probleem gebeurt, aanvragen mogelijk geblokkeerd. Informatie over hoe u kunt voor hulp bij het analyseren van problemen, [controleren en bewaken van uw logische apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Gefeliciteerd, u hebt nu een logische app gemaakt die kan communiceren met uw SAP-server. Nu dat u een SAP-verbinding voor uw logische app instellen hebt, kunt u andere beschikbare SAP-acties, zoals BAPI en RFC kunt verkennen.

## <a name="connector-reference"></a>Connector-verwijzing

Zie deze artikelen verwijzing voor technische informatie over de connector zoals is beschreven in de connectors Swagger-bestanden: 

* [SAP-toepassingsserver](/connectors/sapapplicationserver/)
* [SAP-berichtenserver](/connectors/sapmessageserver/)

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* [Verbinding maken met on-premises systemen](../logic-apps/logic-apps-gateway-connection.md) vanuit logische apps
* Informatie over het transformeren, valideren en andere berichtbewerkingen met de [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md)
* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
