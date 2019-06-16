---
title: Maken en beheren van handelspartnerovereenkomsten - Azure Logic Apps
description: Maken en beheren van overeenkomsten tussen zakelijke partners met behulp van Azure Logic Apps en Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 447ffb8e-3e91-4403-872b-2f496495899d
ms.date: 04/05/2019
ms.openlocfilehash: 26d653b873e959f0804e0456ed87ee68c39413e5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64720668"
---
# <a name="create-and-manage-trading-partner-agreements-by-using-azure-logic-apps-and-enterprise-integration-pack"></a>Maken en beheren van handelspartnerovereenkomsten met behulp van Azure Logic Apps en Enterprise Integration Pack

Een [handelspartner](../logic-apps/logic-apps-enterprise-integration-partners.md) 
*overeenkomst* helpt organisaties en bedrijven naadloos met elkaar communiceren met het definiëren van de te gebruiken tijdens het uitwisselen van de specifieke industriestandaard-protocol berichten van de Business-to-business (B2B). Overeenkomsten bieden algemene voordelen, bijvoorbeeld:

* Kunnen organisaties voor het uitwisselen van gegevens met behulp van een bekende indeling.
* De efficiëntie verbeteren bij het uitvoeren van B2B-transacties.
* Zijn gemakkelijk te maken, beheren en gebruiken voor het bouwen van zakelijke oplossingen voor integratie.

Dit artikel wordt beschreven hoe u kunt maken van een AS2, EDIFACT of X12 overeenkomst die u gebruiken kunt bij het bouwen van enterprise integratieoplossingen voor B2B-scenario's met behulp van de [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md) en [Azure Logic Apps](../logic-apps/logic-apps-overview.md). Nadat u een overeenkomst hebt gemaakt, kunt u vervolgens de AS2, EDIFACT of X12 connectors voor het uitwisselen van B2B-berichten.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u een Azure-abonnement nog geen [zich aanmelden voor een gratis Azure-account](https://azure.microsoft.com/free/).

* Een [integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) voor het opslaan van uw overeenkomst en andere B2B-artefacten. Deze integratie-account moet worden gekoppeld aan uw Azure-abonnement.

* Ten minste twee [handelspartners](../logic-apps/logic-apps-enterprise-integration-partners.md) die u al hebt gemaakt in uw integratie-account. Een overeenkomst vereist zowel een gastpartner als een hostpartner. Beide partners moeten de kwalificatie van de dezelfde 'bedrijfsidentiteit' gebruiken als de overeenkomst die u maken wilt, zoals AS2, X 12 of EDIFACT.

