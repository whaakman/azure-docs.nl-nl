---
title: Verbinding maken met SAP-systemen - Azure Logic Apps
description: Toegang tot en beheer van SAP-resources door het automatiseren van werkstromen met Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: divswa, LADocs
ms.topic: article
ms.date: 05/09/2019
tags: connectors
ms.openlocfilehash: 8232bf90b4dc160583959345a257846aaabad690
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67458936"
---
# <a name="connect-to-sap-systems-from-azure-logic-apps"></a>Verbinding maken met SAP-systemen in Azure Logic Apps

Dit artikel wordt beschreven hoe u kunt toegang tot uw on-premises SAP-bronnen uit in een logische app met behulp van de SAP-connector. De connector werkt met klassieke van SAP-releases zoals R/3 en ECC systemen on-premises. De connector maakt ook integratie met SAP nieuwere SAP HANA gebaseerde systemen, zoals s/4 HANA, of ze worden gehost on-premises of in de cloud. De SAP-connector biedt ondersteuning voor integratie van bericht of gegevens naar en van SAP NetWeaver-systemen via een tussenliggende Document (IDoc), Business Application Programming Interface (BAPI) of externe functie aanroepen (RFC).

De SAP-connector gebruikt de [SAP .NET Connector (NCo)-bibliotheek](https://support.sap.com/en/product/connectors/msnet.html) en biedt deze bewerkingen of acties:

* **Verzenden naar SAP**: IDoc verzenden via tRFC, BAPI functies aanroepen via RFC of RFC/tRFC aanroepen in SAP-systemen.
* **Ontvangen van SAP**: IDoc via tRFC ontvangen, BAPI functies aanroepen via tRFC of RFC/tRFC aanroepen in SAP-systemen.
* **Schema's genereren**: Schema's voor de SAP-artefacten voor IDoc, BAPI of RFC genereren.

De SAP-connector biedt ondersteuning voor basisverificatie via gebruikersnamen en wachtwoorden voor deze bewerkingen. De connector ondersteunt ook [Secure Network Communications (SNC)](https://help.sap.com/doc/saphelp_nw70/7.0.31/e6/56f466e99a11d1a5b00000e835363f/content.htm?no_cache=true). SNC kan worden gebruikt voor SAP NetWeaver eenmalige aanmelding (SSO) of voor aanvullende beveiligingsmogelijkheden geleverd door een externe beveiliging-product.

De SAP-connector kan worden geïntegreerd met on-premises SAP-systemen via de [on-premises gegevensgateway](../logic-apps/logic-apps-gateway-connection.md). In scenario's verzenden, bijvoorbeeld wanneer een bericht wordt verzonden vanuit een logische app met een SAP-systeem, de data gateway fungeert als een RFC-client en stuurt de aanvragen ontvangen van de logische app SAP.
Ontvangt ook in scenario's, de data gateway fungeert als een RFC-server die u ontvangt verzoeken van SAP en ze doorstuurt naar de logische app.

In dit artikel laat zien hoe voorbeeld logica om apps te maken die kunnen worden geïntegreerd met SAP terwijl die betrekking hebben op de eerder beschreven integratiescenario's.

<a name="pre-reqs"></a>

## <a name="prerequisites"></a>Vereisten

Als u wilt volgen, samen met dit artikel, moet u deze items:

* Een Azure-abonnement. Als u een Azure-abonnement nog geen [zich aanmelden voor een gratis Azure-account](https://azure.microsoft.com/free/).
* De logische app van waar u toegang tot uw SAP-systeem en een trigger die de werkstroom van uw logische app wordt gestart. Als u geen ervaring met logische apps, Zie [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md) en [Quick Start: Maak uw eerste logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).
* Uw [SAP-toepassingsserver](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) of [SAP-berichtenserver](https://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm).
* Download en installeer de meest recente [on-premises gegevensgateway](https://www.microsoft.com/download/details.aspx?id=53127) op een on-premises computer. Zorg ervoor dat u uw gateway in Azure portal hebt ingesteld, voordat u doorgaat. De gateway kunt u veilig toegang tot on-premises gegevens en resources. Zie voor meer informatie, [een on-premises gegevensgateway installeren voor Azure Logic Apps](../logic-apps/logic-apps-gateway-install.md).
* Als u SNC met eenmalige aanmelding gebruikt, zorg er dan voor dat de gateway wordt uitgevoerd als een gebruiker die toegewezen op basis van de SAP-gebruiker. De standaard-serviceaccount wilt wijzigen, selecteert u **account wijzigen**, en voer de referenties van de gebruiker.

  ![Gateway-account wijzigen](./media/logic-apps-using-sap-connector/gateway-account.png)

* Als u SNC met een externe beveiliging-product inschakelen, kopieert u de SNC-bibliotheek of de bestanden op dezelfde computer waarop de gateway is geïnstalleerd. Enkele voorbeelden van SNC-producten zijn [sapseculib](https://help.sap.com/saphelp_nw74/helpdata/en/7a/0755dc6ef84f76890a77ad6eb13b13/frameset.htm), Kerberos en NTLM.
* Download en installeer de meest recente SAP-clientbibliotheek, dat zich momenteel [SAP-Connector (NCo) 3.0.21.0 voor Microsoft .NET Framework 4.0 en Windows 64-bits (x64)](https://softwaredownloads.sap.com/file/0020000001865512018), op dezelfde computer als de on-premises gegevensgateway. Deze versie installeren of hoger om volgende redenen:

  * Eerdere versies van de SAP-NCo mogelijk in een impasse worden geraakt wanneer meer dan één IDoc-bericht is verzonden op hetzelfde moment. Dit probleem blokkeert alle latere berichten die worden verzonden naar de bestemming SAP, waardoor de berichten time-out.
  * De on-premises gegevensgateway wordt uitgevoerd alleen op 64-bits systemen. Anders treedt er een fout 'Ongeldige installatiekopie' omdat de data gateway host-service biedt geen ondersteuning voor 32-bits-assembly's.
  * Zowel de data gateway host-service en de SAP-Adapter van Microsoft .NET Framework 4.5 gebruiken De SAP-NCo voor .NET Framework 4.0 werkt met processen die gebruikmaken van de .NET runtime 4.0-4.7.1. De SAP-NCo voor .NET Framework 2.0 werkt met processen die gebruikmaken van de .NET runtime 2.0 naar 3.5, maar niet meer werkt met de meest recente on-premises gegevensgateway.

* Inhoud van het bericht die u naar uw SAP-server, zoals een voorbeeldbestand IDoc verzenden kunt moet zich in XML-indeling en vermeld de naamruimte voor de SAP-actie die u wilt gebruiken.

<a name="add-trigger"></a>

## <a name="send-to-sap"></a>Verzenden naar SAP

Dit voorbeeld wordt een logische app die u kunt activeren met een HTTP-aanvraag. De logische app een IDoc verzendt naar een SAP-server en retourneert een antwoord naar de aanvrager dat de logische app genoemd. 

### <a name="add-an-http-request-trigger"></a>Een trigger HTTP-aanvraag toevoegen

In Azure Logic Apps, elke logische app moet beginnen met een [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts), die wordt geactiveerd wanneer een bepaalde gebeurtenis wordt uitgevoerd of wanneer een bepaalde voorwaarde wordt voldaan. Telkens wanneer de trigger wordt geactiveerd, de Logic Apps-engine een exemplaar van de logische app maakt en werkstroom van uw app wordt gestart.

In dit voorbeeld u een logische app maken met een eindpunt in Azure zodat u kunt verzenden *HTTP POST-aanvragen* aan uw logische app. Wanneer uw logische app deze HTTP-aanvragen ontvangt, wordt de trigger wordt geactiveerd en wordt uitgevoerd de volgende stap in uw werkstroom.

1. In de [Azure-portal](https://portal.azure.com), maak een lege logische app, die de ontwerper van logische App wordt geopend.

1. Typ 'http-aanvraag' als filter in het zoekvak. Uit de **Triggers** in de lijst met **wanneer een HTTP-aanvraag wordt ontvangen**.

   ![HTTP-aanvraag trigger toevoegen](./media/logic-apps-using-sap-connector/add-trigger.png)

1. Nu uw logische app opslaan zodat u een eindpunt-URL voor uw logische app kunt genereren. Selecteer op de werkbalk van de ontwerper **opslaan**.

   Het eindpunt van de URL wordt nu weergegeven in de trigger, bijvoorbeeld:

   ![URL genereren voor het eindpunt](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

<a name="add-action"></a>

### <a name="add-an-sap-action"></a>Een SAP-actie toevoegen

In Azure Logic Apps, een [actie](../logic-apps/logic-apps-overview.md#logic-app-concepts) is een stap in uw werkstroom die volgt op een trigger of een andere actie. Als u dit nog niet hebt toegevoegd een trigger aan uw logische app nog en voer dit voorbeeld wilt [toevoegen van de trigger die in deze sectie beschreven](#add-trigger).

1. Selecteer in de Logic App Designer onder de trigger **nieuwe stap**.

   ![Selecteer 'Nieuwe stap'](./media/logic-apps-using-sap-connector/add-action.png)

1. Typ 'sap' als filter in het zoekvak. Uit de **acties** in de lijst met **bericht verzenden naar SAP**.
  
   ![Selecteer de actie van de SAP-verzenden](media/logic-apps-using-sap-connector/select-sap-send-action.png)

   Of Kies in plaats van te zoeken, de **Enterprise** tabblad, en selecteer de SAP-actie.

   ![Selecteer SAP verzendactie in Enterprise-tabblad](media/logic-apps-using-sap-connector/select-sap-send-action-ent-tab.png)

1. Als u wordt gevraagd of u voor de verbindingsgegevens, maakt u nu uw SAP-verbinding. Anders als de verbinding al bestaat, gaat u door met de volgende stap zodat u uw SAP-actie kunt instellen.

   **Maken van een on-premises SAP-verbinding**

    1. De verbindingsgegevens voor uw SAP-server opgeven. Voor de **gegevensgateway** eigenschap, selecteert u de data gateway die u hebt gemaakt in de Azure-portal voor uw gatewayinstallatie.

         - Als de **aanmeldingstype** eigenschap is ingesteld op **toepassingsserver**, deze eigenschappen, die doorgaans optioneel verschijnen, zijn vereist:

            ![Serververbinding voor SAP-toepassing maken](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

         - Als de **aanmeldingstype** eigenschap is ingesteld op **groep**, deze eigenschappen, die doorgaans optioneel verschijnen, zijn vereist:

            ![SAP bericht serververbinding maken](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)

           Standaard wordt sterke typering die gebruikt om te controleren op ongeldige waarden door te voeren van XML-validatie op basis van het schema. Dit gedrag kunt u problemen eerder detecteren. De **veilige typen** optie is beschikbaar voor achterwaartse compatibiliteit en de lengte van de tekenreeks wordt alleen gecontroleerd. Meer informatie over de [veilige typen optie](#safe-typing).

    1. Wanneer u klaar bent, selecteert u **maken**.

       Logic Apps wordt ingesteld en test de verbinding om ervoor te zorgen dat de verbinding naar behoren werkt.

1. Nu zoeken en selecteer een actie van uw SAP-server.

    1. In de **SAP actie** vak, selecteer het pictogram van de map. In de lijst, zoek en selecteer het SAP-bericht dat u wilt gebruiken. Ga in de lijst, gebruikt u de pijlen.

       In dit voorbeeld wordt een IDoc met de **Orders** type.

       ![Zoek en selecteer de actie IDoc](./media/logic-apps-using-sap-connector/SAP-app-server-find-action.png)

       Als u niet de actie die u wilt vinden, kunt u een pad, bijvoorbeeld handmatig invoeren:

       ![Pad naar IDoc actie handmatig opgeven](./media/logic-apps-using-sap-connector/SAP-app-server-manually-enter-action.png)

       > [!TIP]
       > Geef de waarde voor **SAP actie** via de expressie-editor. Op die manier kunt u dezelfde actie voor verschillende typen.

       Zie voor meer informatie over de werking van IDoc [schema's voor IDOC operations bericht](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

    1. Klik in de **invoerbericht** vak zodat de lijst met dynamische inhoud wordt weergegeven. In die lijst onder **wanneer een HTTP-aanvraag wordt ontvangen**, selecteer de **hoofdtekst** veld.

       Deze stap omvat de inhoud van de hoofdtekst van de trigger HTTP-aanvraag en stuurt die uitvoer naar uw SAP-server.

       ![Veld 'Hoofdtekst' selecteren](./media/logic-apps-using-sap-connector/SAP-app-server-action-select-body.png)

       Wanneer u klaar bent, ziet uw SAP-actie eruit zoals in dit voorbeeld:

       ![Complete SAP-actie](./media/logic-apps-using-sap-connector/SAP-app-server-complete-action.png)

1. Sla uw logische app op. Selecteer op de werkbalk van de ontwerper **opslaan**.

<a name="add-response"></a>

### <a name="add-an-http-response-action"></a>Een HTTP-antwoord actie toevoegen

Nu een reactie toevoegen aan uw logische app-werkstroom en de uitvoer van de SAP-actie bevatten. Op die manier kunnen uw logische app retourneert de resultaten van uw SAP-server naar de oorspronkelijke aanvrager.

1. Selecteer in de Logic App Designer onder de actie SAP **nieuwe stap**.

1. Typ 'response' als filter in het zoekvak. Uit de **acties** in de lijst met **antwoord**.

1. Klik in de **hoofdtekst** vak zodat de lijst met dynamische inhoud wordt weergegeven. In die lijst onder **bericht verzenden naar SAP**, selecteer de **hoofdtekst** veld.

   ![Complete SAP-actie](./media/logic-apps-using-sap-connector/select-sap-body-for-response-action.png)

1. Sla uw logische app op.

### <a name="test-your-logic-app"></a>Uw logische app testen

1. Als uw logische app nog niet ingeschakeld, klikt u op het menu van uw logische app, selecteert u **overzicht**. Selecteer op de werkbalk **inschakelen**.

1. Selecteer op de werkbalk van de ontwerper **uitvoeren**. Deze stap kunt u handmatig uw logische app start.

1. Uw logische app te verzenden van een HTTP POST-aanvraag naar de URL op uw HTTP-aanvraagtrigger activeren.
Uw inhoud met uw aanvraag bericht opnemen. Voor het verzenden van de aanvraag, kunt u een hulpprogramma zoals [Postman](https://www.getpostman.com/apps).

   In dit artikel stuurt de aanvraag een IDoc-bestand dat moet worden in XML-indeling en vermeld de naamruimte voor de SAP-actie die u gebruikt, bijvoorbeeld:

   ``` xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <Send xmlns="http://Microsoft.LobServices.Sap/2007/03/Idoc/2/ORDERS05//720/Send">
      <idocData>
         <...>
      </idocData>
   </Send>
   ```

1. Nadat u de HTTP-aanvraag verzendt, wacht u totdat de reactie van uw logische app.

   > [!NOTE]
   > Uw logische app time-out mogelijk als alle stappen die nodig zijn voor het antwoord is niet voltooid binnen de [aanvraag time-outlimiet](./logic-apps-limits-and-config.md). Als dit probleem gebeurt, aanvragen mogelijk geblokkeerd. Informatie over hoe u kunt voor hulp bij het analyseren van problemen, [controleren en bewaken van uw logische apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).

U hebt nu een logische app gemaakt die kan communiceren met uw SAP-server. Nu dat u een SAP-verbinding voor uw logische app instellen hebt, kunt u andere beschikbare SAP-acties, zoals BAPI en RFC kunt verkennen.

## <a name="receive-from-sap"></a>Ontvangen van SAP

Dit voorbeeld wordt een logische app die wordt geactiveerd wanneer de app een bericht van een SAP-systeem ontvangt.

### <a name="add-an-sap-trigger"></a>Een SAP-trigger toevoegen

1. Maak een lege logische app, die de ontwerper van logische App wordt geopend in de Azure-portal.

1. Typ 'sap' als filter in het zoekvak. Uit de **Triggers** in de lijst met **wanneer een bericht wordt ontvangen van SAP**.

   ![SAP-trigger toevoegen](./media/logic-apps-using-sap-connector/add-sap-trigger.png)

   Of, gaat u naar de **Enterprise** tabblad, en selecteer de trigger:

   ![SAP-trigger toevoegen van Enterprise-tabblad](./media/logic-apps-using-sap-connector/add-sap-trigger-ent-tab.png)

1. Als u wordt gevraagd of u voor de verbindingsgegevens, maakt u nu uw SAP-verbinding. Als de verbinding al bestaat, gaat u verder met de volgende stap, zodat u uw SAP-actie kunt instellen.

   **Maken van een on-premises SAP-verbinding**

   - De verbindingsgegevens voor uw SAP-server opgeven. Voor de **gegevensgateway** eigenschap, selecteert u de data gateway die u hebt gemaakt in de Azure-portal voor uw gatewayinstallatie.

      - Als de **aanmeldingstype** eigenschap is ingesteld op **toepassingsserver**, deze eigenschappen, die doorgaans optioneel verschijnen, zijn vereist:

         ![Serververbinding voor SAP-toepassing maken](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      - Als de **aanmeldingstype** eigenschap is ingesteld op **groep**, deze eigenschappen, die doorgaans optioneel verschijnen, zijn vereist:

          ![SAP bericht serververbinding maken](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)  

      Standaard wordt sterke typering die gebruikt om te controleren op ongeldige waarden door te voeren van XML-validatie op basis van het schema. Dit gedrag kunt u problemen eerder detecteren. De **veilige typen** optie is beschikbaar voor achterwaartse compatibiliteit en de lengte van de tekenreeks wordt alleen gecontroleerd. Meer informatie over de [veilige typen optie](#safe-typing).

1. Geef de vereiste parameters op basis van de configuratie van uw SAP-systeem.

   U kunt desgewenst een of meer SAP-acties. Deze lijst met acties Hiermee geeft u de berichten die de trigger van uw SAP-server via de gegevensgateway ontvangt. Een lege lijst geeft aan dat alle berichten worden ontvangen door de trigger. Als de lijst meer dan één bericht heeft, ontvangt de trigger alleen de berichten die zijn opgegeven in de lijst. Andere berichten van uw SAP-server worden geweigerd door de gateway.

   U kunt een SAP-actie selecteren uit de bestandenkiezer:

   ![SAP-actie selecteren](media/logic-apps-using-sap-connector/select-SAP-action-trigger.png)  

   Of u kunt handmatig een actie opgeven:

   ![Voer handmatig de SAP-actie](media/logic-apps-using-sap-connector/manual-enter-SAP-action-trigger.png) 

   Hier volgt een voorbeeld waarin wordt weergegeven hoe de actie wordt weergegeven bij het instellen van de trigger om meer dan één bericht te ontvangen.

   ![Voorbeeld van de trigger](media/logic-apps-using-sap-connector/example-trigger.png)  

   Zie voor meer informatie over de SAP-actie [bericht schema's voor IDOC-bewerkingen](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations)

1. Nu uw logische app opslaan zodat u kunt beginnen met het ontvangen van berichten van uw SAP-systeem.
Selecteer op de werkbalk van de ontwerper **opslaan**.

Uw logische app is nu klaar om berichten te ontvangen van uw SAP-systeem.

> [!NOTE]
> De SAP-trigger is een polling-trigger, maar is een op basis van een webhook-trigger in plaats daarvan. De trigger wordt aangeroepen vanuit de gateway alleen als er een bericht bestaat, zodat er geen polling nodig is.

### <a name="test-your-logic-app"></a>Uw logische app testen

1. Voor het activeren van uw logische app, moet u een bericht verzenden vanuit uw SAP-systeem.

1. Selecteer op het menu van de logische app, **overzicht**. Controleer de **geschiedenis van uitvoeringen** voor elke nieuwe uitgevoerd voor uw logische app.

1. Open de meest recente uitvoering, waarin het bericht verzonden van uw SAP-systeem in de sectie van de trigger-uitvoer.

## <a name="generate-schemas-for-artifacts-in-sap"></a>Schema's voor artefacten in SAP genereren

Dit voorbeeld wordt een logische app die u kunt activeren met een HTTP-aanvraag. De SAP-actie verzendt een aanvraag met een SAP-systeem voor het genereren van de schema's voor het opgegeven IDoc en BAPI. Schema's die in het antwoord retourneren worden geüpload naar een integratieaccount met de Azure Resource Manager-connector.

### <a name="add-an-http-request-trigger"></a>Een trigger HTTP-aanvraag toevoegen

1. Maak een lege logische app, die de ontwerper van logische App wordt geopend in de Azure-portal.

1. Typ 'http-aanvraag' als filter in het zoekvak. Uit de **Triggers** in de lijst met **wanneer een HTTP-aanvraag wordt ontvangen**.

   ![HTTP-aanvraag trigger toevoegen](./media/logic-apps-using-sap-connector/add-trigger.png)

1. Nu uw logische app opslaan, zodat u een eindpunt-URL voor uw logische app kunt genereren.
Selecteer op de werkbalk van de ontwerper **opslaan**.

   Het eindpunt van de URL wordt nu weergegeven in de trigger, bijvoorbeeld:

   ![URL genereren voor het eindpunt](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

### <a name="add-an-sap-action-to-generate-schemas"></a>Een SAP-actie voor het genereren van schema's toevoegen

1. Selecteer in de Logic App Designer onder de trigger **nieuwe stap**.

   ![Selecteer 'Nieuwe stap'](./media/logic-apps-using-sap-connector/add-action.png)

1. Typ 'sap' als filter in het zoekvak. Uit de **acties** in de lijst met **schema's genereren**.
  
   ![Selecteer de actie van de SAP-verzenden](media/logic-apps-using-sap-connector/select-sap-schema-generator-action.png)

   Of u kunt ook kiezen de **Enterprise** tabblad, en selecteer de SAP-actie.

   ![Selecteer SAP verzendactie in Enterprise-tabblad](media/logic-apps-using-sap-connector/select-sap-schema-generator-ent-tab.png)

1. Als u wordt gevraagd of u voor de verbindingsgegevens, maakt u nu uw SAP-verbinding. Als de verbinding al bestaat, gaat u verder met de volgende stap, zodat u uw SAP-actie kunt instellen.

   **Maken van een on-premises SAP-verbinding**

    1. De verbindingsgegevens voor uw SAP-server opgeven. Voor de **gegevensgateway** eigenschap, selecteert u de data gateway die u hebt gemaakt in de Azure-portal voor uw gatewayinstallatie.

       - Als de **aanmeldingstype** eigenschap is ingesteld op **toepassingsserver**, deze eigenschappen, die doorgaans optioneel verschijnen, zijn vereist:

         ![Serververbinding voor SAP-toepassing maken](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

       - Als de **aanmeldingstype** eigenschap is ingesteld op **groep**, deze eigenschappen, die doorgaans optioneel verschijnen, zijn vereist:

         ![SAP bericht serververbinding maken](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)

        Standaard wordt sterke typering die gebruikt om te controleren op ongeldige waarden door te voeren van XML-validatie op basis van het schema. Dit gedrag kunt u problemen eerder detecteren. De **veilige typen** optie is beschikbaar voor achterwaartse compatibiliteit en de lengte van de tekenreeks wordt alleen gecontroleerd. Meer informatie over de [veilige typen optie](#safe-typing).

    1. Wanneer u klaar bent, selecteert u **maken**. 
   
       Logic Apps wordt ingesteld en test de verbinding om ervoor te zorgen dat de verbinding naar behoren werkt.

1. Het pad naar het artefact waarvan u wilt voor het genereren van het schema opgeven.

   U kunt de SAP-actie selecteren uit de bestandenkiezer:

   ![SAP-actie selecteren](media/logic-apps-using-sap-connector/select-SAP-action-schema-generator.png)  

   Of u kunt de actie handmatig invoeren:

   ![Voer handmatig de SAP-actie](media/logic-apps-using-sap-connector/manual-enter-SAP-action-schema-generator.png)

   Voor het genereren van schema's voor meer dan één artefact, bieden u de details van de SAP-actie voor elke artefact, bijvoorbeeld:

   ![Selecteer Nieuw item toevoegen](media/logic-apps-using-sap-connector/schema-generator-array-pick.png)

   ![Twee items weergeven](media/logic-apps-using-sap-connector/schema-generator-example.png)

   Zie voor meer informatie over de SAP-actie [schema's voor IDOC operations bericht](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

1. Sla uw logische app op. Selecteer op de werkbalk van de ontwerper **opslaan**.

### <a name="test-your-logic-app"></a>Uw logische app testen

1. Selecteer op de werkbalk van de ontwerper **uitvoeren** voor het activeren van een uitvoering voor uw logische app.

1. Open de uitvoering en of de uitvoer voor de **schema's genereren** actie.

   De uitvoer ziet de gegenereerde schema's voor de opgegeven lijst met berichten.

### <a name="upload-schemas-to-an-integration-account"></a>Schema's uploaden naar een integratieaccount

Desgewenst kunt u downloaden of de gegenereerde schema's opslaan in opslagplaatsen, zoals een blob-, opslag- of integratie-account. Integratieaccounts bieden een eersteklas ervaring met andere acties XML, zodat dit voorbeeld het schema's uploaden naar een integratieaccount voor dezelfde logische app wordt met behulp van de Azure Resource Manager-connector.

1. Selecteer in de Logic App Designer onder de trigger **nieuwe stap**.

1. Typ 'Resource Manager' als filter in het zoekvak. Selecteer **maken of bijwerken van een resource**.

   ![Azure Resource Manager-actie selecteren](media/logic-apps-using-sap-connector/select-azure-resource-manager-action.png)

1. Geef de details voor de actie, met inbegrip van uw Azure-abonnement, de Azure-resourcegroep en de integratie-account. SAP-tokens toevoegen aan de velden, klikt u in de vakken voor deze velden en selecteer in de lijst met dynamische inhoud die wordt weergegeven.

    1. Open de **toevoegen van nieuwe parameter** lijst en selecteer de **locatie** en **eigenschappen** velden.

    1. Geef details op voor deze nieuwe velden zoals wordt weergegeven in dit voorbeeld.

       ![Geef details op voor de Azure Resource Manager-actie](media/logic-apps-using-sap-connector/azure-resource-manager-action.png)

   De SAP **schema's genereren** genereert u schema's als een verzameling, zodat de ontwerper automatisch toegevoegd een **voor elk** lus met de actie. Hier volgt een voorbeeld waarin wordt weergegeven hoe deze actie wordt weergegeven:

   ![Azure Resource Manager-actie met 'voor elke' lus](media/logic-apps-using-sap-connector/azure-resource-manager-action-foreach.png)  

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

1. Sla uw logische app op. Selecteer op de werkbalk van de ontwerper **opslaan**.

### <a name="test-your-logic-app"></a>Uw logische app testen

1. Selecteer op de werkbalk van de ontwerper **uitvoeren** aan uw logische app handmatig te activeren.

1. Na een geslaagde uitvoeren, gaat u naar het integratieaccount en controleer of de gegenereerde schema's bestaan.

## <a name="enable-secure-network-communications"></a>Beveiligde netwerkcommunicatie inschakelen

Voordat u begint, moet u ervoor zorgen dat u de eerder vermelde voldaan [vereisten](#pre-reqs):

* De on-premises gegevensgateway is geïnstalleerd op een computer die zich in hetzelfde netwerk bevinden als uw SAP-systeem.
* Voor eenmalige aanmelding, wordt de gateway uitgevoerd als een gebruiker die toegewezen aan een SAP-gebruiker.
* De SNC-bibliotheek met de extra beveiligingsfuncties is op dezelfde computer als de data gateway geïnstalleerd. Enkele voorbeelden zijn [sapseculib](https://help.sap.com/saphelp_nw74/helpdata/en/7a/0755dc6ef84f76890a77ad6eb13b13/frameset.htm), Kerberos en NTLM.

Als u SNC voor uw aanvragen naar of van de SAP-systeem, schakelt de **gebruik SNC** selectievakje in de SAP-verbinding en geef deze eigenschappen:

   ![SAP SNC in verbinding configureren](media/logic-apps-using-sap-connector/configure-sapsnc.png)

   | Eigenschap | Description |
   |----------| ------------|
   | **Pad naar de SNC-bibliotheek** | De naam van SNC-bibliotheek of pad ten opzichte van de installatielocatie NCo of absoluut pad zijn. Voorbeelden zijn `sapsnc.dll` of `.\security\sapsnc.dll` of `c:\security\sapsnc.dll`. |
   | **SNC EENMALIGE AANMELDING** | Wanneer u verbinding via SNC maakt, wordt de identiteit van de SNC wordt doorgaans gebruikt voor het verifiëren van de oproepende functie. Een andere optie is om op te heffen, zodat de gebruiker en het wachtwoord kan worden gebruikt voor het verifiëren van de oproepende functie, maar de regel is nog steeds versleuteld. |
   | **SNC mijn naam** | In de meeste gevallen kan deze eigenschap worden weggelaten. De geïnstalleerde SNC-oplossing kent doorgaans een eigen SNC-naam. Alleen voor oplossingen die ondersteuning bieden voor meerdere identiteiten, moet u mogelijk de identiteit moet worden gebruikt voor deze specifieke bestemming of de server opgeven. |
   | **Naam van SNC-Partner** | De naam voor de SNC back-end. |
   | **SNC-kwaliteit van beveiliging** | De quality of service moet worden gebruikt voor de SNC-communicatie van deze specifieke bestemming of de server. De standaardwaarde is gedefinieerd door het systeem back-end. De maximale waarde wordt gedefinieerd door de security-product voor SNC gebruikt. |
   |||

   > [!NOTE]
   > Stelt niet de omgevingsvariabelen SNC_LIB en SNC_LIB_64 op de computer waar u de data gateway en de SNC-bibliotheek hebt. Als is ingesteld, ze voorrang op de SNC-bibliotheek-waarde die wordt doorgegeven via de connector hebben.

<a name="safe-typing"></a>

## <a name="safe-typing"></a>Veilige typen

Standaard wordt sterke typering die bij het maken van uw SAP-verbinding gebruikt om te controleren op ongeldige waarden door te voeren van XML-validatie op basis van het schema. Dit gedrag kunt u problemen eerder detecteren. De **veilige typen** optie is beschikbaar voor achterwaartse compatibiliteit en de lengte van de tekenreeks wordt alleen gecontroleerd. Als u ervoor kiest **veilige typen**, het type DATS en TIM voert SAP worden behandeld als tekenreeksen in plaats van de XML-equivalenten `xs:date` en `xs:time`, waarbij `xmlns:xs="http://www.w3.org/2001/XMLSchema"`. Veilige typen is van invloed op het gedrag voor het genereren van alle schema, het verzonden bericht voor de nettolading van de 'is verzonden' en het antwoord 'ontvangen', en de trigger. 

Wanneer de sterke typering die wordt gebruikt (**veilige typen** is niet ingeschakeld), het schema wordt de typen DATS en TIM toegewezen aan twee typen XML-typen:

```xml
<xs:element minOccurs="0" maxOccurs="1" name="UPDDAT" nillable="true" type="xs:date"/>
<xs:element minOccurs="0" maxOccurs="1" name="UPDTIM" nillable="true" type="xs:time"/>
```

Wanneer u bij het verzenden van berichten met sterke typering die wordt het antwoord DATS en TIM voldoet aan de overeenkomstige XML-type-indeling:

```xml
<DATE>9999-12-31</DATE>
<TIME>23:59:59</TIME>
```

Wanneer **veilige typen** is ingeschakeld, wordt het schema toegewezen de DATS en TIM typen naar XML string velden met alleen de beperkingen voor de lengte, bijvoorbeeld:

```xml
<xs:element minOccurs="0" maxOccurs="1" name="UPDDAT" nillable="true">
  <xs:simpleType>
    <xs:restriction base="xs:string">
      <xs:maxLength value="8" />
    </xs:restriction>
  </xs:simpleType>
</xs:element>
<xs:element minOccurs="0" maxOccurs="1" name="UPDTIM" nillable="true">
  <xs:simpleType>
    <xs:restriction base="xs:string">
      <xs:maxLength value="6" />
    </xs:restriction>
  </xs:simpleType>
</xs:element>
```

Wanneer berichten worden verzonden met **veilige typen** ingeschakeld, het antwoord DATS en TIM ziet eruit zoals in dit voorbeeld:

```xml
<DATE>99991231</DATE>
<TIME>235959</TIME>
```


## <a name="known-issues-and-limitations"></a>Bekende problemen en beperkingen

Hier worden de bekende problemen en beperkingen voor de SAP-connector:

* Alleen een enkel verzenden naar SAP-oproep of een bericht werkt met tRFC. Het patroon BAPI doorvoeren, zoals meerdere tRFC aanroepen in dezelfde sessie, wordt niet ondersteund.
* Ontvangende batch idoc's van SAP biedt geen ondersteuning voor de SAP-trigger. Deze actie kan leiden tot fout voor RFC-verbinding tussen uw SAP-systeem en de data gateway.
* De trigger SAP biedt geen ondersteuning voor gatewayclusters met gegevens. In sommige gevallen failover kan op het gegevensknooppunt voor gateway die met de SAP-systeem communiceert verschillen van het actieve knooppunt, wat tot onverwacht gedrag leidt. Voor scenario's verzenden, worden data gatewayclusters ondersteund.
* De SAP-connector op dit moment biedt geen ondersteuning voor SAP-router tekenreeksen. De on-premises gegevensgateway moet bestaan in hetzelfde LAN bevinden als de SAP-systeem dat u verbinding wilt maken.

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over triggers en acties limieten die worden beschreven van de connector openapi (voorheen Swagger) beschrijving, Controleer de [van de connector-verwijzingspagina](/connectors/sap/).

## <a name="next-steps"></a>Volgende stappen

* [Verbinding maken met on-premises systemen](../logic-apps/logic-apps-gateway-connection.md) van Azure Logic Apps.
* Meer informatie over het valideren, transformeren en gebruiken van andere berichtbewerkingen met de [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).
* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md).
