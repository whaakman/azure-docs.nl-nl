---
title: Wijzigingen in virtuele machines bewaken met Azure Event Grid en Logic Apps | Microsoft Docs
description: Hier leert u hoe u met behulp van Azure Event Grid en Logic Apps kunt controleren of de configuratie van virtuele machines (VM's) is gewijzigd
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: tutorial
ms.date: 11/30/2017
ms.openlocfilehash: 06fa9b9191104db3b141b6268a90a7c8f206280e
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53106070"
---
# <a name="tutorial-monitor-virtual-machine-changes-with-azure-event-grid-and-logic-apps"></a>Zelfstudie: Wijzigingen in virtuele machines bewaken met Azure Event Grid en Logic Apps

U kunt een geautomatiseerde [werkstroom van een logische app](../logic-apps/logic-apps-overview.md) starten wanneer specifieke gebeurtenissen plaatsvinden in Azure-resources of in resources van derden. Deze resources kunnen die gebeurtenissen publiceren naar een [Azure-gebeurtenisraster](../event-grid/overview.md). Het gebeurtenisraster verstuurt die gebeurtenissen vervolgens naar abonnees die webhooks, wachtrijen of [gebeurtenishubs](../event-hubs/event-hubs-what-is-event-hubs.md) als eindpunt hebben. Als een abonnee kan uw logische app wachten op die gebeurtenissen uit het gebeurtenisraster voordat geautomatiseerde werkstromen worden gestart voor het uitvoeren van taken, zonder dat u hiervoor code hoeft te schrijven.

Dit zijn enkele voorbeelden van gebeurtenissen die uitgevers naar abonnees kunnen versturen via de service Azure Event Grid:

* Maken, lezen, bijwerken of verwijderen van een resource. U kunt bijvoorbeeld controleren op wijzigingen die extra kosten op uw Azure-abonnement tot gevolg kunnen hebben en dus van invloed zijn op de hoogte van uw factuur. 
* Toevoegen of verwijderen van een persoon uit een Azure-abonnement.
* Uitvoeren van een bepaalde actie door uw app.
* Nieuw bericht in een wachtrij.

In deze zelfstudie maakt u een logische app waarmee u controleert op wijzigingen van een virtuele machine en e-mailberichten verstuurt over deze wijzigingen. Wanneer u een logische app maakt met een gebeurtenisabonnement voor een Azure-resource, worden gebeurtenissen vanuit die resource via een gebeurtenisraster naar de logische app geleid. In de zelfstudie gaat u deze logische app bouwen:

![Overzicht - wijzigingen in virtuele machines bewaken met een gebeurtenisraster en een logische app](./media/monitor-virtual-machine-changes-event-grid-logic-app/monitor-virtual-machine-event-grid-logic-app-overview.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een logische app maken die gebeurtenissen uit een gebeurtenisraster bewaakt.
> * Een voorwaarde toevoegen die specifiek controleert op wijzigingen van de virtuele machine.
> * Een e-mail verzenden wanneer de virtuele machine wordt gewijzigd.

## <a name="prerequisites"></a>Vereisten

* Een e-mailaccount van [een e-mailprovider die door Azure Logic Apps wordt ondersteund](../connectors/apis-list.md) voor het verzenden van meldingen, bijvoorbeeld Office 365 Outlook, Outlook.com of Gmail. In deze zelfstudie wordt Office 365 Outlook gebruikt.

* Een [virtuele machine](https://azure.microsoft.com/services/virtual-machines). Als u dit nog niet hebt gedaan, maakt u een virtuele machine aan de hand van de [documentatie voor virtuele machines](https://docs.microsoft.com/azure/virtual-machines/). U [hoeft verder niets te doen](../event-grid/overview.md) om gebeurtenissen te laten publiceren door de virtuele machine.

## <a name="create-a-logic-app-that-monitors-events-from-an-event-grid"></a>Een logische app maken die gebeurtenissen uit een gebeurtenisraster bewaakt

U gaat eerst een logische app maken en een trigger voor Event Grid toevoegen die de resourcegroep voor uw virtuele machine bewaakt. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 

2. Kies in de linkerbovenhoek van het hoofdmenu van Azure **Een resource maken** > **Bedrijfsintegratie** > **Logische app**.

   ![Logische app maken](./media/monitor-virtual-machine-changes-event-grid-logic-app/azure-portal-create-logic-app.png)

3. Maak de logische app aan de hand van de instellingen in de volgende tabel:

   ![Gegevens van logische app opgeven](./media/monitor-virtual-machine-changes-event-grid-logic-app/create-logic-app-for-event-grid.png)

   | Instelling | Voorgestelde waarde | Beschrijving | 
   | ------- | --------------- | ----------- | 
   | **Naam** | *{naam-van-uw-logische-app}* | Geef een unieke naam voor de logische app op. | 
   | **Abonnement** | *{uw-Azure-abonnement}* | Selecteer hetzelfde Azure-abonnement voor alle services in deze zelfstudie. | 
   | **Resourcegroep** | *{uw-Azure-resourcegroep}* | Selecteer dezelfde resourcegroep voor alle services in deze tutorial. | 
   | **Locatie** | *{uw Azure-regio}* | Selecteer dezelfde regio voor alle services in deze tutorial. | 
   | | | 

4. Als u klaar bent, selecteert u **Vastmaken aan dashboard** en kiest u **Maken**.

   U hebt nu een Azure-resource voor uw logische app gemaakt. 
   Nadat de logische app is geïmplementeerd, toont de ontwerper van logische apps sjablonen voor algemene patronen, zodat u sneller aan de slag kunt.

   > [!NOTE] 
   > Als u **Vastmaken aan Dashboard** selecteert, wordt de logische app automatisch geopend in de functie Ontwerper van logische apps. Anders kunt u de logische app handmatig zoeken en openen.

5. Kies nu een sjabloon voor de logische app. Kies onder **Sjablonen** de optie **Lege logische app**, zodat u de logische app helemaal zelf kunt ontwerpen.

   ![Sjabloon voor logische app kiezen](./media/monitor-virtual-machine-changes-event-grid-logic-app/choose-logic-app-template.png)

   U ziet nu [*connectors*](../connectors/apis-list.md) en [*triggers*](../logic-apps/logic-apps-overview.md#logic-app-concepts) die u gebruiken kunt om te beginnen met uw logische app, en ook acties die u kunt toevoegen na een trigger om taken uit te voeren. Een trigger is een gebeurtenis die een exemplaar van een logic app maakt en de werkstroom van uw logische app start. 
   Het eerste item van een logische app moet een trigger zijn.

6. Typ 'event grid' als filter in het zoekvak. Selecteer deze trigger: **Azure Event Grid - Wanneer een resourcegebeurtenis optreedt**

   ![Selecteer deze trigger: 'Azure Event Grid - Wanneer een resourcegebeurtenis optreedt'](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger.png)

7. Wanneer dat wordt gevraagd, meldt u zich aan bij Azure Event Grid met uw Azure-referenties.

   ![Aanmelden met uw Azure-referenties](./media/monitor-virtual-machine-changes-event-grid-logic-app/sign-in-event-grid.png)

   > [!NOTE]
   > Als u zich aanmeldt met een persoonlijk Microsoft-account, zoals @outlook.com of @hotmail.com, wordt de trigger voor het gebeurtenisraster mogelijk niet juist weergegeven. Als tijdelijke oplossing kunt u [Verbinding maken met service-principal](../active-directory/develop/howto-create-service-principal-portal.md) kiezen of verifiëren als een lid van de Azure Active Directory die is gekoppeld aan uw Azure-abonnement, bijvoorbeeld *gebruikersnaam*@emailoutlook.onmicrosoft.com.

8. U gaat de logische app nu abonneren op gepubliceerde gebeurtenissen. Geef de details voor uw gebeurtenisabonnement op zoals deze worden beschreven in de volgende tabel:

   ![Details opgeven voor gebeurtenisabonnement](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details-generic.png)

   | Instelling | Voorgestelde waarde | Beschrijving | 
   | ------- | --------------- | ----------- | 
   | **Abonnement** | *{Azure-abonnement-virtuele-machine}* | Selecteer het Azure-abonnement van de gebeurtenisuitgever. Selecteer voor deze zelfstudie het Azure-abonnement voor uw virtuele machine. | 
   | **Resourcetype** | Microsoft.Resources.resourceGroups | Selecteer het resourcetype van de gebeurtenisuitgever. Selecteer voor deze zelfstudie de opgegeven waarde, zodat uw logische app alleen resourcegroepen bewaakt. | 
   | **Resourcenaam** | *{naam-van-resourcegroep-virtuele-machine}* | Selecteer de resourcenaam van de uitgever. Selecteer voor deze zelfstudie de naam van de resourcegroep voor uw virtuele machine. | 
   | Kies **Geavanceerde opties weergeven** om optionele instellingen op te geven. | *{zie de beschrijvingen}* | * **Voorvoegselfilter**: Laat deze instelling leeg voor deze zelfstudie. Het standaardgedrag komt overeen met alle waarden. U kunt echter een voorvoegseltekenreeks opgeven als een filter, bijvoorbeeld een pad en een parameter voor een specifieke resource. <p>* **Achtervoegselfilter**: Laat deze instelling leeg voor deze zelfstudie. Het standaardgedrag komt overeen met alle waarden. U kunt echter een achtervoegseltekenreeks opgeven als een filter, bijvoorbeeld een bestandsnaamextensie, als u alleen bepaalde bestandstypen wilt gebruiken.<p>* **Abonnementsnaam**: Geef een unieke naam op voor het gebeurtenisabonnement. |
   | | | 

   Als u klaar bent, ziet de trigger voor het gebeurtenisraster er mogelijk zo uit:
   
   ![Voorbeeld van gegevens voor trigger voor gebeurtenisraster](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details.png)

9. Sla uw logische app op. Kies **Opslaan** op de werkbalk van de ontwerper. Kies de titelbalk van de actie om de details van een actie in uw logische app samen te vouwen en te verbergen.

   ![Uw logische app opslaan](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save.png)

   Wanneer u uw logische app met een trigger voor het gebeurtenisraster opslaat, maakt Azure automatisch een gebeurtenisabonnement voor uw logische app voor de geselecteerde resource. Wanneer de resource nu een gebeurtenis naar het gebeurtenisraster publiceert, wordt die gebeurtenis automatisch door het raster naar de logische app verstuurd. Deze gebeurtenis triggert de logische app, en maakt vervolgens een exemplaar van de werkstroom die u in de volgende stappen definieert. Ten slotte wordt de werkstroom uitgevoerd.

Uw logische app is nu live en luistert naar gebeurtenissen uit het gebeurtenisraster, maar kan verder nog niks totdat u acties aan de werkstroom toevoegt. 

## <a name="add-a-condition-that-checks-for-virtual-machine-changes"></a>Een voorwaarde toevoegen die controleert op wijzigingen van de virtuele machine

Als u de werkstroom van de logische app alleen wilt uitvoeren uitgevoerd als er een bepaalde gebeurtenis optreedt, voegt u een voorwaarde toe waarmee wordt gecontroleerd op 'schrijfbewerkingen' naar de virtuele machine. Als aan deze voorwaarde wordt voldaan (is Waar), verstuurt de logische app een e-mail met meer gegevens van de bijgewerkte virtuele machine.

1. Kies in Ontwerper van logische apps onder de trigger voor het gebeurtenisraster **+ Nieuwe stap** > **Een actie toevoegen**.

   ![Een voorwaarde toevoegen aan uw logische app](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-add-condition-step.png)

   Er wordt een lege voorwaarde voor uw werkstroom toegevoegd, inclusief de te volgen actiepaden wanneer de voorwaarde Waar of Onwaar is.

   ![Lege voorwaarde](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-add-empty-condition.png)

2. Kies **Bewerken in geavanceerde modus** in het vak **Voorwaarde**.
Voer deze expressie in:

   `@equals(triggerBody()?['data']['operationName'], 'Microsoft.Compute/virtualMachines/write')`

   Uw voorwaarde ziet er nu uit als in dit voorbeeld:

   ![Lege voorwaarde](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-expression.png)

   Deze expressie controleert de gebeurtenis `body` op een `data`-object waar de eigenschap `operationName` is ingesteld op `Microsoft.Compute/virtualMachines/write`. 
   Lees hier meer over het [gebeurtenisschema van Event Grid](../event-grid/event-schema.md).

3. Als u een beschrijving wilt opgeven voor de voorwaarde, kiest u het **beletselteken** (**...** ) op de vorm voor de voorwaarde en kiest u vervolgens **Naam wijzigen**.

   > [!NOTE] 
   > De voorbeelden verderop in deze zelfstudie bevatten ook beschrijvingen voor de stappen in de werkstroom van de logische app.

4. Kies nu **Bewerken in basismodus**, zodat de expressie automatisch wordt opgelost zoals hier wordt weergegeven:

   ![Voorwaarde van logische app](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-1.png)

5. Sla uw logische app op.

## <a name="send-email-when-your-virtual-machine-changes"></a>Een e-mail verzenden wanneer de virtuele machine wordt gewijzigd

U gaat nu een [*actie*](../logic-apps/logic-apps-overview.md#logic-app-concepts) toevoegen, zodat u een e-mail krijgt wanneer aan de opgegeven voorwaarde wordt voldaan.

1. Kies **Een actie toevoegen** in het vak **Indien waar** van de voorwaarde.

   ![Actie toevoegen voor wanneer de voorwaarde waar is](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-2.png)

2. Typ 'e-mail' als filter in het zoekvak. Zoek en selecteer de bijbehorende connector op basis van uw e-mailprovider. Selecteer vervolgens de actie 'e-mail verzenden' voor de connector. Bijvoorbeeld: 

   * Selecteer voor een werk- of schoolaccount van Azure de Office 365 Outlook-connector. 
   * Selecteer voor een persoonlijk Microsoft-account de Outlook.com-connector. 
   * Selecteer voor een Gmail-account de Gmail-connector. 

   Hier wordt doorgegaan met de Office 365 Outlook-connector. 
   Als u een andere provider gebruikt, blijven de stappen gelijk, maar uw gebruikersinterface kan er anders uitzien. 

   ![De actie 'Een e-mail verzenden' selecteren](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email.png)

3. Als u nog geen verbinding voor uw e-mailprovider hebt, meldt u zich aan bij uw e-mailaccount wanneer er om verificatie wordt gevraagd.

4. Geef informatie op voor het e-mailbericht zoals wordt aangegeven in de volgende tabel:

   ![Lege e-mailactie](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-empty-email-action.png)

   > [!TIP]
   > Als u een keuze wilt maken uit de beschikbare velden in de werkstroom, klikt u in een invoervak zodat de lijst **Dynamische inhoud** wordt geopend. U kunt ook **Dynamische inhoud toevoegen** kiezen. Als u meer velden wilt zien, kiest u **Meer weergeven** voor elke sectie in de lijst. U sluit de lijst **Dynamische inhoud** door **Dynamische inhoud toevoegen** te kiezen.

   | Instelling | Voorgestelde waarde | Beschrijving | 
   | ------- | --------------- | ----------- | 
   | **Aan** | *{e-mailadres-geadresseerde}* |Voer het e-mailadres van de ontvanger in. Voor testdoeleinden kunt u uw eigen e-mailadres gebruiken. | 
   | **Onderwerp** | Resource bijgewerkt: **Onderwerp**| Voer de inhoud van het onderwerp van de e-mail in. Voor deze zelfstudie voert u de voorgestelde tekst in en selecteert u het veld **Onderwerp** van de gebeurtenis. Hier bevat het onderwerp van de e-mail de naam voor de bijgewerkte resource (virtuele machine). | 
   | **Hoofdtekst** | Resourcegroep: **Onderwerp** <p>Gebeurtenistype: **Gebeurtenistype**<p>Gebeurtenis-id: **ID**<p>Tijdstip: **Tijdstip van gebeurtenis** | Voer de inhoud voor de hoofdtekst van de e-mail in. Voor deze zelfstudie voert u de voorgestelde tekst in en selecteert u de velden **Onderwerp**, **Gebeurtenistype**, **Id** en **Tijd van gebeurtenis** van de gebeurtenis, zodat uw e-mailbericht de naam van de resourcegroep bevat, evenals het type gebeurtenis, het tijdstempel van de gebeurtenis en de gebeurtenis-id voor de update. <p>U kunt lege regels toevoegen aan de inhoud door op Shift+Enter te drukken. | 
   | | | 

   > [!NOTE] 
   > Als u een veld selecteert dat een matrix vertegenwoordigt, wordt in de ontwerpfunctie automatisch een lus **Voor elke** toegevoegd rond de actie die naar de matrix verwijst. Op die manier wordt de actie uitgevoerd voor elk matrixitem.

   De e-mailactie ziet er nu ongeveer uit zoals in dit voorbeeld:

   ![Velden selecteren voor opname in e-mailbericht](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email-details.png)

   De voltooide logische app ziet er dan ongeveer zo uit:

   ![Voltooide logische app](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-completed.png)

5. Sla uw logische app op. Kies de titelbalk van een actie om de details van die actie in uw logische app samen te vouwen en te verbergen.

   ![Uw logische app opslaan](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save-completed.png)

   Uw logische app is nu live, maar er wordt gewacht op wijzigingen van de virtuele machine voordat er iets gebeurt. 
   Ga door naar de volgende sectie als u de logische app nu wilt testen.

## <a name="test-your-logic-app-workflow"></a>De werkstroom van uw logische app testen

1. Om te controleren of uw logische app de opgegeven gebeurtenissen ontvangt, moet u de virtuele machine bijwerken. 

   U kunt bijvoorbeeld de grootte van de virtuele machine aanpassen in Azure Portal of [met Azure PowerShell](../virtual-machines/windows/resize-vm.md). 

   Binnen enkele ogenblikken moet u dan een e-mailbericht krijgen. Bijvoorbeeld:

   ![E-mail over update van virtuele machine](./media/monitor-virtual-machine-changes-event-grid-logic-app/email.png)

2. Als u de uitvoerings- en triggergeschiedenis van de logische app wilt bekijken, kiest u **Overzicht** in het menu van de logische app. Als u meer details over een run wilt bekijken, kiest u de rij voor die run.

   ![Uitvoeringsgeschiedenis van logische app](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history.png)

3. Als u de in- en uitvoer van elke stap wilt bekijken, vouwt u de stap uit die u wilt bekijken. Aan de hand van deze gegevens kunt u problemen met de logische app vaststellen en oplossen.
 
   ![Details van uitvoeringsgeschiedenis van logische app](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history-details.png)

U hebt nu een logische app gemaakt en uitgevoerd die resourcegebeurtenissen controleert met behulp van een gebeurtenisraster en u een e-mailbericht stuurt wanneer deze gebeurtenissen optreden. Daarnaast hebt u geleerd hoe eenvoudig het is om werkstromen te maken voor het automatiseren van processen en hoe u systemen en cloudservices kunt integreren.

U kunt andere configuratiewijzigingen bewaken met gebeurtenisrasters en logische apps, zoals:

* Een virtuele machine krijgt rechten voor toegangsbeheer op basis van rollen (RBAC).
* Een netwerkbeveiligingsgroep (NSG) op een netwerkinterface (NIC) wordt gewijzigd.
* Schijven voor een virtuele machine worden toegevoegd of verwijderd.
* Er wordt een openbaar IP-adres toegewezen aan de NIC van een virtuele machine.

## <a name="clean-up-resources"></a>Resources opschonen

In deze zelfstudie worden resources gebruikt en acties uitgevoerd die kosten voor uw Azure-abonnement met zich mee kunnen brengen. Als u klaar bent met de zelfstudie en het testen, denkt u er dan aan dat u resources waarvoor u geen kosten wilt maken, uitschakelt of verwijdert.

* Als u uw logische app niet meer wilt uitvoeren, maar uw werk niet wilt verwijderen, schakelt u uw app uit. Kies **Overzicht** in het menu van de logische app. Kies **Uitschakelen** op de werkbalk.

  ![De logische app uitschakelen](./media/monitor-virtual-machine-changes-event-grid-logic-app/turn-off-disable-logic-app.png)

  > [!TIP]
  > Als u het menu van de logische app niet ziet, probeer dan terug te gaan naar het Azure-dashboard. Open uw logische app dan opnieuw.

* Als u de logische app permanent wilt verwijderen, kiest u **Overzicht** in het menu van de logische app. Kies **Verwijderen** op de werkbalk. Bevestig dat u uw logische app wilt verwijderen en kies vervolgens **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

* [Aangepaste gebeurtenissen maken en routeren met behulp van Event Grid](../event-grid/custom-event-quickstart.md)
