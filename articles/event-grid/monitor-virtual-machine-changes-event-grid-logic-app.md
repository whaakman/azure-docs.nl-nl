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
ms.date: 05/14/2019
ms.openlocfilehash: 33634773b436114f4a5f2942028710ae50e0e703
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65737141"
---
# <a name="tutorial-monitor-virtual-machine-changes-with-azure-event-grid-and-logic-apps"></a>Zelfstudie: Wijzigingen in virtuele machines bewaken met Azure Event Grid en Logic Apps

U kunt een geautomatiseerde [werkstroom van een logische app](../logic-apps/logic-apps-overview.md) starten wanneer specifieke gebeurtenissen plaatsvinden in Azure-resources of in resources van derden. Deze resources kunnen die gebeurtenissen publiceren naar een [Azure-gebeurtenisraster](../event-grid/overview.md). Het gebeurtenisraster verstuurt die gebeurtenissen vervolgens naar abonnees die webhooks, wachtrijen of [gebeurtenishubs](../event-hubs/event-hubs-what-is-event-hubs.md) als eindpunt hebben. Als een abonnee kan uw logische app wachten op die gebeurtenissen uit het gebeurtenisraster voordat geautomatiseerde werkstromen worden gestart voor het uitvoeren van taken, zonder dat u hiervoor code hoeft te schrijven.

Dit zijn enkele voorbeelden van gebeurtenissen die uitgevers naar abonnees kunnen versturen via de service Azure Event Grid:

* Maken, lezen, bijwerken of verwijderen van een resource. U kunt bijvoorbeeld controleren op wijzigingen die extra kosten op uw Azure-abonnement tot gevolg kunnen hebben en dus van invloed zijn op de hoogte van uw factuur. 
* Toevoegen of verwijderen van een persoon uit een Azure-abonnement.
* Uitvoeren van een bepaalde actie door uw app.
* Nieuw bericht in een wachtrij.

Deze zelfstudie maakt u een logische app die wijzigingen aan een virtuele machine bewaakt, en verzendt van e-mailberichten over deze wijzigingen. Wanneer u een logische app maakt met een gebeurtenisabonnement voor een Azure-resource, worden gebeurtenissen vanuit die resource via een gebeurtenisraster naar de logische app geleid. In de zelfstudie gaat u deze logische app bouwen:

