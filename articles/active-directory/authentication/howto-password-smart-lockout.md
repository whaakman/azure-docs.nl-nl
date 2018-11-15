---
title: Zo wordt voorkomen dat brute-force-aanvallen met behulp van intelligente vergrendeling van het Azure AD
description: Azure Active Directory slimme accountvergrendeling helpt uw organisatie beschermen tegen beveiligingsaanvallen raden wachtwoorden
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: rogoya
ms.openlocfilehash: 957aa05efab68f9531fb6576de775aa9901ab44d
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51685800"
---
# <a name="azure-active-directory-smart-lockout"></a>Azure Active Directory slimme accountvergrendeling

Slimme accountvergrendeling maakt gebruik van intelligentie van de cloud te vergrendelen beveiligingsrisico die toegang proberen te raden wachtwoorden van uw gebruikers of brute-force-methoden gebruiken om op te halen. Die intelligence kunt aanmeldingen die afkomstig zijn van geldige gebruikers herkennen en ze anders dan de wijzigingsaanvragen van aanvallers en andere onbekende bronnen te behandelen. Hiermee vergrendelt u slimme vergrendeling van het uit de aanvallers, terwijl waarin die uw gebruikers toegang krijgen tot hun accounts en productief blijven.

Standaard, slimme accountvergrendeling Hiermee vergrendelt u het account van aanmeldpogingen voor één minuut na 10 mislukte pogingen. Het account wordt vergrendeld opnieuw na elke latere mislukte aanmeldingspoging, voor één minuut bij de eerste en meer voor in de daaropvolgende pogingen worden gedaan.

* De laatste drie onjuist wachtwoord-hashes om te voorkomen dat de teller voor accountvergrendeling reincrementing, slimme accountvergrendeling worden bijgehouden. Als iemand op meerdere keren de dezelfde onjuist wachtwoord invoert, wordt dit gedrag niet leiden tot het account te vergrendelen.
   * Deze functionaliteit is niet beschikbaar voor klanten met Pass through-verificatie is ingeschakeld.

Slimme accountvergrendeling is altijd ingeschakeld voor alle klanten van Azure AD met deze standaardinstellingen die worden geboden door de juiste combinatie van beveiliging en gebruiksgemak. Aanpassing van de vergrendeling van het smart-instellingen met waarden die specifiek zijn voor uw organisatie, vereist Azure AD Basic of hoger licenties voor uw gebruikers.

Met behulp van intelligente accountvergrendeling is geen garantie dat een legitieme gebruikers nooit vergrendeld. Wanneer de slimme accountvergrendeling Hiermee vergrendelt u een gebruikersaccount, dan proberen we ons best om de vergrendeling niet de originele gebruiker. De vergrendeling van het service probeert om ervoor te zorgen dat ongewenste actoren kan geen toegang tot een legitieme gebruikersaccount krijgen.  

* Elke Azure Active Directory-Datacenter houdt accountvergrendeling onafhankelijk van elkaar. Een gebruiker heeft (threshold_limit * datacenter_count) aantal pogingen, als de gebruiker komt binnen via elk Datacenter.
* Smart Lockout maakt gebruik van vertrouwde locatie vs onbekende locatie wilt onderscheiden van een actor beschadigd en de legitieme gebruiker. Niet bekend en vertrouwd locaties hebben beide vergrendeling van het afzonderlijke items.

Slimme accountvergrendelingen kan worden geïntegreerd met hybride implementaties, met behulp van de synchronisatie van wachtwoordhashes Pass through-verificatie, om te voorkomen dat ze worden geblokkeerd door aanvallers on-premises Active Directory-accounts. Door in te stellen op de juiste wijze slimme vergrendeling van het beleid in Azure AD, kunnen aanvallen worden gefilterd voordat ze on-premises Active Directory bereiken.

Bij het gebruik van [Pass through-verificatie](../hybrid/how-to-connect-pta.md), moet u ervoor zorgen dat:

   * De drempelwaarde voor vergrendeling van Azure AD is **minder** dan de drempelwaarde voor vergrendeling van Active Directory-account. Stel de waarden, zodat de drempelwaarde voor vergrendeling van Active Directory-account ten minste twee of drie keer langer zijn dan de drempelwaarde voor vergrendeling van Azure AD is. 
   * De duur van de Azure AD-vergrendeling **in een paar seconden** is **langer** dan de Active Directory accountvergrendelingsteller opnieuw na duur instellen **minuten**.

> [!IMPORTANT]
> Een beheerder kan op dit moment niet de gebruikersaccounts cloud ontgrendelen als ze hebben uit is vergrendeld door de functie Smart Lockout. De beheerder moet wachten tot de duur van de vergrendeling verloopt binnenkort.

## <a name="verify-on-premises-account-lockout-policy"></a>Controleer of de on-premises account lockout beleid

Gebruik de volgende instructies om te controleren of uw on-premises Active Directory-account lockout beleid:

1. Open het hulpprogramma voor Groepsbeleidsbeheer.
2. Bewerk het groepsbeleid met vergrendelingsbeleid voor uw organisatie, bijvoorbeeld de **standaarddomeinbeleid**.
3. Blader naar **Computerconfiguratie** > **beleid** > **Windows-instellingen** > **beveiligingsinstellingen**   >  **Accountbeleid** > **Account Lockout beleid**.
4. Controleer of uw **drempel voor accountvergrendelingen** en **teller voor accountvergrendeling opnieuw instellen na** waarden.

![De on-premises Active Directory-account lockout beleid met behulp van een Group Policy Object wijzigen](./media/howto-password-smart-lockout/active-directory-on-premises-account-lockout-policy.png)

## <a name="manage-azure-ad-smart-lockout-values"></a>Azure AD slimme accountvergrendeling waarden beheren

Op basis van de vereisten van uw organisatie, slimme accountvergrendeling waarden moet mogelijk worden aangepast. Aanpassing van de vergrendeling van het smart-instellingen met waarden die specifiek zijn voor uw organisatie, vereist Azure AD Basic of hoger licenties voor uw gebruikers.

Als u wilt controleren of wijzigen van de vergrendeling van het smart-waarden voor uw organisatie, gebruikt u de volgende stappen uit:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en klik op **Azure Active Directory**, klikt u vervolgens **verificatiemethoden**.
1. Stel de **blokkeringsdrempel**, op basis van het aantal mislukte aanmeldingen zijn toegestaan voor een account voordat u de vergrendeling van het eerste. De standaardwaarde is 10.
1. Stel de **blokkeringsduur in seconden**, met de lengte in seconden van de vergrendeling van het. De standaardwaarde is 60 seconden (één minuut).

> [!NOTE]
> Als de eerste aanmelding nadat een vergrendeling van het is ook mislukt, de account vergrendeld opnieuw. Als een account herhaaldelijk wordt vergrendeld, wordt de vergrendelingsduur verhoogd.

![Het Azure AD slimme vergrendeling van het beleid in de Azure-portal aanpassen](./media/howto-password-smart-lockout/azure-active-directory-custom-smart-lockout-policy.png)
## <a name="next-steps"></a>Volgende stappen

[Meer informatie over het blokkeren van onjuiste wachtwoorden in uw organisatie met behulp van Azure AD.](howto-password-ban-bad.md)

[Self-service voor wachtwoord opnieuw instellen zodat gebruikers kunnen hun eigen accounts ontgrendelen configureren.](quickstart-sspr.md)