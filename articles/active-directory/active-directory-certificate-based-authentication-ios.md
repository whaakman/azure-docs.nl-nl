---
title: Azure Active Directory gebaseerde verificatie op iOS | Microsoft Docs
description: Meer informatie over de ondersteunde scenario's en de vereisten voor het configureren op certificaten gebaseerde verificatie in oplossingen met iOS-apparaten
services: active-directory
author: MarkusVi
documentationcenter: na
manager: mtillman
ms.assetid: 26a6fc54-0153-44fb-b970-9b432c99e9f9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: c9ae098df6e2357ca4bda513c0de5f5d42c7d9af
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="azure-active-directory-certificate-based-authentication-on-ios"></a>Azure Active Directory gebaseerde verificatie op iOS

Certificaat gebaseerde verificatie (CBA) kunt u moeten worden geverifieerd door Azure Active Directory met een clientcertificaat op een Windows-, Android of iOS-apparaat verbinding te maken met uw Exchange online-account in:

* Mobiele Office-toepassingen zoals Microsoft Outlook en Microsoft Word   
* Exchange ActiveSync (EAS)-clients

Configuratie van deze functie wordt voorkomen moet een combinatie van gebruikersnaam en wachtwoord invoeren in bepaalde e-mail en Microsoft Office-toepassingen op uw mobiele apparaat.

Dit onderwerp vindt u de vereisten en de ondersteunde scenario's voor het configureren van CBA op een apparaat iOS(Android) voor gebruikers van tenants in Office 365 Enterprise, Business, Education, US Government, China en Duitsland plant.

Deze functie is beschikbaar in preview in Office 365 US Government verdediging en Federal plannen.




## <a name="microsoft-mobile-applications-support"></a>Ondersteuning voor mobiele toepassingen van Microsoft

| Apps | Ondersteuning |
| --- | --- |
| Azure Information Protection-app |![Selecteren][1] |
| Intune-bedrijfsportal |![Selecteren][1] |
| Microsoft Teams |![Selecteren][1] |
| OneNote |![Selecteren][1] |
| OneDrive |![Selecteren][1] |
| Outlook |![Selecteren][1] |
| Power BI |![Selecteren][1] |
| Skype voor Bedrijven |![Selecteren][1] |
| Word / Excel / PowerPoint |![Selecteren][1] |
| Yammer |![Selecteren][1] |


## <a name="requirements"></a>Vereisten

De versie van het besturingssysteem van het apparaat moet iOS 9 en hoger

Een federation-server moet worden geconfigureerd.  

Microsoft Authenticator is vereist voor de Office-toepassingen op iOS.  

Voor Azure Active Directory voor het intrekken van een clientcertificaat, moet het AD FS-token hebben de volgende claims:  

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
  (Het serienummer van het clientcertificaat)
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
  (De tekenreeks voor de verlener van het clientcertificaat)

Azure Active Directory toegevoegd deze claims naar het vernieuwingstoken als ze beschikbaar in de AD FS-token (of andere SAML-token zijn). Wanneer het vernieuwingstoken dat worden gevalideerd moet, wordt deze informatie wordt gebruikt om te controleren van de intrekking.

Als een best practice moet u de AD FS-foutpagina's bijwerken met de volgende opties:

* De vereiste voor het installeren van de Microsoft-Authenticator op iOS
* Instructies voor het ophalen van het certificaat van een gebruiker.

Zie voor meer informatie [aanpassen van de AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).

Sommige Office-apps (met moderne verificatie is ingeschakeld) verzenden '*prompt = aanmelding*' naar Azure AD in de aanvraag. Standaard Azure AD zet dit in de aanvraag voor ADFS naar '*wauth = usernamepassworduri*' (AD FS wilt U/P auth vragen) en '*wfresh = 0*' (vraagt ADFS te negeren van SSO-status en een nieuwe verificatie). Als u verificatie inschakelen op basis van certificaten voor deze apps wilt, moet u het standaardgedrag voor Azure AD te wijzigen. Stelt u de '*PromptLoginBehavior*'in de instellingen van het federatieve domein naar'*uitgeschakelde*'.
U kunt de [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) cmdlet deze taak uit te voeren:

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`


## <a name="exchange-activesync-clients-support"></a>Ondersteuning voor Exchange ActiveSync-clients
In iOS 9 of hoger, wordt de systeemeigen iOS-e-mailclient ondersteund. Voor alle andere Exchange ActiveSync-toepassingen om te bepalen of deze functie wordt ondersteund, contact op met de ontwikkelaar van uw toepassing.  


## <a name="next-steps"></a>Volgende stappen

Als u verificatie configureren op basis van certificaten in uw omgeving wilt, Zie [aan de slag met verificatie op basis van certificaten op Android](active-directory-certificate-based-authentication-get-started.md) voor instructies.


<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png
