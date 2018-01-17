---
title: Verbinding maken met Dynamics 365 (online) van Azure Logic Apps | Microsoft Docs
description: Logic app werkstromen maken die Dynamics 365 (online) entiteiten via de API die door de connector Dynamics 365 beheren
services: logic-apps
cloud: Azure Stack
author: Mattp123
manager: anneta
documentationcenter: 
tags: connectors
ms.assetid: 0dc2abef-7d2c-4a2d-87ca-fad21367d135
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: matp; LADocs
ms.openlocfilehash: d35647921ff540167a3a591fb489d3bab031a5c1
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="connect-to-dynamics-365-from-logic-app-workflows"></a>Verbinding maken met Dynamics 365 vanuit logic app-werkstromen

Met Logic Apps kunt u verbinding met Dynamics 365 (online) en nuttig business stromen die records maken, bijwerken items of retourneren een lijst met records maken. Met de connector Dynamics 365 kunt u:

* Bouw uw zakelijke flow op basis van de gegevens die u vanuit Dynamics 365 (online).
* Gebruik de acties die reageert en vervolgens de uitvoer beschikbaar voor andere acties. Wanneer een item wordt bijgewerkt in Dynamics 365 (online), kunt u bijvoorbeeld een e-mailbericht met Office 365 verzenden.

Dit onderwerp leest u het maken van een logische app die u een taak in Dynamics 365 maakt zodra er een nieuwe lead in Dynamics 365 is gemaakt.

## <a name="prerequisites"></a>Vereisten
* Een Azure-account.
* Een account met Dynamics 365 (online).

## <a name="create-a-task-when-a-new-lead-is-created-in-dynamics-365"></a>Een taak wordt gemaakt wanneer een nieuwe lead in Dynamics 365 wordt gemaakt

