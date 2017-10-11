---
title: Beheren van eenmalige aanmelding voor Azure AD-toepassingsproxy | Microsoft Docs
description: Meer informatie over de basisprincipes van eenmalige aanmelding met toepassingsproxy
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 1deb3d91049d45fe26791783e13bd23e0a7d9f95
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="how-does-azure-ad-application-proxy-provide-single-sign-on"></a>Hoe biedt Azure AD-toepassingsproxy eenmalige aanmelding

Eenmalige aanmelding is een essentieel element van Azure AD-toepassingsproxy.  Het biedt de beste gebruikerservaring omdat uw gebruikers alleen aan te melden bij Azure Active Directory in de cloud. Zodra ze naar Azure Active Directory verifiëren, wordt door de connector voor toepassingsproxy de verificatie naar de on-premises toepassing verwerkt. De back-end-toepassing kan niet het verschil tussen een externe gebruiker aanmelden via toepassingsproxy en een reguliere gebruik op een apparaat domein niet bepalen. 

Voor het gebruik van Azure Active Directory voor eenmalige aanmelding voor uw toepassingen, moet u selecteren **Azure Active Directory** als de methode voor verificatie vooraf. Als u selecteert **Passthrough** en vervolgens uw gebruikers helemaal niet met Azure Active Directory verifiëren, maar direct door naar de toepassing omgeleid. U kunt deze instelling kunt configureren wanneer u eerst een toepassing publiceren of navigeer naar uw toepassing in de Azure portal en de Application Proxy-instellingen bewerken. 

Overzicht van de opties voor eenmalige aanmelding als volgt:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Navigeer naar **Azure Active Directory** > **bedrijfstoepassingen** > **alle toepassingen**.
3. Selecteer de app waarvan opties voor eenmalige aanmelding u wilt beheren.
4. Selecteer **eenmalige aanmelding**.

   ![Het vervolgmenu van eenmalige aanmelding](./media/application-proxy-sso-overview/single-sign-on-mode.png)

Het vervolgkeuzemenu ziet vijf opties voor eenmalige aanmelding voor uw toepassing:

* Azure AD eenmalige aanmelding uitgeschakeld
* Op basis van wachtwoorden eenmalige aanmelding
* Gekoppelde aanmelding
* Geïntegreerde Windows-verificatie
* Op basis van een koptekst eenmalige aanmelding

## <a name="azure-ad-single-sign-on-disabled"></a>Azure AD eenmalige aanmelding uitgeschakeld

Als u niet wilt met Azure Active Directory-integratie voor eenmalige aanmelding voor uw toepassing, kiest u **Azure AD eenmalige aanmelding uitgeschakeld**. Met deze optie selecteert, kunnen uw gebruikers tweemaal verifiëren. Eerst geverifieerd bij Azure Active Directory en vervolgens weer aanmelden op de toepassing zelf. 

Deze optie is een goede keuze als uw on-premises toepassing niet vereisen dat gebruikers om te verifiëren, maar u wilt toevoegen van Azure Active Directory als een beveiligingslaag voor externe toegang. 

## <a name="password-based-sign-on"></a>Op basis van wachtwoorden eenmalige aanmelding

Als u gebruiken van Azure Active Directory als een kluis wachtwoord voor uw on-premises toepassingen wilt, kiest u **op basis van wachtwoorden aanmelding**. Deze optie is een goede keuze als uw toepassing wordt geverifieerd met een gebruikersnaam en wachtwoord keuzelijst met invoervak in plaats van de toegangstokens of headers. Met op basis van wachtwoorden aanmelding moeten uw gebruikers zich aanmelden bij de toepassing de eerste keer dat ze het willen openen. Daarna biedt Azure Active Directory de gebruikersnaam en wachtwoord namens de gebruiker. 

Zie voor meer informatie over het instellen van wachtwoorden gebaseerde aanmelding [wachtwoord vaulting voor eenmalige aanmelding met toepassingsproxy](application-proxy-sso-azure-portal.md).

## <a name="linked-sign-on"></a>Gekoppelde aanmelding

Als u al een oplossing voor eenmalige aanmelding instellen voor uw on-premises identiteiten, kiest u **gekoppelde aanmelding**. Deze optie kunt Azure Active Directory voor het benutten van bestaande oplossingen voor eenmalige aanmelding, maar nog steeds geeft uw gebruikers externe toegang tot de toepassing. 

Zie voor meer informatie over de gekoppelde aanmelding (voorheen bekend als bestaande eenmalige aanmelding) [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

## <a name="integrated-windows-authentication"></a>Geïntegreerde Windows-verificatie

Als uw on-premises toepassingen met geïntegreerde Windows-Authentication(IWA) gebruikt of als u wilt gebruiken van Kerberos-beperkt delegatie (KCD) voor eenmalige aanmelding, kiest u **geïntegreerde Windows-verificatie**. Met deze optie hoeft alleen uw gebruikers te verifiëren bij Azure Active Directory en vervolgens de connector voor toepassingsproxy de gebruiker een Kerberos-token verkrijgen en meld u bij de toepassing imiteert. 

Zie voor meer informatie over het instellen van geïntegreerde Windows-verificatie [Kerberos-beperkte delegatie voor eenmalige aanmelding met toepassingsproxy](active-directory-application-proxy-sso-using-kcd.md).

## <a name="header-based-sign-on"></a>Op basis van een koptekst eenmalige aanmelding 

Als uw toepassingen headers voor verificatie gebruikt, kiest u **headers gebaseerde aanmelding**. Met deze optie kunnen hoeft uw gebruikers alleen te verificatie de Azure Active Directory. Microsoft werkt samen met een derde partij verificatieservice aangeroepen PingAccess waarmee het toegangstoken van Azure Active Directory vertaald naar een header-indeling voor de toepassing. 

Zie voor meer informatie over het instellen van verificatie op basis van een koptekst [verificatie op basis van een koptekst voor eenmalige aanmelding met toepassingsproxy](application-proxy-ping-access.md).

## <a name="next-steps"></a>Volgende stappen

- [Wachtwoord voor eenmalige aanmelding met toepassingsproxy vaulting](application-proxy-sso-azure-portal.md)
- [Kerberos-beperkte delegatie voor eenmalige aanmelding met toepassingsproxy](active-directory-application-proxy-sso-using-kcd.md)
- [Verificatie op basis van een koptekst voor eenmalige aanmelding met toepassingsproxy](application-proxy-ping-access.md) 
