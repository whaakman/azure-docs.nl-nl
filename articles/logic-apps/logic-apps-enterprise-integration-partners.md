---
title: Maken van partners voor berichten in business-to-business (B2B) - Azure Logic Apps | Microsoft Docs
description: Meer informatie over het toevoegen van partners aan uw account integratie met de Enterprise-integratiepakket en Logic Apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: divyaswarnkar
manager: jeconnoc
editor: ''
ms.assetid: b179325c-a511-4c1b-9796-f7484b4f6873
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4abfdaa8145f14bc6496e57ebce0da10cdd2c365
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35299736"
---
# <a name="add-or-update-partners-in-business-to-business-agreements-in-your-workflow"></a>Toevoegen of bijwerken van partners in uw werkstroom business-to-business-overeenkomsten

Partners zijn entiteiten die deelnemen aan business-to-business (B2B) transacties en uitwisselen van berichten tussen elkaar. Voordat u partners die u en een andere organisatie deze transacties vertegenwoordigen maken kunt, moet u beide delen van gegevens die u identificeert en berichten is verzonden door elkaar worden gevalideerd. Nadat u deze gegevens worden behandeld en klaar bent om uw zakelijke relatie te, kunt u partners in uw account integratie te vertegenwoordigen u beide.

## <a name="what-roles-do-partners-play-in-your-integration-account"></a>Welke rollen speelt partners in uw integratie-account?

Als u meer informatie over de berichten die worden uitgewisseld tussen partners definieert, maakt u overeenkomsten tussen de partners. Echter, voordat u een overeenkomst maken kunt, moet u hebben toegevoegd ten minste twee partners aan uw account integratie. Uw organisatie moet deel uitmaken van de overeenkomst als de **host partner**. De andere partner of **Gast partner** vertegenwoordigt de organisatie die berichten met uw organisatie uitwisselt. De Gast-partner kan zijn van een ander bedrijf, of zelfs een afdeling in uw eigen organisatie.

Nadat u deze partners hebt toegevoegd, kunt u een overeenkomst kunt maken.

Ontvangen en verzenden van de instellingen zijn gericht vanuit het oogpunt van de Partner die worden gehost. Bijvoorbeeld, bepalen de Receive-instellingen in een overeenkomst hoe de gehoste partner ontvangt berichten die worden verzonden vanaf een partner Gast. Op dezelfde manier de instellingen voor uitgaande berichten op de overeenkomst aangeven hoe de gehoste partner-berichten verzendt naar de Gast-partner.

## <a name="create-partner"></a>Partner maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer op het Azure hoofdmenu **alle services**. Voer in het zoekvak "-integratie" en selecteer vervolgens **integratieaccounts**.

   ![Integratie-account vinden](./media/logic-apps-enterprise-integration-partners/account-1.png)

3. Onder **Integratieaccounts**, selecteert u de integratie account waar u uw partners toevoegen.

   ![Integratie-account selecteren](./media/logic-apps-enterprise-integration-partners/account-2.png)

4. Kies de **Partners** tegel.

   ![Kies 'Partners'](./media/logic-apps-enterprise-integration-partners/partner-1.png)

5. Onder **Partners**, kies **toevoegen**.

   ![Kies 'Add'](./media/logic-apps-enterprise-integration-partners/partner-2.png)

6. Voer een naam voor uw partner en selecteer vervolgens een **kwalificatie**. Voer een **waarde** voor het identificeren van documenten die uw apps ontvangen. Als u bent klaar, kiest u **OK**.

   ![Gegevens van de partner toevoegen](./media/logic-apps-enterprise-integration-partners/partner-3.png)

7. Kies de **Partners** tegel opnieuw.

   ![Kies 'Partners' tegel](./media/logic-apps-enterprise-integration-partners/partner-5.png)

   Er verschijnt nu uw nieuwe partner. 

   ![Nieuwe partner weergeven](./media/logic-apps-enterprise-integration-partners/partner-6.png)

## <a name="edit-partner"></a>Partner bewerken

1. In de [Azure-portal](https://portal.azure.com), zoeken en selecteert u uw integratie-account. Kies de **Partners** tegel.

   ![Kies 'Partners' tegel](./media/logic-apps-enterprise-integration-partners/edit.png)

2. Onder **Partners**, selecteert u de partner die u wilt bewerken.

   ![Selecteer partner verwijderen](./media/logic-apps-enterprise-integration-partners/edit-1.png)

3. Onder **Update Partner**, breng uw wijzigingen.
Nadat u bent klaar, kiest u **opslaan**. 

   ![Uw wijzigingen aanbrengen en opslaan](./media/logic-apps-enterprise-integration-partners/edit-2.png)

   Selecteer uw om wijzigingen te annuleren, **negeren**.

## <a name="delete-partner"></a>Partner verwijderen

1. In de [Azure-portal](https://portal.azure.com), zoeken en selecteert u uw integratie-account. Kies de **Partners** tegel.

   ![Kies 'Partners' tegel](./media/logic-apps-enterprise-integration-partners/delete.png)

2. Onder **Partners**, selecteert u de partner die u wilt verwijderen.
Kies **verwijderen**.

   ![Partner verwijderen](./media/logic-apps-enterprise-integration-partners/delete-1.png)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over de overeenkomsten](../logic-apps/logic-apps-enterprise-integration-agreements.md "meer informatie over enterprise integration-overeenkomsten")  

