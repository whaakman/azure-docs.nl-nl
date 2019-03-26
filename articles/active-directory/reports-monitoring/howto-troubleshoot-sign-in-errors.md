---
title: Oplossen aanmelden met behulp van Azure Active Directory-rapporten | Microsoft Docs
description: Informatie over het oplossen van aanmelding bij fouten met behulp van Azure Active Directory-rapporten in Azure portal
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 26a3594b00f19c2790e9dfd5f09fbdc7d73d478f
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58434888"
---
# <a name="how-to-troubleshoot-sign-in-errors-using-azure-active-directory-reports"></a>Procedure: Aanmelden-fouten oplossen met behulp van Azure Active Directory-rapporten

De [aanmeldingenrapport](concept-sign-ins.md) in Azure Active Directory (Azure AD) kunt u zoeken naar antwoorden op vragen over het beheren van toegang tot de toepassingen in uw organisatie, met inbegrip van:

- Wat is het aanmeldingspatroon van een gebruiker?
- Hoeveel keer hebben gebruikers zich aangemeld gedurende een week?
- Wat is de status van deze aanmeldingen?


Bovendien kunt het aanmeldingenrapport u ook problemen oplossen met mislukte aanmelding voor gebruikers in uw organisatie. In deze handleiding leert u hoe u een aanmeldingsfout opgetreden in het rapport aanmeldingen isoleren, en deze gebruiken om de hoofdoorzaak van het probleem te begrijpen.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende zaken nodig:

* Een Azure AD-tenant met een licentie premium (P1/P2). Zie [Aan de slag met Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) om uw versie van Azure Active Directory te upgraden.
* Een gebruiker, die in de **hoofdbeheerder**, **beveiligingsbeheerder**, **beveiligingslezer**, of **rapporteren lezer** rol voor de tenant. Bovendien kan elke gebruiker toegang krijgen tot eigen aanmeldingen. 

## <a name="troubleshoot-sign-in-errors-using-the-sign-ins-report"></a>Aanmelden-fouten oplossen met behulp van de rapport-aanmeldingen

1. Ga naar de [Azure-portal](https://portal.azure.com) en selecteer uw map.
2. Selecteer **Azure Active Directory** en selecteer in de sectie **Controle** de optie **Aanmeldingen**. 
3. De opgegeven filters gebruiken om de fout, ofwel door de gebruikersnaam of object-id, de toepassingsnaam of de datum verfijnen. Selecteer daarnaast **fout** uit de **Status** vervolgkeuzelijst om alleen de mislukte aanmeldingen weer te geven. 

    ![Resultaten filteren](./media/howto-troubleshoot-sign-in-errors/filters.png)
        
4. Identificeer de mislukte aanmelding die u wilt onderzoeken. Selecteer deze om het venster Extra details met meer informatie over de mislukte aanmelding te openen. Noteer de **aanmelden foutcode** en **reden van fout**. 

    ![Record selecteren](./media/howto-troubleshoot-sign-in-errors/sign-in-failures.png)
        
5. U kunt ook deze informatie vinden in de **probleemoplossing en ondersteuning** tabblad in het venster details.

    ![Problemen oplossen en ondersteuning](./media/howto-troubleshoot-sign-in-errors/troubleshooting-and-support.png)

6. De reden van fout beschrijving van de fout. Bijvoorbeeld, in dit scenario, reden van de fout is **ongeldige gebruikersnaam of wachtwoord of ongeldige on-premises-gebruikersnaam of wachtwoord**. De oplossing wordt gewoon aanmelden opnieuw met de juiste gebruikersnaam en wachtwoord.

7. Krijgt u meer informatie, waaronder ideeën voor herstel, door te zoeken naar de foutcode **50126** in dit voorbeeld in de [naslaginformatie over foutcodes in het aanmeldingen](reference-sign-ins-error-codes.md). 

8. Als niets werkt, of als het probleem zich blijft voordoen ondanks de aanbevolen cursus van actie [open een ondersteuningsticket](../fundamentals/active-directory-troubleshooting-support-howto.md) volgens de stappen in de **probleemoplossing en ondersteuning** tabblad. 

## <a name="next-steps"></a>Volgende stappen

* [Aanmeldingen-foutcodes verwijzen naar](reference-sign-ins-error-codes.md)
* [Overzicht van aanmeldingen](concept-sign-ins.md)