1.  [Aanmelden bij Azure](https://portal.azure.com).

2.  Typ in het Azure search `Logic apps`, en druk op ENTER.

      ![Logische Apps zoeken](./media/connectors-create-api-crmonline/find-logic-apps.png)

3.  Onder **Logic apps**, klikt u op **toevoegen**.

      ![LogicApp toevoegen](./media/connectors-create-api-crmonline/add-logic-app.png)

4.  Voor het maken van de logische app, voer de **naam**, **abonnement**, **resourcegroep**, en **locatie** velden en klik vervolgens op **Maken**.

5.  Selecteer de nieuwe logische app. Wanneer u ontvangt de **implementatie is voltooid** melding, klikt u op **vernieuwen**.

6.  Onder **ontwikkelingsprogramma's**, klikt u op **Logic App-ontwerper**. Klik in de lijst met sjablonen op **lege logische App**.

7.  Typ in het zoekvak `Dynamics 365`. Selecteer in de lijst van de triggers Dynamics 365 **Dynamics 365 – wanneer een record gemaakt**.

8.  Als u wordt gevraagd aan te melden bij Dynamics 365, moet u dit nu doen.

9.  Voer de volgende gegevens in de trigger-details:

  * **Naam van de organisatie**. Selecteer het Dynamics 365-exemplaar dat u wilt dat de logische app om te luisteren.

  * **Naam van de entiteit**. Selecteer de entiteit die u wilt luisteren. Deze gebeurtenis fungeert als een trigger voor het starten van de logische app. 
  In dit overzicht **leidt** is geselecteerd.

  * **Hoe vaak wilt u om te controleren op items?** Deze waarden instellen hoe vaak de logische app controleert op updates met betrekking tot de trigger. De standaardinstelling is om te controleren op updates elke drie minuten.

    * **Frequentie**. Selecteer seconden, minuten, uren of dagen.

    * **Interval**. Voer het aantal seconden, minuten, uren of dagen dat u wilt laten verstrijken voordat de controle van de volgende.

      ![Details van de logica voor App Trigger](./media/connectors-create-api-crmonline/trigger-details.png)

10. Klik op **nieuwe stap**, en klik vervolgens op **een actie toevoegen**.

11. Typ in het zoekvak `Dynamics 365`. Selecteer in de lijst met acties **Dynamics 365 – Maak een nieuwe record**.

12. Voer de volgende informatie in:

    * **Naam van de organisatie**. Selecteer de Dynamics 365 instantie waar u de stroom om de record te maken. 
    Merk op dat dit exemplaar hoeft te zijn van hetzelfde exemplaar waar de gebeurtenis wordt geactiveerd vanuit.

    * **Naam van de entiteit**. Selecteer de entiteit die u een record maken wilt als de gebeurtenis wordt geactiveerd. 
    In dit overzicht **taken** is geselecteerd.

13. Klik in de **onderwerp** vak dat wordt weergegeven. In de dynamische inhoud lijst die wordt weergegeven, kunt u een van deze velden selecteren:

    * **Achternaam**. Voegt de achternaam van de lead als u dit veld in het veld onderwerp voor de taak in wanneer de taak wordt gemaakt.
    * **Onderwerp**. Als u dit veld voegt in het veld onderwerp voor de lead in het veld onderwerp voor de taak wanneer de taak wordt gemaakt. 
    Klik op **onderwerp** om toe te voegen die u wilt de **onderwerp** vak.

      ![Logische App maken nieuwe records details](./media/connectors-create-api-crmonline/create-record-details.png)

14. Klik op de werkbalk Logic App-ontwerper **opslaan**.

    ![Logic App-ontwerper werkbalk opslaan](./media/connectors-create-api-crmonline/designer-toolbar-save.png)

15. Voor het starten van de logische App, klikt u op **uitvoeren**.

    ![Logic App-ontwerper werkbalk opslaan](./media/connectors-create-api-crmonline/designer-toolbar-run.png)

16. Nu een leadrecord maken in Dynamics 365 voor verkoop en uw flow in actie zien!

## <a name="set-advanced-options-for-a-logic-app-step"></a>Geavanceerde opties voor de stap van een logische app instellen

Als u wilt filteren van gegevens in een logische app stap opgeven, klikt u op **geavanceerde opties weergeven** in deze stap voegt u een filter of de volgorde door query.

Bijvoorbeeld, kunt u een filterquery alleen actieve accounts en volgorde ophalen door de accountnaam. Als u wilt uitvoeren van deze taak voert u de OData-filter-query `statuscode eq 1`, en selecteer **accountnaam** uit de lijst met dynamische inhoud. Meer informatie: [MSDN: $filter](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_1) en [$orderby](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_2).

![Geavanceerde opties voor logische-app](./media/connectors-create-api-crmonline/advanced-options.png)

### <a name="best-practices-when-using-advanced-options"></a>Aanbevolen procedures voor het gebruik van geavanceerde opties

Wanneer u een waarde aan een veld toevoegt, moet u het veldtype overeenkomen met of u typt u een waarde of Selecteer een waarde in de lijst met dynamische inhoud.

Veldtype  |Gebruiksinstructies  |Waar vind ik  |Naam  |Gegevenstype  
---------|---------|---------|---------|---------
Tekstvelden|Tekstvelden vereisen een enkele regel tekst of dynamische inhoud die is een type tekstveld. Voorbeelden hiervan zijn de categorie en subcategorie velden.|Instellingen > Aanpassingen > past u het systeem > entiteiten > taak > velden |category |Enkele regel tekst        
Velden geheel getal | Sommige velden vereist geheel getal of dynamische inhoud die een veld van het type geheel getal is. Voorbeelden zijn percentage voltooid en duur. |Instellingen > Aanpassingen > past u het systeem > entiteiten > taak > velden |PercentComplete |Geheel getal         
Datumvelden | Sommige velden vereisen een datum is opgegeven in de notatie dd-mm-jjjj of dynamische inhoud die is een veld van het type datum. Voorbeelden zijn gemaakt op, begindatum, werkelijke begindatum laatste op de tijd houdt, Werkelijk einde en vervaldatum. | Instellingen > Aanpassingen > past u het systeem > entiteiten > taak > velden |createdon |Datum en tijd
Typ velden waarvoor zowel een record-ID en lookup |Sommige velden die verwijzen naar een andere entiteitsrecord vereisen zowel de record-ID en het Opzoektype. |Instellingen > Aanpassingen > past u het systeem > entiteiten > Account > velden  | accountid  | Primaire sleutel

### <a name="more-examples-of-fields-that-require-both-a-record-id-and-lookup-type"></a>Typ voor meer voorbeelden van de velden die vereisen een record-ID en de lookup dat
Voortbouwend op de vorige tabel, vindt hier u meer voorbeelden van de velden die niet met waarden die in de lijst met dynamische inhoud geselecteerd werken. Deze velden vereisen in plaats daarvan beide een lookup-ID en het recordtype in de velden in PowerApps ingevoerd.  
* Eigenaar en eigenaarstype. Het veld eigenaar moet een geldige gebruiker of het team record-ID. Het Type van de eigenaar moet een **systemusers** of **teams**.
* De klant en het klanttype. Het veld moet een geldig account of neem contact op met de record-ID. Het Type van de eigenaar moet een **accounts** of **contactpersonen**.
* Met betrekking tot en met betrekking tot Type. Het veld betreft moet een geldige record-ID, zoals een account of neem contact op met de record-ID. Het Type met betrekking tot moet het type lookup voor de record, zoals **accounts** of **contactpersonen**.

Het volgende voorbeeld van taak maken actie wordt een record voor een account dat overeenkomt met de id van de record toe te voegen aan het betreffende veld van de taak.

![Stroom recordId en het type account](./media/connectors-create-api-crmonline/recordid-type-account.png)

In dit voorbeeld wordt de taak ook toegewezen aan een specifieke gebruiker op basis van de gebruiker record-ID.

![Stroom recordId en het type account](./media/connectors-create-api-crmonline/recordid-type-user.png)

Een record-ID, Zie de volgende sectie: *vinden van de record-ID*

## <a name="find-the-record-id"></a>De record-ID vinden

1. Open een record bestaat, zoals een record.

2. Klik op de werkbalk Acties **Pop uit** ![popout record](./media/connectors-create-api-crmonline/popout-record.png).
U kunt ook de acties op de werkbalk om te kopiëren van de volledige URL in het standaard e-mailprogramma, **e-MAILBERICHT een koppeling**.

   De record-ID wordt weergegeven tussen de 7 ter en %7 %d tekens van de URL-codering.

   ![Stroom recordId en het type account](./media/connectors-create-api-crmonline/recordid.png)

## <a name="troubleshooting"></a>Problemen oplossen
Voor het oplossen van een mislukte stap in een logische app, moet u de statusdetails van de gebeurtenis weergeven.

1. Onder **Logic Apps**, selecteer uw logische app en klik vervolgens op **overzicht**. 

   In het samenvattingsgebied wordt weergegeven en de uitvoeringsstatus biedt voor de logische app. 

   ![Logische app uitvoeringsstatus](./media/connectors-create-api-crmonline/tshoot1.png)

2. Als u meer informatie over eventuele mislukte wordt uitgevoerd, klikt u op de gebeurtenis is mislukt. Als een mislukte stap wilt uitbreiden, klikt u op die stap.

   ![Vouw de mislukte stap](./media/connectors-create-api-crmonline/tshoot2.png)

   De details van de stap worden weergegeven en de oorzaak van het probleem kunnen oplossen.

   ![Details van de stap is mislukt](./media/connectors-create-api-crmonline/tshoot3.png)

Zie voor meer informatie over het oplossen van logische apps [logic app fouten opsporen](../logic-apps/logic-apps-diagnosing-failures.md).

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook de beperkingen in de [connector details](/connectors/crm/). 

## <a name="next-steps"></a>Volgende stappen
Bekijk de beschikbare connectors in Logic Apps op onze [API's lijst](apis-list.md).
