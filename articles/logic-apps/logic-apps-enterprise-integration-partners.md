---
title: Maken van partners voor berichten in business-to-business (B2B) - Azure Logic Apps | Microsoft Docs
description: Meer informatie over het toevoegen van partners aan uw account integratie met de Enterprise-integratiepakket en Logic Apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: b179325c-a511-4c1b-9796-f7484b4f6873
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 950cb449b53f400f0f0f860caf5415bbb5212269
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="add-or-update-partners-in-business-to-business-agreements-in-your-workflow"></a>Toevoegen of bijwerken van partners in uw werkstroom business-to-business-overeenkomsten

Partners zijn entiteiten die deelnemen aan business-to-business (B2B) transacties en uitwisselen van berichten tussen elkaar. Voordat u partners die u en een andere organisatie deze transacties vertegenwoordigen maken kunt, moet u beide delen van gegevens die u identificeert en berichten is verzonden door elkaar worden gevalideerd. Nadat u deze gegevens worden behandeld en klaar bent om uw zakelijke relatie te, kunt u partners in uw account integratie te vertegenwoordigen u beide.

## <a name="what-roles-do-partners-have-in-your-integration-account"></a>Welke rollen hebt partners in uw integratie-account?

Als u meer informatie over de berichten die worden uitgewisseld tussen partners definieert, maakt u overeenkomsten tussen de partners. Echter, voordat u een overeenkomst maken kunt, moet u hebben toegevoegd ten minste twee partners aan uw account integratie. Uw organisatie moet deel uitmaken van de overeenkomst als de **host partner**. De andere partner of **Gast partner** vertegenwoordigt de organisatie die berichten met uw organisatie uitwisselt. De Gast-partner kan zijn van een ander bedrijf, of zelfs een afdeling in uw eigen organisatie.

Nadat u deze partners hebt toegevoegd, kunt u een overeenkomst kunt maken.

Ontvangen en verzenden van de instellingen zijn gericht vanuit het oogpunt van de Partner die worden gehost. Bijvoorbeeld, bepalen de receive-instellingen in een overeenkomst hoe de gehoste partner ontvangt berichten die worden verzonden vanaf een partner Gast. Op dezelfde manier de instellingen voor uitgaande berichten op de overeenkomst aangeven hoe de gehoste partner-berichten verzendt naar de Gast-partner.

## <a name="how-to-create-a-partner"></a>Het maken van een partner?

1. Selecteer in de Azure-portal **Bladeren**.

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. Voer in het zoekvak filter **integratie**, selecteer daarna **Integratieaccounts** in de lijst met resultaten.

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. De integratie rekening waar u uw partners wilt selecteren.

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Selecteer de **Partners** tegel.

    ![](./media/logic-apps-enterprise-integration-partners/partner-1.png)

5. Kies in de blade Partners **toevoegen**.

    ![](./media/logic-apps-enterprise-integration-partners/partner-2.png)

6. Voer een naam voor uw partner en selecteer vervolgens een **kwalificatie**. Voer ten slotte een **waarde** om documenten die worden geleverd in uw apps te identificeren.

    ![](./media/logic-apps-enterprise-integration-partners/partner-3.png)

7. Overzicht van de voortgang van een proces voor het maken van uw partner, selecteer de *bel* meldingspictogram.

    ![](./media/logic-apps-enterprise-integration-partners/partner-4.png)

8. Om te bevestigen dat uw nieuwe partners zijn toegevoegd, selecteer de **Partners** tegel.

    ![](./media/logic-apps-enterprise-integration-partners/partner-5.png)

    Nadat u de tegel Partners selecteert, ziet u ook toegevoegde partners in de blade Partners.

    ![](./media/logic-apps-enterprise-integration-partners/partner-6.png)

## <a name="how-to-edit-existing-partners-in-your-integration-account"></a>Het bewerken van bestaande partners in uw account integratie

1. Selecteer de **Partners** tegel.
2. Nadat de Partners blade wordt geopend, selecteert u de partner die u wilt bewerken.
3. Op de **Update Partner** tegel, breng uw wijzigingen.
4. Nadat u bent klaar, kiest u **opslaan**, of om uw wijzigingen annuleren, selecteer **negeren**.

    ![](./media/logic-apps-enterprise-integration-partners/edit-1.png)

## <a name="how-to-delete-a-partner"></a>Het verwijderen van een partner

1. Selecteer de **Partners** tegel.
2. Nadat de Partner-blade wordt geopend, selecteert u de partner die u wilt verwijderen.
3. Kies **verwijderen**.

    ![](./media/logic-apps-enterprise-integration-partners/delete-1.png)

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over de overeenkomsten](../logic-apps/logic-apps-enterprise-integration-agreements.md "meer informatie over enterprise integration-overeenkomsten")  

