---
title: 'Azure AD Connect: Pass through-verificatie - Smart accountvergrendeling | Microsoft Docs'
description: Dit artikel wordt beschreven hoe Pass-through-verificatie voor Azure Active Directory (Azure AD) beveiligd met uw lokale accounts van wachtwoord beveiligingsaanvallen in de cloud.
services: active-directory
keywords: Azure AD Connect Pass through-verificatie, install Active Directory onderdelen vereist voor Azure AD, SSO, Single Sign-on
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: billmath
ms.openlocfilehash: 771741fd7da8c9b6932851851aaca148f9596643
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-pass-through-authentication-smart-lockout"></a>Pass-through-verificatie van Azure Active Directory: Smart accountvergrendeling

## <a name="overview"></a>Overzicht

Azure AD biedt bescherming tegen beveiligingsaanvallen wachtwoord en voorkomt dat legitieme gebruikers hun Office 365 en SaaS-toepassingen wordt vergrendeld. Deze mogelijkheid, aangeroepen **Smart accountvergrendeling**, wordt ondersteund als u Pass-through-verificatie als uw contactmethode aanmelden gebruiken. Smartcard-vergrendeling is standaard ingeschakeld voor alle tenants en beveiligt uw gebruikersaccounts voortdurend; hoeft niet te worden ingeschakeld.

Smart vergrendeling werkt door de mislukte aanmeldpogingen te houden en na een bepaalde **Blokkeringsdrempel**, te beginnen een **vergrendelingsduur**. Alle pogingen aanmelden van de hacker tijdens de vergrendelingsduur worden geweigerd. Als de aanval blijft bestaan, aanmeldingspogingen de daaropvolgende mislukte na afloop van de vergrendelingsduur leiden tot een langere duur van de vergrendeling.

>[!NOTE]
>De standaardwaarde Blokkeringsdrempel is 10 mislukte pogingen en de vergrendelingsduur van de standaardwaarde is 60 seconden.

Smart accountvergrendeling is ook onderscheid gemaakt tussen de aanmeldingen van legitieme gebruikers en tegen kwaadwillende personen en alleen vergrendelingen uit de aanvallers in de meeste gevallen. Deze functionaliteit wordt voorkomen dat aanvallers met kwaadaardige bedoelingen accountvergrendeling legitieme gebruikers. We gebruiken uit het verleden aanmelden gedrag, gebruikers-apparaten en browsers en andere signalen onderscheid maken tussen legitieme gebruikers en aanvallers. We zijn voortdurend verbeteren van onze algoritmen.