![Overzicht - wijzigingen in virtuele machines bewaken met een gebeurtenisraster en een logische app](./media/monitor-virtual-machine-changes-event-grid-logic-app/monitor-virtual-machine-event-grid-logic-app-overview.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een logische app maken die gebeurtenissen uit een gebeurtenisraster bewaakt.
> * Een voorwaarde toevoegen die specifiek controleert op wijzigingen van de virtuele machine.
> * Een e-mail verzenden wanneer de virtuele machine wordt gewijzigd.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, [registreer u dan nu voor een gratis Azure-account](https://azure.microsoft.com/free/).

* Een e-mailaccount van een e-mailprovider die door Logic Apps wordt ondersteund voor het verzenden van meldingen, zoals Office 365 Outlook, Outlook.com of Gmail. Voor andere providers [kunt u hier de lijst met connectors bekijken](/connectors/). 

  In deze zelfstudie maakt gebruik van een Office 365 Outlook-account. Als u een ander e-mailaccount gebruikt, blijven de algemene stappen gelijk, maar uw gebruikersinterface kan er iets anders uitzien.

* Een [virtuele machine](https://azure.microsoft.com/services/virtual-machines). Als u dit nog niet hebt gedaan, maakt u een virtuele machine via de [maken van een VM-zelfstudie](../virtual-machines/windows/quick-create-portal.md). U [hoeft verder niets te doen](../event-grid/overview.md) om gebeurtenissen te laten publiceren door de virtuele machine.

## <a name="create-blank-logic-app"></a>Een lege, logische app maken

1. Gebruik de referenties van uw Azure-account om u aan melden bij het [Azure Portal](https://portal.azure.com). 

1. Selecteer in het hoofdmenu van Azure **een resource maken** > **integratie** > **logische App**.

   ![Logische app maken](./media/monitor-virtual-machine-changes-event-grid-logic-app/azure-portal-create-logic-app.png)

1. Onder **logische App**, bevatten informatie over uw logische app. Wanneer u klaar bent, kiest u **Maken**.

   ![Gegevens van logische app opgeven](./media/monitor-virtual-machine-changes-event-grid-logic-app/create-logic-app-for-event-grid.png)

   | Eigenschap | Voorgestelde waarde | Description |
   | -------- | --------------- | ----------- |
   | **Naam** | <*logic-app-name*> | Geef een unieke naam voor uw logische app. |
   | **Abonnement** | <*Azure-subscription-name*> | Selecteer hetzelfde Azure-abonnement voor alle services in deze zelfstudie. |
   | **Resourcegroep** | <*Azure-resource-group*> | Selecteer dezelfde resourcegroep voor alle services in deze tutorial. |
   | **Locatie** | <*Azure-datacenter-region*> | Selecteer dezelfde regio voor alle services in deze tutorial. |
   |||

   U hebt nu een Azure-resource voor uw logische app gemaakt. 

1. Nadat Azure uw logische app is geïmplementeerd, wordt de ontwerper van logische Apps ziet u een pagina met een inleidende video en veelgebruikte triggers. Scrol voorbij de video en triggers. 

1. Kies onder **Sjablonen** de optie **Lege logische app**.

   ![Sjabloon voor logische app kiezen](./media/monitor-virtual-machine-changes-event-grid-logic-app/choose-logic-app-template.png)

   De Logic Apps Designer ziet u nu [ *triggers* ](../logic-apps/logic-apps-overview.md#logic-app-concepts) die u gebruiken kunt om uw logische app te starten. Elke logische app moet beginnen met een trigger, die wordt geactiveerd wanneer er een bepaalde gebeurtenis plaatsvindt of wanneer er aan een bepaalde voorwaarde is voldaan. 
   Telkens wanneer de trigger wordt geactiveerd, Azure Logic Apps maakt een werkstroomexemplaar dat wordt uitgevoerd voor uw logische app.

## <a name="add-event-grid-trigger"></a>Event Grid-trigger toevoegen 

Voeg nu de Event Grid-trigger die de resourcegroep voor uw virtuele machine bewaakt toe. 

1. De ontwerpfunctie in het zoekvak, typ 'event grid' als filter. Selecteer deze trigger uit de lijst met triggers: **Wanneer een resourcegebeurtenis optreedt - Azure Event Grid**

   ![Selecteer deze trigger: "Op een resourcegebeurtenis"](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger.png)

1. Wanneer u hierom wordt gevraagd, meldt u zich met Azure Event Grid met de referenties van uw Azure-account. In de **Tenant** lijst waarin de Azure Active Directory-tenant die is gekoppeld aan uw Azure-abonnement, Controleer of de juiste tenant wordt weergegeven.

   ![Aanmelden met uw Azure-referenties](./media/monitor-virtual-machine-changes-event-grid-logic-app/sign-in-event-grid.png)

   > [!NOTE]
   > Als u zich aanmeldt met een persoonlijk Microsoft-account, zoals @outlook.com of @hotmail.com, wordt de trigger voor het gebeurtenisraster mogelijk niet juist weergegeven. Als tijdelijke oplossing kunt u [Verbinding maken met service-principal](../active-directory/develop/howto-create-service-principal-portal.md) kiezen of verifiëren als een lid van de Azure Active Directory die is gekoppeld aan uw Azure-abonnement, bijvoorbeeld *gebruikersnaam*@emailoutlook.onmicrosoft.com.

1. U gaat de logische app nu abonneren op gepubliceerde gebeurtenissen. Geef de details voor uw gebeurtenisabonnement op zoals deze worden beschreven in de volgende tabel:

   ![Details opgeven voor gebeurtenisabonnement](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details-generic.png)

   | Eigenschap | Vereist | Value | Description |
   | -------- | -------- | ----- | ----------- |
   | **Abonnement** | Ja | <*event-publisher-Azure-subscription-name*> | Selecteer de naam voor de Azure-abonnement dat is gekoppeld aan de gebeurtenisuitgever. Voor deze zelfstudie selecteert u de naam van het Azure-abonnement voor uw virtuele machine. |
   | **Resourcetype** | Ja | <*event-publisher-Azure-resource-type*> | Selecteer het type Azure-resource voor de uitgever van gebeurtenissen. Voor deze zelfstudie selecteert u deze waarde voor het bewaken van Azure-resourcegroepen: <p><p>**Microsoft.Resources.ResourceGroups** |
   | **Resourcenaam** |  Ja | <*event-publisher-Azure-resource-name*> | Selecteer de naam voor de Azure-resource voor de uitgever van gebeurtenissen. Deze lijst varieert op basis van het resourcetype dat u hebt geselecteerd. Selecteer de naam voor de Azure-resourcegroep voor uw virtuele machine voor deze zelfstudie. |
   | **Artikel van het Type gebeurtenis** |  Nee | <*event-types*> | Selecteer een of meer specifieke gebeurtenistypen filteren en te verzenden naar uw event grid. Bijvoorbeeld, kunt u deze typen gebeurtenissen om te detecteren wanneer resources worden gewijzigd of verwijderd (optioneel) toevoegen: <p><p>- **Microsoft.Resources.ResourceActionSuccess** <br>- **Microsoft.Resources.ResourceDeleteSuccess** <br>- **Microsoft.Resources.ResourceWriteSuccess** <p>Zie de volgende onderwerpen voor meer informatie: <p><p>- [Informatie over het filteren van gebeurtenissen](../event-grid/event-filtering.md) <br>- [Gebeurtenissen filteren op Event Grid](../event-grid/how-to-filter-events.md) <br>- [Azure Event Grid-gebeurtenisschema voor resourcegroepen](../event-grid/event-schema-resource-groups.md) |
   | **De naam van abonnement** | Nee | <*event-subscription-name*> | Geef een unieke naam op voor het gebeurtenisabonnement. |
   | Optionele instellingen, kiest u **toevoegen van nieuwe parameter**. | Nee | {Zie de beschrijvingen van} | * **Voorvoegselfilter**: Voor deze zelfstudie laat u deze eigenschap leeg. Het standaardgedrag komt overeen met alle waarden. U kunt echter een voorvoegseltekenreeks opgeven als een filter, bijvoorbeeld een pad en een parameter voor een specifieke resource. <p>* **Achtervoegselfilter**: Voor deze zelfstudie laat u deze eigenschap leeg. Het standaardgedrag komt overeen met alle waarden. U kunt echter een achtervoegseltekenreeks opgeven als een filter, bijvoorbeeld een bestandsnaamextensie, als u alleen bepaalde bestandstypen wilt gebruiken. |
   |||

   Wanneer u klaar bent, wordt de trigger Gebeurtenisraster uitzien zoals in dit voorbeeld:

   ![De details van een voorbeeld van de Event Grid-trigger](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details.png)

1. Sla uw logische app op. Kies **Opslaan** op de werkbalk van de ontwerper. Kies de titelbalk van de actie om de details van een actie in uw logische app samen te vouwen en te verbergen.

   ![Uw logische app opslaan](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save.png)

   Wanneer u uw logische app met een trigger voor het gebeurtenisraster opslaat, maakt Azure automatisch een gebeurtenisabonnement voor uw logische app voor de geselecteerde resource. Wanneer de resource nu een gebeurtenis naar het gebeurtenisraster publiceert, wordt die gebeurtenis automatisch door het raster naar de logische app verstuurd. Deze gebeurtenis triggert de logische app, en maakt vervolgens een exemplaar van de werkstroom die u in de volgende stappen definieert. Ten slotte wordt de werkstroom uitgevoerd.

Uw logische app is nu live en luistert naar gebeurtenissen uit het gebeurtenisraster, maar kan verder nog niks totdat u acties aan de werkstroom toevoegt. 

## <a name="add-condition"></a>Voorwaarde toevoegen

Als u de werkstroom van de logische app alleen wilt uitvoeren uitgevoerd als er een bepaalde gebeurtenis optreedt, voegt u een voorwaarde toe waarmee wordt gecontroleerd op 'schrijfbewerkingen' naar de virtuele machine. Als aan deze voorwaarde wordt voldaan (is Waar), verstuurt de logische app een e-mail met meer gegevens van de bijgewerkte virtuele machine.

1. In Logic App Designer, onder de trigger gebeurtenisraster, kies **nieuwe stap**.

   ![Kies de optie 'Nieuwe stap'](./media/monitor-virtual-machine-changes-event-grid-logic-app/choose-new-step-condition.png)

1. Typ 'voorwaarde' als filter in het zoekvak. Selecteer deze actie uit de lijst met acties: **voorwaarde**

   ![Een voorwaarde toevoegen](./media/monitor-virtual-machine-changes-event-grid-logic-app/select-condition.png)

   Er wordt een lege voorwaarde voor uw werkstroom toegevoegd, inclusief de te volgen actiepaden wanneer de voorwaarde Waar of Onwaar is.

   ![Lege voorwaarde](./media/monitor-virtual-machine-changes-event-grid-logic-app/empty-condition.png)

1. Wijzig de naam van de titel van voorwaarde `If a virtual machine in your resource group has changed`. Op de titelbalk van de voorwaarde kiest u de weglatingstekens (**...** ) en selecteer **naam**.

   ![Naam voorwaarde wijzigen](./media/monitor-virtual-machine-changes-event-grid-logic-app/rename-condition.png)

1. Een voorwaarde waarmee wordt gecontroleerd of de gebeurtenis `body` voor een `data` object waar de `operationName` eigenschap is gelijk aan de `Microsoft.Compute/virtualMachines/write` bewerking. Lees hier meer over het [gebeurtenisschema van Event Grid](../event-grid/event-schema.md).

   1. Klink in het linkervakje in de eerste rij onder **En**. Kies in de lijst met dynamische inhoud die wordt weergegeven, **expressie**.

      ![Kies 'Expression'](./media/monitor-virtual-machine-changes-event-grid-logic-app/condition-choose-expression.png)

   1. Voer deze expressie in de expressie-editor, en kies **OK**: 

      `triggerBody()?['data']['operationName']`

      Bijvoorbeeld:

      ![Kies 'Expression'](./media/monitor-virtual-machine-changes-event-grid-logic-app/condition-add-data-operation-name.png)

   1. In het middelste vakje behoud u **is gelijk aan** voor de operator.

   1. Geef deze waarde in het vak rechts:

      `Microsoft.Compute/virtualMachines/write`

   Uw voltooide voorwaarde ziet er nu uit zoals in dit voorbeeld:

   ![Voltooide voorwaarde](./media/monitor-virtual-machine-changes-event-grid-logic-app/complete-condition.png)

   Als u van de ontwerpweergave van overschakelt code voor weergave en terug naar de ontwerpweergave, de expressie die u hebt opgegeven in de voorwaarde wordt omgezet naar de **data.operationName** token:

   ![Opgelost probleem](./media/monitor-virtual-machine-changes-event-grid-logic-app/resolved-condition.png)

1. Sla uw logische app op.

## <a name="send-email-notifications"></a>E-mailmeldingen verzenden

U gaat nu een [*actie*](../logic-apps/logic-apps-overview.md#logic-app-concepts) toevoegen, zodat u een e-mail krijgt wanneer aan de opgegeven voorwaarde wordt voldaan.

1. Kies **Een actie toevoegen** in het vak **Indien waar** van de voorwaarde.

   ![Actie toevoegen voor wanneer de voorwaarde waar is](./media/monitor-virtual-machine-changes-event-grid-logic-app/condition-true-add-action.png)

1. Voer 'een e-mail verzenden' als filter in het zoekvak in. Zoek en selecteer de bijbehorende connector op basis van uw e-mailprovider. Selecteer vervolgens de actie 'e-mail verzenden' voor de connector. Bijvoorbeeld: 

   * Selecteer voor een werk- of schoolaccount van Azure de Office 365 Outlook-connector. 

   * Selecteer voor een persoonlijk Microsoft-account de Outlook.com-connector. 

   * Selecteer voor een Gmail-account de Gmail-connector. 

   In deze zelfstudie gaat door met de Office 365 Outlook-connector. 
   Als u een andere provider gebruikt, blijven de stappen hetzelfde, maar uw gebruikersinterface kan er iets anders uitzien. 

   ![De actie 'Een e-mail verzenden' selecteren](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email.png)

1. Als u nog geen verbinding voor uw e-mailprovider hebt, meldt u zich aan bij uw e-mailaccount wanneer er om verificatie wordt gevraagd.

1. Wijzig de naam van de titel van het e-mailbericht verzenden naar deze titel: `Send email when virtual machine updated`

1. Geef informatie op voor het e-mailbericht zoals wordt aangegeven in de volgende tabel:

   ![Lege e-mailactie](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-empty-email-action.png)

   > [!TIP]
   > Selecteer in de resultaten uit de vorige stappen in uw werkstroom, klikt u op in een invoervak, zodat de lijst met dynamische inhoud wordt weergegeven of kies **dynamische inhoud toevoegen**. Kies voor meer resultaten **meer** voor elke sectie in de lijst. Als u wilt sluiten in de lijst met dynamische inhoud, kies **dynamische inhoud toevoegen** opnieuw.

   | Eigenschap | Vereist | Value | Description |
   | -------- | -------- | ----- | ----------- |
   | **Aan** | Ja | <*ontvanger\@domein*> | Voer het e-mailadres van de ontvanger in. Voor testdoeleinden kunt u uw eigen e-mailadres gebruiken. |
   | **Onderwerp** | Ja | Resource bijgewerkt: **Onderwerp** | Voer de inhoud van het onderwerp van de e-mail in. Voor deze zelfstudie voert u de opgegeven tekst en selecteren van de gebeurtenis **onderwerp** veld. Hier bevat het onderwerp van de e-mail de naam voor de bijgewerkte resource (virtuele machine). |
   | **Hoofdtekst** | Ja | Resource: **Onderwerp** <p>Gebeurtenistype: **Gebeurtenistype**<p>Gebeurtenis-id: **ID**<p>Tijdstip: **Tijdstip van gebeurtenis** | Voer de inhoud voor de hoofdtekst van de e-mail in. Voor deze zelfstudie voert u de opgegeven tekst en selecteren van de gebeurtenis **onderwerp**, **gebeurtenistype**, **ID**, en **gebeurtenistijd** velden zodat uw e-mailbericht bevat de resource die de gebeurtenis, gebeurtenistype, gebeurtenis-timestamp en gebeurtenis-ID voor de update wordt gestart. Voor deze zelfstudie is de bron de Azure-resourcegroep hebt geselecteerd in de trigger. <p>U kunt lege regels toevoegen aan de inhoud door op Shift+Enter te drukken. |
   ||||

   > [!NOTE]
   > Als u een veld selecteert dat een matrix vertegenwoordigt, wordt in de ontwerpfunctie automatisch een lus **Voor elke** toegevoegd rond de actie die naar de matrix verwijst. Op die manier wordt de actie uitgevoerd voor elk matrixitem.

   De e-mailactie ziet er nu ongeveer uit zoals in dit voorbeeld:

   ![Velden selecteren voor opname in e-mailbericht](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email-details.png)

   De voltooide logische app ziet er dan ongeveer zo uit:

   ![Voltooide logische app](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-completed.png)

1. Sla uw logische app op. Kies de titelbalk van een actie om de details van die actie in uw logische app samen te vouwen en te verbergen.

   Uw logische app is nu live, maar er wordt gewacht op wijzigingen van de virtuele machine voordat er iets gebeurt. 
   Ga door naar de volgende sectie als u de logische app nu wilt testen.

## <a name="test-your-logic-app-workflow"></a>De werkstroom van uw logische app testen

1. Om te controleren of uw logische app de opgegeven gebeurtenissen ontvangt, moet u de virtuele machine bijwerken.

   U kunt bijvoorbeeld de grootte van de virtuele machine aanpassen in Azure Portal of [met Azure PowerShell](../virtual-machines/windows/resize-vm.md).

   Binnen enkele ogenblikken moet u dan een e-mailbericht krijgen. Bijvoorbeeld:

   ![E-mail over update van virtuele machine](./media/monitor-virtual-machine-changes-event-grid-logic-app/email.png)

1. Om te controleren van de uitvoeringen en triggergeschiedenis van voor uw logische app, op het menu van uw logische app, selecteer **overzicht**. Als u wilt meer informatie over een uitvoering weergeven, selecteert u de rij voor die run.

   ![Uitvoeringsgeschiedenis van logische app](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history.png)

1. Als u de in- en uitvoer van elke stap wilt bekijken, vouwt u de stap uit die u wilt bekijken. Aan de hand van deze gegevens kunt u problemen met de logische app vaststellen en oplossen.

   ![Details van uitvoeringsgeschiedenis van logische app](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history-details.png)

U hebt nu een logische app gemaakt en uitgevoerd die resourcegebeurtenissen controleert met behulp van een gebeurtenisraster en u een e-mailbericht stuurt wanneer deze gebeurtenissen optreden. Daarnaast hebt u geleerd hoe eenvoudig het is om werkstromen te maken voor het automatiseren van processen en hoe u systemen en cloudservices kunt integreren.

U kunt andere configuratiewijzigingen bewaken met gebeurtenisrasters en logische apps, zoals:

* Een virtuele machine krijgt rechten voor toegangsbeheer op basis van rollen (RBAC).
* Een netwerkbeveiligingsgroep (NSG) op een netwerkinterface (NIC) wordt gewijzigd.
* Schijven voor een virtuele machine worden toegevoegd of verwijderd.
* Er wordt een openbaar IP-adres toegewezen aan de NIC van een virtuele machine.

## <a name="clean-up-resources"></a>Resources opschonen

In deze zelfstudie worden resources gebruikt en acties uitgevoerd die kosten voor uw Azure-abonnement met zich mee kunnen brengen. Als u klaar bent met de zelfstudie en het testen, denkt u er dan aan dat u resources waarvoor u geen kosten wilt maken, uitschakelt of verwijdert.

* Als u uw logische app niet meer wilt uitvoeren, maar uw werk niet wilt verwijderen, schakelt u uw app uit. Selecteer op het menu van uw logische app, **overzicht**. Kies **Uitschakelen** op de werkbalk.

  ![De logische app uitschakelen](./media/monitor-virtual-machine-changes-event-grid-logic-app/turn-off-disable-logic-app.png)

  > [!TIP]
  > Als u het menu van de logische app niet ziet, probeer dan terug te gaan naar het Azure-dashboard. Open uw logische app dan opnieuw.

* Selecteer wilt verwijderen van uw logische app, in het menu logic app permanent **overzicht**. Kies **Verwijderen** op de werkbalk. Bevestig dat u wilt uw logische app verwijderen en kies **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

* [Aangepaste gebeurtenissen maken en routeren met behulp van Event Grid](../event-grid/custom-event-quickstart.md)
