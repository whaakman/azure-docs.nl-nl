---
title: Het plaatsen van een aankondiging voor aan een lab in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het toevoegen van een aankondiging aan een lab in Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: 67a09946-4584-425e-a94c-abe57c9cbb82
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 3282a90069ecd119154df492ac6dc366d26b5300
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611202"
---
# <a name="post-an-announcement-to-a-lab-in-azure-devtest-labs"></a>Het plaatsen van een aankondiging naar een lab in Azure DevTest Labs

Als de beheerder van een lab, kunt u een aangepaste aankondiging posten in een bestaande lab, waar gebruikers te informeren over recente wijzigingen in of toevoegingen aan het lab. U wilt bijvoorbeeld gebruikers informeren over:

- Nieuwe VM-grootten die beschikbaar zijn
- Installatiekopieën die op dit moment onbruikbaar zijn
- Updates voor beleid voor lab maken

Zodra geplaatst, de aankondiging wordt weergegeven op de pagina overzicht van de testomgeving en de gebruiker voor meer informatie selecteren kunt.

De functie aankondiging is bedoeld om te worden gebruikt voor tijdelijke meldingen.  Nadat deze is niet meer nodig hebt, kunt u eenvoudig een aankondiging uitschakelen.

## <a name="steps-to-post-an-announcement-in-an-existing-lab"></a>Stappen voor het plaatsen van een aankondiging in een bestaande lab

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer indien nodig, **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst. (Uw lab mogelijk al worden weergegeven op het Dashboard onder **alle Resources**).
1. Selecteer in het lab waarin u wilt plaatsen van een aankondiging uit de lijst met labs.  
1. Op van het lab **overzicht** gedeelte **configuratie en het beleid**.  

    ![Knop voor configuratie en het beleid](./media/devtest-lab-announcements/devtestlab-config-and-policies.png)

1. Aan de linkerkant onder **instellingen**, selecteer **Lab aankondiging**.

    ![Knop voor Lab-aankondiging](./media/devtest-lab-announcements/devtestlab-announcements.png)

1. Als u een bericht voor de gebruikers in dit lab, stelt u **ingeschakeld** naar **Ja**.

1. U kunt een **vervaldatum** om op te geven van een datum en tijd waarna de aankondiging niet meer wordt weergegeven aan gebruikers. Als u een vervaldatum niet opgeeft, wordt de aankondiging blijft totdat u deze uitschakelen.

   > [!NOTE]
   > Nadat de aankondiging is verlopen, deze niet meer wordt weergegeven aan gebruikers, maar nog steeds aanwezig in de **Lab aankondiging** deelvenster. U kunt wijzigingen aanbrengen en weer inschakelen om het te activeren opnieuw.
   >
   >

1. Voer een **aankondiging titel** en de **aankondiging tekst**.

   De titel mag maximaal 100 tekens bestaan en wordt weergegeven aan de gebruiker op de pagina overzicht van de testomgeving. Als de gebruiker de titel selecteert, wordt de aankondiging tekst weergegeven.

   De tekst van de aankondiging accepteert markdown. Als u de tekst van de aankondiging invoert, kunt u het bericht weergeven in de Preview-gebied aan de onderkant van het scherm.

    ![Lab aankondiging scherm om het bericht te maken.](./media/devtest-lab-announcements/devtestlab-post-announcement.png)


1. Selecteer **opslaan** zodra uw aankondiging is klaar om te posten.

Als u niet meer weergeven van deze aankondiging voor labgebruikers wilt, Ga terug naar de **Lab aankondiging** pagina en stel **ingeschakeld** naar **Nee**. Als u een verloopdatum hebt opgegeven, is de aankondiging automatisch uitgeschakeld op die datum en tijd.

## <a name="steps-for-users-to-view-an-announcement"></a>Stappen voor gebruikers om weer te geven van een aankondiging

1. Uit de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), selecteert u een lab.

1. Als het lab een aankondiging voor geplaatst heeft, wordt een aankondiging wordt weergegeven boven aan de pagina overzicht van de testomgeving. Deze aankondiging is de titel van de aankondiging die is opgegeven tijdens het maken van de aankondiging.

    ![Lab-aankondiging op de pagina overzicht](./media/devtest-lab-announcements/devtestlab-user-announcement.png)

1. De gebruiker kan het bericht om weer te geven van de volledige aankondiging selecteren.

    ![Meer informatie voor de aankondiging in het lab](./media/devtest-lab-announcements/devtestlab-user-announcement-text.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
* Als u een lab-beleid instellen of wijzigt, is het raadzaam om te posten van een aankondiging om gebruikers te informeren. [Beleid en planningen instellen](devtest-lab-set-lab-policy.md) bevat informatie over het toepassen van beperkingen en conventies voor uw abonnement met behulp van aangepast beleid.
* Verken de [DevTest Labs Azure Resource Manager QuickStart-sjablonen](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
