---
title: Herstel na noodgevallen voor integratieaccounts voor B2B - Azure Logic Apps | Microsoft Docs
description: Voorbereidingen voor herstel na noodgevallen regio-overschrijdende in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.date: 04/10/2017
ms.openlocfilehash: 3d465123f814887282bf2b29a5b6e0836601c243
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43123901"
---
# <a name="cross-region-disaster-recovery-for-b2b-integration-accounts-in-azure-logic-apps"></a>Regio-overschrijdende noodherstel voor B2B-integratie-accounts in Azure Logic Apps

B2B-werkbelastingen hebben betrekking op geld-transacties, zoals de orders en facturen. Tijdens een noodgebeurtenis, het is essentieel voor bedrijven te snel herstellen om te voldoen aan de zakelijke Sla's overeengekomen met hun partners. In dit artikel wordt gedemonstreerd hoe u een plan voor bedrijfscontinuïteit voor B2B-werkbelastingen. 

* Gereedheid voor herstel na noodgevallen 
* Failover naar een secundaire regio tijdens een noodgebeurtenis 
* Terugvallen op de primaire regio na een ramp-gebeurtenis

## <a name="disaster-recovery-readiness"></a>Gereedheid voor herstel na noodgevallen  

1. Een secundaire regio identificeert en maakt u een [integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) in de secundaire regio.

2. Partners, schema's, en overeenkomsten voor de vereiste stromen waar de uitvoeringsstatus moet worden gerepliceerd naar de secundaire regio integratie-account toevoegen.

   > [!TIP]
   > Zorg ervoor dat de integratie-account-artefact naamconventie is consistentie tussen regio's. 

3. Om op te halen de uitvoeringsstatus van de primaire regio, moet u een logische app maken in de secundaire regio. 

   Deze logische app moet een *trigger* en een *actie*. 
   De trigger moet verbinding maken met de primaire regio integratie-account en de actie moet verbinding maken met secundaire regio van het integratieaccount. 
   De trigger op basis van het tijdsinterval, pollt de primaire regio statustabel uitvoeren en haalt de nieuwe records, indien van toepassing. De actie bijgewerkt naar de secundaire regio van het integratieaccount. 
   Dit helpt om op te halen van incrementele runtimestatus van de primaire regio naar de secundaire regio.

4. Zakelijke continuïteit in het integratieaccount voor logische Apps is ontworpen ter ondersteuning van op basis van B2B-protocollen - X12, AS2 en EDIFACT. Selecteer de respectieve koppelingen voor gedetailleerde stappen vindt.

5. U wordt aangeraden alle primaire regio resources in een secundaire regio te implementeren. 

   Primaire regio resources omvatten Azure SQL Database of Azure Cosmos DB, Azure Service Bus en Azure Event Hubs gebruikt voor berichten, Azure API Management en de Azure Logic Apps-functie in Azure App Service.   

6. Stel een verbinding van een primaire regio naar een secundaire regio. Als u wilt de status van de uitvoerbewerking ophalen uit een primaire regio, moet u een logische app maken in een secundaire regio. 

   De logische app moet een trigger en een actie hebben. 
   De trigger moet verbinding maken met een primaire regio van het integratieaccount. 
   De actie moet verbinding maken met een secundaire regio van het integratieaccount. 
   De trigger op basis van het tijdsinterval, pollt de primaire regio statustabel uitvoeren en haalt de nieuwe records, indien van toepassing. 
   De actie naar een secundaire regio integratie-account bijgewerkt. 
   Dit proces zorgt incrementele runtimestatus ophalen van de primaire regio naar de secundaire regio.

