---
title: Een interne ondersteuningsverklaring toevoegen aan een lab in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het plaatsen van een interne ondersteuningsverklaring tot een lab in Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: c9900333-6c5e-4d7d-b0f4-889015e9550c
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/20/2017
ms.author: v-craic
ms.openlocfilehash: 33c2f1e9d1ffa83003b7f0dbba46a7a4588dd57a
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="add-an-internal-support-statement-to-a-lab-in-azure-devtest-labs"></a>Een interne ondersteuningsverklaring toevoegen aan een lab in Azure DevTest Labs

Azure DevTest Labs kunt u uw testomgeving met een interne ondersteuningsverklaring waarmee gebruikers met voor ondersteuningsinformatie over het lab aanpassen. U kunt bijvoorbeeld contactgegevens opgeven zodat een gebruiker interne ondersteuning bereiken kent wanneer ze hulp nodig bij het oplossen van problemen of toegang krijgen tot bronnen in de testomgeving. U kunt ook koppelingen naar interne websites of Veelgestelde vragen die toegang heeft tot uw team voordat u contact op met ondersteuning opgeven.

Een interne ondersteuningsverklaring is bedoeld om te laten u lab-gegevens die doorgaans te vaak niet wijzigen. Zie naar gebruikers te informeren over lab info die meer tijdelijke aard – zoals recente updates voor labbeleidsregels – [Post aankondiging in een testomgeving](devtest-lab-announcements.md).

U kunt eenvoudig uitschakelen of een ondersteuningsverklaring bewerken nadat het is niet langer van toepassing.

## <a name="steps-to-add-a-support-statement-to-an-existing-lab"></a>Stappen voor het toevoegen van een ondersteuningsverklaring aan een bestaande lab

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer indien nodig, **alle Services**, en selecteer vervolgens **DevTest Labs** uit de lijst. (Uw lab mogelijk al worden weergegeven op het Dashboard onder **alle Resources**).
1. Selecteer in het lab waarin u wilt toevoegen van een ondersteuningsverklaring uit de lijst van labs.  
1. Op de testomgeving **overzicht** gebied, selecteer **configuratie en het beleid**.  

    ![Knop voor configuratie en het beleid](./media/devtest-lab-internal-support-message/devtestlab-config-and-policies.png)

1. Aan de linkerkant onder **instellingen**, selecteer **interne ondersteuning**.

    ![Ondersteuning voor interne knop](./media/devtest-lab-internal-support-message/devtestlab-internal-support.png)

1. Stel Enabled in op voor het maken van een bericht interne ondersteuning voor de gebruikers in dit lab, **Ja**.

1. In de **ondersteuning bericht** en voer de interne ondersteuningsverklaring die u aan uw gebruikers lab wilt aanbieden. Het bericht ondersteuning accepteert Markdown. Als u de berichttekst invoert, ziet u de **Preview** gebied aan de onderkant van het scherm om te zien hoe het bericht wordt weergegeven aan gebruikers.

    ![Ondersteuning voor interne scherm voor het maken van het bericht.](./media/devtest-lab-internal-support-message/devtestlab-add-support-statement.png)


1. Selecteer **opslaan** zodra uw ondersteuningsverklaring is gereed om te plaatsen.

Wanneer u niet meer weergeven van dit bericht ondersteuning voor lab gebruikers wilt, terug naar de **interne ondersteuning** pagina en stel **ingeschakeld** naar **Nee**.

## <a name="steps-for-users-to-view-the-support-message"></a>Stappen voor gebruikers om weer te geven van het bericht ondersteuning

1. Van de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), selecteert u een testomgeving.

1. Op de testomgeving **overzicht** gebied, selecteer **interne ondersteuning**.  

    ![Ondersteuning voor interne knop](./media/devtest-lab-internal-support-message/devtestlab-internal-support.png)


1. Als een bericht ondersteuning wordt gepost, kan de gebruiker bekijken in het deelvenster interne ondersteuning.

    ![Ondersteuning voor interne deelvenster geposte ondersteuning bericht wordt weergegeven](./media/devtest-lab-internal-support-message/devtestlab-view-suport-statement.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
* Interne ondersteuningsverklaringen worden meestal gebruikt om informatie over ondersteuning die niet die regelmatig worden gewijzigd. U kunt ook meer te weten hoe [plaatsen van een aankondiging naar een lab](devtest-lab-announcements.md) om gebruikers tijdelijk wijzigingen en updates voor het lab te informeren.
* [Beleidsregels en schema's instellen](devtest-lab-set-lab-policy.md) bevat informatie over hoe u toepassen kunt andere beperkingen en conventies voor uw abonnement met behulp van aangepaste beleid.