* Optioneel: De logische app waar u het gebruik van uw overeenkomst en een trigger die de werkstroom van uw logische app wordt gestart. Als u wilt uw integratieaccount en B2B-artefacten maken, hoeft u niet een logische app. Voordat uw logische app u de B2B-artefacten in uw integratie-account gebruiken kunt, moet u uw integratie-account koppelen aan uw logische app. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps](../logic-apps/logic-apps-overview.md) en [Quick Start: Maak uw eerste logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-agreements"></a>Overeenkomsten maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
Selecteer in het hoofdmenu van Azure **alle services**. Typ 'integration' als filter in het zoekvak. Selecteer deze resource in de resultaten: **Integratieaccounts**

   ![Uw integratie-account zoeken](./media/logic-apps-enterprise-integration-agreements/find-integration-accounts.png)

1. Onder **integratieaccounts**, selecteert u de integratieaccount waar u wilt maken van de overeenkomst.

   ![Selecteer het integratieaccount waar moet worden gemaakt van de overeenkomst](./media/logic-apps-enterprise-integration-agreements/select-integration-account.png)

1. In het rechterdeelvenster onder **onderdelen**, kiest u de **overeenkomsten** tegel.

   ![Kies "Overeenkomsten"](./media/logic-apps-enterprise-integration-agreements/agreement-1.png)

1. Onder **overeenkomsten**, kiest u **toevoegen**. In de **toevoegen** deelvenster bevatten informatie over uw overeenkomst, bijvoorbeeld:

   ![Kies 'Toevoegen'](./media/logic-apps-enterprise-integration-agreements/agreement-2.png)

   | Eigenschap | Vereist | Value | Beschrijving |
   |----------|----------|-------|-------------|
   | **Naam** | Ja | <*agreement-name*> | De naam van uw overeenkomst |
   | **Overeenkomsttype selecteren** | Ja | **AS2**, **X12**, of **EDIFACT** | Het protocoltype voor uw overeenkomst. Wanneer u uw overeenkomst-bestand maakt, moet het overeenkomsttype overeenkomen met de inhoud van dat bestand. | |  
   | **Hostpartner** | Ja | <*host-partner-name*> | De hostpartner vertegenwoordigt de organisatie die Hiermee geeft u de overeenkomst |
   | **Identiteit van de host** | Ja | <*host-partner-identifier*> | De hostpartner-id |
   | **Gastpartner** | Ja | <*naam van een gast van partner*> | De gastpartner vertegenwoordigt de organisatie die wordt zakendoen met de hostpartner |
   | **Gastidentiteit** | Ja | <*guest-partner-identifier*> | De gastpartner-id |
   | **Ontvangstinstellingen** | Varieert | Varieert | Deze eigenschappen opgeven voor het verwerken van alle binnenkomende berichten ontvangen door de overeenkomst. Zie voor meer informatie de betreffende overeenkomsttype selecteren: <p>- [Instellingen voor AS2-bericht](../logic-apps/logic-apps-enterprise-integration-as2-message-settings.md) <br>- [Instellingen van de EDIFACT-bericht](logic-apps-enterprise-integration-edifact.md) <br>- [Instellingen voor X12 message](logic-apps-enterprise-integration-x12.md) |
   | **Verzendinstellingen** | Varieert | Varieert | Deze eigenschappen opgeven voor het verwerken van alle uitgaande berichten verzonden door de overeenkomst. Zie voor meer informatie de betreffende overeenkomsttype selecteren: <p>- [Instellingen voor AS2-bericht](../logic-apps/logic-apps-enterprise-integration-as2-message-settings.md) <br>- [Instellingen van de EDIFACT-bericht](logic-apps-enterprise-integration-edifact.md) <br>- [Instellingen voor X12 message](logic-apps-enterprise-integration-x12.md) |
   |||||

1. Wanneer u klaar bent voor het maken van uw overeenkomst op de **toevoegen** pagina, kies **OK**, en Ga terug naar uw integratie-account.

   De **overeenkomsten** lijst ziet u nu uw nieuwe overeenkomst.

## <a name="edit-agreements"></a>Overeenkomsten bewerken

1. In de [Azure-portal](https://portal.azure.com), selecteer in het hoofdmenu van Azure **alle services**.

1. Typ 'integration' als filter in het zoekvak. Selecteer deze resource in de resultaten: **Integratieaccounts**

1. Onder **integratieaccounts**, selecteert u het integratieaccount met de overeenkomst die u wilt bewerken.

1. In het rechterdeelvenster onder **onderdelen**, kiest u de **overeenkomsten** tegel.

1. Onder **overeenkomsten**, selecteert u uw overeenkomst en kies **bewerken**.

1. Controleer en sla uw wijzigingen op.

## <a name="delete-agreements"></a>Overeenkomsten verwijderen

1. In de [Azure-portal](https://portal.azure.com), selecteer in het hoofdmenu van Azure **alle services**.

1. Typ 'integration' als filter in het zoekvak. Selecteer deze resource in de resultaten: **Integratieaccounts**

1. Onder **integratieaccounts**, selecteert u het integratieaccount met de overeenkomst die u wilt verwijderen.

1. In het rechterdeelvenster onder **onderdelen**, kiest u de **overeenkomsten** tegel.

1. Onder **overeenkomsten**, selecteert u uw overeenkomst en kies **verwijderen**.

1. Bevestig dat u wilt verwijderen van de geselecteerde overeenkomst.

## <a name="next-steps"></a>Volgende stappen

* [AS2-berichten uitwisselen](logic-apps-enterprise-integration-as2.md)
* [EDIFACT-berichten uitwisselen](logic-apps-enterprise-integration-edifact.md)
* [Exchange X12-berichten](logic-apps-enterprise-integration-x12.md)
