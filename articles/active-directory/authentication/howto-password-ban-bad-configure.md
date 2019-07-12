---
title: Het blokkeren van zwakke wachtwoorden in Azure AD - Azure Active Directory
description: Zwakke wachtwoorden van uw envirionment met Azure AD dynamisch uitsluiten van passwrords blokkeren
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: f531174c889948308e27109ab4fd80a481ec6bdc
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798190"
---
# <a name="configuring-the-custom-banned-password-list"></a>De lijst met aangepaste uitgesloten wachtwoorden configureren

Veel organisaties zien dat gebruikers maken met behulp van algemene lokale woorden, zoals een school, sportteam of beroemde personen, zodat ze gemakkelijk te raden wachtwoorden. Lijst met aangepaste uitgesloten wachtwoorden van Microsoft, waarmee organisaties om toe te voegen tekenreeksen om te evalueren en te blokkeren, naast de globale verboden lijst met wachtwoorden, wanneer gebruikers en beheerders probeert te wijzigen of opnieuw instellen van een wachtwoord.

## <a name="add-to-the-custom-list"></a>Aan de aangepaste lijst toevoegen

De lijst met aangepaste uitgesloten wachtwoorden configureren, is een Azure Active Directory Premium P1 of P2-licentie vereist. Zie voor meer informatie over de licentieverlening voor Azure Active Directory, de [Azure Active Directory pagina met prijzen](https://azure.microsoft.com/pricing/details/active-directory/).

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en blader naar **Azure Active Directory**, **verificatiemethoden**, klikt u vervolgens **wachtwoordbeveiliging**.
1. Stel de optie **afdwingen aangepaste lijst**naar **Ja**.
1. Toevoegen van tekenreeksen die moeten worden de **aangepaste lijst met wachtwoorden verboden**, één tekenreeks per regel
   * De lijst met aangepaste uitgesloten wachtwoorden kan maximaal 1000 termen bevatten.
   * De lijst met aangepaste uitgesloten wachtwoorden is niet hoofdlettergevoelig.
   * De lijst met uitgesloten wachtwoorden aangepaste rekening gehouden met algemene tekens vervangen.
      * Voorbeeld: "o" en "0" of "a" en "\@"
   * De minimale tekenreekslengte is vier tekens en de maximumwaarde is 16 tekens.
1. Wanneer u alle tekenreeksen hebt toegevoegd, klikt u op **opslaan**.

> [!NOTE]
> Het duurt enkele uren voor updates aan de lijst met uitgesloten wachtwoorden aangepaste moet worden toegepast.

> [!NOTE]
> De lijst met aangepaste uitgesloten wachtwoorden is beperkt tot maximaal 1000 voorwaarden met. Het is niet ontworpen voor het blokkeren van zeer grote lijsten van wachtwoorden. Microsoft adviseert om volledig gebruik maken van de voordelen van de lijst met aangepaste uitgesloten wachtwoorden, dat u eerst controleren en begrijpen van de bedoelde ontwerp en het gebruik van de lijst met uitgesloten wachtwoorden aangepaste (Zie [aangepaste lijst met wachtwoorden verboden](concept-password-ban-bad.md#custom-banned-password-list)), en ook het wachtwoord evaluatie-algoritme (Zie [hoe wachtwoorden worden geëvalueerd](concept-password-ban-bad.md#how-are-passwords-evaluated)).

![Wijzigen van de aangepaste lijst met uitgesloten wachtwoorden onder verificatiemethoden in de Azure-portal](./media/howto-password-ban-bad/authentication-methods-password-protection.png)

## <a name="how-it-works"></a>Hoe werkt het?

Telkens wanneer een gebruiker of beheerder wordt opnieuw ingesteld of een Azure AD-wachtwoord wijzigt stroomt hiervandaan het via de lijsten met uitgesloten wachtwoorden te bevestigen dat deze zich niet op een lijst. Deze controle is opgenomen in alle wachtwoorden instellen of wijzigen met behulp van Azure AD.

## <a name="what-do-users-see"></a>Wat gebruikers zien

Wanneer een gebruiker probeert een wachtwoord opnieuw wordt ingesteld op iets dat zou worden geblokkeerd, zien ze een van de volgende foutberichten:

* Helaas komt bevat uw wachtwoord een woord, woordgroep of patroon dat uw wachtwoord gemakkelijk te raden maakt. Probeer het opnieuw met een ander wachtwoord.
* U kunt dat wachtwoord helaas niet gebruiken omdat het woorden of tekens die zijn geblokkeerd door uw beheerder bevat. Probeer het opnieuw met een ander wachtwoord.

## <a name="next-steps"></a>Volgende stappen

[Conceptueel overzicht van de lijsten met uitgesloten wachtwoorden](concept-password-ban-bad.md)

[Conceptueel overzicht van Azure AD-wachtwoordbeveiliging](concept-password-ban-bad-on-premises.md)

[On-premises integratie met de lijsten met uitgesloten wachtwoorden inschakelen](howto-password-ban-bad-on-premises.md)
