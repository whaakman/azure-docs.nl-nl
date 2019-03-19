---
title: Gebruikers migreren naar een groepslicentie - Azure Active Directory | Microsoft Docs
description: Overstappen van afzonderlijke gebruikerslicenties aan op basis van een groep met behulp van Azure Active Directory-licentieverlening
services: active-directory
keywords: Azure AD-licenties
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: users-groups-roles
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: sumitp
ms.custom: seohack1;it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7fd800097ca5107f7df1e67c91ff4c96b9cfcd74
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58107743"
---
# <a name="how-to-add-licensed-users-to-a-group-for-licensing-in-azure-active-directory"></a>Gelicentieerde gebruikers toevoegen aan een groep voor licentieverlening in Azure Active Directory

Mogelijk hebt u bestaande licenties die zijn geïmplementeerd naar gebruikers in de organisaties via 'directe toewijzing'; dat wil zeggen, gebruik van PowerShell-scripts of andere hulpprogramma's voor afzonderlijke gebruikerslicenties toewijzen. Als u beginnen met behulp van Groepslicenties om licenties in uw organisatie te beheren wilt, moet u een migratieplan naadloos vervangen bestaande oplossingen met Groepslicenties.

Het belangrijkste dat rekening moet houden is dat u een situatie waarbij migreren naar Groepslicenties leidt gebruikers tijdelijk verlies van hun momenteel toegewezen licenties te vermijden. Elke proces dat tot het verwijderen van licenties leiden kan moet worden vermeden als u wilt verwijderen van het risico van gebruikers die toegang tot services en hun gegevens verliezen.

## <a name="recommended-migration-process"></a>Aanbevolen migratieproces

1. U hebt bestaande automatisering (bijvoorbeeld, PowerShell) voor het beheren van toewijzing van licenties en verwijderen voor gebruikers. Laat het wordt uitgevoerd.

2. Maak een nieuwe licentie-groep (of bepalen welke bestaande groepen moeten worden gebruikt) en zorg ervoor dat alle vereiste dat gebruikers worden toegevoegd als leden.

3. De vereiste licenties toewijzen aan deze groepen; het doel moet in overeenstemming met de dezelfde licentieverleningsstatus heeft die uw bestaande automatisering (bijvoorbeeld, PowerShell) wordt toegepast op gebruikers.