Bedrijfscontinuïteit in een integratieaccount voor logische Apps biedt ondersteuning voor op basis van de B2B-protocollen X12, AS2 en EDIFACT. Zie voor gedetailleerde stappen over het gebruik van X12 en AS2 [X12](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#x12) en [AS2](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#as2) in dit artikel.

## <a name="fail-over-to-a-secondary-region-during-a-disaster-event"></a>Failover naar een secundaire regio tijdens een noodgebeurtenis

Tijdens een noodgebeurtenis, wanneer de primaire regio niet beschikbaar voor bedrijfscontinuïteit, verkeer naar de secundaire regio. Een secundaire regio helpt een zakelijke functies snel om te voldoen aan de RPO/RTO herstellen overeengekomen door hun partners. Het minimaliseert ook inspanningen failover van de ene regio naar een andere regio. 

Er is een verwachte latentie tijdens het kopiëren van controlenummers van een primaire regio naar een secundaire regio. Om te voorkomen tijdens een noodgebeurtenis dubbele gegenereerde controlenummers naar partners verzonden, wordt aangeraden de controlenummers in de secundaire regio overeenkomsten verhogen met behulp van [PowerShell-cmdlets](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).

## <a name="fall-back-to-a-primary-region-post-disaster-event"></a>Terugvallen op een primaire regio na noodgebeurtenis

Als u wilt terugvallen op een primaire regio als deze beschikbaar is, de volgende stappen uit:

1. Accepteert geen berichten van partners in de secundaire regio.  

2. De gegenereerde controlenummers voor alle overeenkomsten in de primaire regio verhogen met behulp van [PowerShell-cmdlets](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).  

3. Direct verkeer van de secundaire regio naar de primaire regio.

4. Controleer of de logische app gemaakt in de secundaire regio voor het ophalen van de uitvoeringsstatus van de primaire regio is ingeschakeld.

## <a name="x12"></a>X12 

Bedrijfscontinuïteit voor EDI, X 12 documenten is gebaseerd op controlenummers:

> [!TIP]
> U kunt ook de [X12 snel starten-sjabloon](https://azure.microsoft.com/resources/templates/201-logic-app-b2b-disaster-recovery-replication/) om logische apps te maken. Het maken van het primaire en secundaire integratieaccounts zijn vereisten voor het gebruik van de sjabloon. De sjabloon helpt bij het maken van twee logic apps, één voor de controlenummers die ontvangen en een andere voor gegenereerde controlenummers. Bijbehorende triggers en acties worden gemaakt in de logic apps kunt verbinden met de trigger de primaire integratie-account en de actie naar het secundaire integratieaccount.

**Vereisten**

Als herstel na noodgevallen voor inkomende berichten, schakelt u de instellingen voor controle op dubbele items in de X12 instellingen voor het ontvangen van de overeenkomst.

![Instellingen voor controle op dubbele items selecteren](./media/logic-apps-enterprise-integration-b2b-business-continuity/dupcheck.png)  

1. Maak een [logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md) in een secundaire regio.    

2. Zoeken op **X12**, en selecteer **X12-wanneer een controlenummer is gewijzigd**.   

   ![Zoeken naar X12](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn1.png)

   De trigger vraagt u om een verbinding met een integratieaccount. 
   De trigger moet worden verbonden met een primaire regio van het integratieaccount.

3. Voer de naam van een verbinding, selecteert u uw *primaire regio van het integratieaccount* in de lijst en kies **maken**.   

   ![Primaire regio integratieaccountnaam](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn2.png)

4. De **datum/tijd te synchroniseren van het controlenummer** instelling is optioneel. De **frequentie** kan worden ingesteld op **dag**, **uur**, **minuut**, of **tweede** met een interval.   

   ![Datum/tijd en de frequentie](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

5. Selecteer **nieuwe stap** > **een actie toevoegen**.

   ![Nieuwe stap, een actie toevoegen](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

6. Zoeken op **X12**, en selecteer **X12-toevoegen of bijwerken van de controlenummers**.   

   ![Controlenummers toevoegen of bijwerken](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn5.png)

7. Selecteer voor een actie verbinding met een secundaire regio integratie-account, **verbinding wijzigen** > **nieuwe verbinding toevoegen** voor een lijst van de beschikbare integratieaccounts. Voer de naam van een verbinding, selecteert u uw *secundaire regio van het integratieaccount* in de lijst en kies **maken**. 

   ![Naam van het integratieaccount secundaire regio](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

8. Schakel over naar onbewerkte invoer door te klikken op het pictogram in de rechterbovenhoek.

   ![Schakel over naar onbewerkte invoer](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12rawinputs.png)

9. Selecteer hoofdtekst van de kiezer voor dynamische inhoud en sla de logische app.

   ![Dynamische inhoud velden](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn7.png)

   Op basis van het interval, de trigger de primaire regio ontvangen besturingselement nummer tabel worden opgevraagd en haalt de nieuwe records. 
   De actie werkt u de records in het integratieaccount van de secundaire regio. 
   Als er geen updates beschikbaar zijn, de Triggerstatus van de wordt weergegeven als **overgeslagen**.   

   ![Aantal controle-tabel](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

Op basis van het interval, de incrementele runtimestatus worden gerepliceerd van een primaire regio naar een secundaire regio. Tijdens een noodgebeurtenis, wanneer de primaire regio niet beschikbaar, direct verkeer naar de secundaire regio voor bedrijfscontinuïteit. 

## <a name="edifact"></a>EDIFACT 

Bedrijfscontinuïteit voor EDI EDIFACT-documenten zijn gebaseerd op controlenummers.

**Vereisten**

Als herstel na noodgevallen voor inkomende berichten, schakelt u de instellingen voor controle op dubbele items in de instellingen voor het ontvangen van de EDIFACT-overeenkomst.

![Instellingen voor controle op dubbele items selecteren](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactdupcheck.png)  

1. Maak een [logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md) in een secundaire regio.    

2. Zoeken op **EDIFACT**, en selecteer **EDIFACT - wanneer een controlenummer is gewijzigd**.

   ![Zoeken naar EDIFACT](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactcn1.png)

   De trigger vraagt u om een verbinding met een integratieaccount. 
   De trigger moet worden verbonden met een primaire regio van het integratieaccount. 

3. Voer de naam van een verbinding, selecteert u uw *primaire regio van het integratieaccount* in de lijst en kies **maken**.    

   ![Primaire regio integratieaccountnaam](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN2.png)

4. De **datum/tijd te synchroniseren van het controlenummer** instelling is optioneel. De **frequentie** kan worden ingesteld op **dag**, **uur**, **minuut**, of **tweede** met een interval.    

   ![Datum/tijd en de frequentie](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

6. Selecteer **nieuwe stap** > **een actie toevoegen**.    

   ![Nieuwe stap, een actie toevoegen](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

7. Zoeken op **EDIFACT**, en selecteer **EDIFACT - toevoegen of bijwerken van de controlenummers**.   

   ![Controlenummers toevoegen of bijwerken](./media/logic-apps-enterprise-integration-b2b-business-continuity/EdifactChooseAction.png)

8. Selecteer voor een actie verbinding met een secundaire regio integratie-account, **verbinding wijzigen** > **nieuwe verbinding toevoegen** voor een lijst van de beschikbare integratieaccounts. Voer de naam van een verbinding, selecteert u uw *secundaire regio van het integratieaccount* in de lijst en kies **maken**.

   ![Naam van het integratieaccount secundaire regio](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

9. Schakel over naar onbewerkte invoer door te klikken op het pictogram in de rechterbovenhoek.

   ![Schakel over naar onbewerkte invoer](./media/logic-apps-enterprise-integration-b2b-business-continuity/Edifactrawinputs.png)

10. Selecteer hoofdtekst van de kiezer voor dynamische inhoud en sla de logische app.   

   ![Dynamische inhoud velden](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN7.png)

   Op basis van het interval, de trigger de primaire regio ontvangen besturingselement nummer tabel worden opgevraagd en haalt de nieuwe records.
   De actie updates de records voor de secundaire regio van het integratieaccount. 
   Als er geen updates beschikbaar zijn, de Triggerstatus van de wordt weergegeven als **overgeslagen**.

   ![Aantal controle-tabel](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

Op basis van het interval, de incrementele runtimestatus worden gerepliceerd van een primaire regio naar een secundaire regio. Tijdens een noodgebeurtenis, wanneer de primaire regio niet beschikbaar, direct verkeer naar de secundaire regio voor bedrijfscontinuïteit. 

## <a name="as2"></a>AS2 

Bedrijfscontinuïteit voor documenten die gebruikmaken van de AS2-protocol is gebaseerd op de bericht-ID en de MIC-waarde.

> [!TIP]
> U kunt ook de [AS2 snel starten-sjabloon](https://github.com/Azure/azure-quickstart-templates/pull/3302) om logische apps te maken. Het maken van het primaire en secundaire integratieaccounts zijn vereisten voor het gebruik van de sjabloon. De sjabloon kunt een logische app met een trigger en een actie maken. De logische app maakt een verbinding van een trigger voor een primaire integratie-account en een actie naar een secundaire integratieaccount.

1. Maak een [logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md) in de secundaire regio.  

2. Zoeken op **AS2**, en selecteer **AS2 - wanneer een MIC-waarde wordt gemaakt**.   

   ![Zoeken naar AS2](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid1.png)

   Een trigger vraagt u om een verbinding met een integratieaccount. 
   De trigger moet worden verbonden met een primaire regio van het integratieaccount. 
   
3. Voer de naam van een verbinding, selecteert u uw *primaire regio van het integratieaccount* in de lijst en kies **maken**.

   ![Primaire regio integratieaccountnaam](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid2.png)

4. De **DateTime om te beginnen met synchroniseren van MIC-waarde** instelling is optioneel. De **frequentie** kan worden ingesteld op **dag**, **uur**, **minuut**, of **tweede** met een interval.   

   ![Datum/tijd en de frequentie](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid3.png)

5. Selecteer **nieuwe stap** > **een actie toevoegen**.  

   ![Nieuwe stap, een actie toevoegen](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid4.png)

6. Zoeken op **AS2**, en selecteer **AS2 - toevoegen of bijwerken van de MIC-inhoud**.  

   ![MIC toevoegen of bijwerken](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid5.png)

7. Selecteer voor een actie verbinding met een secundaire integratie-account, **verbinding wijzigen** > **nieuwe verbinding toevoegen** voor een lijst van de beschikbare integratieaccounts. Voer de naam van een verbinding, selecteert u uw *secundaire regio van het integratieaccount* in de lijst en kies **maken**.

   ![Naam van het integratieaccount secundaire regio](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid6.png)

8. Schakel over naar onbewerkte invoer door te klikken op het pictogram in de rechterbovenhoek.

   ![Schakel over naar onbewerkte invoer](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2rawinputs.png)

9. Selecteer hoofdtekst van de kiezer voor dynamische inhoud en sla de logische app.   

   ![Dynamische inhoud](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid7.png)

   Op basis van het interval, de trigger in de tabel van de primaire regio worden opgevraagd en haalt de nieuwe records. De actie bijgewerkt naar de secundaire regio van het integratieaccount. 
   Als er geen updates beschikbaar zijn, de Triggerstatus van de wordt weergegeven als **overgeslagen**.  

   ![Tabel van de primaire regio](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid8.png)

Op basis van het interval, de incrementele runtimestatus worden gerepliceerd van de primaire regio naar de secundaire regio. Tijdens een noodgebeurtenis, wanneer de primaire regio niet beschikbaar, direct verkeer naar de secundaire regio voor bedrijfscontinuïteit. 

## <a name="next-steps"></a>Volgende stappen

[B2B-berichten bewaken](logic-apps-monitor-b2b-message.md)

