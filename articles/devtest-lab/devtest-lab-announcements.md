---
title: Een Announcement tot een lab boeken in Azure DevTest Labs | Microsoft Docs
description: Informatie over het toevoegen van een aankondiging naar een testomgeving in Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 67a09946-4584-425e-a94c-abe57c9cbb82
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/30/2017
ms.author: v-craic
ms.openlocfilehash: d376909a46da11ac1b6b1fa968e53ebef8f3dbf7
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="post-an-announcement-to-a-lab-in-azure-devtest-labs"></a>Een aankondiging naar een lab boeken in Azure DevTest Labs

Als de beheerder van een testomgeving, kunt u een aangepaste aankondiging boeken in een bestaande testomgeving om gebruikers te informeren over recente wijzigingen of toevoegingen aan het lab. U wilt bijvoorbeeld gebruikers informeren over:

- Nieuwe VM-grootten die beschikbaar zijn
- Installatiekopieën die momenteel onbruikbaar zijn
- Updates aan labbeleidsregels voor

Geboekt, de aankondiging wordt weergegeven op de pagina overzicht van het testlab en kan de gebruiker kunt selecteren voor meer informatie.

De aankondiging-functie is bedoeld om te worden gebruikt voor tijdelijke meldingen.  U kunt gemakkelijk een aankondiging uitschakelen nadat deze is niet meer nodig.

## <a name="steps-to-post-an-announcement-in-an-existing-lab"></a>Stappen voor het plaatsen van een aankondiging in een bestaande lab

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer indien nodig, **alle Services**, en selecteer vervolgens **DevTest Labs** uit de lijst. (Uw lab mogelijk al worden weergegeven op het Dashboard onder **alle Resources**).
1. Selecteer in het lab waarin u wilt een aankondiging boeken uit de lijst van labs.  
1. Op de testomgeving **overzicht** gebied, selecteer **configuratie en het beleid**.  

    ![Knop voor configuratie en het beleid](./media/devtest-lab-announcements/devtestlab-config-and-policies.png)

1. Aan de linkerkant onder **instellingen**, selecteer **Lab aankondiging**.

    ![Lab aankondiging knop](./media/devtest-lab-announcements/devtestlab-announcements.png)

1. Als u een bericht voor de gebruikers in dit lab, stelt **ingeschakeld** naar **Ja**.

1. Voer een **aankondiging titel** en de **aankondiging tekst**.

   De titel mag maximaal 100 tekens en wordt getoond aan de gebruiker op de pagina overzicht van de testomgeving. Als de gebruiker de titel selecteert, wordt de tekst van de aankondiging weergegeven.

   De tekst van de aankondiging accepteert markdown. Als u de aankondiging tekst invoert, ziet u het bericht in het gebied voorbeeld aan de onderkant van het scherm.

    ![Lab aankondiging scherm voor het maken van het bericht.](./media/devtest-lab-announcements/devtestlab-post-announcement.png)


1. Selecteer **opslaan** zodra uw aankondiging is gereed om te plaatsen.

Wanneer u niet meer weergeven van deze aankondiging voor gebruikers van de testomgeving wilt, terug naar de **Lab aankondiging** pagina en stel **ingeschakeld** naar **Nee**.

## <a name="steps-for-users-to-view-an-announcement"></a>Stappen voor gebruikers om weer te geven van een aankondiging

1. Van de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), selecteert u een testomgeving.

1. Als het lab een aankondiging geboekt heeft, wordt een aankondiging weergegeven boven aan pagina overzicht van de testomgeving. Deze aankondiging is de titel van de aankondiging die is opgegeven bij het maken van de aankondiging.

    ![Lab aankondiging op de pagina overzicht](./media/devtest-lab-announcements/devtestlab-user-announcement.png)

1. De gebruiker kan het bericht om weer te geven van de hele aankondiging selecteren.

    ![Meer informatie voor de aankondiging lab](./media/devtest-lab-announcements/devtestlab-user-announcement-text.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
* Als u een beleid lab instellen of wijzigt, is het raadzaam te posten een aankondiging om gebruikers te informeren. [Beleidsregels en schema's instellen](devtest-lab-set-lab-policy.md) bevat informatie over de beperkingen en verdragen via uw abonnement met behulp van aangepaste beleidsregels toepassen.
* Verken de [DevTest Labs Azure Resource Manager QuickStart sjablonengalerie](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
