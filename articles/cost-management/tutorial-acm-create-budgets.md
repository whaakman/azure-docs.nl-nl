---
title: Zelfstudie - maken en beheren van Azure budgetten | Microsoft Docs
description: In deze zelfstudie kunt plannen en -account voor de kosten van Azure-services die u verbruikt.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/21/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 6c6143cad04178fcafc825d9dae13c1a0620fb93
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033444"
---
# <a name="tutorial-create-and-manage-azure-budgets"></a>Zelfstudie: Maken en beheren van Azure budgetten

Budgetten in Cost Management kunnen u plannen en station verantwoordelijkheid in de organisatie. Met budgetten, kunt u rekening gehouden met de Azure-services u gebruikt of zich abonneert op een specifieke periode. Ze helpen u anderen te informeren over hun uitgaven aan proactief kosten kunt beheren en controleren hoe uitgaven verloopt na verloop van tijd. U ziet hoe de bestedingslimiet vordert na verloop van tijd. Wanneer de budgetdrempels die u hebt gemaakt, worden overschreden, worden alleen meldingen worden geactiveerd. Geen van uw resources worden beïnvloed en uw verbruik is niet gestopt. U kunt budgetten gebruiken om te vergelijken en als u kosten analyseren uitgave bij te houden.

Budgetten automatisch opnieuw instellen aan het einde van een punt (maandelijks, per kwartaal of per jaar) voor hetzelfde budgetbedrag wanneer u een vervaldatum in de toekomst selecteert. Omdat ze opnieuw met hetzelfde budgetbedrag instelt, moet u afzonderlijke budgetten wanneer gebudgetteerde maken bedragen verschillen voor toekomstige perioden.

De voorbeelden in deze zelfstudie bekijkt u het maken en bewerken van een budget voor een Azure Enterprise Agreement (EA)-abonnement.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maken van een budget in de Azure portal
> * Een budget bewerken

## <a name="prerequisites"></a>Vereisten

Budgetten zijn beschikbaar voor alle Azure EA-klanten. U moet leestoegang tot een Azure EA-abonnement maken en beheren van budgetten hebben. Facturering EA-accounts worden niet ondersteund door budgetten.

Budgetten wordt gemaakt in het abonnement of een niveau van de resourcegroep, afzonderlijk. De volgende Azure-machtigingen worden per abonnement voor budgetten door gebruikers en groepen ondersteund:

- Eigenaar: kunt maken, wijzigen of verwijderen van budgetten voor een abonnement.
- Inzender: kan maken, wijzigen of verwijderen van hun eigen budgetten. Kunt de hoeveelheid budget voor budgetten die zijn gemaakt door anderen.
- Lezer: kunt budgetten die ze gemachtigd zijn om te bekijken.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

- Meld u aan bij Azure Portal op http://portal.azure.com.

## <a name="create-a-budget-in-the-azure-portal"></a>Maken van een budget in de Azure portal

U kunt het budget van een Azure-abonnement maken voor een bepaalde maand, kwartaal of jaar. Uw navigeerbare inhoud in de Azure-portal bepaalt u of u een budget voor een abonnement of voor een resourcegroep maken.

In de Azure-portal, gaat u naar **kostenbeheer en facturering** &gt; **abonnementen** &gt; Selecteer een abonnement &gt; **budgetten**. In dit voorbeeld is het budget dat u hebt gemaakt voor het abonnement dat u hebt geselecteerd.

Nadat u budgetten maakt, worden ze een eenvoudige weergave van uw huidige uitgaven op basis hiervan weergegeven.

Klik op **Add**.

![Budgetten voor kosten beheren](./media/tutorial-acm-create-budgets/budgets01.png)

In de **maken budget** venster, voer een naam van het budget en het budgetbedrag. Kies vervolgens een maandelijks, elk kwartaal, of de jaarlijkse duur. Selecteer vervolgens een einddatum. Budgetten moeten ten minste één kosten drempel (% van budget) en een bijbehorende e-mailadres. Eventueel kunt u maximaal vijf drempelwaarden en vijf e-mailadressen in één budget opnemen.

Hier volgt een voorbeeld van het maken van een maandelijks budget voor $4500. Een e-mailmelding wordt gegenereerd na het verstrijken van 90% van het budget.

![Voorbeeld voor een maand budget](./media/tutorial-acm-create-budgets/monthly-budget01.png)

Wanneer u een per kwartaal budget maakt, wordt het werkt op dezelfde manier als een maandelijkse budget. Het verschil is dat het budgetbedrag voor het kwartaal gelijkmatig wordt verdeeld over de drie maanden van het kwartaal. Als u had verwacht, wordt een jaarlijkse budgetbedrag gelijkmatig verdeeld over alle 12 maanden van het kalenderjaar.

Huidige uitgaven op basis van budgetten wordt bijgewerkt wanneer Cost Management bijgewerkte factureringsgegevens ontvangt. Normaal gesproken, dagelijks.

![Huidige uitgaven op basis van budgetten](./media/tutorial-acm-create-budgets/budgets-current-spending.png)

Nadat u een budget gemaakt, wordt deze weergegeven in kostenanalyse. Een van de eerste stappen wanneer u met begint het weergeven van uw budget ten opzichte van uw uitgaventrend is [analyseren van uw kosten en uitgaven](quick-acm-cost-analysis.md).

![Budget wordt weergegeven in kostenanalyse](./media/tutorial-acm-create-budgets/cost-analysis.png)

In het voorgaande voorbeeld, moet u een budget voor een abonnement gemaakt. U kunt echter ook een budget voor een resourcegroep maken. Als u maken van een budget voor een resourcegroep wilt, gaat u naar **kostenbeheer en facturering** &gt; **abonnementen** &gt; Selecteer een abonnement > **Resource groepen** > Selecteer een resourcegroep > **budgetten** > en vervolgens **toevoegen** een budget.

## <a name="edit-a-budget"></a>Een budget bewerken

Afhankelijk van het toegangsniveau dat u hebt, kunt u een budget om de eigenschappen te wijzigen. In het voorbeeld volgen zijn een aantal van de eigenschappen alleen-lezen omdat de gebruiker alleen de machtiging Inzender voor het abonnement heeft. Op dit moment de **vervaldatum** is uitgeschakeld en kan niet worden gewijzigd nadat deze is ingesteld.

![Budget: machtiging Inzender bewerken](./media/tutorial-acm-create-budgets/edit-budget.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Maken van een budget in de Azure portal
> * Een budget bewerken

Ga naar de volgende zelfstudie voor het maken van een terugkerende exporteren voor uw gegevens van cost management.

> [!div class="nextstepaction"]
> [Maken en beheren van de geëxporteerde gegevens](tutorial-export-acm-data.md)
