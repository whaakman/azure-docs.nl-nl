---
title: Verbinding maken met SAP-systemen - Azure Logic Apps | Microsoft Docs
description: Over het openen en SAP-resources beheren met het automatiseren van werkstromen met Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: divswa, LADocs
ms.topic: article
ms.date: 09/14/2018
tags: connectors
ms.openlocfilehash: dd86fd1aa8b1dab9f329f12924ff37db1256d1eb
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49377900"
---
# <a name="connect-to-sap-systems-from-azure-logic-apps"></a>Verbinding maken met SAP-systemen in Azure Logic Apps

Dit artikel wordt beschreven hoe u kunt toegang tot uw on-premises SAP-bronnen uit in een logische app met behulp van de connector SAP ERP centraal onderdeel (ECC). De connector SAP ECC biedt ondersteuning voor integratie van bericht of gegevens naar en van SAP Netweaver-systemen via tussenliggende Document (IDoc) of Business Application Programming Interface (BAPI) of externe functie aanroepen (RFC).

De connector SAP ECC gebruikt de <a href="https://support.sap.com/en/product/connectors/msnet.html">SAP .net Connector (NCo)-bibliotheek</a> en biedt deze bewerkingen of acties:

- **Verzenden naar SAP**: IDoc verzenden of aanroep BAPI-functies via tRFC in SAP-systemen.
- **Ontvangen van SAP**: IDoc ontvangen of BAPI functie aanroepen via tRFC van SAP-systemen.
- **Schema's genereren**: schema's voor de SAP-artefacten voor IDoc of BAPI of RFC genereren.

