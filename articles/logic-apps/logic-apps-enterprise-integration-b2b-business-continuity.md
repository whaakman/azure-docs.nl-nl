---
title: Herstel na noodgevallen voor B2B-integratie account - Azure Logic Apps | Microsoft Docs
description: Logic Apps B2B-noodherstel
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 0ef3965adac03f21c386765a43290c93d58d1c18
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="logic-apps-b2b-cross-region-disaster-recovery"></a>Logic Apps B2B regio-overschrijdende noodherstel

B2B-werkbelastingen hebben betrekking op geld transacties zoals orders en facturen. Tijdens een gebeurtenis na noodgevallen is het essentieel voor een bedrijf aan snel herstellen om te voldoen aan de serviceovereenkomsten zakelijke overeengekomen met hun partners. Dit artikel wordt beschreven hoe u een plan voor bedrijfscontinuïteit voor B2B-werkbelastingen. 

* Gereedheid voor herstel na noodgevallen 
* Failover naar de secundaire regio tijdens een gebeurtenis na noodgevallen 
* Terugvallen op de primaire regio na een gebeurtenis na noodgevallen

## <a name="disaster-recovery-readiness"></a>Gereedheid voor herstel na noodgevallen  

1. Een secundaire regio identificeert en maakt u een [integratie account](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) in de secundaire regio.

2. Partners, schema's en overeenkomsten voor de vereiste stromen waarin de uitvoeringsstatus moet worden gerepliceerd naar de secundaire regio integratie account toevoegen.

   > [!TIP]
   > Zorg ervoor dat er consistentie in het integratie account artefact naamconventie tussen regio's. 

3. De uitvoeringsstatus ophalen uit de primaire regio, moet u een logische app maken in de secundaire regio. 

   Deze logische app moet een *trigger* en een *actie*. 
   De trigger moet verbinding met de primaire regio integratie-account en de actie moet verbinding maken met de secundaire regio integratie-account. 
   Op basis van het interval, de trigger peilt de primaire regio statustabel uitvoeren en de nieuwe records ophaalt, indien van toepassing. De actie ze naar de secundaire regio integratie account bijgewerkt. 
   Dit helpt incrementele runtimestatus van de primaire regio ophalen met secundaire regio.

4. Zakelijke continuïteit in Logic Apps integratie-account is ontworpen ter ondersteuning van op basis van B2B-protocollen - X12, AS2 en EDIFACT. Selecteer de respectieve koppelingen Ga voor gedetailleerde stappen.

5. De aanbeveling is alle primaire regio resources in een secundaire regio te implementeren. 

   Primaire regio resources omvatten Azure SQL Database of Azure Cosmos DB, Azure Service Bus en Azure Event Hubs gebruikt voor messaging Azure API Management en de functie Azure Logic Apps in Azure App Service.   

6. Stel een verbinding van een primaire regio naar een secundaire regio. De uitvoeringsstatus ophalen uit een primaire regio, moet u een logische app maken in een secundaire regio. 

   De logische app moet een trigger en een actie hebben. 
   De trigger moet verbinding maken met een primaire regio integratie-account. 
   De actie moet verbinding maken met een secundaire regio integratie-account. 
   Op basis van het interval, de trigger peilt de primaire regio statustabel uitvoeren en de nieuwe records ophaalt, indien van toepassing. 
   De actie updates ze naar een secundaire regio integratie-account. 
   Dit proces kunt u incrementele runtimestatus ophalen uit de primaire regio naar de secundaire regio.

