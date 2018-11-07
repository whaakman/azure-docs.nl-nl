---
title: Een interne ondersteuningsverklaring toevoegen aan een lab in Azure DevTest Labs | Microsoft Docs
description: Informatie over het plaatsen van een interne ondersteuningsverklaring aan een lab in Azure DevTest Labs
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: c9900333-6c5e-4d7d-b0f4-889015e9550c
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: deb98c2c633200ab4be1d763a94fd2a04979a3b1
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51235697"
---
# <a name="add-an-internal-support-statement-to-a-lab-in-azure-devtest-labs"></a>Een interne ondersteuningsverklaring toevoegen aan een lab in Azure DevTest Labs

Azure DevTest Labs kunt u aanpassen van uw testomgeving met een interne ondersteuningsverklaring waarmee gebruikers met voor ondersteuningsinformatie over het lab. U kunt bijvoorbeeld contactgegevens opgeven zodat een gebruiker hoe u ondersteuning voor interne bereiken weet wanneer ze nodig hebben hulp bij het oplossen van problemen of toegang tot bronnen in het lab. U kunt ook koppelingen naar interne websites of Veelgestelde vragen over in uw team openen kunt voordat u contact opnemen met ondersteuning opgeven.

Een interne ondersteuningsverklaring is bedoeld om te laten u lab-gegevens die te vaak doorgaans niet wijzigen. Aan de gebruikers te informeren over lab-informatie die wordt meer tijdelijke aard, zoals recente updates voor beleid voor lab maken: Zie [Post-aankondiging in een lab](devtest-lab-announcements.md).

U kunt eenvoudig uitschakelen of een ondersteuningsverklaring bewerken nadat het is niet langer van toepassing.

## <a name="steps-to-add-a-support-statement-to-an-existing-lab"></a>Stappen voor het toevoegen van een ondersteuningsverklaring aan een bestaande lab

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer indien nodig, **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst. (Uw lab mogelijk al worden weergegeven op het Dashboard onder **alle Resources**).
1. Selecteer in het lab waarin u wilt toevoegen van een ondersteuningsverklaring uit de lijst met labs.  
1. Op van het lab **overzicht** gedeelte **configuratie en het beleid**.  

    ![Knop voor configuratie en het beleid](./media/devtest-lab-internal-support-message/devtestlab-config-and-policies.png)

1. Aan de linkerkant onder **instellingen**, selecteer **ondersteuning voor interne**.

    ![Ondersteuning voor interne knop](./media/devtest-lab-internal-support-message/devtestlab-internal-support.png)

1. Stel Enabled in op voor het maken van een bericht interne ondersteuning voor de gebruikers in dit lab, **Ja**.

1. In de **ondersteuning bericht** en voer de interne ondersteuningsverklaring die u voor uw labgebruikers wilt presenteren. Het bericht ondersteuning accepteert Markdown. Als u de tekst van het invoert, ziet u de **Preview** gebied aan de onderkant van het scherm om te zien hoe het bericht wordt weergegeven aan gebruikers.

    ![Ondersteuning voor interne scherm om het bericht te maken.](./media/devtest-lab-internal-support-message/devtestlab-add-support-statement.png)


1. Selecteer **opslaan** zodra uw ondersteuningsverklaring is klaar om te posten.

Als u niet meer weergeven van dit bericht ondersteuning voor labgebruikers wilt, Ga terug naar de **ondersteuning voor interne** pagina en stel **ingeschakeld** naar **Nee**.

## <a name="steps-for-users-to-view-the-support-message"></a>Stappen voor gebruikers om weer te geven van het bericht ondersteuning

1. Uit de [Azure-portal](https://go.microsoft.com/fwlink/p/?LinkID=525040), selecteert u een lab.

1. Op van het lab **overzicht** gedeelte **ondersteuning voor interne**.  

    ![Ondersteuning voor interne knop](./media/devtest-lab-internal-support-message/devtestlab-internal-support.png)


1. Als een bericht ondersteuning wordt geplaatst, kan de gebruiker bekijken in het deelvenster interne ondersteuning.

    ![Ondersteuning voor interne deelvenster met ondersteuning van bericht is geplaatst](./media/devtest-lab-internal-support-message/devtestlab-view-suport-statement.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
* Interne ondersteuningsverklaringen worden meestal gebruikt om te bieden ondersteuning voor gegevens die niet die regelmatig worden gewijzigd. U leert ook hoe u [plaatsen van een aankondiging naar een lab](devtest-lab-announcements.md) gebruikers tijdelijk wijzigingen en updates voor de testomgeving moeten worden geïnformeerd.
* [Beleid en planningen instellen](devtest-lab-set-lab-policy.md) vindt u informatie over hoe u toepassen kunt andere beperkingen en conventies voor uw abonnement met behulp van aangepast beleid.