De SAP-connector kan worden geïntegreerd met on-premises SAP-systemen via de [on-premises gegevensgateway](https://www.microsoft.com/download/details.aspx?id=53127). In scenario's verzenden, bijvoorbeeld wanneer een bericht verzenden vanuit logische Apps met een SAP-systeem, de data gateway fungeert als een RFC-client en stuurt de aanvragen van Logic Apps ontvangen naar SAP.
Ook in scenario's ontvangen fungeert de data gateway als een RFC-server ontvangt verzoeken van SAP en doorgestuurd naar de logische App. 

In dit artikel laat zien hoe voorbeeld logica om apps te maken die kunnen worden geïntegreerd met SAP terwijl die betrekking hebben op de eerder beschreven integratiescenario's.

## <a name="prerequisites"></a>Vereisten

Als u wilt volgen, samen met dit artikel, moet u deze items:

* Een Azure-abonnement. Als u een Azure-abonnement nog geen <a href="https://azure.microsoft.com/free/" target="_blank">zich aanmelden voor een gratis Azure-account</a>.

* De logische app van waar u toegang tot uw SAP-systeem en een trigger die de werkstroom van uw logische app wordt gestart. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps](../logic-apps/logic-apps-overview.md) en [Snelstartgids: uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Uw <a href="https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server" target="_blank">SAP-toepassingsserver</a> of <a href="https://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm" target="_blank">SAP-berichtenserver</a>

* Download en installeer de meest recente [on-premises gegevensgateway](https://www.microsoft.com/download/details.aspx?id=53127) op een on-premises computer. Zorg ervoor dat u uw gateway in Azure portal hebt ingesteld, voordat u doorgaat. De gateway kunt u veilig toegang tot gegevens en resources zijn on-premises. Zie voor meer informatie, [installeren on-premises gegevensgateway voor Azure Logic Apps](../logic-apps/logic-apps-gateway-install.md).

* Download en installeer de meest recente SAP-clientbibliotheek, dat zich momenteel <a href="https://softwaredownloads.sap.com/file/0020000001865512018" target="_blank">SAP-Connector (NCo) 3.0.21.0 voor Microsoft .NET Framework 4.0 en Windows 64-bits (x64)</a>, op dezelfde computer als de on-premises gegevensgateway. Deze versie installeren of hoger om volgende redenen:

  * Eerdere versies van de SAP-NCo mogelijk in een impasse worden geraakt wanneer meer dan één IDoc-berichten worden verzonden op hetzelfde moment. 
  Dit probleem blokkeert alle latere berichten die worden verzonden naar de bestemming voor SAP, waardoor de berichten time-out.

  * De on-premises gegevensgateway wordt uitgevoerd alleen op 64-bits systemen. 
  Anders treedt er een fout 'Ongeldige installatiekopie' omdat de data gateway host-service biedt geen ondersteuning voor 32-bits-assembly's.

  * Zowel de data gateway host-service en de SAP-Adapter van Microsoft .NET Framework 4.5 gebruiken De SAP-NCo voor .NET Framework 4.0 werkt met processen die gebruikmaken van de .NET runtime 4.0-4.7.1. 
  De SAP-NCo voor .NET Framework 2.0 werkt met processen die gebruikmaken van de .NET runtime 2.0-3.5 en niet meer werkt met de meest recente on-premises data gateway.

* Inhoud van het bericht die u uw SAP-server, bijvoorbeeld een voorbeeldbestand IDoc kunt verzenden. Deze inhoud moet zich in de XML-indeling en vermeld de naamruimte voor de SAP-actie die u wilt gebruiken.

<a name="add-trigger"></a>

## <a name="send-to-sap"></a>Verzenden naar SAP

Dit voorbeeld wordt een logische app die u kunt activeren met een HTTP-aanvraag. De logische app verzendt een tussenliggende Document (IDoc) naar een SAP-server en retourneert een antwoord naar de aanvrager dat de logische app genoemd. 

### <a name="add-http-request-trigger"></a>HTTP-aanvraag als trigger toevoegen

In Azure Logic Apps, elke logische app moet beginnen met een [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts), die wordt geactiveerd wanneer een bepaalde gebeurtenis wordt uitgevoerd of wanneer een bepaalde voorwaarde wordt voldaan. Telkens wanneer de trigger wordt geactiveerd, de Logic Apps-engine een exemplaar van de logische app maakt en werkstroom van uw app wordt gestart.

In dit voorbeeld u een logische app maken met een eindpunt in Azure zodat u kunt verzenden *HTTP POST-aanvragen* aan uw logische app. Wanneer uw logische app deze HTTP-aanvragen ontvangt, wordt de trigger wordt geactiveerd en wordt uitgevoerd de volgende stap in uw werkstroom.

1. In de [Azure-portal](https://portal.azure.com), maak een lege logische app, die de ontwerper van logische App wordt geopend. 

2. Typ 'http-aanvraag' als filter in het zoekvak. Selecteer in de lijst met triggers deze trigger: **aanvraag: wanneer een HTTP-aanvraag wordt ontvangen**

   ![HTTP-aanvraag trigger toevoegen](./media/logic-apps-using-sap-connector/add-trigger.png)

3. Nu uw logische app opslaan, zodat u een eindpunt-URL voor uw logische app kunt genereren.
Kies **Opslaan** op de werkbalk van de ontwerper. 

   Het eindpunt van de URL wordt nu weergegeven in de trigger, bijvoorbeeld:

   ![URL genereren voor het eindpunt](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

<a name="add-action"></a>

### <a name="add-sap-action"></a>SAP-actie toevoegen

In Azure Logic Apps, een [actie](../logic-apps/logic-apps-overview.md#logic-app-concepts) is een stap in uw werkstroom die volgt op een trigger of een andere actie. Als u dit nog niet hebt toegevoegd een trigger aan uw logische app nog en voer dit voorbeeld wilt [toevoegen van de trigger die in deze sectie beschreven](#add-trigger).

1. Kies in de Logic App Designer onder de trigger **nieuwe stap** > **een actie toevoegen**.

   ![Een actie toevoegen](./media/logic-apps-using-sap-connector/add-action.png) 

2. Typ 'sap' als filter in het zoekvak. Selecteer in de lijst met acties met deze actie: **bericht verzenden voor SAP**
  
   ![Selecteer de actie van de SAP-verzenden](media/logic-apps-using-sap-connector/select-sap-send-action.png)

   U kunt ook in plaats van te zoeken, kiest u de **Enterprise** tabblad, en selecteer de SAP-actie.

   ![Selecteer SAP verzendactie in Enterprise-tabblad](media/logic-apps-using-sap-connector/select-sap-send-action-ent-tab.png)

3. Als u wordt gevraagd of u voor de verbindingsgegevens, maakt u nu uw SAP-verbinding. Anders als de verbinding al bestaat, gaat u door met de volgende stap, zodat u uw SAP-actie kunt instellen. 

   **On-premises SAP-verbinding maken**

   1. De verbindingsgegevens voor uw SAP-server opgeven. 
   Voor de **gegevensgateway** eigenschap, selecteert u de data gateway die u hebt gemaakt in de Azure-portal voor uw gatewayinstallatie.

      Als de **aanmeldingstype** eigenschap is ingesteld op **toepassingsserver**, deze eigenschappen, die doorgaans optioneel verschijnen, zijn vereist:

      ![Serververbinding voor SAP-toepassing maken](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png) 

      Als de **aanmeldingstype** eigenschap is ingesteld op **groep**, deze eigenschappen, die doorgaans optioneel verschijnen, zijn vereist: 

      ![SAP bericht serververbinding maken](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png) 

   2. Wanneer u klaar bent, kiest u **Maken**. 
   
      Logic Apps wordt ingesteld en test de verbinding, ervoor te zorgen dat de verbinding naar behoren werkt.

4. Nu zoeken en selecteer een actie van uw SAP-server. 

   1. In de **SAP actie** Kies het pictogram van de map. 
   In de lijst, zoek en selecteer het SAP-bericht dat u wilt gebruiken. 
   Ga in de lijst, gebruikt u de pijlen.

      In dit voorbeeld wordt een IDoc met **volgorde** type. 

      ![Zoek en selecteer de actie IDoc](./media/logic-apps-using-sap-connector/SAP-app-server-find-action.png)

      Als u niet de actie die u wilt vinden, kunt u een pad, bijvoorbeeld handmatig invoeren:

      ![Pad naar IDoc actie handmatig opgeven](./media/logic-apps-using-sap-connector/SAP-app-server-manually-enter-action.png)

      > [!TIP]
      > Geef de waarde voor SAP-actie tot en met de expressie-editor. Op die manier kunt u dezelfde actie voor verschillende typen.

      Zie voor meer informatie over de werking van IDoc [schema's voor IDOC operations bericht](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

   2. Klik in de **invoerbericht** vak zodat de lijst met dynamische inhoud wordt weergegeven. 
   In die lijst onder **wanneer een HTTP-aanvraag wordt ontvangen**, selecteer de **hoofdtekst** veld. 

      Deze stap bevat de hoofdtekst van uw HTTP-aanvraag als trigger en verstuurt die uitvoer naar uw SAP-server.

      ![Veld 'Hoofdtekst' selecteren](./media/logic-apps-using-sap-connector/SAP-app-server-action-select-body.png)

      Wanneer u klaar bent, ziet uw SAP-actie eruit zoals in dit voorbeeld:

      ![Complete SAP-actie](./media/logic-apps-using-sap-connector/SAP-app-server-complete-action.png)

5. Sla uw logische app op. Kies **Opslaan** op de werkbalk van de ontwerper.

<a name="add-response"></a>

### <a name="add-http-response-action"></a>HTTP-antwoord actie toevoegen

Nu een reactie toevoegen aan uw logische app-werkstroom en de uitvoer van de SAP-actie bevatten. Op die manier kunnen uw logische app retourneert de resultaten van uw SAP-server naar de oorspronkelijke aanvrager. 

1. Kies in de Logic App Designer onder de actie SAP **nieuwe stap** > **een actie toevoegen**.

2. Typ 'response' als filter in het zoekvak. Selecteer in de lijst met acties met deze actie: **aanvraag - reactie**

3. Klik in de **hoofdtekst** vak zodat de lijst met dynamische inhoud wordt weergegeven. In deze lijst onder **verzenden naar SAP**, selecteer de **hoofdtekst** veld. 

   ![Complete SAP-actie](./media/logic-apps-using-sap-connector/select-sap-body-for-response-action.png)

4. Sla uw logische app op. 

### <a name="test-your-logic-app"></a>Uw logische app testen

1. Als uw logische app nog niet ingeschakeld, klikt u op het menu van uw logische app, kiest u **overzicht**. Kies op de werkbalk **inschakelen**. 

2. Kies op de werkbalk Logic App Designer **uitvoeren**. Deze stap kunt u handmatig uw logische app start.

3. Activeer uw logische app door een HTTP POST-aanvraag te verzenden naar de URL op uw HTTP-aanvraag als trigger en uw inhoud met uw aanvraag bericht opnemen. Voor het verzenden van de aanvraag, kunt u een hulpprogramma zoals [Postman](https://www.getpostman.com/apps). 

   In dit artikel stuurt de aanvraag een IDoc-bestand dat moet worden in XML-indeling en vermeld de naamruimte voor de SAP-actie die u gebruikt, bijvoorbeeld: 

   ``` xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <Send xmlns="http://Microsoft.LobServices.Sap/2007/03/Idoc/2/ORDERS05//720/Send">
      <idocData>
         <...>
      </idocData>
   </Send>
   ```

4. Nadat u de HTTP-aanvraag verzendt, wacht u totdat de reactie van uw logische app.

   > [!NOTE]
   > Uw logische app time-out mogelijk als alle stappen die nodig zijn voor het antwoord is niet voltooid binnen de [aanvraag time-outlimiet](./logic-apps-limits-and-config.md). Als dit probleem gebeurt, aanvragen mogelijk geblokkeerd. Informatie over hoe u kunt voor hulp bij het analyseren van problemen, [controleren en bewaken van uw logische apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Gefeliciteerd, u hebt nu een logische app gemaakt die kan communiceren met uw SAP-server. Nu dat u een SAP-verbinding voor uw logische app instellen hebt, kunt u andere beschikbare SAP-acties, zoals BAPI en RFC kunt verkennen.

## <a name="receive-from-sap"></a>Ontvangen van SAP

Dit voorbeeld wordt een logische app die wordt geactiveerd wanneer een bericht ontvangen van een SAP-systeem. 

### <a name="add-sap-trigger"></a>SAP-trigger toevoegen

1. Maak een lege logische app, die de ontwerper van logische App wordt geopend in de Azure-portal. 

2. Typ 'sap' als filter in het zoekvak. Selecteer in de lijst met triggers deze trigger: **wanneer een bericht wordt ontvangen van SAP**

   ![SAP-trigger toevoegen](./media/logic-apps-using-sap-connector/add-sap-trigger.png)

   U kunt ook gaat u naar het tabblad Enterprise en selecteer de trigger

   ![SAP-trigger toevoegen van ent-tabblad](./media/logic-apps-using-sap-connector/add-sap-trigger-ent-tab.png)

3. Als u wordt gevraagd of u voor de verbindingsgegevens, maakt u nu uw SAP-verbinding. Anders als de verbinding al bestaat, gaat u door met de volgende stap, zodat u uw SAP-actie kunt instellen. 

   **On-premises SAP-verbinding maken**

   1. De verbindingsgegevens voor uw SAP-server opgeven. 
   Voor de **gegevensgateway** eigenschap, selecteert u de data gateway die u hebt gemaakt in de Azure-portal voor uw gatewayinstallatie.

      Als de **aanmeldingstype** eigenschap is ingesteld op **toepassingsserver**, deze eigenschappen, die doorgaans optioneel verschijnen, zijn vereist:

      ![Serververbinding voor SAP-toepassing maken](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png) 

      Als de **aanmeldingstype** eigenschap is ingesteld op **groep**, deze eigenschappen, die doorgaans optioneel verschijnen, zijn vereist:

      ![SAP bericht serververbinding maken](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)  

4. Geef de vereiste parameters op basis van de configuratie van uw SAP-systeem. 

   U kunt desgewenst een of meer SAP-acties. 
   Deze lijst met acties Hiermee geeft u de berichten die de trigger van uw SAP-server via de gegevensgateway ontvangt. 
   Een lege lijst geeft aan dat alle berichten worden ontvangen door de trigger. 
   Als de lijst meer dan één bericht heeft, ontvangt de trigger alleen de berichten die zijn opgegeven in de lijst. Andere berichten van uw SAP-server worden geweigerd door de gateway.

   U kunt een SAP-actie selecteren uit de bestandenkiezer:

   ![SAP-actie selecteren](media/logic-apps-using-sap-connector/select-SAP-action-trigger.png)  

   Of u kunt handmatig een actie opgeven:

   ![Voer handmatig de SAP-actie](media/logic-apps-using-sap-connector/manual-enter-SAP-action-trigger.png) 

   Hier volgt een voorbeeld waarin wordt weergegeven hoe de actie wordt weergegeven bij het instellen van de trigger om meer dan één bericht te ontvangen.

   ![Voorbeeld van de trigger](media/logic-apps-using-sap-connector/example-trigger.png)  

   Zie voor meer informatie over de SAP-actie [bericht schema's voor IDOC-bewerkingen](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations)

5. Nu uw logische app opslaan zodat u kunt beginnen met het ontvangen van berichten van uw SAP-systeem.
Kies **Opslaan** op de werkbalk van de ontwerper. 

Uw logische app is nu klaar om berichten te ontvangen van uw SAP-systeem. 

> [!NOTE]
> De SAP-trigger is niet een polling-trigger, maar een op basis van een webhook-trigger in plaats daarvan. De trigger wordt aangeroepen vanuit de gateway alleen als er een bericht bestaat, zodat er geen polling nodig is. 

### <a name="test-your-logic-app"></a>Uw logische app testen

1. Voor het activeren van uw logische app, moet u een bericht verzenden vanuit uw SAP-systeem.

2. Kies op het menu van de logische app, **overzicht**, en bekijk de **geschiedenis van uitvoeringen** voor elke nieuwe uitgevoerd voor uw logische app. 

3. Open de meest recente uitvoering, waarin het bericht verzonden van uw SAP-systeem in de sectie van de trigger-uitvoer.

## <a name="generate-schemas-for-artifacts-in-sap"></a>Schema's voor artefacten in SAP genereren

Dit voorbeeld wordt een logische app die u kunt activeren met een HTTP-aanvraag. De SAP-actie verzendt een aanvraag met een SAP-systeem voor het genereren van de schema's voor het opgegeven tussenliggende Document (IDoc) en BAPI. Schema's die in het antwoord retourneren worden geüpload naar een Integratieaccount met de Azure Resource Manager-connector.

### <a name="add-http-request-trigger"></a>HTTP-aanvraag als trigger toevoegen

1. Maak een lege logische app, die de ontwerper van logische App wordt geopend in de Azure-portal. 

2. Typ 'http-aanvraag' als filter in het zoekvak. Selecteer in de lijst met triggers deze trigger: **aanvraag: wanneer een HTTP-aanvraag wordt ontvangen**

   ![HTTP-aanvraag trigger toevoegen](./media/logic-apps-using-sap-connector/add-trigger.png)

3. Nu uw logische app opslaan, zodat u een eindpunt-URL voor uw logische app kunt genereren.
Kies **Opslaan** op de werkbalk van de ontwerper. 

   Het eindpunt van de URL wordt nu weergegeven in de trigger, bijvoorbeeld:

   ![URL genereren voor het eindpunt](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

### <a name="add-sap-action-to-generate-schemas"></a>SAP-actie voor het genereren van schema's toevoegen

1. Kies in de Logic App Designer onder de trigger **nieuwe stap** > **een actie toevoegen**.

   ![Een actie toevoegen](./media/logic-apps-using-sap-connector/add-action.png) 

2. Typ 'sap' als filter in het zoekvak. Selecteer in de lijst met acties met deze actie: **schema's genereren**
  
   ![Selecteer de actie van de SAP-verzenden](media/logic-apps-using-sap-connector/select-sap-schema-generator-action.png)

   U kunt ook u kunt ook de **Enterprise** tabblad, en selecteer de SAP-actie.

   ![Selecteer SAP verzendactie in Enterprise-tabblad](media/logic-apps-using-sap-connector/select-sap-schema-generator-ent-tab.png)

3. Als u wordt gevraagd of u voor de verbindingsgegevens, maakt u nu uw SAP-verbinding. Anders als de verbinding al bestaat, gaat u door met de volgende stap, zodat u uw SAP-actie kunt instellen. 

   **On-premises SAP-verbinding maken**

   1. De verbindingsgegevens voor uw SAP-server opgeven. 
   Voor de **gegevensgateway** eigenschap, selecteert u de data gateway die u hebt gemaakt in de Azure-portal voor uw gatewayinstallatie.

      Als de **aanmeldingstype** eigenschap is ingesteld op **toepassingsserver**, deze eigenschappen, die doorgaans optioneel verschijnen, zijn vereist:

      ![Serververbinding voor SAP-toepassing maken](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png) 

      Als de **aanmeldingstype** eigenschap is ingesteld op **groep**, deze eigenschappen, die doorgaans optioneel verschijnen, zijn vereist:
   
      ![SAP bericht serververbinding maken](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png) 

   2. Wanneer u klaar bent, kiest u **Maken**. Logic Apps wordt ingesteld en test de verbinding, ervoor te zorgen dat de verbinding naar behoren werkt.

4. Het pad naar het artefact waarvan u wilt voor het genereren van het schema opgeven.

   U kunt de SAP-actie selecteren uit de bestandenkiezer:

   ![SAP-actie selecteren](media/logic-apps-using-sap-connector/select-SAP-action-schema-generator.png)  

   Of u kunt de actie handmatig invoeren:

   ![Voer handmatig de SAP-actie](media/logic-apps-using-sap-connector/manual-enter-SAP-action-schema-generator.png) 

   Voor het genereren van schema's voor meer dan één artefact, bieden u de details van de SAP-actie voor elke artefact, bijvoorbeeld:

   ![Selecteer Nieuw item toevoegen](media/logic-apps-using-sap-connector/schema-generator-array-pick.png) 

   ![Twee items weergeven](media/logic-apps-using-sap-connector/schema-generator-example.png) 

   Zie voor meer informatie over de SAP-actie [schema's voor IDOC operations bericht](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

5. Sla uw logische app op. Kies **Opslaan** op de werkbalk van de ontwerper.

### <a name="test-your-logic-app"></a>Uw logische app testen

1. Kies op de werkbalk van de ontwerper **uitvoeren** voor het activeren van een uitvoering voor uw logische app.

2. Open de uitvoering en of de uitvoer voor de **schema genereren** actie. 

   De uitvoer ziet de gegenereerde schema's voor de opgegeven lijst met berichten.

### <a name="upload-schemas-to-integration-account"></a>Schema's uploaden naar het integratieaccount

Desgewenst kunt u downloaden of de gegenereerde schema's opslaan in opslagplaatsen, zoals een blob-, opslag- of integratie-account. Integratieaccounts bieden een eersteklas ervaring met andere acties XML, zodat dit voorbeeld het schema's uploaden naar een integratieaccount voor dezelfde logische app wordt met behulp van de Azure Resource Manager-connector.

1. In Logic App Designer, onder de trigger kiezen **nieuwe stap** > **een actie toevoegen**. Typ 'resource manager' als filter in het zoekvak. Selecteer deze actie: **maken of bijwerken van een resource**

   ![Azure Resource Manager-actie selecteren](media/logic-apps-using-sap-connector/select-arm-action.png) 

2. Geef de details, met inbegrip van uw Azure-abonnement, de Azure-resourcegroep en de integratie-account. Ga als volgt het voorbeeld hieronder voor andere velden.

   ![Geef details op voor de Azure Resource Manager-actie](media/logic-apps-using-sap-connector/arm-action.png)

   De SAP **schema's genereren** genereert u schema's als een verzameling, zodat de ontwerper automatisch toegevoegd een **voor elk** lus met de actie. 
   Hier volgt een voorbeeld waarin wordt weergegeven hoe deze actie wordt weergegeven:

   ![Azure Resource Manager-actie met 'voor elke' lus](media/logic-apps-using-sap-connector/arm-action-foreach.png)  

   > [!NOTE]
   > De schema's met base64 gecodeerde indeling gebruiken. Als u wilt de schema's uploaden naar een integratieaccount, moeten ze worden ontsleuteld met behulp van de `base64ToString()` functie. Hier volgt een voorbeeld waarin de code voor de `"properties"` element:
   >
   > ```json
   > "properties": {
   >    "Content": "@base64ToString(items('For_each')?['Content'])",
   >    "ContentType": "application/xml",
   >    "SchemaType": "Xml"
   > }
   > ```

3. Sla uw logische app op. Kies **Opslaan** op de werkbalk van de ontwerper.

### <a name="test-your-logic-app"></a>Uw logische app testen

1. Kies op de werkbalk van de ontwerper **uitvoeren** aan uw logische app handmatig te activeren.

2. Na een geslaagde uitvoeren, gaat u naar het integratieaccount en controleer of de gegenereerde schema's die worden gegenereerd bestaan.

## <a name="known-issues-and-limitations"></a>Bekende problemen en beperkingen

Hier worden de bekende problemen en beperkingen voor de SAP-connector:

* De SAP-trigger does't ondersteuning voor batch idoc's ontvangen van SAP. Deze actie kan leiden tot fout voor RFC-verbinding tussen uw SAP-systeem en de data gateway.

* De trigger SAP biedt geen ondersteuning voor gatewayclusters met gegevens. In sommige gevallen failover kan op het gegevensknooppunt voor gateway die met de SAP-systeem communiceert verschillen van het actieve knooppunt, wat resulteert in onverwacht gedrag. Voor scenario's verzenden, worden data gatewayclusters ondersteund.

* In scenario's ontvangen, wordt niet een niet-null-antwoord wordt geretourneerd ondersteund. Een logische app met een trigger en een reactie leidt tot onverwacht gedrag. 

* Alleen een enkel verzenden naar SAP-oproep of een bericht werkt met tRFC. Het patroon Business Application Programming Interface (BAPI) doorvoeren, zoals meerdere tRFC aanroepen in dezelfde sessie, wordt niet ondersteund.

* RFC's met bijlagen worden niet ondersteund voor zowel de verzenden naar SAP- en schema's acties te genereren.

* De SAP-connector op dit moment biedt geen ondersteuning voor SAP-router tekenreeksen. De on-premises gegevensgateway moet bestaan in hetzelfde LAN bevinden als de SAP-systeem dat u verbinding wilt maken.

* De conversie van ontbreekt (null), leeg, minimale en maximale waarden voor velden DATS en TIM SAP wordt mogelijk gewijzigd in latere updates voor de on-premises gegevensgateway.

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* [Verbinding maken met on-premises systemen](../logic-apps/logic-apps-gateway-connection.md) vanuit logische apps
* Informatie over het transformeren, valideren en andere berichtbewerkingen met de [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md)
* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
