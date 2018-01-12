---
title: 'Azure AD Connect: Pass through-verificatie - Smart accountvergrendeling | Microsoft Docs'
description: Dit artikel wordt beschreven hoe Pass-through-verificatie voor Azure Active Directory (Azure AD) beveiligd met uw lokale accounts van wachtwoord beveiligingsaanvallen in de cloud
services: active-directory
keywords: Azure AD Connect Pass through-verificatie, install Active Directory onderdelen vereist voor Azure AD, SSO, Single Sign-on
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: billmath
ms.openlocfilehash: fc46fe1d68538757ba5a8c5aa1acb4b51f8a171b
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="azure-active-directory-pass-through-authentication-smart-lockout"></a>Pass-through-verificatie van Azure Active Directory: Smart accountvergrendeling

## <a name="overview"></a>Overzicht

Azure Active Directory (Azure AD) beschermt tegen aanvallen met brute kracht wachtwoord en voorkomen dat legitieme gebruikers hun Office 365 en SaaS-toepassingen wordt vergrendeld. Deze mogelijkheid, aangeroepen *Smart accountvergrendeling*, wordt ondersteund als u Pass-through-verificatie als uw contactmethode aanmelden gebruiken. Smartcard-vergrendeling is standaard ingeschakeld voor alle tenants en het continu uw gebruikersaccounts beveiligt.

Smart accountvergrendeling houdt van mislukte pogingen voor aanmelden. Na een bepaalde *blokkeringsdrempel*, wordt het een *vergrendelingsduur*. Smart vergrendeling wordt elke poging om te ondertekenen van de hacker tijdens de duur van de vergrendeling geweigerd. Als de aanval blijft, aanmeldingspogingen daaropvolgende mislukte nadat de vergrendelingsduur resultaat in langere duur van de vergrendeling eindigt.

>[!NOTE]
>De vergrendeling drempelwaarde is standaard 10 mislukte pogingen. De vergrendelingsduur standaardwaarde is 60 seconden.

Smart accountvergrendeling is ook onderscheid gemaakt tussen de aanmeldingen van legitieme gebruikers en aanmeldingen tegen kwaadwillende personen. In de meeste gevallen wordt het vergrendeld uit de kwaadwillende personen. Deze functionaliteit wordt voorkomen dat aanvallers met kwaadaardige bedoelingen accountvergrendeling legitieme gebruikers. Smart accountvergrendeling gebruikt voorbij aanmelden gedrag, apparaten en browsers van de gebruikers en andere signalen onderscheid maken tussen legitieme gebruikers en aanvallers. De algoritmen zijn voortdurend verbeterd.

