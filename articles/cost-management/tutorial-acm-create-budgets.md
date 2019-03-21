---
title: Zelfstudie - maken en beheren van Azure budgetten | Microsoft Docs
description: In deze zelfstudie kunt plannen en -account voor de kosten van Azure-services die u verbruikt.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 03/13/2019
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: seodec18
ms.openlocfilehash: e6e20db39be8a6e60833bf5c4f9b6a34a9ead461
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58013034"
---
# <a name="tutorial-create-and-manage-azure-budgets"></a>Zelfstudie: Maken en beheren van Azure budgetten

Met budgetten in Cost Management kunt u de verantwoordelijkheid in de organisatie beter plannen en stimuleren. Met budgetten kunt u de Azure-services die u gebruikt of waarop u zich abonneert voor een specifieke periode bijhouden. Ze helpen u anderen te informeren over hun uitgaven aan proactief kosten kunt beheren en controleren hoe uitgaven verloopt na verloop van tijd. Wanneer de budgetdrempels die u hebt gemaakt, worden overschreden, worden alleen meldingen worden geactiveerd. Geen van uw resources worden beïnvloed en uw verbruik is niet gestopt. U kunt budgetten gebruiken om te vergelijken en als u kosten analyseren uitgave bij te houden.

Maandelijkse budgetten worden geëvalueerd op basis van elke vier uur uitgaven. Echter, gegevens en meldingen voor de verbruikte resources zijn beschikbaar binnen acht uur.  

Budgetten automatisch opnieuw instellen aan het einde van een punt (maandelijks, per kwartaal of per jaar) voor hetzelfde budgetbedrag wanneer u een vervaldatum in de toekomst selecteert. Omdat ze opnieuw met hetzelfde budgetbedrag instelt, moet u afzonderlijke budgetten wanneer gebudgetteerde maken bedragen verschillen voor toekomstige perioden.

De voorbeelden in deze zelfstudie begeleidt u bij het maken en bewerken van een budget voor een Azure Enterprise Agreement (EA)-abonnement.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maken van een budget in de Azure portal
> * Een budget bewerken

## <a name="prerequisites"></a>Vereisten

Budgetten worden ondersteund voor verschillende typen Azure-account. Zie voor de volledige lijst met ondersteunde accounttypen [Gegevens van Azure Cost Management begrijpen](understand-cost-mgt-data.md). Als u wilt weergeven van budgetten, moet u minimaal leestoegang voor uw Azure-account.

 Voor Azure EA-abonnementen, moet u leestoegang tot het weergeven van budgetten hebben. Als u wilt maken en beheren van budgetten, moet u de machtiging Inzender hebt. U kunt afzonderlijke budgetten voor EA-abonnementen en resourcegroepen. U kan echter budgetten maken voor facturering van accounts EA.

De volgende Azure-machtigingen of bereiken, worden ondersteund per abonnement voor budgetten per gebruiker en groep. Zie voor meer informatie over bereiken [begrijpen en werk met een bereik](understand-work-scopes.md).

- Eigenaar: eigenaren kunnen budgetten voor een abonnement instellen, wijzigen en verwijderen.
- Inzender en Inzender van Cost Management – kunt maken, wijzigen of verwijderen van hun eigen budgetten. Inzenders kunnen ook de budgetbedragen aanpassen voor budgetten die zijn gemaakt door anderen.
- Lezer en kostenbeheer lezer – kunt budgetten die ze gemachtigd zijn om te bekijken.

Zie voor meer informatie over het toewijzen van machtigingen aan gegevens van Cost Management [toegang tot gegevens van kostenbeheer toewijzen](assign-access-acm-data.md).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

- Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="create-a-budget-in-the-azure-portal"></a>Maken van een budget in de Azure portal

U kunt het budget van een Azure-abonnement maken voor een bepaalde maand, kwartaal of jaar. Uw navigeerbare inhoud in Azure portal wordt bepaald of u een budget voor een abonnement of voor een beheergroep maakt.

Als u wilt maken of een budget weergeven, opent u het gewenste bereik in de Azure portal en selecteer **budgetten** in het menu. Bijvoorbeeld, gaat u naar **abonnementen**, selecteer een abonnement in de lijst en selecteer vervolgens **budgetten** in het menu. Gebruik de **bereik** pill om over te schakelen naar een ander bereik, zoals een beheergroep, budgetten. Zie voor meer informatie over bereiken [begrijpen en werk met een bereik](understand-work-scopes.md).

Nadat u budgetten maakt, worden ze een eenvoudige weergave van uw huidige uitgaven op basis hiervan weergegeven.

Klik op **Add**.

![Cost Management budgetten wordt weergegeven in de Azure-portal](./media/tutorial-acm-create-budgets/budgets01.png)

In de **maken budget** venster, voer een naam van het budget en het budgetbedrag. Kies vervolgens een maandelijks, elk kwartaal, of de jaarlijkse duur. Selecteer vervolgens een einddatum. Budgetten moeten ten minste één kosten drempel (% van budget) en een bijbehorende e-mailadres. Eventueel kunt u maximaal vijf drempelwaarden en vijf e-mailadressen in één budget opnemen. Wanneer een budgetdrempelwaarde is bereikt, worden normaal gesproken e-mailmeldingen ontvangen in minder dan acht uur. Zie voor meer informatie over meldingen [gebruik kosten waarschuwingen](cost-mgt-alerts-monitor-usage-spending.md).