Omdat Pass through-verificatie stuurt wachtwoord validatie aanvragen naar uw lokale Active Directory (AD), moet u voorkomen dat kwaadwillende personen buiten uw gebruikers AD-accounts te sluiten. Omdat u uw eigen beleid AD accountvergrendeling hebt (met name [ **Accountvergrendelingsdrempel** ](https://technet.microsoft.com/library/hh994574(v=ws.11).aspx) en [ **beleid voor opnieuw instellen Account Lockout teller na**](https://technet.microsoft.com/library/hh994568(v=ws.11).aspx)), moet u de Blokkeringsdrempel Azure AD en de vergrendelingsduur waarden op de juiste wijze configureren om te filteren voordat ze bereiken met uw on-premises aanvallen in de cloud AD.

>[!NOTE]
>De functie Smart accountvergrendeling is gratis en is _op_ standaard voor alle klanten. Wijzigen van de Blokkeringsdrempel en de vergrendelingsduur waarden Graph API met Azure AD heeft uw tenant te hebben van ten minste één Azure AD Premium P2-licentie nodig. U hoeft niet met een licentie voor Azure AD Premium-P2 _per gebruiker_ ophalen van de functie Smart accountvergrendeling met Pass through-verificatie.

Om ervoor te zorgen dat uw gebruikers lokale AD-accounts zijn beveiligd, moet u ervoor te zorgen dat:

1.  Azure AD-Blokkeringsdrempel is _minder_ dan de drempelwaarde voor vergrendeling van AD-Account. Het is raadzaam dat u de waarden instellen, zodat AD van Accountvergrendelingsdrempel ten minste twee is of drie keer Blokkeringsdrempel Azure AD.
2.  Azure AD-vergrendelingsduur (weergegeven in seconden) is _langer_ dan van AD opnieuw Account Lockout teller na (weergegeven in minuten).

>[!IMPORTANT]
>Een beheerder kan momenteel niet de gebruikersaccounts cloud ontgrendelen als hebben is vergrendeld door de smartcard-vergrendeling. Ze moeten wachten op de duur van de vergrendeling verloopt.

## <a name="verify-your-ad-account-lockout-policies"></a>Controleer of uw AD-accountvergrendeling-beleid

Gebruik de volgende instructies om te controleren of uw AD-accountvergrendelingsbeleid:

1.  Open het hulpprogramma Group Policy Management.
2.  Bewerk het groepsbeleid wordt toegepast op alle gebruikers bevat, bijvoorbeeld het standaarddomeinbeleid.
3.  Navigeer naar de Computer Computerconfiguratie\Beleid\Windows Settings\Security Instellingen\beveiligingsinstellingen\accountbeleid\accountvergrendelingsbeleid.
4.  Controleer of uw Accountvergrendelingsdrempel en stellen Account Lockout teller na waarden.

![AD accountvergrendeling beleid](./media/active-directory-aadconnect-pass-through-authentication/pta5.png)

## <a name="use-the-graph-api-to-manage-your-tenants-smart-lockout-values-needs-premium-license"></a>Gebruik de Graph-API voor het beheren van uw tenant Smart accountvergrendeling waarden (moeten Premium-licentie)

>[!IMPORTANT]
>Wijzigen van de Blokkeringsdrempel en de vergrendelingsduur waarden Graph API met Azure AD is een Azure AD Premium P2-functie. Ook moet u een globale beheerder zijn op uw tenant.

U kunt [grafiek Explorer](https://developer.microsoft.com/graph/graph-explorer) lezen, instellen en Azure AD Smart accountvergrendeling waarden bijwerken. Maar u kunt deze bewerkingen ook programmatisch doen.

### <a name="read-smart-lockout-values"></a>Waarden van de smartcard accountvergrendeling lezen

Volg deze stappen voor het lezen van uw tenant Smart accountvergrendeling waarden:

1. Meld u aan bij de grafiek Explorer als globale beheerder van uw tenant. Als u wordt gevraagd, moet u toegang voor de aangevraagde machtigingen verlenen.
2. Klik op 'Machtigingen wijzigen' en selecteer de machtiging 'Directory.ReadWrite.All'.
3. Configureer de Graph API-aanvraag als volgt: Set versie naar "BÈTAVERSIE", van het type 'Ophalen' en de URL die u wilt `https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.
4. Klik op 'Query uitvoeren' om te zien van uw tenant Smart accountvergrendeling waarden. Als u waarden voor uw tenant voordat dit nog niet hebt ingesteld, ziet u een lege set.

### <a name="set-smart-lockout-values"></a>Smart accountvergrendeling waarden instellen

Volg deze stappen voor het instellen van uw tenant Smart accountvergrendeling waarden (voor het eerst alleen):

1. Meld u aan bij de grafiek Explorer als globale beheerder van uw tenant. Als u wordt gevraagd, moet u toegang voor de aangevraagde machtigingen verlenen.
2. Klik op 'Machtigingen wijzigen' en selecteer de machtiging 'Directory.ReadWrite.All'.
3. De aanvraag Graph API als volgt configureren: Stel de versie naar "BÈTAVERSIE", aanvraagtype "POST" en de URL die u wilt `https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.
4. Kopieer en plak de volgende JSON-aanvraag in het veld 'Aanvraag hoofdtekst'.
5. Klik op 'Query uitvoeren' om in te stellen van uw tenant Smart accountvergrendeling waarden.

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
>Als u deze niet gebruikt, kunt u laten de **BannedPasswordList** en **EnableBannedPasswordCheck** waarden als leeg ("") en 'false' respectievelijk.

Controleer of u van uw tenant Smart accountvergrendeling waarden correct hebt ingesteld [deze stappen](#read-smart-lockout-values).

### <a name="update-smart-lockout-values"></a>Smart accountvergrendeling waarden bijwerken

Volg deze stappen voor het bijwerken van uw tenant Smart accountvergrendeling waarden (als u deze voordat u al hebt ingesteld):

1. Meld u aan bij de grafiek Explorer als globale beheerder van uw tenant. Als u wordt gevraagd, moet u toegang voor de aangevraagde machtigingen verlenen.
2. Klik op 'Machtigingen wijzigen' en selecteer de machtiging 'Directory.ReadWrite.All'.
3. [Volg deze stappen voor het lezen van uw tenant Smart accountvergrendeling waarden](#read-smart-lockout-values). Kopieer de `id` waarde (GUID) van het item met 'weergavenaam' als 'PasswordRuleSettings'.
4. De Graph API-aanvraag als volgt configureren: Stel de versie naar "BÈTAVERSIE", van het type 'PATCH' en de URL naar `https://graph.microsoft.com/beta/<your-tenant-domain>/settings/<id>` -gebruik van de GUID van stap 3 voor `<id>`.
5. Kopieer en plak de volgende JSON-aanvraag in het veld 'Aanvraag hoofdtekst'. De smartcard accountvergrendeling waarden waar nodig wijzigen.
6. Klik op 'Query uitvoeren' voor het bijwerken van uw tenant Smart accountvergrendeling waarden.

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

Controleer of u hebt uw tenant Smart accountvergrendeling waarden correct met bijgewerkt [deze stappen](#read-smart-lockout-values).

## <a name="next-steps"></a>Volgende stappen
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - voor de nieuwe functieaanvragen indienen.
