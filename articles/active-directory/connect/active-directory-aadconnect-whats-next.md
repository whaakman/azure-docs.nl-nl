---
title: 'Azure AD Connect: De volgende stappen en het beheren van Azure AD Connect | Microsoft Docs'
description: Informatie over het uitbreiden van de standaardconfiguratie en operationele taken voor Azure AD Connect.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: curtand
ms.assetid: c18bee36-aebf-4281-b8fc-3fe14116f1a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: e898ee203276b072605fe0f21af633ecfdaae1fe
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>Volgende stappen en het beheren van Azure AD Connect
Gebruik de operationele procedures in dit artikel voor het aanpassen van Azure Active Directory (Azure AD) verbinden om te voldoen aan de behoeften van uw organisatie en de vereisten.  

## <a name="add-additional-sync-admins"></a>Voeg aanvullende sync beheerders toe
Standaard alleen de gebruiker die heeft de installatie en lokale beheerders zich voor het beheren van de geïnstalleerde synchronisatie-engine. Voor andere personen om toegang tot en beheer van de synchronisatie-engine te kunnen vinden van de groep ADSyncAdmins op de lokale server en toe te voegen aan deze groep.

## <a name="assign-licenses-to-azure-ad-premium-and-enterprise-mobility-suite-users"></a>Licenties toewijzen aan gebruikers van Azure AD Premium en Enterprise Mobility Suite
Nu uw gebruikers zijn gesynchroniseerd met de cloud, moet u ze een licentie toewijzen zodat ze kunnen aan de slag met cloud-apps, zoals Office 365.

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>Een Azure AD Premium of Enterprise Mobility Suite-licentie toewijzen

1. Meld u aan bij de Azure portal als een beheerder.
2. Selecteer aan de linkerkant **Active Directory**.
3. Op de **Active Directory** pagina, dubbelklikt u op de map met de gebruikers die u wilt instellen.
4. Selecteer boven aan de pagina met de adreslijst **Licenties**.
5. Op de **licenties** pagina **Active Directory Premium** of **Enterprise Mobility Suite**, en klik vervolgens op **toewijzen**.
6. Selecteer in het dialoogvenster de gebruikers aan wie u een licentie wilt toewijzen en klik op het vinkje om de wijzigingen op te slaan.

## <a name="verify-the-scheduled-synchronization-task"></a>Controleer of de taak geplande synchronisatie
De Azure portal gebruiken om de status van een synchronisatie te controleren.

### <a name="to-verify-the-scheduled-synchronization-task"></a>De geplande synchronisatietaak controleren
1. Meld u aan bij de Azure portal als een beheerder.
2. Selecteer aan de linkerkant **Active Directory**.
3. Op de **Active Directory** pagina, dubbelklikt u op de map met de gebruikers die u wilt instellen.
4. Selecteer boven aan de directorypagina **Adreslijstintegratie**.
5. Onder **integratie met lokale active directory**, houd er rekening mee de tijd van laatste synchronisatie.

<center>![Tijd van de Directory-synchronisatie](./media/active-directory-aadconnect-whats-next/verify.png)</center>

## <a name="start-a-scheduled-synchronization-task"></a>Start een geplande synchronisatietaak
Als u een synchronisatietaak uitvoeren wilt, kunt u dit doen door het opnieuw met de Azure AD Connect-wizard uit te voeren.  U moet uw Azure AD-referenties op te geven.  Selecteer in de wizard de **Synchronisatieopties aanpassen** en op **volgende** om de wizard te doorlopen. Zorg ervoor dat aan het einde de **Start het synchronisatieproces zodra de eerste configuratie is voltooid** is ingeschakeld.

<center>![Synchronisatie starten](./media/active-directory-aadconnect-whats-next/startsynch.png)</center>

Zie voor meer informatie over de Azure AD Connect sync Scheduler [met Azure AD Connect Scheduler](active-directory-aadconnectsync-feature-scheduler.md).

## <a name="additional-tasks-available-in-azure-ad-connect"></a>Aanvullende taken die beschikbaar zijn in Azure AD Connect
Na de initiële installatie van Azure AD Connect, kunt u altijd de wizard opnieuw starten vanuit de snelkoppeling Azure AD Connect start pagina of het bureaublad.  U ziet dat opnieuw de verbinding met de wizard een aantal nieuwe opties in de vorm van aanvullende taken biedt.  

De volgende tabel biedt een overzicht van deze taken en een korte beschrijving van elke taak.

![Lijst met extra taken](./media/active-directory-aadconnect-whats-next/addtasks.png)

| Extra taak | Beschrijving |
| --- | --- |
| **Het geselecteerde scenario weergeven** |Uw huidige Azure AD Connect-oplossing weergeven.  Dit omvat algemene instellingen, gesynchroniseerd mappen en de synchronisatie-instellingen. |
| **Opties voor synchronisatie aanpassen** |De huidige configuratie zoals aanvullende Active Directory-forests toevoegen aan de configuratie of het inschakelen van synchronisatie-opties, zoals gebruikers, groepen, apparaat of terugschrijven van wachtwoord wijzigen. |
| **Faseringsmodus inschakelen** |Fase-informatie die niet direct is gesynchroniseerd en is niet geëxporteerd naar Azure AD of on-premises Active Directory.  Met deze functie kunt u de synchronisaties bekijken voordat ze optreden. |

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md).
