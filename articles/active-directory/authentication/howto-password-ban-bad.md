---
title: Hoe bEen wachtwoorden in Azure AD
description: Zwakke wachtwoorden van uw envirionment met Azure AD dynamisch verboden passwrords bEen
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: rogoya
ms.openlocfilehash: 0c1517f94d4a6d59077b62614eec8fef665b1529
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296139"
---
# <a name="configuring-the-custom-banned-password-list"></a>De lijst met aangepaste verboden wachtwoorden configureren

|     |
| --- |
| Azure AD-wachtwoordbeveiliging en de lijst met aangepaste verboden wachtwoorden zijn openbare preview-functies van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksvoorwaarden voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Veel organisaties vinden dat hun gebruikers wachtwoorden met algemene lokale woorden zoals een school, sportteam of beroemdheid die, zodat ze gemakkelijk te raden maken. Lijst met aangepaste verboden wachtwoorden van Microsoft kunnen organisaties om toe te voegen tekenreeksen als u wilt evalueren en te blokkeren, naast de globale verboden lijst met wachtwoorden, wanneer gebruikers en beheerders probeert te wijzigen of opnieuw instellen van een wachtwoord.

## <a name="add-to-the-custom-list"></a>Toevoegen aan de aangepaste lijst

De lijst met aangepaste verboden wachtwoorden configureren, is een Azure Active Directory Premium P1 of P2-licentie vereist. Zie voor meer informatie over licenties voor Azure Active Directory, de [Azure Active Directory pagina met prijzen](https://azure.microsoft.com/pricing/details/active-directory/). |

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en blader naar **Azure Active Directory**, **verificatiemethoden**, klikt u vervolgens **wachtwoordbeveiliging (Preview)**.
1. Stel de optie **afdwingen aangepaste lijst**, naar **Ja**.
1. Toevoegen van tekenreeksen voor de **aangepaste lijst met wachtwoorden verboden**, één tekenreeks per regel
   * De lijst met aangepaste verboden wachtwoorden kan maximaal 1000 woorden bevatten.
   * De lijst met aangepaste verboden wachtwoorden is niet hoofdlettergevoelig.
   * De lijst met aangepaste verboden wachtwoorden beschouwt algemene tekens vervangen.
      * Voorbeeld: "o" en "0" of "a" en "@"
   * De minimale tekenreeks is vier tekens en de maximumwaarde is 16 tekens.
1. Wanneer u alle tekenreeksen hebt toegevoegd, klikt u op **opslaan**.

> [!NOTE]
> Het duurt enkele uren voor updates aan de wachtwoordenlijst met aangepaste verboden moet worden toegepast.

![Wijzigen van de lijst met aangepaste verboden wachtwoorden onder verificatiemethoden in de Azure portal](./media/howto-password-ban-bad/authentication-methods-password-protection.png)

## <a name="how-it-works"></a>Hoe werkt het?

Telkens wanneer een gebruiker of beheerder wordt opnieuw ingesteld of een Azure AD-wachtwoord wijzigt die deze loopt via de wachtwoordenlijsten verboden te bevestigen dat deze zich niet op een lijst. Deze controle is opgenomen in wachtwoorden instellen of gewijzigd met behulp van Azure AD.

## <a name="what-do-users-see"></a>Wat gebruikers zien

Wanneer een gebruiker probeert een wachtwoord opnieuw instellen met een probleem dat zou worden verboden, zien ze het volgende foutbericht weergegeven:

Helaas bevat uw wachtwoord een woord, woordgroep of patroon waardoor uw wachtwoord gemakkelijk te raden. Probeer het opnieuw met een ander wachtwoord.

## <a name="next-steps"></a>Volgende stappen

[Conceptueel overzicht van de wachtwoordenlijsten verboden](concept-password-ban-bad.md)

[Conceptueel overzicht van Azure AD-wachtwoordbeveiliging](concept-password-ban-bad-on-premises.md)

[On-premises integratie met de wachtwoordenlijsten verboden inschakelen](howto-password-ban-bad-on-premises.md)
