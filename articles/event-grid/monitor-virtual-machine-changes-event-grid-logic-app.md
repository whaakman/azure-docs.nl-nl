---
title: Wijzigingen van de virtuele machine - Azure gebeurtenis raster & Logic Apps | Microsoft Docs
description: Controleren op wijzigingen van de configuratie in virtuele machines (VM's) met behulp van Azure Event raster en Logic Apps
keywords: Logic apps, gebeurtenis rasters, virtuele machine, VM
services: logic-apps
author: ecfan
manager: anneta
ms.assetid: 
ms.workload: logic-apps
ms.service: logic-apps
ms.topic: article
ms.date: 08/16/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 4d4c16860dbec10162797a13c8f9f57106abd17f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-virtual-machine-changes-with-azure-event-grid-and-logic-apps"></a>Wijzigingen van de virtuele machine met Azure Event raster en Logic Apps bewaken

U kunt een geautomatiseerde starten [logic app werkstroom](../logic-apps/logic-apps-what-are-logic-apps.md) wanneer specifieke gebeurtenissen plaatsvinden in Azure-resources of resources van derden. Deze resources kunnen publiceren die gebeurtenissen naar een [Azure event raster](../event-grid/overview.md). Op zijn beurt het raster voor de gebeurtenis die gebeurtenissen verstuurd naar abonnees waarvoor webhooks, wachtrijen of [event hubs](../event-hubs/event-hubs-what-is-event-hubs.md) als eindpunten. Als een abonnee kan uw logische app die gebeurtenissen uit het raster gebeurtenis wacht voordat het uitvoeren van geautomatiseerde werkstromen voor het uitvoeren van taken - zonder dat u code schrijven.

Dit zijn bijvoorbeeld sommige gebeurtenissen die uitgevers naar abonnees via de service Azure gebeurtenis raster verzenden kunnen:

* Maken, lezen, bijwerken of verwijderen van een resource. U kunt bijvoorbeeld de wijzigingen die mogelijk kosten op uw Azure-abonnement en van invloed zijn op uw factuur bewaken. 
* Toevoegen of verwijderen van een persoon van een Azure-abonnement.
* Uw app in uitvoert een bepaalde actie.
* Een nieuw bericht wordt weergegeven in een wachtrij.

Deze zelfstudie maakt een logische app waarmee wijzigingen aan een virtuele machine bewaakt en e-mailberichten over deze wijzigingen worden verzonden. Wanneer u een logische app met een gebeurtenisabonnement voor een Azure-resource maakt, wordt de status van gebeurtenissen stromen van die bron via een raster gebeurtenis aan de logische app. De zelfstudie leert u deze logische app bouwen:

![Overzicht - monitor voor virtuele machine met de gebeurtenis raster en logic app](./media/monitor-virtual-machine-changes-event-grid-logic-app/monitor-virtual-machine-event-grid-logic-app-overview.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maak een logische app die u gebeurtenissen vanuit een raster gebeurtenis bewaakt.
> * Een voorwaarde die specifiek op wijzigingen van de virtuele machine controleert toevoegen.
> * E-mail verzenden wanneer de virtuele machine wordt gewijzigd.

## <a name="prerequisites"></a>Vereisten

* Een e-mailaccount van [elke e-provider wordt ondersteund door Azure Logic Apps](../connectors/apis-list.md), zoals Outlook van Office 365, Outlook.com of Gmail, voor het verzenden van meldingen. Deze zelfstudie maakt gebruik van Outlook van Office 365.

* Een [virtuele machine](https://azure.microsoft.com/services/virtual-machines). Als u dit nog niet hebt gedaan, maakt u een virtuele machine via een [maken van een VM-zelfstudie](https://docs.microsoft.com/azure/virtual-machines/). Aan de virtuele machine publiceren van gebeurtenissen, u [hoeft niet te doen iets anders](../event-grid/overview.md).

## <a name="create-a-logic-app-that-monitors-events-from-an-event-grid"></a>Een logische app die u gebeurtenissen vanuit een raster gebeurtenis bewaakt maken

Eerst een logische app maken en toevoegen van een trigger voor de gebeurtenis raster dat de resourcegroep voor uw virtuele machine bewaakt. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 

2. Kies in de linkerbovenhoek van het Azure hoofdmenu **nieuw** > **Enterprise Integration** > **logische App**.

   ![Logische app maken](./media/monitor-virtual-machine-changes-event-grid-logic-app/azure-portal-create-logic-app.png)

3. Maak uw logische app met de instellingen die zijn opgegeven in de volgende tabel:

   ![Geef logic app-details](./media/monitor-virtual-machine-changes-event-grid-logic-app/create-logic-app-for-event-grid.png)

   | Instelling | Voorgestelde waarde | Beschrijving | 
   | ------- | --------------- | ----------- | 
   | **Naam** | *{uw logica-app-naam}* | Geef een unieke logic app-naam. | 
   | **Abonnement** | *{uw Azure-abonnement}* | Selecteer de dezelfde Azure-abonnement voor alle services in deze zelfstudie. | 
   | **Resourcegroep** | *{uw-Azure-resourcegroep}* | Selecteer de dezelfde Azure-resourcegroep voor alle services in deze zelfstudie. | 
   | **Locatie** | *{uw Azure-regio}* | Selecteer de dezelfde regio voor alle services in deze zelfstudie. | 
   | | | 

4. Wanneer u klaar bent, selecteert u **vastmaken aan dashboard**, en kies **maken**.

   U hebt nu een Azure-resource gemaakt voor uw logische app. 
   Nadat u Azure implementeert uw logische app, ziet de ontwerper van Logic Apps u sjablonen voor algemene patronen zodat u kunt aan de slag sneller.

   > [!NOTE] 
   > Wanneer u selecteert **vastmaken aan dashboard**, uw logische app automatisch wordt geopend in Logic Apps Designer. U kunt anders handmatig zoeken en open uw logische app.

5. Kies nu een sjabloon voor logic Apps. Onder **sjablonen**, kies **lege logische App** zodat u uw logische app maken.

   ![Kies logic app-sjabloon](./media/monitor-virtual-machine-changes-event-grid-logic-app/choose-logic-app-template.png)

   De ontwerpfunctie van Logic Apps ziet u nu [ *connectors* ](../connectors/apis-list.md) en [ *triggers* ](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) dat u gebruiken kunt om te beginnen met uw logische app, en ook acties die u kunt toevoegen na een trigger taken uit te voeren. Een trigger is een gebeurtenis die een logic app-exemplaar gemaakt en gestart van uw logische app-werkstroom. 
   Uw logische app moet een trigger als het eerste item.

6. Voer in het zoekvak 'gebeurtenis raster' als filter. Selecteer deze trigger: **Azure gebeurtenis raster - op een gebeurtenis**

   ![Selecteer deze trigger: 'Azure gebeurtenis raster - op een gebeurtenis'](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger.png)

7. Wanneer u wordt gevraagd, aanmelden bij Azure gebeurtenis raster met uw Azure-referenties.

   ![Meld u aan met uw Azure-referenties](./media/monitor-virtual-machine-changes-event-grid-logic-app/sign-in-event-grid.png)

   > [!NOTE]
   > Als u bent aangemeld met een persoonlijk Microsoft-account, zoals @outlook.com of @hotmail.com, de trigger gebeurtenis raster mogelijk niet correct weergegeven. Als tijdelijke oplossing kiezen [verbinding maken met de Service-Principal](/azure-resource-manager/resource-group-create-service-principal-portal.md), of als lid van de Azure Active Directory die is gekoppeld aan uw Azure-abonnement, bijvoorbeeld verifiëren *gebruikersnaam*@emailoutlook.onmicrosoft.com.

8. Nu uw logische app publisher gebeurtenissen abonneren. Geef details voor uw abonnement op gebeurtenissen die zijn opgegeven in de volgende tabel:

   ![Informatie opgeven voor een gebeurtenisabonnement](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details-generic.png)

   | Instelling | Voorgestelde waarde | Beschrijving | 
   | ------- | --------------- | ----------- | 
   | **Abonnement** | *{virtuele-machine-Azure-abonnement}* | Selecteer de gebeurtenisuitgever Azure-abonnement. Selecteer het Azure-abonnement voor uw virtuele machine voor deze zelfstudie. | 
   | **Resourcetype** | Microsoft.Resources.resourceGroups | Selecteer het brontype van de gebeurtenisuitgever. Voor deze zelfstudie selecteert u de opgegeven waarde zodat uw logische app alleen resourcegroepen bewaakt. | 
   | **Resourcenaam** | *{virtuele-machine-resource-group-name}* | Selecteer de resourcenaam van de uitgever. Selecteer de naam van de resourcegroep voor uw virtuele machine voor deze zelfstudie. | 
   | Kies voor optionele instellingen **geavanceerde opties weergeven**. | *{Zie beschrijvingen}* | * **Filter het voorvoegsel**: voor deze zelfstudie laat u deze instelling leeg. Het standaardgedrag komt overeen met alle waarden. U kunt echter de voorvoegseltekenreeks opgeven als een filter, bijvoorbeeld een pad en een parameter voor een specifieke bron. <p>* **Filter achtervoegsel**: voor deze zelfstudie laat u deze instelling leeg. Het standaardgedrag komt overeen met alle waarden. U kunt echter een achtervoegseltekenreeks opgeven als een filter, bijvoorbeeld een bestandsnaamextensie, als u wilt dat alleen bepaalde bestandstypen.<p>* **De naam van abonnement**: Geef een unieke naam voor uw gebeurtenisabonnement. |
   | | | 

   Wanneer u bent klaar, de gebeurtenis raster trigger als volgt uitzien in dit voorbeeld:
   
   ![Raster voorbeeld-trigger Gebeurtenisdetails](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details.png)

9. Sla uw logische app. Kies op de werkbalk designer **opslaan**. Om samen te vouwen en een actie-gegevens in uw logische app verbergen, kies de titelbalk van de actie.

   ![Uw logische app opslaan](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save.png)

   Wanneer u uw logische app met een gebeurtenis raster trigger opslaat, maakt Azure automatisch een gebeurtenisabonnement voor uw logische app aan de geselecteerde resource. Wanneer de bron een gebeurtenis aan het raster gebeurtenis publiceert, die gebeurtenis raster ook de gebeurtenis aan uw logische app automatisch duwt. Deze gebeurtenis geactiveerd uw logische app, en vervolgens maakt en een exemplaar van de werkstroom die u in deze volgende stappen definieert uitvoert.

Uw logische app is nu live en luistert naar gebeurtenissen uit het raster gebeurtenis, maar geen reactie totdat u acties aan de werkstroom toevoegt. 

## <a name="add-a-condition-that-checks-for-virtual-machine-changes"></a>Een voorwaarde waarmee wordt gecontroleerd op wijzigingen van de virtuele machine toevoegen

Toevoegen als u wilt uw logische app werkstroom alleen uitgevoerd als een bepaalde gebeurtenis gebeurt, een voorwaarde waarmee wordt gecontroleerd of voor de virtuele machine 'schrijfbewerkingen'. Bij deze voorwaarde waar is, wordt uw logische app verzendt dat u een e-mail met meer informatie over de bijgewerkte virtuele machine.

1. In Logic App Designer onder de trigger van het raster gebeurtenis kiezen **nieuwe stap** > **een voorwaarde toevoegen**.

   ![Een voorwaarde toevoegen aan uw logische app](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-add-condition-step.png)

   De Logic App-ontwerper voegt een lege voorwaarde voor uw werkstroom, inclusief de actie paden te volgen op basis of de voorwaarde waar of ONWAAR is.

   ![Lege voorwaarde](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-add-empty-condition.png)

2. In de **voorwaarde** Kies **bewerken in de geavanceerde modus**.
Typ deze expressie:

   `@equals(triggerBody()?['data']['operationName'], 'Microsoft.Compute/virtualMachines/write')`

   De voorwaarde is nu ziet eruit als in dit voorbeeld:

   ![Lege voorwaarde](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-expression.png)

   Deze expressie controleert de gebeurtenis `body` voor een `data` object waar de `operationName` eigenschap is de `Microsoft.Compute/virtualMachines/write` bewerking. 
   Meer informatie over [gebeurtenis raster gebeurtenis schema](../event-grid/event-schema.md).

3. Voor een beschrijving voor de voorwaarde, kies de **weglatingstekens** (**...** ) knop op de voorwaardeshape en kies vervolgens **naam**.

   > [!NOTE] 
   > Later in de voorbeelden in deze zelfstudie bieden ook beschrijvingen voor de stappen in de logic app-werkstroom.

4. Nu kiezen **bewerken in de standaardmodus** zodat de expressie wordt automatisch opgelost zoals wordt weergegeven:

   ![Logic app voorwaarde](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-1.png)

5. Sla uw logische app.

## <a name="send-email-when-your-virtual-machine-changes"></a>E-mail verzenden wanneer de virtuele machine wordt gewijzigd

Voeg nu een [ *actie* ](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) zodat u een e-mail krijgt wanneer de opgegeven voorwaarde voldaan wordt.

1. In de toestand **als de waarde true** Kies **een actie toevoegen**.

   ![Actie voor wanneer de voorwaarde waar is toevoegen](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-2.png)

2. Voer in het zoekvak 'e' als filter. Op basis van uw e-mailprovider, zoek en selecteer de overeenkomende connector. Selecteer vervolgens de actie 'e-mail verzenden' voor de connector. Bijvoorbeeld: 

   * Voor een Azure werkt of schoolaccount, selecteer de Outlook van Office 365-connector. 
   * Selecteer de Outlook.com-connector voor persoonlijke Microsoft-accounts. 
   * Selecteer de Gmail-connector voor Gmail-accounts. 

   We gaan om door te gaan met de Outlook van Office 365-connector. 
   Als u een andere provider gebruikt, worden de stappen hetzelfde blijven, maar uw gebruikersinterface mogelijk anders weergegeven. 

   ![Selecteer actie 'e-mail verzenden'](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email.png)

3. Als u nog een verbinding voor uw e-mailprovider hebt, aanmelden bij uw e-mailaccount wanneer er voor de verificatie wordt gevraagd.

4. Informatie opgeven voor het e-mailbericht zoals opgegeven in de volgende tabel:

   ![Lege e-mail actie](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-empty-email-action.png)

   > [!TIP]
   > Klik om te selecteren in uw werkstroom beschikbare velden, in een invoervak zodat die de **dynamische inhoud** lijst wordt geopend, of kies **dynamische inhoud toevoegen**. Voor meer velden kiezen **Zie voor meer informatie** voor elke sectie in de lijst. Sluit de **dynamische inhoud** Kies **dynamische inhoud toevoegen**.

   | Instelling | Voorgestelde waarde | Beschrijving | 
   | ------- | --------------- | ----------- | 
   | **Aan** | *{ontvanger e-mailadres}* |Voer het e-mailadres van de geadresseerde. U kunt uw eigen e-mailadres gebruiken voor testdoeleinden. | 
   | **Onderwerp** | Resource bijgewerkt: **onderwerp**| De inhoud voor de onderwerpnaam van het e-mailadres invoeren. Voor deze zelfstudie, voer de voorgestelde tekst en selecteert u de gebeurtenis **onderwerp** veld. Hier bevat uw e-onderwerp de naam voor de bijgewerkte resource (virtuele machine). | 
   | **Hoofdtekst** | Resourcegroep: **onderwerp** <p>Gebeurtenistype: **gebeurtenistype**<p>Gebeurtenis-ID: **ID**<p>Tijd: **tijd van gebeurtenis** | De inhoud voor de hoofdtekst van het e-mailadres invoeren. Voor deze zelfstudie, voer de voorgestelde tekst en selecteert u de gebeurtenis **onderwerp**, **gebeurtenistype**, **ID**, en **gebeurtenistijd** velden zodat uw e-mailbericht de naam van een resourcegroep, gebeurtenistype, tijdstempel van gebeurtenis en gebeurtenis-ID voor de update bevat. <p>Als u wilt toevoegen lege regels in de inhoud, drukt u op Shift + Enter. | 
   | | | 

   > [!NOTE] 
   > Als u een veld dat een matrix vertegenwoordigt, de designer voegt automatisch een **voor elk** lus rond de actie die verwijst naar de matrix. Op die manier uw logische app wordt op elk matrixitem die actie uitgevoerd.

   Nu uw e-mail in te grijpen als volgt uitzien in dit voorbeeld:

   ![Selecteer uitvoer wilt opnemen in e-mailbericht](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email-details.png)

   En uw klaar logische app kan eruitzien als in dit voorbeeld:

   ![Voltooide logische app](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-completed.png)

5. Sla uw logische app. Om samen te vouwen en details van elke actie in uw logische app verbergen, kies de titelbalk van de actie.

   ![Uw logische app opslaan](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save-completed.png)

   Uw logische app is nu live, maar er wordt gewacht op wijzigingen in uw virtuele machine voordat u actie uitvoert. 
   Als u wilt testen nu uw logische app, blijven de volgende sectie.

## <a name="test-your-logic-app-workflow"></a>Testen van uw logische app-werkstroom

1. Om te controleren dat de opgegeven gebeurtenissen is het ophalen van uw logische app, werk de virtuele machine. 

   Bijvoorbeeld, u kunt de grootte van de virtuele machine in de Azure portal of [vergroten of verkleinen van uw virtuele machine met Azure PowerShell](../virtual-machines/windows/resize-vm.md). 

   Na enkele ogenblikken krijgt u een e-mailbericht. Bijvoorbeeld:

   ![Stuur een e-mail over de update van virtuele machine](./media/monitor-virtual-machine-changes-event-grid-logic-app/email.png)

2. Om te bekijken van de wordt uitgevoerd en activeren van de geschiedenis voor uw logische app, op uw logische app-menu kiezen **overzicht**. Als u meer details over een uitvoeren, kies de rij voor die uitvoeren.

   ![Logische app geschiedenis wordt uitgevoerd](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history.png)

3. Als u wilt weergeven in de invoer en uitvoer voor elke stap, vouwt u de stap die u wilt bekijken. Deze informatie kunt u onderzoeken en het opsporen en oplossen in uw logische app.
 
   ![Logische app-geschiedenisdetails uitvoeren](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history-details.png)

Gefeliciteerd, u hebt gemaakt en uitvoeren van een logische app die resource gebeurtenissen tot en met een raster gebeurtenis bewaakt en u e-mailberichten wanneer deze gebeurtenissen optreden. U hebt ook geleerd hoe eenvoudig kunt u werkstromen die processen automatiseren en het integreren van systemen en cloudservices.

## <a name="clean-up-resources"></a>Resources opschonen

Deze zelfstudie maakt gebruik van bronnen en acties uitvoert die op uw Azure-abonnement kosten. Dus wanneer u met de zelfstudie bent klaar en testen, zorg ervoor dat u uitschakelen of verwijderen van alle resources waarop u niet wilt worden kosten in rekening.

U kunt uw logische app uitgevoerd en verzenden van e-mail zonder te verwijderen van de app stoppen. Kies in het menu van uw app logica **overzicht**. Kies op de werkbalk **uitschakelen**.

![Uw logische app uitschakelen](./media/monitor-virtual-machine-changes-event-grid-logic-app/turn-off-disable-logic-app.png)

## <a name="faq"></a>Veelgestelde vragen

**Q**: welke andere virtuele machine controletaken kan ik uitvoeren met de gebeurtenis rasters en logic apps? </br>
**Een**: U kunt bijvoorbeeld andere configuratiewijzigingen bewaken:

* Een virtuele machine opgehaald toegangsgroepen op basis van rechten voor toegangsbeheer (RBAC).
* Wijzigingen zijn aangebracht aan een netwerkbeveiligingsgroep (NSG) op de netwerkinterface (NIC).
* Schijven voor een virtuele machine worden toegevoegd of verwijderd.
* Een openbaar IP-adres is toegewezen aan een virtuele machine NIC.

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van Event raster](../event-grid/overview.md)
* [Gebeurtenis raster concepten](../event-grid/concepts.md)
* [Snelstartgids: Maken en aangepaste gebeurtenissen met de gebeurtenis raster routeren](../event-grid/custom-event-quickstart.md)
* [Gebeurtenis raster gebeurtenis schema](../event-grid/event-schema.md)
* [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md)
* [Logic app werkstromen maken met vooraf gedefinieerde sjablonen](../logic-apps/logic-apps-use-logic-app-templates.md)