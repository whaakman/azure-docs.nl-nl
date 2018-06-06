---
title: Verbinding maken met de SAP-systemen - Azure Logic Apps | Microsoft Docs
description: Het openen en SAP-resources beheren door automatisering van werkstromen met Azure Logic Apps
author: ecfan
manager: cfowler
ms.author: estfan
ms.date: 05/31/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, divswa, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 658fb40f618b61fcd973a626e4e00afc3ad8151a
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34756708"
---
# <a name="connect-to-sap-systems-from-azure-logic-apps"></a>Verbinding maken met de SAP-systemen vanaf Azure Logic Apps

Dit artikel laat zien hoe u toegang hebt tot uw SAP-resources in een logische app met behulp van de SAP-toepassingsserver en SAP berichtenserver-connectors. Op die manier kunt u taken, processen en werkstromen die uw SAP-gegevens en bronnen beheren door te maken van logische apps automatiseren.

Dit voorbeeld wordt een logische app die u kunt activeren met een HTTP-aanvraag. De logische app verzendt een tussenliggende Document (IDoc) naar een SAP-server en retourneert een antwoord aan de aanvrager die de logische app aangeroepen.
De huidige SAP-connectors acties, maar geen triggers hebben, zodat dit voorbeeld wordt de [HTTP-aanvraag trigger](../connectors/connectors-native-reqres.md) als de eerste stap in de logische app-werkstroom. Voor SAP connector-specifieke technische informatie, Zie de referentie-artikelen: 

* <a href="https://docs.microsoft.com/connectors/sapapplicationserver/" target="blank">SAP Application Server-connector</a>
* <a href="https://docs.microsoft.com/connectors/sapmessageserver/" target="blank">SAP berichtenserver-connector</a>

Als u een Azure-abonnement nog geen <a href="https://azure.microsoft.com/free/" target="_blank">aanmelden voor een gratis Azure-account</a>.

## <a name="prerequisites"></a>Vereisten

Als u wilt volgen samen met dit artikel, moet u deze items:

