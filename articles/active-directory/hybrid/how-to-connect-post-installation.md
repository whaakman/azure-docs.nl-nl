---
title: 'Azure AD Connect: Volgende stappen en over het beheren van Azure AD Connect | Microsoft Docs'
description: Informatie over het uitbreiden van de standaardconfiguratie en operationele taken voor Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: c18bee36-aebf-4281-b8fc-3fe14116f1a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/12/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 291b3d506993cfea89be072684835c0d4efe75f6
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58095446"
---
# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>Volgende stappen en over het beheren van Azure AD Connect
Gebruik de operationele procedures in dit artikel om aan te passen van Azure Active Directory (Azure AD) Connect om te voldoen aan de behoeften en vereisten van uw organisatie.  

## <a name="add-additional-sync-admins"></a>Aanvullende synchronisatie beheerders toevoegen
Standaard worden alleen de gebruiker die heeft de installatie- en lokale beheerders kunnen voor het beheren van de geïnstalleerde synchronisatie-engine. Voor extra gebruikers kunnen toegang tot en beheer van de synchronisatie-engine, zoek de groep ADSyncAdmins met de naam op de lokale server en deze toevoegen aan deze groep.

## <a name="assign-licenses-to-azure-ad-premium-and-enterprise-mobility-suite-users"></a>Licenties toewijzen aan gebruikers van Azure AD Premium en Enterprise Mobility Suite
Nu dat uw gebruikers zijn gesynchroniseerd naar de cloud, moet u ze een licentie toewijzen, zodat ze kunnen aan de slag met cloud-apps zoals Office 365.

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>Een Azure AD Premium of Enterprise Mobility Suite-licentie toewijzen

1. Aanmelden bij de Azure-portal als een beheerder.
2. Selecteer aan de linkerkant **Active Directory**.
3. Op de **Active Directory** pagina, dubbelklik op de map met de gebruikers die u wilt instellen.
4. Selecteer boven aan de pagina met de adreslijst **Licenties**.
5. Op de **licenties** weergeeft, schakelt **Active Directory Premium** of **Enterprise Mobility Suite**, en klik vervolgens op **toewijzen**.
6. Selecteer in het dialoogvenster de gebruikers aan wie u een licentie wilt toewijzen en klik op het vinkje om de wijzigingen op te slaan.

## <a name="verify-the-scheduled-synchronization-task"></a>Controleer of de geplande synchronisatie-taak
De Azure portal gebruiken om de status van een synchronisatie te controleren.

### <a name="to-verify-the-scheduled-synchronization-task"></a>Om te controleren of de geplande synchronisatie-taak
1. Aanmelden bij de Azure-portal als een beheerder.
2. Selecteer aan de linkerkant **Active Directory**.
3. Op de **Active Directory** pagina, dubbelklik op de map met de gebruikers die u wilt instellen.
4. Aan de bovenkant van de directory-pagina, selecteer **Adreslijstintegratie**.
5. Onder **integratie met lokale active directory**, houd er rekening mee de tijd van laatste synchronisatie.

<center>

![Tijd van Directory-synchronisatie](./media/how-to-connect-post-installation/verify.png)</center>

## <a name="start-a-scheduled-synchronization-task"></a>Een taak geplande synchronisatie starten
Als u een synchronisatietaak uitvoeren wilt, kunt u dit doen door met de Azure AD Connect-wizard nogmaals uit te voeren.  U moet uw Azure AD-referenties op te geven.  Selecteer in de wizard de **aanpassen Synchronisatieopties** taak en klik op **volgende** verplaatsen via de wizard. Zorg ervoor dat aan het einde de **Start het synchronisatieproces zodra de initiële configuratie is voltooid** is ingeschakeld.

<center>

![Synchronisatie starten](./media/how-to-connect-post-installation/startsynch.png)</center>

Zie voor meer informatie over de Azure AD Connect sync Scheduler [Azure AD Connect Scheduler](how-to-connect-sync-feature-scheduler.md).

## <a name="additional-tasks-available-in-azure-ad-connect"></a>Aanvullende taken die beschikbaar zijn in Azure AD Connect
Na de initiële installatie van Azure AD Connect, kunt u altijd de wizard opnieuw starten vanuit de Azure AD Connect-start-snelkoppeling pagina of het bureaublad.  U ziet dat de wizard opnieuw doorlopen aantal nieuwe opties in de vorm van aanvullende taken biedt.  

De volgende tabel bevat een overzicht van deze taken en een korte beschrijving van elke taak.

![Lijst met extra taken](./media/how-to-connect-post-installation/addtasks.png)

| Extra taken | Description |
| --- | --- |
| **Het geselecteerde scenario weergeven** |Bekijk uw huidige Azure AD Connect-oplossing.  Dit omvat algemene instellingen, gesynchroniseerd mappen en synchronisatie-instellingen. |
| **Synchronisatieopties aanpassen** |De huidige configuratie, zoals aanvullende Active Directory-forests toevoegen aan de configuratie of het inschakelen van synchronisatie-opties, zoals gebruikers, groepen, apparaat of terugschrijven van wachtwoord wijzigen. |
| **De Faseringsmodus inschakelen** |Fase-informatie die niet direct is gesynchroniseerd en is niet geëxporteerd naar Azure AD of on-premises Active Directory.  Met deze functie kunt u de synchronisaties bekijken voordat ze optreden. |

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md).
