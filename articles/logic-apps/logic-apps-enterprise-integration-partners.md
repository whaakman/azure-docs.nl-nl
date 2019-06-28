---
title: Handelspartners voor B2B - integratie van Azure Logic Apps toevoegen
description: Handelspartners in uw integratie-account te gebruiken met Azure Logic Apps maken
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/22/2019
ms.openlocfilehash: 681f16132c1de2ec5f3b27f80633d32879b0746c
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67330153"
---
# <a name="add-trading-partners-to-integration-accounts-for-azure-logic-apps"></a>Handelspartners toevoegen aan integratieaccounts voor Azure Logic Apps

In [Azure Logic Apps](../logic-apps/logic-apps-overview.md), kunt u geautomatiseerde werkstromen business-to-business (B2B) voor gegevensintegratie maken met behulp van een [integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) met uw logische apps. Om weer te geven en andere gebruikers van uw organisatie, u maakt en handelspartners als artefacten toevoegen aan uw integratieaccount. Partners zijn de entiteiten die deel uitmaken van B2B-transacties en -berichten uitwisselen met elkaar.

Voordat u deze partners, zorg bespreken en gegevens delen met uw partners over het identificeren en valideren van de berichten die de andere verzendt. Nadat u akkoord op deze gegevens gaat, bent u klaar om te maken van partners in het integratieaccount.

## <a name="partner-roles-in-integration-accounts"></a>Partner-rollen in integratieaccounts

Voor het definiëren van de details over de berichten uitgewisseld met uw partners, u maakt en toevoegt [overeenkomsten](../logic-apps/logic-apps-enterprise-integration-agreements.md) als artefacten voor uw integratieaccount. Overeenkomsten vereist ten minste twee partners in het integratieaccount. Uw organisatie is altijd de *hostpartner* in de overeenkomst. De organisatie die uitwisseling van berichten met uw organisatie is de *gastpartner*. Een gastpartner mag bestaan uit een ander bedrijf of zelfs een afdeling in uw eigen organisatie. Nadat u deze partners hebt toegevoegd, kunt u een overeenkomst kunt maken.

In een overeenkomst geeft u de details voor het verwerken van inkomende en uitgaande berichten vanuit het perspectief van de hostpartner. Voor inkomende berichten, het **instellingen ontvangen** opgeven hoe de hostpartner, ontvangt berichten van de gastpartner in de overeenkomst. Voor uitgaande berichten, het **instellingen voor verzenden** opgeven hoe de hostpartner-berichten verzendt naar de gastpartner.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u een Azure-abonnement nog geen [zich aanmelden voor een gratis Azure-account](https://azure.microsoft.com/free/).

* Een [integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) voor het opslaan van uw partners, overeenkomsten en andere B2B-artefacten. Deze integratie-account moet worden gekoppeld aan uw Azure-abonnement.

## <a name="create-partner"></a>Maken van partner

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer in het hoofdmenu van Azure **alle services**. Voer 'integration' in het zoekvak en selecteer **integratieaccounts**.

   ![Selecteer "Integratieaccounts"](./media/logic-apps-enterprise-integration-partners/find-integration-accounts.png)

1. Onder **Integratieaccounts**, selecteert u de integratieaccount waar u wilt toevoegen, uw partners.

   ![Integratie-account selecteren](./media/logic-apps-enterprise-integration-partners/select-integration-account.png)

1. Kies de **Partners** tegel.

   ![Kies 'Partners' tegel](./media/logic-apps-enterprise-integration-partners/choose-partners.png)

1. Onder **Partners**, kiest u **toevoegen**. Onder **Partner toevoegen**, geef de details van de partner zoals is beschreven in de onderstaande tabel.

   ![Kies 'Toevoegen' en geef Partnerdetails](./media/logic-apps-enterprise-integration-partners/add-partners.png)

   | Eigenschap | Vereist | Description |
   |----------|----------|-------------|
   | **Naam** | Ja | De naam van de partner |
   | **De kwalificatie** | Ja | De verificatie-instantie die unieke zakelijke identiteiten met organisaties, bijvoorbeeld biedt **D-U-N-S (Dun & Bradstreet)** . <p>Partners kunnen voor een sluiten elkaar wederzijds gedefinieerde bedrijfsidentiteit kiezen. Selecteer voor deze scenario's, **sluiten elkaar wederzijds gedefinieerd** voor EDIFACT of **sluiten elkaar wederzijds gedefinieerd (X12)** voor X12. <p>Selecteer alleen voor RosettaNet, **DUNS**, dit is de standaard. |
   | **Waarde** | Ja | Een waarde die aangeeft van de documenten die uw logische apps worden weergegeven. <p>Deze waarde moet een getal van negen cijfers die overeenkomt met het nummer DUNS voor RosettaNet. |
   ||||

   > [!NOTE]
   > Voor partners die RosettaNet gebruiken, kunt u aanvullende informatie opgeven door eerst deze partners en vervolgens [bewerken hiervan daarna](#edit-partner).

1. Als u klaar bent, kiest u **Done**.

   Uw nieuwe partner wordt nu weergegeven op de **Partners** lijst. Ook de **Partners** tegel werkt het huidige aantal partners.

   ![Nieuwe partner](./media/logic-apps-enterprise-integration-partners/new-partner.png)

<a name="edit-partner"></a>

## <a name="edit-partner"></a>Partner bewerken

1. In de [Azure-portal](https://portal.azure.com), zoek en selecteer uw integratie-account.
Kies de **Partners** tegel.

   ![Kies 'Partners' tegel](./media/logic-apps-enterprise-integration-partners/edit.png)

1. Onder **Partners**, selecteert u de partner die u wilt bewerken, en kies **bewerken**. Onder **bewerken**, breng uw wijzigingen.

   ![Uw wijzigingen aanbrengen en opslaan](./media/logic-apps-enterprise-integration-partners/edit-partner.png)

   Voor RosettaNet, onder **RosettaNet Partner eigenschappen**, kunt u deze extra informatie:

   | Eigenschap | Vereist | Description |
   |----------|----------|-------------|
   | **Partner-classificatie** | Nee | Type van de organisatie van de partner |
   | **Supply chain code** | Nee | Van de partner aanbod keten code, bijvoorbeeld 'Information Technology' of "Elektronische onderdelen" |
   | **Neem contact op met de naam** | Nee | Neem contact op met naam van de partner |
   | **E-mail** | Nee | E-mailadres van de partner |
   | **Fax** | Nee | Faxnummer van de partner |
   | **Telefoonnummer** | Nee | Telefoonnummer van de partner |
   ||||

1. Wanneer u klaar bent, kiest u **OK** uw wijzigingen op te slaan.

## <a name="delete-partner"></a>Partner verwijderen

1. In de [Azure-portal](https://portal.azure.com), zoek en selecteer uw integratie-account. Kies de **Partners** tegel.

   ![Kies 'Partners' tegel](./media/logic-apps-enterprise-integration-partners/choose-partners-to-delete.png)

1. Onder **Partners**, selecteert u de partner die u wilt verwijderen. Kies **verwijderen**.

   ![Partner verwijderen](./media/logic-apps-enterprise-integration-partners/delete-partner.png)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [overeenkomsten](../logic-apps/logic-apps-enterprise-integration-agreements.md)