Zakelijke continuïteit in Logic Apps integratie-account biedt ondersteuning op basis van de B2B-protocollen X12, AS2 en EDIFACT. Zie voor gedetailleerde stappen voor het gebruik van X12 en AS2 [X12](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#x12) en [AS2](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#as2) in dit artikel.

## <a name="fail-over-to-a-secondary-region-during-a-disaster-event"></a>Failover naar een secundaire regio tijdens een gebeurtenis na noodgevallen

Tijdens een gebeurtenis na noodgevallen, wanneer de primaire regio niet beschikbaar voor bedrijfscontinuïteit, direct verkeer naar de secundaire regio. Een secundaire regio helpt een zakelijke functies snel om te voldoen aan de RPO/RTO herstellen overeengekomen door partners. Ook beperkt inspanningen failover van één regio naar een andere regio. 

Er is een verwachte latentie tijdens het kopiëren van besturingselement cijfers van een primaire regio naar een secundaire regio. Om te voorkomen dat dubbele gegenereerde besturingselement cijfers verzenden naar partners tijdens een gebeurtenis na noodgevallen, wordt aangeraden wordt om te verhogen van de cijfers van het besturingselement in de secundaire regio overeenkomsten met behulp van [PowerShell-cmdlets](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).

## <a name="fall-back-to-a-primary-region-post-disaster-event"></a>Terugvallen op een primaire regio na noodgevallen gebeurtenis

Als u wilt terugvallen op een primaire regio wanneer deze beschikbaar is, de volgende stappen uit:

1. Stop de berichten van partners in de secundaire regio worden geaccepteerd.  

2. De gegenereerde besturingselement getallen voor alle primaire regio overeenkomsten verhogen met behulp van [PowerShell-cmdlets](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).  

3. Directe verkeer van de secundaire regio naar de primaire regio.

4. Controleer dat de logische app gemaakt in de secundaire regio voor het ophalen van de status van uitvoeren vanaf de primaire regio is ingeschakeld.

## <a name="x12"></a>X12 

Zakelijke continuïteit voor EDI X 12 documenten is gebaseerd op het besturingselement getallen:

> [!TIP]
> U kunt ook de [X12 snel starten sjabloon](https://azure.microsoft.com/documentation/templates/201-logic-app-x12-disaster-recovery-replication/) om logic apps te maken. Maken primaire en secundaire integratieaccounts zijn vereisten voor het gebruik van de sjabloon. De sjabloon kunt u twee logische apps, één voor ontvangen besturingselement getallen en één voor de gegenereerde besturingselement getallen maken. Respectieve triggers en acties worden gemaakt in de logic apps, de trigger verbinding te maken met de primaire integratie-account en de actie aan het account van de secundaire-integratie.

**Vereisten**

Als u herstel na noodgevallen voor inkomende berichten, selecteert u de dubbele controle-instellingen in de x12-overeenkomst(en) instellingen voor het ontvangen van de overeenkomst.

![Instellingen voor controle op dubbele items selecteren](./media/logic-apps-enterprise-integration-b2b-business-continuity/dupcheck.png)  

1. Maak een [logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md) in een secundaire regio.    

2. Zoeken op **X12**, en selecteer **X12-als een getal van het besturingselement wordt gewijzigd**.   

   ![Zoeken naar X12](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn1.png)

   De trigger vraagt u een verbinding maken met een account voor de integratie. 
   De trigger moet worden verbonden met een primaire regio integratie-account.

3. Geef de verbindingsnaam van een, selecteer uw *primaire regio integratie account* uit de lijst en kies **maken**.   

   ![Primaire regio integratie-accountnaam](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn2.png)

4. De **DateTime besturingselement nummer synchronisatie starten** instelling is optioneel. De **frequentie** kan worden ingesteld op **dag**, **uur**, **minuut**, of **tweede** met een interval.   

   ![Datum/tijd- en frequentie](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

5. Selecteer **nieuwe stap** > **een actie toevoegen**.

   ![Nieuwe stap, een actie toevoegen](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

6. Zoeken op **X12**, en selecteer **X12-toevoegen of bijwerken van besturingselement cijfers**.   

   ![Controlenummers toevoegen of bijwerken](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn5.png)

7. Selecteer voor een actie verbinding met een secundaire regio integratie-account, **verbinding wijzigen** > **nieuwe verbinding toevoegen** voor een lijst met de integratieaccounts beschikbaar. Geef de verbindingsnaam van een, selecteer uw *secundaire regio integratie account* uit de lijst en kies **maken**. 

   ![Secundaire regio integratie-accountnaam](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

8. Overschakelen naar onbewerkte invoer door te klikken op het pictogram in de rechterbovenhoek.

   ![Overschakelen naar onbewerkte invoer](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12rawinputs.png)

9. Hoofdtekst van de dynamische inhoud objectkiezer selecteren en opslaan van de logische app.

   ![Dynamische inhoud velden](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn7.png)

   Op basis van het interval, de trigger de primaire regio ontvangen besturingselement nummer tabel worden opgevraagd en haalt de nieuwe records. 
   De actie updates de records in de secundaire regio integratie-account. 
   Als er geen updates zijn, de status van de trigger wordt weergegeven als **overgeslagen**.   

   ![Besturingselement number tabel](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

Op basis van het interval, repliceert de runtimestatus van incrementele van een primaire regio naar een secundaire regio. Tijdens een gebeurtenis na noodgevallen, wanneer de primaire regio niet beschikbaar, directe verkeer naar de secundaire regio voor bedrijfscontinuïteit. 

## <a name="edifact"></a>EDIFACT 

Zakelijke continuïteit voor EDI EDIFACT documenten is gebaseerd op het besturingselement cijfers.

**Vereisten**

Als u herstel na noodgevallen voor inkomende berichten, selecteert u de dubbele controle-instellingen in de instellingen voor het ontvangen van uw EDIFACT-overeenkomst.

![Instellingen voor controle op dubbele items selecteren](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactdupcheck.png)  

1. Maak een [logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md) in een secundaire regio.    

2. Zoeken op **EDIFACT**, en selecteer **EDIFACT - als een getal van het besturingselement wordt gewijzigd**.

   ![Zoeken naar EDIFACT](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactcn1.png)

   De trigger vraagt u een verbinding maken met een account voor de integratie. 
   De trigger moet worden verbonden met een primaire regio integratie-account. 

3. Geef de verbindingsnaam van een, selecteer uw *primaire regio integratie account* uit de lijst en kies **maken**.    

   ![Primaire regio integratie-accountnaam](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN2.png)

4. De **DateTime besturingselement nummer synchronisatie starten** instelling is optioneel. De **frequentie** kan worden ingesteld op **dag**, **uur**, **minuut**, of **tweede** met een interval.    

   ![Datum/tijd- en frequentie](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

6. Selecteer **nieuwe stap** > **een actie toevoegen**.    

   ![Nieuwe stap, een actie toevoegen](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

7. Zoeken op **EDIFACT**, en selecteer **EDIFACT - toevoegen of bijwerken van besturingselement cijfers**.   

   ![Controlenummers toevoegen of bijwerken](./media/logic-apps-enterprise-integration-b2b-business-continuity/EdifactChooseAction.png)

8. Selecteer voor een actie verbinding met een secundaire regio integratie-account, **verbinding wijzigen** > **nieuwe verbinding toevoegen** voor een lijst met de integratieaccounts beschikbaar. Geef de verbindingsnaam van een, selecteer uw *secundaire regio integratie account* uit de lijst en kies **maken**.

   ![Secundaire regio integratie-accountnaam](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

9. Overschakelen naar onbewerkte invoer door te klikken op het pictogram in de rechterbovenhoek.

   ![Overschakelen naar onbewerkte invoer](./media/logic-apps-enterprise-integration-b2b-business-continuity/Edifactrawinputs.png)

10. Hoofdtekst van de dynamische inhoud objectkiezer selecteren en opslaan van de logische app.   

   ![Dynamische inhoud velden](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN7.png)

   Op basis van het interval, de trigger de primaire regio ontvangen besturingselement nummer tabel worden opgevraagd en haalt de nieuwe records.
   De actie updates de records voor de secundaire regio integratie-account. 
   Als er geen updates zijn, de status van de trigger wordt weergegeven als **overgeslagen**.

   ![Besturingselement number tabel](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

Op basis van het interval, repliceert de runtimestatus van incrementele van een primaire regio naar een secundaire regio. Tijdens een gebeurtenis na noodgevallen, wanneer de primaire regio niet beschikbaar, directe verkeer naar de secundaire regio voor bedrijfscontinuïteit. 

## <a name="as2"></a>AS2 

Zakelijke continuïteit voor documenten die gebruikmaken van het AS2-protocol is gebaseerd op de bericht-ID en de MIC-waarde.

> [!TIP]
> U kunt ook de [AS2 snel starten sjabloon](https://github.com/Azure/azure-quickstart-templates/pull/3302) om logic apps te maken. Maken primaire en secundaire integratieaccounts zijn vereisten voor het gebruik van de sjabloon. De sjabloon kunt een logische app met een trigger en een actie maken. De logische app maakt een verbinding van een trigger aan een primaire integratie-account en een actie die een secundaire integratie-account.

1. Maak een [logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md) in de secundaire regio.  

2. Zoeken op **AS2**, en selecteer **AS2 - waarde als een MIC is gemaakt**.   

   ![Zoeken naar AS2](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid1.png)

   Een trigger vraagt u een verbinding maken met een account voor de integratie. 
   De trigger moet worden verbonden met een primaire regio integratie-account. 
   
3. Geef de verbindingsnaam van een, selecteer uw *primaire regio integratie account* uit de lijst en kies **maken**.

   ![Primaire regio integratie-accountnaam](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid2.png)

4. De **DateTime MIC waarde synchronisatie starten** instelling is optioneel. De **frequentie** kan worden ingesteld op **dag**, **uur**, **minuut**, of **tweede** met een interval.   

   ![Datum/tijd- en frequentie](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid3.png)

5. Selecteer **nieuwe stap** > **een actie toevoegen**.  

   ![Nieuwe stap, een actie toevoegen](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid4.png)

6. Zoeken op **AS2**, en selecteer **AS2 - toevoegen of bijwerken van de inhoud van de MIC**.  

   ![MIC toevoegen of bijwerken](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid5.png)

7. Selecteer voor een actie verbinding met een account secundaire integratie, **verbinding wijzigen** > **nieuwe verbinding toevoegen** voor een lijst met de integratieaccounts beschikbaar. Geef de verbindingsnaam van een, selecteer uw *secundaire regio integratie account* uit de lijst en kies **maken**.

   ![Secundaire regio integratie-accountnaam](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid6.png)

8. Overschakelen naar onbewerkte invoer door te klikken op het pictogram in de rechterbovenhoek.

   ![Overschakelen naar onbewerkte invoer](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2rawinputs.png)

9. Hoofdtekst van de dynamische inhoud objectkiezer selecteren en opslaan van de logische app.   

   ![Dynamische inhoud](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid7.png)

   Op basis van het interval, de trigger in de tabel van de primaire regio worden opgevraagd en haalt de nieuwe records. De actie updates ze naar de secundaire regio integratie-account. 
   Als er geen updates zijn, de status van de trigger wordt weergegeven als **overgeslagen**.  

   ![Primaire regio tabel](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid8.png)

Op basis van het interval, repliceert de runtimestatus van incrementele vanaf de primaire regio naar de secundaire regio. Tijdens een gebeurtenis na noodgevallen, wanneer de primaire regio niet beschikbaar, directe verkeer naar de secundaire regio voor bedrijfscontinuïteit. 

## <a name="next-steps"></a>Volgende stappen

[B2B-berichten bewaken](logic-apps-monitor-b2b-message.md)