Hier volgt een voorbeeld van het maken van een maandelijks budget voor $4500. Een e-mailmelding wordt gegenereerd na het verstrijken van 90% van het budget.

![Van de voorbeeldinformatie die wordt weergegeven in het budget maken](./media/tutorial-acm-create-budgets/monthly-budget01.png)

Wanneer u een per kwartaal budget maakt, wordt het werkt op dezelfde manier als een maandelijkse budget. Het verschil is dat het budgetbedrag voor het kwartaal gelijkmatig wordt verdeeld over de drie maanden van het kwartaal. Als u had verwacht, wordt een jaarlijkse budgetbedrag gelijkmatig verdeeld over alle 12 maanden van het kalenderjaar.

Huidige uitgaven op basis van budgetten wordt bijgewerkt wanneer Cost Management bijgewerkte factureringsgegevens ontvangt. Normaal gesproken, dagelijks.

![Voorbeeld van de gegevens van huidige uitgaven op basis van budgetten](./media/tutorial-acm-create-budgets/budgets-current-spending.png)

Nadat u een budget gemaakt, wordt deze weergegeven in kostenanalyse. Een van de eerste stappen wanneer u met begint het weergeven van uw budget ten opzichte van uw uitgaventrend is [analyseren van uw kosten en uitgaven](quick-acm-cost-analysis.md).

![Voorbeeld van budget en uitgaven wordt weergegeven in kostenanalyse](./media/tutorial-acm-create-budgets/cost-analysis.png)

In het voorgaande voorbeeld, moet u een budget voor een abonnement gemaakt. U kunt echter ook een budget voor een resourcegroep maken. Als u maken van een budget voor een resourcegroep wilt, gaat u naar **kostenbeheer en facturering** &gt; **abonnementen** &gt; Selecteer een abonnement > **Resource groepen** > Selecteer een resourcegroep > **budgetten** > en vervolgens **toevoegen** een budget.

## <a name="edit-a-budget"></a>Een budget bewerken

Afhankelijk van het toegangsniveau dat u hebt, kunt u een budget om de eigenschappen te wijzigen. In het volgende voorbeeld wordt zijn een aantal van de eigenschappen alleen-lezen omdat de gebruiker alleen de machtiging Inzender voor het abonnement heeft. Op dit moment de **vervaldatum** is uitgeschakeld en kan niet worden gewijzigd nadat deze is ingesteld.

![Voorbeeld van het bewerken van een budget om verschillende eigenschappen te wijzigen](./media/tutorial-acm-create-budgets/edit-budget.png)

## <a name="trigger-an-action-group"></a>Een actiegroep activeren

Wanneer u maakt of een budget voor een abonnement of een resource groepsbereik bewerkt, kunt u configureren om aan te roepen een actiegroep. De actiegroep kunt tal van verschillende acties uitvoeren als uw budgetdrempelwaarde is bereikt. Zie voor meer informatie over actiegroepen [maken en beheren van actiegroepen in Azure portal](../azure-monitor/platform/action-groups.md). Zie voor meer informatie over het gebruik van automation op basis van budget met actiegroepen [kosten beheren met Azure budgetten](../billing/billing-cost-management-budget-scenario.md).

Als u wilt maken of bijwerken van actiegroepen, klikt u op **beheren actiegroepen** terwijl u het maken of bewerken van een budget.

![Voorbeeld van het maken van een budget beheren actiegroepen weergeven](./media/tutorial-acm-create-budgets/manage-action-groups01.png)

Klik vervolgens op **actiegroep toevoegen** en de actiegroep te maken.


![Afbeelding van het vak toevoegen actie groep](./media/tutorial-acm-create-budgets/manage-action-groups02.png)

Na de actie groep is gemaakt, sluit het vak om terug te keren naar uw budget.

Configureer uw budget voor het gebruik van de actiegroep als een afzonderlijke drempelwaarde is bereikt. Er worden maximaal vijf verschillende drempelwaarden ondersteund.

![Voorbeeld van groepsselectie actie voor de voorwaarde voor een waarschuwing](./media/tutorial-acm-create-budgets/manage-action-groups03.png)

Het volgende voorbeeld ziet budgetdrempels ingesteld op 50%, 75% en 100%. Elk is geconfigureerd voor het activeren van de opgegeven acties in de aangewezen actiegroep.

![Voorbeeld van waarschuwing bepalingen die zijn geconfigureerd met verschillende actiegroepen en type van acties](./media/tutorial-acm-create-budgets/manage-action-groups04.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Maken van een budget in de Azure portal
> * Een budget bewerken

Ga naar de volgende zelfstudie voor het maken van een terugkerende exporteren voor uw gegevens van cost management.

> [!div class="nextstepaction"]
> [Maken en beheren van de geëxporteerde gegevens](tutorial-export-acm-data.md)