Pass through-verificatie stuurt wachtwoord validatie aanvragen door naar de lokale Active Directory, dus moet u voorkomen dat kwaadwillende personen buiten uw gebruikers Active Directory-accounts te sluiten. Active Directory heeft een eigen accountvergrendelingsbeleid in het bijzonder [accountvergrendelingsdrempel](https://technet.microsoft.com/library/hh994574(v=ws.11).aspx) en [accountvergrendelingsteller opnieuw instellen na](https://technet.microsoft.com/library/hh994568(v=ws.11).aspx) beleid. De Azure AD vergrendelen drempel en de vergrendelingsduur duurwaarden op de juiste wijze configureren om te filteren voordat ze op de lokale Active Directory bereiken aanvallen in de cloud.

>[!NOTE]
>>De functie Smart accountvergrendeling is gratis en is _op_ standaard voor alle klanten. Wijzigen van de Blokkeringsdrempel en de vergrendelingsduur waarden Graph API met Azure AD moet echter uw tenant voor Azure AD Premium-P2 worden geactiveerd. 

Om ervoor te zorgen dat uw gebruikers op de lokale Active Directory-accounts goed zijn beveiligd, moet u ervoor te zorgen dat:

   * De Azure AD-blokkeringsdrempel is _minder_ dan de drempelwaarde voor vergrendeling van Active Directory-account. Stel de waarden, zodat de Active Directory-accountvergrendelingsdrempel ten minste twee of drie keer langer is dan de drempelwaarde voor vergrendeling van Azure AD is.
   * De duur van de Azure AD-vergrendeling (weergegeven in seconden) is _langer_ dan de Active Directory accountvergrendelingsteller opnieuw na duur instellen (weergegeven in minuten).

>[!IMPORTANT]
>Een beheerder kan momenteel niet de gebruikersaccounts cloud ontgrendelen als hebben is vergrendeld door de smartcard-vergrendeling. De beheerder moet wachten tot de duur van de vergrendeling verloopt.

## <a name="verify-your-active-directory-account-lockout-policies"></a>Controleer of uw Active Directory-accountvergrendelingsbeleid

Gebruik de volgende instructies om te controleren of uw Active Directory-accountvergrendelingsbeleid:

1.  Open het hulpprogramma Group Policy Management.
2.  Bewerk het groepsbeleidobject dat wordt toegepast op alle gebruikers bevat, bijvoorbeeld de **standaarddomeinbeleid**.
3.  Blader naar **Computerconfiguratie** > **beleid** > **Windows-instellingen** > **beveiligingsinstellingen**   >  **Beleidsregels van account** > **vergrendelingsbeleid voor Account**.
4.  Controleer of uw **accountvergrendelingsdrempel** en **accountvergrendelingsteller opnieuw instellen na** waarden.

![Active Directory-accountvergrendelingsbeleid](./media/active-directory-aadconnect-pass-through-authentication/pta5.png)

## <a name="use-the-graph-api-to-manage-your-tenants-smart-lockout-values-requires-a-premium-license"></a>Gebruik de Graph-API voor het beheren van uw tenant Smart accountvergrendeling waarden (vereist een Premium-licentie)

>[!IMPORTANT]
>De Azure AD vergrendelen drempel en de vergrendelingsduur duurwaarden wijzigen met behulp van Graph API is een Azure AD Premium P2-functie. Ook moet u een globale beheerder zijn op uw tenant.

U kunt [grafiek Explorer](https://developer.microsoft.com/graph/graph-explorer) lezen, instellen en werk de waarden voor Azure AD Smart-vergrendeling. U kunt deze bewerkingen ook programmatisch doen.

### <a name="view-smart-lockout-values"></a>Waarden van de smartcard vergrendeling weergeven

Volg deze stappen om uw tenant Smart accountvergrendeling waarden weer te geven:

1. Meld u aan grafiek Explorer als globale beheerder van uw tenant. Als u wordt gevraagd, moet u toegang voor de aangevraagde machtigingen verlenen.
2. Selecteer **wijzigingsmachtiging**, en selecteer vervolgens de **Directory.ReadWrite.All** machtiging.
3. De Graph API-aanvraag als volgt configureren: de versie wordt ingesteld op **BETA**, het aanvraagtype voor **ophalen**, en de URL van `https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.
4. Selecteer **Query uitvoeren** om uw tenant Smart accountvergrendeling waarden te bekijken. Als u waarden voor uw tenant voordat dit nog niet hebt ingesteld, ziet u een lege set.

### <a name="set-smart-lockout-values"></a>Smart accountvergrendeling waarden instellen

Volg deze stappen om uw tenant Smart accountvergrendeling waarden (vereist voor het eerst):

1. Meld u aan grafiek Explorer als globale beheerder van uw tenant. Als u wordt gevraagd, moet u toegang voor de aangevraagde machtigingen verlenen.
2. Selecteer **wijzigingsmachtiging**, en selecteer vervolgens de **Directory.ReadWrite.All** machtiging.
3. De Graph API-aanvraag als volgt configureren: de versie wordt ingesteld op **BETA**, het aanvraagtype voor **POST**, en de URL van `https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.
4. Kopieer en plak de volgende JSON-aanvraag in de **aanvraagtekst** veld.
5. Selecteer **Query uitvoeren** van uw tenant Smart accountvergrendeling waarden in te stellen.

```
{
  "templateId": "5cf42378-d67d-4f36-ba46-e8b86229381d",
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "300"
    },
    {
      "name": "LockoutThreshold",
      "value": "5"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

>[!NOTE]
>Als u deze niet gebruikt, kunt u laten de **BannedPasswordList** en **EnableBannedPasswordCheck** waarden als leeg (**' '**) en **false** respectievelijk.

Controleer of u hebt ingesteld van uw tenant Smart accountvergrendeling waarden correct met behulp van de stappen in [weergave Smart accountvergrendeling waarden](#view-smart-lockout-values).

### <a name="update-smart-lockout-values"></a>Smart accountvergrendeling waarden bijwerken

Volg deze stappen voor het bijwerken van uw tenant Smart accountvergrendeling waarden (als u deze hebt ingesteld):

1. Meld u aan grafiek Explorer als globale beheerder van uw tenant. Als u wordt gevraagd, moet u toegang voor de aangevraagde machtigingen verlenen.
2. Selecteer **wijzigingsmachtiging**, en selecteer vervolgens de **Directory.ReadWrite.All** machtiging.
3. [Volg deze stappen voor het weergeven van uw tenant Smart accountvergrendeling waarden](#view-smart-lockout-values). Kopieer de `id` waarde (GUID) van het item met **displayName** als **PasswordRuleSettings**.
4. De Graph API-aanvraag als volgt configureren: de versie wordt ingesteld op **BETA**, het aanvraagtype voor **PATCH**, en de URL van `https://graph.microsoft.com/beta/<your-tenant-domain>/settings/<id>`. Gebruik de GUID van stap 3 voor `<id>`.
5. Kopieer en plak de volgende JSON-aanvraag in de **aanvraagtekst** veld. De smartcard accountvergrendeling waarden waar nodig wijzigen.
6. Selecteer **Query uitvoeren** van uw tenant Smart accountvergrendeling waarden wilt bijwerken.

```
{
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "30"
    },
    {
      "name": "LockoutThreshold",
      "value": "4"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

Controleer of u hebt bijgewerkt van uw tenant Smart accountvergrendeling waarden correct met behulp van de stappen in [weergave Smart accountvergrendeling waarden](#view-smart-lockout-values).

## <a name="next-steps"></a>Volgende stappen
[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): de Azure Active Directory-Forum gebruiken om nieuwe functieaanvragen.
