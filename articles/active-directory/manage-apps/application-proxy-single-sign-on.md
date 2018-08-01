---
title: Beheren van eenmalige aanmelding voor Azure AD-toepassingsproxy | Microsoft Docs
description: Meer informatie over de basisprincipes van eenmalige aanmelding met Application Proxy
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: dbdbe8b83af20f66ad2cc99d2a5665262479b4a9
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39364145"
---
# <a name="how-does-azure-ad-application-proxy-provide-single-sign-on"></a>Hoe biedt Azure AD-toepassingsproxy eenmalige aanmelding?

Eenmalige aanmelding is een belangrijk element van een Azure AD-toepassingsproxy.  Het biedt de beste gebruikerservaring omdat uw gebruikers alleen hebben aanmelden bij Azure Active Directory in de cloud. Wanneer ze naar Azure Active Directory zijn geverifieerd, verwerkt de Application Proxy-connector de verificatie naar de on-premises toepassing. De back-endtoepassing weet niet het verschil tussen een externe gebruiker aanmelden via de toepassingsproxy en een normaal gebruik op een apparaat dat domein. 

Voor het gebruik van Azure Active Directory voor eenmalige aanmelding voor uw toepassingen, moet u selecteren **Azure Active Directory** als de methode voor verificatie vooraf. Als u selecteert **Passthrough** en vervolgens uw gebruikers helemaal niet bij Azure Active Directory verifiëren, maar meteen naar de toepassing worden doorgestuurd. U kunt deze instelling kunt configureren wanneer u eerst een toepassing publiceren of gaat u naar uw toepassing in Azure portal en de Application Proxy-instellingen bewerken. 

Als u wilt zien van de opties voor eenmalige aanmelding, de volgende stappen uit:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Navigeer naar **Azure Active Directory** > **bedrijfstoepassingen** > **alle toepassingen**.
3. Selecteer de app met opties voor eenmalige aanmelding u wilt beheren.
4. Selecteer **eenmalige aanmelding**.

   ![In het vervolgkeuzemenu eenmalige aanmelding](./media/application-proxy-single-sign-on/single-sign-on-mode.png)

De in het vervolgkeuzemenu ziet vijf opties voor eenmalige aanmelding voor uw toepassing:

* Eenmalige aanmelding voor Azure AD is uitgeschakeld
* Wachtwoord gebaseerde aanmelding
* Gekoppelde aanmelding
* Geïntegreerde Windows-verificatie
* Headers gebaseerde aanmelding

## <a name="azure-ad-single-sign-on-disabled"></a>Eenmalige aanmelding voor Azure AD is uitgeschakeld

Als u niet wilt met Azure Active Directory-integratie voor single sign-on bij uw toepassing, kiest u **Azure AD eenmalige aanmelding uitgeschakeld**. Met deze optie is geselecteerd, kunnen uw gebruikers twee keer verifiëren. Eerst worden geverifieerd bij Azure Active Directory en meldt u zich aan op de toepassing zelf. 

Deze optie is een goede keuze als uw on-premises toepassing niet vereisen dat gebruikers om te verifiëren, maar u wilt toevoegen van Azure Active Directory als een beveiligingslaag voor externe toegang. 

## <a name="password-based-sign-on"></a>Wachtwoord gebaseerde aanmelding

Als u gebruiken van Azure Active Directory als de kluis van een wachtwoord voor uw on-premises toepassingen wilt, kiest u **wachtwoord gebaseerde aanmelding**. Deze optie is een goede keuze als uw toepassing met een gebruikersnaam en wachtwoord keuzelijst met invoervak in plaats van toegangstokens of headers verifieert. Met wachtwoord gebaseerde sign-on moeten uw gebruikers zich aanmelden bij de toepassing de eerste keer dat ze het willen openen. Hierna levert Azure Active Directory de gebruikersnaam en het wachtwoord namens de gebruiker. 

Zie voor meer informatie over het instellen van wachtwoorden gebaseerde aanmelding [wachtwoord voor eenmalige aanmelding met Application Proxy vaulting](application-proxy-configure-single-sign-on-password-vaulting.md).

## <a name="linked-sign-on"></a>Gekoppelde aanmelding

Als u al een oplossing voor eenmalige aanmelding instellen voor uw on-premises identiteiten hebt, kiest u **gekoppelde aanmelding**. Deze optie kunt Azure Active Directory gebruikmaken van bestaande oplossingen voor eenmalige aanmelding, maar nog steeds biedt uw gebruikers externe toegang tot de toepassing. 

Zie voor meer informatie over gekoppelde aanmelding (voorheen bekend als bestaande eenmalige aanmelding) [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](what-is-single-sign-on.md#how-does-single-sign-on-with-azure-active-directory-work).

## <a name="integrated-windows-authentication"></a>Geïntegreerde Windows-verificatie

Als uw on-premises toepassingen met geïntegreerde Windows-Authentication(IWA) gebruikt of als u wilt gebruiken van Kerberos-beperkte delegatie (KCD) voor eenmalige aanmelding, kiest u **geïntegreerde Windows-verificatie**. Met deze optie hoeft alleen uw gebruikers om te verifiëren bij Azure Active Directory en vervolgens de gebruiker een Kerberos-token verkrijgen en aanmelden bij de toepassing in de Application Proxy-connector worden geïmiteerd. 

Zie voor meer informatie over het instellen van geïntegreerde Windows-verificatie [beperkte Kerberos-delegering voor eenmalige aanmelding met Application Proxy](application-proxy-configure-single-sign-on-with-kcd.md).

## <a name="header-based-sign-on"></a>Headers gebaseerde aanmelding 

Als uw toepassingen headers voor verificatie gebruiken, kiest u **headers gebaseerde aanmelding**. Met deze optie worden hoeft uw gebruikers verificatie de Azure Active Directory. Microsoft-partners met een verificatieservice van derden genoemd PingAccess, die de Azure Active Directory-toegangstoken vertaald naar een kop-indeling voor de toepassing. 

Zie voor meer informatie over het instellen van verificatie op basis van een koptekst [koptekst gebaseerde verificatie voor eenmalige aanmelding met Application Proxy](application-proxy-configure-single-sign-on-with-ping-access.md).

## <a name="next-steps"></a>Volgende stappen

- [Wachtwoord voor eenmalige aanmelding met Application Proxy vaulting](application-proxy-configure-single-sign-on-password-vaulting.md)
- [Beperkte Kerberos-delegering voor eenmalige aanmelding met Application Proxy](application-proxy-configure-single-sign-on-with-kcd.md)
- [Koptekst gebaseerde verificatie voor eenmalige aanmelding met Application Proxy](application-proxy-configure-single-sign-on-with-ping-access.md) 
