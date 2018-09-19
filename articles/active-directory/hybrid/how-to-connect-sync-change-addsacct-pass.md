---
title: 'Azure AD Connect-synchronisatie: het wachtwoord van het AD DS-account wijzigen | Microsoft Docs'
description: Dit document onderwerp wordt beschreven hoe u Azure AD Connect bijwerken nadat het wachtwoord van het AD DS-account is gewijzigd.
services: active-directory
keywords: AD DS-account, Active Directory-account, wachtwoord
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 051ff6aa4e650f884a4712376b5dc420cc86fc3a
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46311673"
---
# <a name="changing-the-ad-ds-account-password"></a>Het wachtwoord van het AD DS-account wijzigen
De AD DS-account verwijst naar het gebruikersaccount dat door Azure AD Connect gebruikt om te communiceren met on-premises Active Directory. Als u het wachtwoord van het AD DS-account wijzigt, moet u Azure AD Connect-synchronisatieservice bijwerken met het nieuwe wachtwoord. Anders wordt de synchronisatie kan niet meer correct synchroniseren met de on-premises Active Directory en u de volgende fouten optreden:

* In de bewerking Synchronization Service Manager, importeren of exporteren met on-premises AD die is mislukt met **geen-start-referenties** fout.

* In Windows-Logboeken, het logboek voor toepassingsgebeurtenissen bevat een fout opgetreden bij het **gebeurtenis-ID 6000** en het bericht **'de beheeragent 'contoso.com' kan niet worden uitgevoerd omdat de referenties ongeldig zijn'**.


## <a name="how-to-update-the-synchronization-service-with-new-password-for-ad-ds-account"></a>Het bijwerken van de Synchronization Service met een nieuw wachtwoord voor AD DS-account
De synchronisatieservice bijwerken met het nieuwe wachtwoord:

1. Start de Synchronization Service Manager (→-synchronisatieservice START).
</br>![Sync Service Manager](./media/how-to-connect-sync-change-addsacct-pass/startmenu.png)  

2. Ga naar de **Connectors** tabblad.

3. Selecteer de **AD-Connector** die overeenkomt met de AD DS-account waarvoor u het wachtwoord is gewijzigd.

4. Onder **acties**, selecteer **eigenschappen**.

5. Selecteer in het pop-updialoogvenster **verbinding maken met Active Directory-Forest**:

6. Voer het nieuwe wachtwoord van de AD DS-account in de **wachtwoord** tekstvak.

7. Klik op **OK** het nieuwe wachtwoord opslaan en sluiten van het pop-updialoogvenster.

8. Opnieuw opstarten van de Azure AD Connect synchronisatieservice onder Windows Service Control Manager. Dit is om ervoor te zorgen dat elke verwijzing naar het oude wachtwoord is verwijderd uit het cachegeheugen.

## <a name="next-steps"></a>Volgende stappen
**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: inzicht in en synchronisatie aanpassen](how-to-connect-sync-whatis.md)

* [Uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md)
