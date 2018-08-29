---
title: Handelspartners voor B2B - integratie van Azure Logic Apps toevoegen | Microsoft Docs
description: Handelspartners voor uw integratie-account maken in Azure Logic Apps met Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: b179325c-a511-4c1b-9796-f7484b4f6873
ms.date: 07/08/2016
ms.openlocfilehash: 20ca5e06cd1cd0d0abfe6d31f622cd6b61b4178f
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43125258"
---
# <a name="add-trading-partners-for-integration-accounts-in-azure-logic-apps-with-enterprise-integration-pack"></a>Handelspartners voor integratieaccounts toevoegen in Azure Logic Apps met Enterprise Integration Pack

Partners zijn de entiteiten die deel uitmaken van business-to-business (B2B)-transacties en uitwisselen van berichten tussen elkaar. Voordat u partners die staan voor u en een andere organisatie in deze transacties maken kunt, moet u beide delen van informatie die u identificeert en valideert de berichten die worden verzonden door elkaar worden verbonden. Nadat u deze details bespreken en klaar bent om uw zakelijke relatie te, kunt u uw integratie-account om weer te geven dat u beide partners kunt maken.

## <a name="what-roles-do-partners-play-in-your-integration-account"></a>Welke rollen speelt partners in uw integratie-account?

Als u meer informatie over de berichten uitgewisseld tussen partners definieert, maakt u overeenkomsten tussen partners. Echter, voordat u een overeenkomst maken kunt, moet u hebt toegevoegd ten minste twee partners aan uw integratie-account. Uw organisatie moet deel uitmaken van de overeenkomst als de **hostpartner**. De andere partner of **gastpartner** vertegenwoordigt de organisatie die berichten met uw organisatie worden uitgewisseld. De gastpartner mag bestaan uit een ander bedrijf of zelfs een afdeling in uw eigen organisatie.

Nadat u deze partners hebt toegevoegd, kunt u een overeenkomst kunt maken.

Ontvangen en verzenden van de instellingen zijn gericht vanuit het oogpunt van de Partner die wordt gehost. Bijvoorbeeld, de instellingen voor ontvangen in een overeenkomst te bepalen hoe de gehoste partner ontvangt berichten van een gastpartner. Op dezelfde manier de instellingen voor het verzenden van de overeenkomst aangeven hoe de gehoste partner-berichten verzendt naar de gastpartner.

## <a name="create-partner"></a>Maken van partner

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer in het hoofdmenu van Azure **alle services**. Voer 'integration' in het zoekvak en selecteer vervolgens **integratieaccounts**.

   ![Integratie-account zoeken](./media/logic-apps-enterprise-integration-partners/account-1.png)

3. Onder **Integratieaccounts**, selecteert u de integratieaccount waar u wilt toevoegen, uw partners.

   ![Integratie-account selecteren](./media/logic-apps-enterprise-integration-partners/account-2.png)

4. Kies de **Partners** tegel.

   ![Kies 'Partners'](./media/logic-apps-enterprise-integration-partners/partner-1.png)

5. Onder **Partners**, kiest u **toevoegen**.

   ![Kies 'Toevoegen'](./media/logic-apps-enterprise-integration-partners/partner-2.png)

6. Voer een naam in voor uw partner, en selecteer vervolgens een **kwalificatie**. Voer een **waarde** voor het identificeren van documenten die uw apps worden weergegeven. Wanneer u klaar bent, kiest u **OK**.

   ![Gegevens van de partner toevoegen](./media/logic-apps-enterprise-integration-partners/partner-3.png)

7. Kies de **Partners** tegel opnieuw.

   ![Kies 'Partners' tegel](./media/logic-apps-enterprise-integration-partners/partner-5.png)

   Uw nieuwe partner wordt nu weergegeven. 

   ![Nieuwe weergave-partner](./media/logic-apps-enterprise-integration-partners/partner-6.png)

## <a name="edit-partner"></a>Partner bewerken

1. In de [Azure-portal](https://portal.azure.com), zoek en selecteer uw integratie-account. Kies de **Partners** tegel.

   ![Kies 'Partners' tegel](./media/logic-apps-enterprise-integration-partners/edit.png)

2. Onder **Partners**, selecteert u de partner die u wilt bewerken.

   ![Selecteer partner verwijderen](./media/logic-apps-enterprise-integration-partners/edit-1.png)

3. Onder **Update Partner**, breng uw wijzigingen.
Nadat u klaar bent, kiest u **opslaan**. 

   ![Uw wijzigingen aanbrengen en opslaan](./media/logic-apps-enterprise-integration-partners/edit-2.png)

   Als u wilt uw wijzigingen annuleren, selecteert u **negeren**.

## <a name="delete-partner"></a>Partner verwijderen

1. In de [Azure-portal](https://portal.azure.com), zoek en selecteer uw integratie-account. Kies de **Partners** tegel.

   ![Kies 'Partners' tegel](./media/logic-apps-enterprise-integration-partners/delete.png)

2. Onder **Partners**, selecteert u de partner die u wilt verwijderen.
Kies **verwijderen**.

   ![Partner verwijderen](./media/logic-apps-enterprise-integration-partners/delete-1.png)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over de overeenkomsten](../logic-apps/logic-apps-enterprise-integration-agreements.md "meer informatie over enterprise integration-overeenkomsten")  

