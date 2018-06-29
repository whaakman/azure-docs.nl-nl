---
title: Brute kracht aanvallen met behulp van Azure AD-smart vergrendeling
description: Azure Active Directory smart accountvergrendeling helpt uw organisatie beschermen tegen gewelddadige aanvallen probeert te achterhalen van wachtwoorden
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/25/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: rogoya
ms.openlocfilehash: d5beb5ce6e167cd100bec2ed54dc6ea0e78ba37b
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035873"
---
# <a name="azure-active-directory-smart-lockout"></a>Azure Active Directory smart vergrendeling

Smart accountvergrendeling maakt gebruik van cloud intelligence vergrendelen ongewenste actoren die toegang proberen te raden wachtwoorden van uw gebruikers of brute-force-methoden gebruiken om op te halen. Die intelligence kan worden herkend aanmeldingen die afkomstig zijn van geldige gebruikers en ze anders dan de waarden van aanvallers en andere onbekende bronnen behandelen. Smart accountvergrendeling kunt vergrendelen uit de aanvallers tijdens waarin die uw gebruikers toegang krijgen tot hun accounts en niet productief blijven.

Smartcard-vergrendeling is altijd ingeschakeld voor alle klanten met standaardinstellingen die worden geboden door de juiste combinatie van beveiliging en bruikbaarheid van Azure AD. Aanpassing van de smartcard accountvergrendelingsinstellingen met waarden die specifiek zijn voor uw organisatie vereist Azure AD Basic of hoger licenties voor uw gebruikers.

Smartcard-vergrendeling kan worden geïntegreerd met hybride implementaties met wachtwoordhashsynchronisatie of Pass through-verificatie bij de lokale Active Directory-accounts beschermen tegen het door aanvallers worden vergrendeld. Door in te stellen op de juiste wijze smart accountvergrendelingsbeleid in Azure AD, kunnen aanvallen worden gefilterd voordat ze op de lokale Active Directory bereiken.

Wanneer u [Pass through-verificatie](../connect/active-directory-aadconnect-pass-through-authentication.md), moet u ervoor zorgen dat:

   * De Azure AD-blokkeringsdrempel is **minder** dan de drempelwaarde voor vergrendeling van Active Directory-account. Stel de waarden, zodat de Active Directory-accountvergrendelingsdrempel ten minste twee of drie keer langer is dan de drempelwaarde voor vergrendeling van Azure AD is. 
   * De duur van de Azure AD-vergrendeling **in seconden** is **langer** dan de Active Directory accountvergrendelingsteller opnieuw na duur instellen **minuten**.

> [!IMPORTANT]
> Een beheerder kan momenteel niet de gebruikersaccounts cloud ontgrendelen als hebben is vergrendeld door de smartcard-vergrendeling. De beheerder moet wachten tot de duur van de vergrendeling verloopt.

## <a name="verify-on-premises-account-lockout-policy"></a>Controleer of de lokale accountvergrendelingsbeleid

Gebruik de volgende instructies om te controleren of het beleid voor uw on-premises Active Directory-account lockout:

1. Open het hulpprogramma Group Policy Management.
2. Bewerk het groepsbeleidobject met accountvergrendelingsbeleid van uw organisatie, bijvoorbeeld de **standaarddomeinbeleid**.
3. Blader naar **Computerconfiguratie** > **beleid** > **Windows-instellingen** > **beveiligingsinstellingen**   >  **Beleidsregels van account** > **vergrendelingsbeleid voor Account**.
4. Controleer of uw **accountvergrendelingsdrempel** en **accountvergrendelingsteller opnieuw instellen na** waarden.

![Wijzigen van de lokale Active Directory accountvergrendelingsbeleid met behulp van een groepsbeleidsobject](./media/howto-password-smart-lockout/active-directory-on-premises-account-lockout-policy.png)

## <a name="manage-azure-ad-smart-lockout-values"></a>Azure AD smart accountvergrendeling waarden beheren

Op basis van de vereisten van uw organisatie, slimme accountvergrendeling waarden moet mogelijk worden aangepast. Aanpassing van de smartcard accountvergrendelingsinstellingen met waarden die specifiek zijn voor uw organisatie vereist Azure AD Basic of hoger licenties voor uw gebruikers.

Om te controleren of wijzig de waarden van de smartcard-vergrendeling voor uw organisatie, gebruikt u de volgende stappen uit:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en klik op **Azure Active Directory**, klikt u vervolgens **verificatiemethoden**.
1. Stel de **blokkeringsdrempel**, op basis van het aantal mislukte aanmeldingen met een account voordat de eerste accountvergrendeling zijn toegestaan. De standaardwaarde is 10.
1. Stel de **vergrendelingsduur in seconden**, aan de lengte in seconden van elke vergrendeling.

> [!NOTE]
> Als de eerste aanmelden nadat een vergrendeling is ook mislukt, de account vergrendeld opnieuw. Als een account herhaaldelijk wordt, verhoogt de duur van de vergrendeling.

![De Azure AD smart vergrendelingsbeleid voor in de Azure portal aanpassen](./media/howto-password-smart-lockout/azure-active-directory-custom-smart-lockout-policy.png)
## <a name="next-steps"></a>Volgende stappen

[Ontdek hoe te verbieden onjuiste wachtwoorden in uw organisatie met behulp van Azure AD.](howto-password-ban-bad.md)

[Configureer zelf uw wachtwoord opnieuw instellen zodat gebruikers hun eigen account ontgrendelen.](quickstart-sspr.md)