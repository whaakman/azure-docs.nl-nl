---
title: Verbinding maken met Dynamics 365 - Azure Logic Apps | Microsoft Docs
description: Maken en beheren van records met Dynamics 365 (online) REST-API's en Azure Logic Apps
author: Mattp123
ms.author: matp
ms.service: logic-apps
services: logic-apps
ms.reviewer: estfan, LADocs
ms.suite: integration
ms.topic: article
ms.date: 08/18/2018
tags: connectors
ms.openlocfilehash: b1ff93f1e03e047ad5ac00259c1aa53afda0c76d
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42818937"
---
# <a name="manage-dynamics-365-records-with-azure-logic-apps"></a>Dynamics 365-records met Azure Logic Apps beheren

Met Azure Logic Apps en de Dynamics 365-connector, kunt u geautomatiseerde taken en werkstromen op basis van de records in Dynamics 365 maken. Uw werkstromen kunnen maken records, update-items, Ga terug en meer in uw Dynamics 365-account. U kunt acties opnemen in uw logische apps die te antwoorden krijgen van Dynamics 365 en de uitvoer beschikbaar voor andere acties. Wanneer een item in Dynamics 365 is bijgewerkt, kunt u bijvoorbeeld een e-mailbericht met behulp van Office 365 verzenden.