4. Controleer of dat licenties zijn toegepast op alle gebruikers in deze groepen. Deze toepassing kan worden gedaan door het controleren van de verwerkingsstatus voor elke groep en door het controleren van de logboeken voor controle.

   - Kunt u afzonderlijke gebruikers controle door te kijken naar de details van hun licentie. U ziet dat ze hebben dezelfde licenties zijn toegewezen "rechtstreeks" en "overgenomen" van groepen.

   - U kunt een PowerShell-script voor uitvoeren [controleren hoe licenties zijn toegewezen aan gebruikers](licensing-group-advanced.md#use-powershell-to-see-who-has-inherited-and-direct-licenses).

   - Als de dezelfde productlicentie is toegewezen aan de gebruiker rechtstreeks en via een groep, wordt slechts één licentie voor gebruikt door de gebruiker. Er zijn geen aanvullende licenties moeten daarom migratie uit te voeren.

5. Controleer of dat er geen licentietoewijzingen kunnen niet door het controleren van elke groep voor gebruikers in de foutstatus. Zie voor meer informatie, [identificeren en oplossen van problemen voor een groep](licensing-groups-resolve-problems.md).

6. Houd rekening met de oorspronkelijke directe toewijzingen; worden verwijderd mogelijk wilt u geleidelijk in 'blokken' voor het bewaken van het resultaat in een subset van gebruikers eerst doen.

   Kan u laat u de oorspronkelijke directe toewijzingen op gebruikers, maar als de gebruikers hun groepen met licenties laat ze nog steeds de oorspronkelijke licentie, is mogelijk niet wat u wilt behouden.

## <a name="an-example"></a>Een voorbeeld

Een organisatie heeft 1000 gebruikers. Alle gebruikers nodig hebben voor Enterprise Mobility + Security (EMS)-licenties. 200 gebruikers zijn op de afdeling Financiën en Office 365 Enterprise E3-licenties vereist. De organisatie heeft momenteel een PowerShell-script dat on-premises, toevoegen en verwijderen van licenties van gebruikers als ze afkomstig zijn en gaan. De organisatie wil echter vervangen door het script op basis van een groep, zodat de licenties automatisch kunnen worden beheerd door Azure AD-licentieverlening.

Dit is wat het migratieproces kan er als volgt uitzien:

1. Met behulp van Azure portal, de EMS-licentie voor het toewijzen de **alle gebruikers** groep in Azure AD. De E3-licentie voor het toewijzen de **de afdeling financiën** groep die de vereiste gebruikers bevat.

2. Bevestig dat de licentietoewijzing is voltooid voor alle gebruikers voor elke groep. Ga naar de blade voor elke groep, schakel **licenties**, en controleer de verwerkingsstatus aan de bovenkant van de **licenties** blade.

   - Zoek naar 'Nieuwste licentie wijzigingen zijn toegepast op alle gebruikers' om te bevestigen van de verwerking is voltooid.

   - Zoek naar een melding bovenaan over gebruikers voor wie licenties mogelijk niet met succes is toegewezen. Is er geen licenties voor bepaalde gebruikers uitvoeren? Sommige gebruikers hebben conflicterende licentie-SKU's die te voorkomen dat ze overgenomen van Groepslicenties?

3. Positie controleren op bepaalde gebruikers om te controleren of dat ze zowel de directe en groep licenties toegepast hebt. Ga naar de blade voor een gebruiker, selecteer **licenties**, en bekijk de status van licenties.

   - Dit is de status van de verwachte gebruiker tijdens de migratie:

      ![verwachte gebruikersstatus](./media/licensing-groups-migrate-users/expected-user-state.png)

   Hiermee bevestigt u dat de gebruiker direct en overgenomen licenties heeft. Zien we dat beide **EMS** en **E3** zijn toegewezen.

   - Selecteer elke licentie om meer informatie over de ingeschakelde services weer te geven. Dit kan worden gebruikt om te controleren als de directe en groep-licenties exact de dezelfde service-plannen voor de gebruiker inschakelen.

      ![Controleer de service-plannen](./media/licensing-groups-migrate-users/check-service-plans.png)

4. Nadat is bevestigd dat dat zowel direct als groep licenties gelijkwaardig zijn, kunt u beginnen met het verwijderen van directe licenties van gebruikers. U kunt dit testen door deze te verwijderen voor afzonderlijke gebruikers in de portal en voer vervolgens automatiseringsscripts van te zijn verwijderd in één bulkbewerking. Hier volgt een voorbeeld van dezelfde gebruiker in de directe licenties verwijderd via de portal. U ziet dat de licentiestatus ongewijzigd blijft, maar niet meer we direct toewijzingen zien.

   ![directe licenties die zijn verwijderd](./media/licensing-groups-migrate-users/direct-licenses-removed.png)


## <a name="next-steps"></a>Volgende stappen

Lees voor meer informatie over andere scenario's voor Licentiebeheer via groepen

* [Wat is licentieverlening in Azure Active Directory op basis van groep?](../fundamentals/active-directory-licensing-whatis-azure-portal.md)
* [Licenties toewijzen aan een groep in Azure Active Directory](licensing-groups-assign.md)
* [Licentieproblemen voor een groep vaststellen en oplossen in Azure Active Directory](licensing-groups-resolve-problems.md)
* [Het migreren van gebruikers tussen productlicenties groepsgebaseerde licentieverlening in Azure Active Directory gebruiken](licensing-groups-change-licenses.md)
* [Aanvullende scenario’s voor Azure Active Directory-licenties op basis van groepen](licensing-group-advanced.md)
* [PowerShell-voorbeelden voor Groepslicenties in Azure Active Directory](licensing-ps-examples.md)
