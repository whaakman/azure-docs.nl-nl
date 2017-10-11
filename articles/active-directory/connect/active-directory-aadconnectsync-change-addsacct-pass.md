---
title: 'Azure AD Connect-synchronisatie: het wachtwoord voor de AD DS-account wijzigen | Microsoft Docs'
description: Dit document onderwerp beschrijft hoe u Azure AD Connect bijwerken nadat het wachtwoord van het AD DS-account wordt gewijzigd.
services: active-directory
keywords: AD DS-account, Active Directory-account, wachtwoord
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 14e16a238e60ecfeeb3cbf88c3922a79349dcc75
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="changing-the-ad-ds-account-password"></a>Het wachtwoord voor de AD DS-account wijzigen
De AD DS-account verwijst naar het gebruikersaccount dat door Azure AD Connect worden gebruikt om te communiceren met de lokale Active Directory. Als u het wachtwoord van de AD DS-account wijzigt, moet u Azure AD Connect-synchronisatieservice bijwerken met het nieuwe wachtwoord. Anders wordt de synchronisatie kan niet meer correct synchroniseren met de lokale Active Directory en de volgende fouten optreden:

* In de bewerking Synchronization Service Manager, een importeren of exporteren met on-premises AD mislukt met **Nee-begin-referenties** fout.

* Onder Windows Logboeken, het logboek voor toepassingsgebeurtenissen bevat een fout opgetreden bij **gebeurtenis-ID 6000** en het bericht **'de beheeragent 'contoso.com' kan niet worden uitgevoerd omdat de referenties ongeldig zijn'**.


## <a name="how-to-update-the-synchronization-service-with-new-password-for-ad-ds-account"></a>Het bijwerken van de synchronisatieservice met een nieuw wachtwoord voor AD DS-account
De synchronisatieservice bijwerken met het nieuwe wachtwoord:

1. Synchronization Service Manager (START → Synchronization-Service) starten.
</br>![Sync-Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)  

2. Ga naar de **Connectors** tabblad.

3. Selecteer de **AD-Connector** die overeenkomt met de AD DS-account waarvoor u het wachtwoord is gewijzigd.

4. Onder **acties**, selecteer **eigenschappen**.

5. Selecteer in het pop-updialoogvenster **verbinding maken met Active Directory-Forest**:

6. Voer het nieuwe wachtwoord van de AD DS-account in de **wachtwoord** textbox.

7. Klik op **OK** naar het nieuwe wachtwoord opslaan en sluiten van een pop-upvenster.

8. Opnieuw opstarten van de Azure AD Connect onder Windows Service Control Manager-synchronisatieservice. Dit is om ervoor te zorgen dat elke verwijzing naar het oude wachtwoord is verwijderd uit het cachegeheugen.

## <a name="next-steps"></a>Volgende stappen
**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: inzicht en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)

* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
