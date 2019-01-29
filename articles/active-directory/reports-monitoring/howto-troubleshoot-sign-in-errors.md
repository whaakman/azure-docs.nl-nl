---
title: Oplossen aanmelden met behulp van Azure Active Directory-rapporten | Microsoft Docs
description: Informatie over het oplossen van aanmelding bij fouten met behulp van Azure Active Directory-rapporten in Azure portal
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 7225ebd91c4c236b5f54574f1657239420c9f541
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55165401"
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
* Een gebruiker die de rol **Globale beheerder**, **Beveiligingsbeheerder**, **Beveiligingslezer** of **Rapportlezer** voor de tenant heeft. Bovendien kan elke gebruiker toegang krijgen tot eigen aanmeldingen. 

## <a name="troubleshoot-sign-in-errors-using-the-sign-ins-report"></a>Aanmelden-fouten oplossen met behulp van de rapport-aanmeldingen

1. Ga naar de [Azure-portal](https://portal.azure.com) en selecteer uw map.
2. Selecteer **Azure Active Directory** en selecteer in de sectie **Controle** de optie **Aanmeldingen**. 
3. De opgegeven filters gebruiken om de fout, ofwel door de gebruikersnaam of object-id, de toepassingsnaam of de datum verfijnen. Selecteer daarnaast **fout** uit de **Status** vervolgkeuzelijst om alleen de mislukte aanmeldingen weer te geven. 

    ![Resultaten filteren](./media/howto-troubleshoot-sign-in-errors/filters.png)
        
4. Identificeer de mislukte aanmelding die u wilt onderzoeken en te selecteren. Hiermee opent u het venster Extra details met meer informatie over de aanmelding is mislukt. Noteer de **aanmelden foutcode** en **reden van fout**. 

    ![Record selecteren](./media/howto-troubleshoot-sign-in-errors/sign-in-failures.png)
        
5. U kunt ook deze informatie vinden in de **probleemoplossing en ondersteuning** tabblad in het venster details.

    ![Problemen oplossen en ondersteuning](./media/howto-troubleshoot-sign-in-errors/troubleshooting-and-support.png)

6. De reden van fout beschrijving van de fout. Bijvoorbeeld, in dit scenario, reden van de fout is **ongeldige gebruikersnaam of wachtwoord of ongeldige on-premises-gebruikersnaam of wachtwoord**. Dit betekent dat de gebruiker hebt ingevoerd een onjuiste gebruikersnaam of het wachtwoord om aan te melden bij Azure portal. De oplossing wordt gewoon aanmelden opnieuw met de juiste gebruikersnaam en wachtwoord.

7. Krijgt u meer informatie, waaronder ideeën voor herstel, door te zoeken naar de foutcode **50126** in dit voorbeeld in de [naslaginformatie over foutcodes in het aanmeldingen](reference-sign-ins-error-codes.md). 

8. Als niets werkt, of als het probleem zich blijft voordoen ondanks de aanbevolen cursus van actie [open een ondersteuningsticket](../fundamentals/active-directory-troubleshooting-support-howto.md) volgens de stappen in de **probleemoplossing en ondersteuning** tabblad. 

## <a name="next-steps"></a>Volgende stappen

* [Aanmeldingen-foutcodes verwijzen naar](reference-sign-ins-error-codes.md)
* [Overzicht van aanmeldingen](concept-sign-ins.md)
