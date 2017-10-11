---
title: Uw afzonderlijke gelicentieerde gebruikers migreren naar een groep in Azure Active Directory | Microsoft Docs
description: Overschakelen van afzonderlijke gebruikerslicenties naar op basis van een groep met Azure Active Directory-licentieverlening
services: active-directory
keywords: Azure AD-licentieverlening
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/05/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6b77dd4e9a6d361a05382397e89b575896fdad84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-add-licensed-users-to-a-group-for-licensing-in-azure-active-directory"></a>Gelicentieerde gebruikers toevoegen aan een groep voor licentieverlening in Azure Active Directory

Wellicht hebt u bestaande licenties die zijn geïmplementeerd voor gebruikers in de organisaties via 'rechtstreekse toewijzing'; dat wil zeggen, met behulp van PowerShell-scripts of andere hulpprogramma's individuele licenties toewijzen. Als u beginnen op basis van een groep licentieverlening gebruiken om licenties in uw organisatie te beheren wilt, moet u een migratieplan naadloos vervangt door bestaande oplossingen op basis van een groep licenties.

Het belangrijkste rekening moet houden is dat u een situatie waarin migreren naar een groep op basis van een licentie kunnen gebruikers tijdelijk verlies van de momenteel toegewezen licenties moet voorkomen. Elke proces dat tot de verwijdering van de licenties leiden kan moet worden vermeden als u wilt verwijderen van het risico van gebruikers die toegang tot services en hun gegevens verliezen.

## <a name="recommended-migration-process"></a>Aanbevolen migratieproces

1. U hebt bestaande automatisering (bijvoorbeeld PowerShell) voor het beheren van de licentietoewijzing en verwijderen voor gebruikers. Laat het actief is.

2. Maak een nieuwe groep in de licentieverlening (of bepalen welke bestaande groepen moeten worden gebruikt) en zorg ervoor dat alle vereiste dat gebruikers worden toegevoegd als leden.

3. De vereiste licenties toewijzen aan deze groepen; het doel moet in overeenstemming met de dezelfde licentiestatus van uw bestaande automatisering (bijvoorbeeld PowerShell) voor deze gebruikers toepassen.

4. Controleren of licenties zijn toegepast op alle gebruikers in deze groepen. Dit kan worden gedaan door het controleren van de verwerkingsstatus van elke groep en door te controleren van controlelogboeken.

  - Kunt u afzonderlijke gebruikers controle door te kijken hun licentiegegevens. U ziet dat zij dezelfde licenties toegewezen 'rechtstreeks' en 'overgenomen' uit groepen hebben.

  - U kunt een PowerShell-script uitvoeren [controleren hoe licenties zijn toegewezen aan gebruikers](active-directory-licensing-group-advanced.md#use-powershell-to-see-who-has-inherited-and-direct-licenses).

  - Wanneer de dezelfde productlicentie is toegewezen aan de gebruiker rechtstreeks en door een groep, wordt slechts één licentie gebruikt door de gebruiker. Er zijn geen aanvullende licenties zijn daarom vereist voor de migratie uit te voeren.

5. Controleer of dat er geen licentietoewijzingen niet door het controleren van elke groep voor gebruikers in een foutstatus. Zie voor meer informatie [identificeren en oplossen van licentieproblemen met een voor een groep](active-directory-licensing-group-problem-resolution-azure-portal.md).

6. Houd rekening met het verwijderen van de oorspronkelijke direct toewijzingen; u kunt geleidelijk in 'golven' voor het bewaken van de uitkomst is op een subset van gebruikers eerst doen.

  Kan u de oorspronkelijke direct toewijzingen behouden op gebruikers, maar wanneer de gebruikers laat hun licentiegroepen die ze behoud de oorspronkelijke licentie, mogelijk is niet wilt dat u wilt.

## <a name="an-example"></a>Een voorbeeld

We hebben een organisatie met 1000 gebruikers. Alle gebruikers vereisen Enterprise Mobility + Security (EMS)-licenties. 200 gebruikers zijn in de afdeling Financiën en Office 365 Enterprise E3-licenties vereist. We hebben een PowerShell-script met on-premises toevoegen en verwijderen van licenties van gebruikers als ze worden geleverd en gaat. We willen vervangen door het script op basis van een groep zodat de licenties worden automatisch beheerd door Azure AD-licentieverlening.

Dit is wat het migratieproces kan er als volgt uitzien:

1. Met de Azure portal toewijzen de EMS-licentie voor de **alle gebruikers** groep in Azure AD. Toewijzen van de licentie E3 voor de **de afdeling financiën** groep die de vereiste gebruikers bevat.

2. Bevestig dat de licentietoewijzing is voltooid voor alle gebruikers voor elke groep. Ga naar de blade voor elke groep, selecteer **licenties**, en controleer de verwerkingsstatus aan de bovenkant van de **licenties** blade.

  - Zoek naar 'Nieuwste licentie wijzigingen zijn toegepast op alle gebruikers' om te bevestigen dat de verwerking is voltooid.

  - Zoek naar een melding op de voorgrond over alle gebruikers voor wie licenties mogelijk niet met succes is toegewezen. Heeft we uitgevoerd buiten de licenties voor sommige gebruikers? Beschikt over sommige gebruikers conflicterende licentie-SKU's die voorkomen dat ze groep licenties overnemen?

3. Positie Controleer sommige gebruikers om te controleren of dat ze zowel de directe en groep licenties toegepast hebt. Ga naar de blade voor een gebruiker, schakelt **licenties**, en bekijk de status van licenties.

  - Dit is de verwachte gebruikersstatus tijdens de migratie:

      ![verwachte gebruikersstatus](media/active-directory-licensing-group-migration-azure-portal/expected-user-state.png)

  Hiermee bevestigt u dat de gebruiker directe en overgenomen licenties heeft. Zien we dat beide **EMS** en **E3** zijn toegewezen.

  - Selecteer elke licentie om meer informatie over de ingeschakelde services weer te geven. Dit kan worden gebruikt om te controleren als de directe en groep licenties exact de dezelfde serviceniveaus voor de gebruiker inschakelen.

      ![Controleer de service-abonnementen](media/active-directory-licensing-group-migration-azure-portal/check-service-plans.png)

4. Nadat is bevestigd dat direct en groep licenties equivalent zijn, kunt u beginnen door directe licenties van gebruikers te verwijderen. U kunt dit testen door deze te verwijderen voor afzonderlijke gebruikers in de portal en voer vervolgens automatiseringsscripts van te zijn verwijderd in bulk. Hier volgt een voorbeeld van dezelfde gebruiker in de directe licenties verwijderd via de portal. U ziet dat de licentiestatus ongewijzigd blijft, maar niet meer we direct toewijzingen zien.

  ![directe licenties die zijn verwijderd](media/active-directory-licensing-group-migration-azure-portal/direct-licenses-removed.png)


## <a name="next-steps"></a>Volgende stappen

Lees voor meer informatie over andere scenario's voor Licentiebeheer via groepen.

* [Licenties toewijzen aan een groep in Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md)
* [Wat is licentieverlening in Azure Active Directory op basis van groep?](active-directory-licensing-whatis-azure-portal.md)
* [Opsporen en oplossen van licentieproblemen met een voor een groep in Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Azure Active Directory-licentieverlening voor aanvullende scenario's op basis van groep](active-directory-licensing-group-advanced.md)
