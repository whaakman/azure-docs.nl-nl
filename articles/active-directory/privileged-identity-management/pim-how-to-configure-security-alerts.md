---
title: Beveiligingswaarschuwingen voor Azure AD directory-rollen in PIM configureren | Microsoft Docs
description: Informatie over het configureren van beveiligingswaarschuwingen voor Azure AD directory-rollen in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 11/21/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 4a715020e37d5885dac26ac0573efe985c3f2cfb
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2018
ms.locfileid: "52291212"
---
# <a name="configure-security-alerts-for-azure-ad-directory-roles-in-pim"></a>Beveiligingswaarschuwingen voor Azure AD directory-rollen in PIM configureren

Azure AD Privileged Identity Management (PIM) genereert waarschuwingen wanneer er verdachte of unsafe activiteit in uw omgeving. Wanneer een waarschuwing wordt geactiveerd, wordt deze weergegeven op de PIM-dashboard. Selecteer de waarschuwing om te zien van een rapport met de gebruikers of rollen die de waarschuwing is geactiveerd.

![Beveiligingswaarschuwingen in PIM - schermopname](./media/pim-how-to-configure-security-alerts/pim-directory-alerts.png)

## <a name="security-alerts"></a>Beveiligingswaarschuwingen

In deze sectie geeft een lijst van alle beveiligingswaarschuwingen voor directory-rollen, samen met het oplossen van en hoe u kunt voorkomen. Ernst heeft de volgende betekenis:

* **Hoge**: direct actie vereist vanwege een schending van het beleid.
* **Gemiddeld**: geen directe actie is vereist, maar geeft een mogelijke schending van het beleid.
* **Lage**: geen directe actie is vereist, maar stelt u een beleidswijziging van de voorkeur boven het.

### <a name="administrators-arent-using-their-privileged-roles"></a>Beheerders hun bevoorrechte rollen niet gebruikt

| | |
| --- | --- |
| **Ernst** | Laag |
| **Waarom krijg ik deze waarschuwing?** | Gebruikers die zijn toegewezen aan de bevoorrechte rollen die ze nodig hebben geen verhoogt de kans op een aanval. Ook is het gemakkelijker voor aanvallers om te blijven ongemerkt in accounts die zijn niet actief wordt gebruikt. |
| **Over het oplossen van?** | Bekijk de gebruikers in de lijst en verwijder ze van bevoorrechte rollen die ze niet hoeven. |
| **Preventie** | Alleen bevoorrechte rollen toewijzen aan gebruikers die een zakelijke rechtvaardiging hebben. </br>Plannen van regelmatige [toegangsbeoordelingen](pim-how-to-start-security-review.md) om te controleren dat gebruikers nog steeds hun toegang nodig. |
| **In de portal risicobeperking actie** | Hiermee verwijdert u het account van hun bevoegde rol. |
| **Trigger** | Geactiveerd als een gebruiker een bepaalde hoeveelheid tijd gaat zonder een rol activeren. |
| **Aantal dagen** | Deze instelling bepaalt u het aantal dagen tussen 0 en 100, die een gebruiker zonder het activeren van een rol kan raadplegen.|

### <a name="roles-dont-require-multi-factor-authentication-for-activation"></a>Rollen vereisen geen meervoudige verificatie voor activering

| | |
| --- | --- |
| **Ernst** | Laag |
| **Waarom krijg ik deze waarschuwing?** | Zonder MFA, kunnen verdachte gebruikers bevoorrechte rollen activeren. |
| **Over het oplossen van?** | Bekijk de lijst met rollen en [MFA vereisen](pim-how-to-change-default-settings.md) voor elke rol. |
| **Preventie** | [MFA vereisen](pim-how-to-change-default-settings.md) voor elke rol.  |
| **In de portal risicobeperking actie** | MFA vereist voor de activering van de bevoorrechte rol maakt. |

### <a name="the-tenant-doesnt-have-azure-ad-premium-p2"></a>De tenant geen Azure AD Premium P2