Dit artikel wordt beschreven hoe u een logische app die u een taak in Dynamics 365 maakt wanneer een nieuwe leadrecord wordt gemaakt in Dynamics 365 kunt samenstellen.
Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* Een [Dynamics 365-account](https://dynamics.microsoft.com)

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app waar u toegang tot uw Dynamics 365-account. Als uw logische app met een Dynamics 365-trigger wilt, moet u een [lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

## <a name="add-dynamics-365-trigger"></a>Dynamics 365-trigger toevoegen

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

Eerst een Dynamics 365-trigger die wordt geactiveerd wanneer een nieuwe leadrecord wordt weergegeven in Dynamics 365 toevoegen.

1. In de [Azure-portal](https://portal.azure.com), open uw lege, logische app in Logic App Designer, als dit niet al geopend.

1. Typ 'Dynamics 365 ' als filter in het zoekvak. In dit voorbeeld onder de lijst met triggers, selecteer deze trigger: **wanneer een record wordt gemaakt**

   ![Trigger selecteren](./media/connectors-create-api-crmonline/select-dynamics-365-trigger.png)

1. Als u wordt gevraagd of u aanmelden bij Dynamics 365, nu aanmelden.

1. Geef de details van deze trigger:

   | Eigenschap | Vereist | Beschrijving | 
   |----------|----------|-------------| 
   | **De naam van organisatie** | Ja | De naam voor Dynamics 365-exemplaar van uw organisatie moeten worden bewaakt, bijvoorbeeld 'Contoso' |
   | **Naam van de entiteit** | Ja | De naam voor de entiteit te bewaken, bijvoorbeeld leidt' " | 
   | **Frequentie** | Ja | De tijdseenheid voor gebruik met intervallen bij het controleren op updates met betrekking tot de trigger |
   | **Interval** | Ja | Het aantal seconden, minuten, uren, dagen, weken of maanden die voordat de volgende controle verstrijken |
   ||| 

   ![Triggerdetails](./media/connectors-create-api-crmonline/trigger-details.png)

## <a name="add-dynamics-365-action"></a>Dynamics 365-actie toevoegen

Nu de Dynamics 365-actie die u een taakrecord voor een nieuwe leadrecord maakt toevoegen.

1. Kies onder de trigger **nieuwe stap**.

1. Typ 'Dynamics 365 ' als filter in het zoekvak. Selecteer in de lijst met acties met deze actie: **een nieuwe record maken**

   ![Actie selecteren](./media/connectors-create-api-crmonline/select-action.png)

1. Geef de volgende actiegegevens:

   | Eigenschap | Vereist | Beschrijving | 
   |----------|----------|-------------| 
   | **De naam van organisatie** | Ja | Het Dynamics 365-exemplaar waarop u maken van de record wilt, hoeft niet dezelfde te zijn van hetzelfde exemplaar in de trigger, maar het 'Contoso' is in dit voorbeeld |
   | **Naam van de entiteit** | Ja | De entiteit waarin u wilt maken van de record, bijvoorbeeld "Taken" | 
   | | |

   ![Actiedetails](./media/connectors-create-api-crmonline/action-details.png)

1. Wanneer de **onderwerp** vak wordt weergegeven in de actie, klikt u in de **onderwerp** vak, zodat de lijst met dynamische inhoud wordt weergegeven. In deze lijst, selecteer de veldwaarden om op te nemen in de taakrecord die zijn gekoppeld aan een record met de nieuwe potentiële klant:

   | Veld | Beschrijving | 
   |-------|-------------| 
   | **Achternaam** | De achternaam van de potentiële klant als de primaire contactpersoon in de record |
   | **Onderwerp** | De beschrijvende naam voor de lead in de record | 
   | | | 

   ![De details van record](./media/connectors-create-api-crmonline/create-record-details.png)

1. Kies op de werkbalk van de ontwerper **opslaan** voor uw logische app. 

1. Kies voor het handmatig starten van de logische app op de werkbalk van de ontwerper, **uitvoeren**.

   ![Logische app uitvoeren](./media/connectors-create-api-crmonline/designer-toolbar-run.png)

1. Nu een leadrecord in Dynamics 365 maken, zodat u kunt uw logic app-werkstroom activeren.

## <a name="add-filter-or-query"></a>Filter of query toevoegen

Als u wilt filteren van gegevens in Dynamics 365-actie opgeeft, kies **geavanceerde opties weergeven** in actie. U kunt vervolgens een filter of een order toevoegen door query.
Bijvoorbeeld, kunt u een filterquery voor het ophalen van alleen de actieve accounts en deze records te sorteren op naam. Volg deze stappen voor deze taak:

1. Onder **filterquery**, deze OData-filterquery invoeren: `statuscode eq 1`

2. Onder **Order By**, wanneer de lijst met dynamische inhoud wordt weergegeven, selecteert u **accountnaam**. 

   ![Filter en de volgorde opgeven](./media/connectors-create-api-crmonline/advanced-options.png)

Zie voor meer informatie deze queryopties voor Dynamics 365 Customer Engagement Web API-systeem: 

* [$filter](https://docs.microsoft.com/dynamics365/customer-engagement/developer/webapi/query-data-web-api#filter-results)
* [$orderby](https://docs.microsoft.com/dynamics365/customer-engagement/developer/webapi/query-data-web-api#order-results)

### <a name="best-practices-for-advanced-options"></a>Aanbevolen procedures voor geavanceerde opties

Wanneer u een waarde voor een veld in een actie of trigger opgeeft, het gegevenstype van de waarde moet overeenkomen met het veldtype of u handmatig invoeren van de waarde of selecteert u de waarde in de lijst met dynamische inhoud.

Deze tabel worden enkele van de veldtypen en de vereiste gegevenstypen voor hun waarden beschreven.

| Veldtype | Gegevens van het type vereist | Beschrijving | 
|------------|--------------------|-------------|
| Tekstvelden | Eén tekstregel | Deze velden vereist één regel tekst of dynamische inhoud het teksttype heeft. <p><p>*Voorbeeld van de velden*: **beschrijving** en **categorie** | 
| Velden voor gehele getallen | Geheel getal | Sommige velden vereist geheel getal of dynamische inhoud het type geheel getal zijn heeft. <p><p>*Voorbeeld van de velden*: **percentage voltooid** en **duur** | 
| Datumvelden | Datum en tijd | Sommige velden vereist een datum met de notatie dd-mm-jjjj of dynamische inhoud het datumtype heeft. <p><p>*Voorbeeld van de velden*: **gemaakt op**, **begindatum**, **werkelijke begindatum**, **werkelijk einde**, en **vervaldatum** | 
| Velden waarvoor een record-ID en een Opzoektype Typ | Primaire sleutel | Sommige velden die verwijzen naar een andere entiteitsrecord vereist zowel een record-ID en een Opzoektype. | 
||||

Voortbouwend op deze veldtypen, vindt hier u voorbeeld van de velden in Dynamics 365-triggers en acties waarvoor een record-ID en het Opzoektype zijn vereist. Deze vereiste betekent dat de waarden die u in de lijst met dynamische selecteert werkt niet. 

| Veld | Beschrijving | 
|-------|-------------|
| **Eigenaar** | Moet een geldige gebruikersnaam of team record-ID. | 
| **Eigenaarstype** | Moet een **systemusers** of **teams**. | 
| **Met betrekking tot** | Moet een geldige record-ID, zoals een account-ID of neem contact op met de record-ID. | 
| **Met betrekking tot het Type** | Moet een lookup-type, zoals **accounts** of **contactpersonen**. | 
| **De klant** | Moet een geldige record-ID, zoals een account-ID of neem contact op met de record-ID. | 
| **Klanttype** | Moet de lookup-type, zoals **accounts** of **contactpersonen**. | 
|||

In dit voorbeeld wordt de actie met de naam **een nieuwe record maken** maakt een nieuwe taakrecord: 

![Taakrecord met de record-id's en lookup-typen maken](./media/connectors-create-api-crmonline/create-record-advanced.png)

Deze actie wordt de taakrecord toegewezen aan een specifieke gebruikers-ID of het team record-ID, op basis van de record-ID in de **eigenaar** veld- en lookup typt u in de **eigenaarstype** veld:

![De record-ID en een Opzoektype eigenaarstype](./media/connectors-create-api-crmonline/owner-record-id-and-lookup-type.png)

Deze actie wordt ook een accountrecord die is gekoppeld aan de record-ID toegevoegd in de **met betrekking tot** veld- en lookup typt u in de **met betrekking tot het Type** veld: 

![Met betrekking tot ID en een Opzoektype recordtype](./media/connectors-create-api-crmonline/regarding-record-id-lookup-type-account.png)

## <a name="find-record-id"></a>Record-ID vinden

Als u wilt zoeken in een record-ID, de volgende stappen uit: 

1. Open in Dynamics 365, een record, zoals een accountrecord.

2. Kies een van deze stappen uit op de werkbalk Acties:

   * Kies **Pop-outmodus**. ![mailprogramma record](./media/connectors-create-api-crmonline/popout-record.png) 
   * Kies **e-MAILBERICHT een koppeling** , zodat u kunt de volledige URL naar uw standaard-e-mailprogramma kopiëren.

   De record-ID in de URL tussen weergegeven de `%7b` en `%7d` coderingstekens:

   ![Record-ID vinden](./media/connectors-create-api-crmonline/find-record-ID.png)

## <a name="troubleshoot-failed-runs"></a>Mislukte uitvoeringen oplossen

Als u wilt zoeken en mislukte stappen in uw logische app, kunt u de geschiedenis van uitvoeringen van uw logische app, status, invoer, uitvoer, enzovoort weergeven.

1. Selecteer in de Azure portal, in het hoofdmenu van uw logische app, **overzicht**. In de **geschiedenis van uitvoeringen** sectie melding ziet de uitvoering statussen voor uw logische app, selecteert u een mislukte uitvoering voor meer informatie.

   ![Status van logische app](./media/connectors-create-api-crmonline/run-history.png)

1. Een mislukte stap uitbreiden, zodat u meer informatie kunt bekijken. 

   ![Vouw de mislukte stap](./media/connectors-create-api-crmonline/expand-failed-step.png)

1. Bekijk de details van de stap, zoals de invoer en uitvoer, waarmee u kunnen de oorzaak van de fout vinden.

   ![Mislukte stap - invoer en uitvoer](./media/connectors-create-api-crmonline/expand-failed-step-inputs-outputs.png)

Zie voor meer informatie over het oplossen van logische apps [diagnose van fouten in logische Apps](../logic-apps/logic-apps-diagnosing-failures.md).

## <a name="connector-reference"></a>Connector-verwijzing

Zie voor technische details, zoals triggers en acties limieten, zoals is beschreven in de Swagger-bestand van de connector, de [van de connector-verwijzingspagina](/connectors/crm/). 

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