* De logische app waaruit u wilt toegang tot uw SAP-systeem en een trigger die uw logische app werkstroom wordt gestart. De SAP-connectors bieden momenteel alleen acties. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps](../logic-apps/logic-apps-overview.md) en [Snelstartgids: uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Uw <a href="https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server" target="_blank">SAP-toepassingsserver</a> of <a href="https://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm" target="_blank">SAP bericht-Server</a>

* Download en installeer de meest recente [lokale gegevensgateway](https://www.microsoft.com/download/details.aspx?id=53127) op een lokale computer. Zorg ervoor dat u uw gateway in de Azure portal hebt ingesteld, voordat u doorgaat. De gateway kunt u veilig toegang krijgen tot gegevens en resources zijn on-premises. Zie voor meer informatie [installatie on-premises gegevensgateway voor Azure Logic Apps](../logic-apps/logic-apps-gateway-install.md).

* Download en installeer de meest recente SAP-clientbibliotheek die momenteel <a href="https://softwaredownloads.sap.com/file/0020000000086282018" target="_blank">SAP-Connector (NCo) 3.0.20.0 voor Microsoft .NET Framework 4.0 en Windows 64-bits (x64)</a>, op dezelfde computer als de lokale data gateway. Deze versie installeren of hoger om deze redenen:

  * Kunnen een van eerdere versies van het SAP NCo worden impasse wanneer meer dan één IDoc berichten worden verzonden op hetzelfde moment. 
  Deze voorwaarde blokkeert alle latere berichten die worden verzonden naar de bestemming SAP, waardoor de berichten time-out.

  * De lokale data gateway werkt alleen op 64-bits systemen. 
  Anders krijgt u een 'slechte installatiekopie' fout omdat de data gateway-hostservice biedt geen ondersteuning voor 32-bits-assembly's.

  * De data gateway-hostservice zowel de SAP-Adapter van Microsoft gebruiken de .NET Framework 4.5. De SAP-NCo voor .NET Framework 4.0 werkt met de processen die gebruikmaken van .NET runtime 4.0-4.7.1. 
  De SAP-NCo voor .NET Framework 2.0 werkt met de processen die gebruikmaken van .NET runtime 2.0-3.5 en niet meer werkt met de meest recente lokale data gateway.

* Inhoud van het bericht die u naar uw SAP-server, zoals een voorbeeldbestand IDoc verzenden kunt. Deze inhoud moet worden in XML-indeling en vermeld de naamruimte voor de SAP-actie die u wilt gebruiken.

<a name="add-trigger"></a>

## <a name="add-http-request-trigger"></a>HTTP-aanvraag trigger toevoegen

In Azure Logic Apps elke logische app moet beginnen met een [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts), die wordt geactiveerd wanneer een bepaalde gebeurtenis wordt uitgevoerd of wanneer een bepaalde voorwaarde is voldaan. Telkens wanneer de trigger wordt geactiveerd, de Logic Apps-engine maakt een logische app-exemplaar en de werkstroom van uw app wordt gestart.

In dit voorbeeld wordt u een logische app maken met een eindpunt in Azure zodat u kunt verzenden *HTTP POST-aanvragen* aan uw logische app. Wanneer uw logische app deze HTTP-aanvragen ontvangt, wordt de trigger wordt geactiveerd en de volgende stap in de werkstroom wordt uitgevoerd.

1. In de Azure-portal maakt een lege logische app, waardoor de Logic App-ontwerper wordt geopend. 

2. Voer in het zoekvak 'http-aanvraag' als filter. Selecteer in de lijst triggers deze trigger: **aanvraag: wanneer een HTTP-aanvraag wordt ontvangen**

   ![HTTP-aanvraag trigger toevoegen](./media/logic-apps-using-sap-connector/add-trigger.png)

3. Nu uw logische app opslaan zodat u kunt een eindpunt-URL voor uw logische app genereren.
Kies **Opslaan** op de werkbalk van de ontwerper. 

   Het eindpunt URL wordt nu weergegeven in de trigger, bijvoorbeeld:

   ![URL voor het eindpunt genereren](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

<a name="add-action"></a>

## <a name="add-sap-action"></a>SAP-actie toevoegen

In Azure Logic Apps een [actie](../logic-apps/logic-apps-overview.md#logic-app-concepts) is een stap in de werkstroom die na een trigger of een andere actie. Als u hebt een trigger aan uw logische app nog toegevoegd en volgt u in dit voorbeeld wilt [toevoegen van de trigger in deze sectie beschreven](#add-trigger).

1. Kies in de ontwerpfunctie Logic App, onder de trigger **nieuwe stap** > **een actie toevoegen**.

   ![Een actie toevoegen](./media/logic-apps-using-sap-connector/add-action.png) 

2. Voer in het zoekvak 'sap-server' als filter. Selecteer de actie voor uw SAP-server in de lijst met acties: 

   * **SAP-toepassingsserver - verzenden naar SAP**
   * **SAP-Server voor bericht - verzenden naar SAP**

   In dit voorbeeld wordt met deze actie: **toepassingsserver SAP - verzenden naar SAP**

   ![Selecteer 'SAP-toepassingsserver' of 'Server SAP bericht'](media/logic-apps-using-sap-connector/select-sap-action.png)

3. Als u wordt gevraagd om Verbindingsdetails, maakt u nu uw SAP-verbinding. Anders als uw verbinding al bestaat, gaat u verder met de volgende stap zodat u uw SAP-actie kunt instellen. 

   **Lokale SAP verbinding maken**

   1. Voor **Gateways**, selecteer **verbinden via on-premises gegevensgateway** zodat de verbindingseigenschappen van de lokale worden weergegeven.

   2. Geef de verbindingsgegevens voor uw SAP-server. 
   Voor de **gateway** eigenschap, selecteert u de data gateway die u hebt gemaakt in de Azure portal voor uw gateway-installatie bijvoorbeeld:

      **SAP-toepassingsserver**

      ![SAP application serververbinding maken](./media/logic-apps-using-sap-connector/create-SAP-app-server-connection.png)  

      **SAP-berichtenserver**

      ![SAP bericht serververbinding maken](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png) 

   2. Wanneer u klaar bent, kiest u **Maken**.

      Logic Apps ingesteld en test uw verbinding, om ervoor te zorgen dat de verbinding goed werkt.

4. Nu zoeken en selecteer een actie van uw SAP-server. 

   1. In de **SAP actie** pictogram van de map kiest. 
   Uit de lijst met mappen Zoek en selecteer de actie die u wilt gebruiken. 

      In dit voorbeeld selecteert de **IDOC** categorie voor de actie IDoc. 

      ![Zoek en selecteer de actie IDoc](./media/logic-apps-using-sap-connector/SAP-app-server-find-action.png)

      Als u niet de actie die u wilt vinden, kunt u een pad, bijvoorbeeld handmatig opgeven:

      ![Pad naar IDoc actie handmatig opgeven](./media/logic-apps-using-sap-connector/SAP-app-server-manually-enter-action.png)

      Zie voor meer informatie over IDoc bewerkingen [bericht schema's voor IDOC bewerkingen](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations)

   2. Klik in de **invoerbericht** vak zodat de lijst met dynamische inhoud wordt weergegeven. 
   In deze lijst onder **wanneer een HTTP-aanvraag wordt ontvangen**, selecteer de **hoofdtekst** veld. 

      Deze stap omvat de inhoud van de hoofdtekst van uw aanvraag HTTP-trigger en uitvoer naar uw SAP-server verzendt.

      ![Selecteer 'Hoofdtekst' veld](./media/logic-apps-using-sap-connector/SAP-app-server-action-select-body.png)

      Wanneer u bent klaar, ziet uw SAP-actie in dit voorbeeld:

      ![SAP-bewerking voltooien](./media/logic-apps-using-sap-connector/SAP-app-server-complete-action.png)

6. Sla uw logische app op. Kies **Opslaan** op de werkbalk van de ontwerper.

<a name="add-response"></a>

## <a name="add-http-response-action"></a>HTTP-antwoord actie toevoegen

Nu een antwoord actie toevoegen aan uw logische app werkstroom en de uitvoer van de actie SAP bevatten. Op die manier uw logische app retourneert de resultaten van uw SAP-server naar de oorspronkelijke aanvrager. 

1. Kies in de ontwerpfunctie Logic App, onder de actie SAP **nieuwe stap** > **een actie toevoegen**.

2. Voer in het zoekvak "antwoord" als filter. Selecteer in de lijst met acties deze actie: **aanvragen - antwoord**

3. Klik in de **hoofdtekst** vak zodat de lijst met dynamische inhoud wordt weergegeven. In deze lijst onder **verzenden naar SAP**, selecteer de **hoofdtekst** veld. 

   ![SAP-bewerking voltooien](./media/logic-apps-using-sap-connector/select-sap-body-for-response-action.png)

4. Sla uw logische app op. 

## <a name="test-your-logic-app"></a>Uw logische app testen

1. Als uw logische app al is niet ingeschakeld, klikt u op uw logische app-menu kiezen **overzicht**. Kies op de werkbalk **inschakelen**. 

2. Kies op de werkbalk Logic App-ontwerper **uitvoeren**. Deze stap start handmatig uw logische app.

3. Uw logische app activeren door een HTTP POST-aanvraag verzenden naar de URL in de aanvraag HTTP-trigger en het bericht dat inhoud bij uw aanvraag. De aanvraag verzenden kunt u een hulpprogramma zoals [Postman](https://www.getpostman.com/apps). 

   Voor dit artikel stuurt de aanvraag een IDoc-bestand, dat moet worden in XML-indeling en vermeld de naamruimte voor de SAP-actie die u gebruikt, bijvoorbeeld: 

   ``` xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <Send xmlns="http://Microsoft.LobServices.Sap/2007/03/Idoc/3/ORDERS05//620/Send">
      <idocData>
         <...>
      </idocData>
   </Send>
   ```

4. Nadat u de HTTP-aanvraag verzonden, wacht u totdat de reactie van uw logische app.

> [!NOTE]
> Uw logische app mogelijk time-out als alle stappen die nodig zijn voor het antwoord is niet voltooid binnen de [aanvraag time-outlimiet](./logic-apps-limits-and-config.md). Als dit probleem zich voordoet, kunnen aanvragen worden geblokkeerd. Om te kunnen analyseren van problemen, meer informatie over hoe u kunt [controleren en bewaken van uw logische apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Gefeliciteerd, u nu een logische app, die kan communiceren met uw SAP-server hebt gemaakt. Nu dat u een SAP-verbinding voor uw logische app instellen hebt, kunt u andere beschikbare SAP-acties, zoals BAPI en RFC kunt verkennen.

## <a name="connector-reference"></a>Connector-verwijzing

Zie voor technische informatie over de connector zoals beschreven door de Swagger-bestanden van de verbindingslijnen deze verwijzing artikelen: 

* [SAP-toepassingsserver](/connectors/sapapplicationserver/)
* [SAP-berichtenserver](/connectors/sapmessageserver/)

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* [Verbinding maken met on-premises systemen](../logic-apps/logic-apps-gateway-connection.md) vanuit logic apps
* Informatie over het valideren, transformeren en andere berichtbewerkingen met de [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md)
* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