| | |
| --- | --- |
| **Ernst** | Laag |
| **Waarom krijg ik deze waarschuwing?** | De huidige tenant heeft geen Azure AD Premium P2. |
| **Over het oplossen van?** | Lees de informatie over [Azure AD-edities](../fundamentals/active-directory-whatis.md). Een upgrade uitvoeren naar Azure AD Premium P2. |

### <a name="potential-stale-accounts-in-a-privileged-role"></a>Mogelijke verouderde accounts in een bevoorrechte rol

| | |
| --- | --- |
| **Ernst** | Middelgroot |
| **Waarom krijg ik deze waarschuwing?** | Accounts die hun wachtwoord niet hebt gewijzigd onlangs mogelijk service of gedeelde accounts die worden niet bijgehouden. Deze accounts in bevoorrechte rollen zijn kwetsbaar voor aanvallen. |
| **Over het oplossen van?** | Bekijk de accounts in de lijst. Als deze toegang niet meer nodig hebt, kunt u ze uit hun bevoorrechte rollen verwijderen. |
| **Preventie** | Zorg ervoor dat de accounts die worden gedeeld sterke wachtwoorden zijn draaien wanneer er een wijziging in de gebruikers die het wachtwoord weten. </br>Controleer regelmatig accounts met bevoorrechte rollen met behulp van [toegangsbeoordelingen](pim-how-to-start-security-review.md) en verwijderen van roltoewijzingen die niet meer nodig zijn. |
| **In de portal risicobeperking actie** | Hiermee verwijdert u het account van hun bevoegde rol. |
| **Aanbevolen procedures** | Gedeeld, service, en toegang in noodgevallen-accounts die worden geverifieerd met een wachtwoord en zijn toegewezen aan maximaal beschermde beheerderrol, zoals globale beheerder of beveiligingsbeheerder moeten hebben hun wachtwoorden gedraaid voor de volgende gevallen:<ul><li>Na een beveiligingsincident met betrekking tot misbruik of inbreuk op de administratieve toegangsrechten</li><li>Nadat de bevoegdheden van een gebruiker zijn gewijzigd zodat ze zijn niet langer een beheerder (bijvoorbeeld: nadat een werknemer die een beheerder leaves IT is of de organisatie verlaat)</li><li>Met regelmatige tussenpozen (voor bijvoorbeeld, per kwartaal of per jaar), zelfs als er is geen bekende nakoming of wijziging van IT personeel</li></ul>Omdat meerdere personen toegang tot deze accounts referenties hebben, moeten de referenties om ervoor te zorgen dat mensen die er nog over hun rollen geen toegang meer de accounts tot kan worden gedraaid. [Meer informatie](https://aka.ms/breakglass) |

### <a name="roles-are-being-assigned-outside-of-pim"></a>Rollen worden toegewezen buiten PIM

| | |
| --- | --- |
| **Ernst** | Hoog |
| **Waarom krijg ik deze waarschuwing?** | Toewijzingen van bevoorrechte rollen die buiten PIM niet correct worden bewaakt en kunnen duiden op een actieve aanval. |
| **Over het oplossen van?** | Bekijk de gebruikers in de lijst en verwijder ze van bevoorrechte rollen toegewezen buiten PIM. |
| **Preventie** | Onderzoeken waar gebruikers bevoorrechte rollen buiten PIM worden toegewezen en toekomstige toewijzingen van daaruit verbieden. |
| **In de portal risicobeperking actie** | Hiermee verwijdert u het account van hun bevoegde rol. |

### <a name="there-are-too-many-global-administrators"></a>Er zijn te veel globale beheerders

| | |
| --- | --- |
| **Ernst** | Laag |
| **Waarom krijg ik deze waarschuwing?** | Globale beheerder is de hoogste bevoorrechte rol. Als een globale beheerder is geknoeid, is de aanvaller heeft toegang tot hun machtigingen, waardoor het hele systeem lopen. |
| **Over het oplossen van?** | De gebruikers in de lijst bekijken en verwijderen die de rol globale beheerder niet absoluut nodig. </br>Deze gebruikers lagere bevoorrechte rollen toewijzen. |
| **Preventie** | Gebruikers toewijzen in de minste bevoorrechte rollen die ze nodig hebben. |
| **In de portal risicobeperking actie** | Hiermee verwijdert u het account van hun bevoegde rol. |
| **Trigger** | Als twee verschillende criteria wordt voldaan, en u van beide configureren kunt geactiveerd. Eerst moet u een bepaalde drempelwaarde voor globale beheerders bereiken. Ten tweede moet een bepaald percentage van het totaal aantal roltoewijzingen globale beheerders. Als u alleen een van deze metingen aan, wordt de waarschuwing niet weergegeven. |
| **Minimum aantal globale beheerders** | Deze instelling bepaalt u het aantal globale beheerders van 2 tot en met 100, dat u rekening houden met een onveilige bedrag. |
| **Percentage globale beheerders** | Deze instelling bepaalt u het minimumpercentage van beheerders die globale beheerders van 0% tot 100%, die is niet veilig in uw omgeving. |

### <a name="roles-are-being-activated-too-frequently"></a>Rollen worden te vaak wordt geactiveerd.

| | |
| --- | --- |
| **Ernst** | Laag |
| **Waarom krijg ik deze waarschuwing?** | Meerdere activeringen naar dezelfde bevoorrechte rol door dezelfde gebruiker is een teken van een aanval. |
| **Over het oplossen van?** | Bekijk de gebruikers in de lijst en zorg ervoor dat de [activeringsduur](pim-how-to-change-default-settings.md) voor hun bevoegde rol lang genoeg zijn voor ze hun taken uit te voeren is ingesteld. |
| **Preventie** | Zorg ervoor dat de [activeringsduur](pim-how-to-change-default-settings.md) voor bevoorrechte rollen is ingesteld lang genoeg is voor gebruikers hun taken uit te voeren.</br>[MFA vereisen](pim-how-to-change-default-settings.md) voor bevoorrechte rollen die accounts die worden gedeeld door meerdere beheerders hebt. |
| **In de portal risicobeperking actie** | N/A |
| **Trigger** | Geactiveerd als een gebruiker dezelfde bevoorrechte rol meerdere keren binnen een opgegeven periode activeert. U kunt zowel de periode en het aantal activeringen configureren. |
| **Activeringsvernieuwing** | Deze instelling bepaalt u in dagen, uren, minuten en vervolgens de periode die u wilt gebruiken voor het bijhouden van verdachte vernieuwingen. |
| **Aantal vernieuwingen van activering** | Deze instelling bepaalt u het aantal activeringen van 2 tot en met 100, waarmee u rekening houden daling van waarschuwing in de periode die u hebt gekozen. U kunt deze instelling wijzigen door de schuifregelaar of een getal in het tekstvak te typen. |

## <a name="configure-security-alert-settings"></a>Instellingen voor beveiligingswaarschuwingen configureren

U kunt enkele van de beveiligingswaarschuwingen in PIM om te werken met uw omgeving en beveiligingsdoelen. Volg deze stappen voor het openen van de instellingen voor beveiligingswaarschuwingen:

1. Open **Azure AD Privileged Identity Management**.

1. Klik op **Azure AD-rollen**.

1. Klik op **instellingen** en vervolgens **waarschuwingen**.

    ![Navigeer naar de beveiligingsinstellingen voor waarschuwingen](./media/pim-how-to-configure-security-alerts/settings-alerts.png)

1. Klik op de naam van een waarschuwing om de instelling voor deze waarschuwing te configureren.

    ![Instellingen voor beveiligingswaarschuwingen](./media/pim-how-to-configure-security-alerts/security-alert-settings.png)

## <a name="next-steps"></a>Volgende stappen

- [Azure AD directory rolinstellingen in PIM configureren](pim-how-to-change-default-settings.md)
