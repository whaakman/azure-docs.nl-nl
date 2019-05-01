---
title: AS2-berichten voor B2B-bedrijfsintegratie - Azure Logic Apps
description: AS2-berichten uitwisselen in Azure Logic Apps met Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 04/22/2019
ms.openlocfilehash: b494f6524e5105a95bc8a24a6fa2521abcca3f7b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64729397"
---
# <a name="exchange-as2-messages-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>AS2-berichten uitwisselen voor B2B-bedrijfsintegratie in Azure Logic Apps met Enterprise Integration Pack

Als u wilt werken met AS2-berichten in Azure Logic Apps, kunt u de AS2-connector, waarmee u triggers en acties voor het beheren van de AS2-communicatie. Bijvoorbeeld, voor het maken van beveiliging en betrouwbaarheid bij het verzenden van berichten, kunt u deze acties:

* [**AS2-bericht coderen** actie](#encode) voor het bieden van versleuteling, digitaal te ondertekenen en bevestigingen via bericht toestand meldingen (MDN), die ter ondersteuning van niet-afwijzing. Bijvoorbeeld, met deze actie is van toepassing AS2/HTTP-headers en voert deze taken uit wanneer geconfigureerd:

  * Symptomen uitgaande berichten.
  * Uitgaande berichten versleutelt.
  * Comprimeren van het bericht.
  * De bestandsnaam in de MIME-header verzendt.

* [**AS2-bericht decoderen** actie](#decode) voor ontsleuteling, digitale ondertekening en bevestigingen via bericht toestand meldingen (MDN) te leveren. Bijvoorbeeld: met deze actie voert deze taken uit: 

  * AS2/HTTP-headers worden verwerkt.
  * Ontvangen MDN's met de oorspronkelijke uitgaande berichten voor overeenstemming zorgt.
  * Updates en records in de database niet-afwijzing verbindt.
  * Schrijft voor AS2-statusrapportage.
  * Uitvoer nettolading inhoud als base64-gecodeerd.
  * Bepaalt of MDN's vereist zijn. Op basis van de AS2 bepaalt-overeenkomst, of MDN's synchrone of asynchrone moeten zijn.
  * Genereert synchrone of asynchrone MDN's op basis van de AS2-overeenkomst.
  * Hiermee stelt u de eigenschappen en correlatietokens op MDN's.

  Deze actie wordt ook uitgevoerd voor deze taken wanneer dit wordt geconfigureerd:

  * Controleert of de handtekening.
  * Hiermee ontsleutelt u de berichten.
  * Decomprimeert het bericht. 
  * Controleer en bericht-ID duplicaten weigeren.

In dit artikel wordt uitgelegd hoe om toe te voegen de AS2-codering en decodering acties aan een bestaande logische app.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u een Azure-abonnement nog geen [zich aanmelden voor een gratis Azure-account](https://azure.microsoft.com/free/).

* De logische app van waar u het gebruik van de AS2-connector en een trigger die de werkstroom van uw logische app wordt gestart. De AS2-connector biedt alleen acties, geen triggers. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps](../logic-apps/logic-apps-overview.md) en [Quick Start: Maak uw eerste logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Een [integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) die is gekoppeld aan uw Azure-abonnement en gekoppeld aan de logische app waar u van plan bent om de AS2-connector te gebruiken. Beide uw logische app en integratie-account moet zich in dezelfde locatie of Azure-regio.

* Ten minste twee [handelspartners](../logic-apps/logic-apps-enterprise-integration-partners.md) dat u al hebt gedefinieerd in uw integratie-account met behulp van de kwalificatie van de AS2-identiteit.

* Voordat u de AS2-connector gebruiken kunt, moet u een AS2 [overeenkomst](../logic-apps/logic-apps-enterprise-integration-agreements.md) tussen uw handelspartners en store die overeenkomst in het integratieaccount.

* Als u [Azure Key Vault](../key-vault/key-vault-overview.md) voor certificaatbeheer, controleert u dat de sleutels van uw kluis toestaan de **versleutelen** en **ontsleutelen** bewerkingen. Anders mislukt de codering en decodering van acties.

  In de Azure-portal, gaat u naar uw key vault, bekijken van de kluissleutel van uw **operations toegestaan**, en zorg ervoor dat de **versleutelen** en **ontsleutelen** bewerkingen zijn geselecteerd.

  ![Key vault-bewerkingen controleren](media/logic-apps-enterprise-integration-as2/vault-key-permitted-operations.png)

<a name="encode"></a>

## <a name="encode-as2-messages"></a>AS2-berichten coderen

1. Als u dat nog niet gedaan hebt, in de [Azure-portal](https://portal.azure.com), opent u uw logische app in Logic App Designer.

1. In de ontwerpfunctie voor de nieuwe actie toevoegen aan uw logische app. 

1. Onder **een actie kiezen** en wordt de zoekopdracht Kies **alle**. Voer 'coderen as2' in het zoekvak en selecteer deze actie: **AS2-bericht coderen**.

   ![Selecteer "Coderen voor AS2-bericht"](./media/logic-apps-enterprise-integration-as2/select-as2-encode.png)

1. Als u geen een bestaande verbinding naar uw integratie-account, wordt u gevraagd om nu deze verbinding te maken. Naam van de verbinding, selecteer de integratieaccount dat u wilt verbinden en kies **maken**.

   ![Maak verbinding met de integratie-account](./media/logic-apps-enterprise-integration-as2/as2-create-connection.png)  
 
1. Nu bevatten informatie voor deze eigenschappen:

   | Eigenschap | Description |
   |----------|-------------|
   | **AS2-From** | De id van de afzender van het bericht zoals opgegeven door de AS2-overeenkomst |
   | **AS2-To** | De id voor het ontvangen bericht zoals opgegeven door de AS2-overeenkomst |
   | **body** | De berichtnettolading van het |
   |||

   Bijvoorbeeld:

   ![Eigenschappen van berichten coderen](./media/logic-apps-enterprise-integration-as2/as2-message-encoding-details.png)

<a name="decode"></a>

## <a name="decode-as2-messages"></a>AS2-berichten decoderen

1. Als u dat nog niet gedaan hebt, in de [Azure-portal](https://portal.azure.com), opent u uw logische app in Logic App Designer.

1. In de ontwerpfunctie voor de nieuwe actie toevoegen aan uw logische app. 

1. Onder **een actie kiezen** en wordt de zoekopdracht Kies **alle**. Voer 'as2-decodering' in het zoekvak en selecteer deze actie: **AS2-bericht decoderen**

   ![Selecteer "Decoderen AS2-bericht"](media/logic-apps-enterprise-integration-as2/select-as2-decode.png)

1. Als u geen een bestaande verbinding naar uw integratie-account, wordt u gevraagd om nu deze verbinding te maken. Naam van de verbinding, selecteer de integratieaccount dat u wilt verbinden en kies **maken**.

   ![Maak verbinding met de integratie-account](./media/logic-apps-enterprise-integration-as2/as2-create-connection.png)  

1. Voor **hoofdtekst** en **Headers**, selecteert u deze waarden uit de vorige trigger of actie-uitvoer.

   Stel bijvoorbeeld dat uw logische app ontvangt berichten via een trigger voor de aanvraag. U kunt de uitvoer van deze trigger selecteren.

   ![Selecteer hoofdtekst en koppen uit de uitvoer van de aanvraag](media/logic-apps-enterprise-integration-as2/as2-message-decoding-details.png) 

## <a name="sample"></a>Voorbeeld

Als u wilt proberen om de implementatie van een volledig operationeel logische app en voorbeeld AS2-scenario, Zie de [AS2 scenario en de sjabloon voor logische app](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie, zoals triggers en acties limieten, zoals is beschreven in van de connector OpenAPI (voorheen Swagger)-bestand, raadpleegt u de [van de connector-verwijzingspagina](/connectors/as2/).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md)
