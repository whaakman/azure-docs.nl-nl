---
title: Sessiebeheer voor verificatie met Azure Active Directory voor voorwaardelijke toegang configureren
description: Azure AD-verificatie-sessieconfiguratie met inbegrip van de aanmelding door een gebruiker in frequentie en de browser sessiepersistentie aanpassen.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45ea34a673b46dda2ec9606952c707a13d1b72d9
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65766878"
---
# <a name="configure-authentication-session-management-with-conditional-access"></a>Sessiebeheer voor verificatie met voorwaardelijke toegang configureren

In complexe implementaties mogelijk organisaties behoefte aan het beperken van verificatiesessies. Sommige scenario's zijn onder andere:

* Toegang tot bronnen vanaf een niet-beheerde of gedeeld apparaat
* Toegang tot gevoelige informatie van een extern netwerk
* Gebruikers met hoge impact
* Kritieke zakelijke toepassingen

Besturingselementen voor voorwaardelijke toegang kunnen u beleidsregels die zijn gericht op specifieke gebruiksvoorbeelden binnen uw organisatie zonder gevolgen voor alle gebruikers maken.

Wilt details over het configureren van het beleid, voordat we gaan de standaardconfiguratie.

## <a name="user-sign-in-frequency"></a>Gebruiker aanmelden frequentie

Aanmelden frequentie definieert de tijdsperiode op waarna een gebruiker wordt gevraagd zich opnieuw aanmelden bij een poging te krijgen tot een resource.

De standaardconfiguratie voor Azure Active Directory (Azure AD) voor aanmelding door een gebruiker in frequentie is een rolling venster van 90 dagen. Gebruikers vragen om referenties vaak denkt dat u een functionele wat u moet doen, maar het kan nadelig: gebruikers die zijn getraind hun referenties moeten opgeven zonder denken kan onbedoeld ze naar een schadelijke referentieprompt opgeven.

Klinkt aanwezig niet vragen om een gebruiker zich opnieuw aanmelden voor 90 dagen in werkelijkheid die een schending van IT-beleid wordt de sessie ingetrokken. Enkele voorbeelden bevatten (maar niet beperkt tot) een wachtwoordwijziging, een incompliant apparaat of een account uitschakelen. U kunt ook expliciet [intrekken van gebruikers-sessies met behulp van PowerShell](https://docs.microsoft.com/powershell/module/azuread/revoke-azureaduserallrefreshtoken?view=azureadps-2.0). De standaardconfiguratie van Azure AD wordt geleverd omlaag voor 'geen gebruikers vragen om hun referenties als beveiligingsstatus van de sessies niet is gewijzigd'.

Aanmelden frequentie-instelling werkt met apps die OATH2 of OIDC-protocollen volgens de normen hebt geïmplementeerd. De meeste Microsoft systeemeigen apps voor Windows, Mac en mobiele voldoet aan de instelling.

## <a name="persistence-of-browsing-sessions"></a>Persistentie van sessies bladeren

Een permanente browsersessie kan gebruikers aangemeld blijven na sluiten en opnieuw openen van het browservenster.

De standaard Azure AD voor sessiepersistentie browser kan gebruikers op persoonlijke apparaten te kiezen of om vast te leggen van de sessie waarin een 'Blijf aangemeld?" na een succesvolle verificatie wordt gevraagd. Als de persistentie van de browser is geconfigureerd in AD FS met behulp van de richtlijnen in het artikel [AD FS Single Sign-On-instellingen](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-single-sign-on-settings#enable-psso-for-office-365-users-to-access-sharepoint-online
), wordt dat beleid voldoen en de Azure AD-sessie tevens behouden. U kunt ook configureren of gebruikers in uw tenant zien de "Blijf aangemeld?" vragen door het veranderen van de juiste instelling in het deelvenster in Azure portal met behulp van de richtlijnen in het artikel aangepaste huisstijl [aanpassen van uw Azure AD-aanmeldingspagina](../fundamentals/customize-branding.md).

## <a name="configuring-authentication-session-controls"></a>Configureren van verificatie-sessiebesturingselementen

Voorwaardelijke toegang is een Azure AD Premium-capaciteit en is een premium-licentie vereist. Als u meer informatie over voorwaardelijke toegang wilt, raadpleegt u [wat is voorwaardelijke toegang in Azure Active Directory?](overview.md#license-requirements-for-using-conditional-access)

> [!WARNING]
> Als u de [configureerbare levensduur van vernieuwingstoken](../develop/active-directory-configurable-token-lifetimes.md) functie momenteel in openbare preview, houd er rekening mee dat we bieden geen ondersteuning voor het maken van twee verschillende beleidsregels voor dezelfde gebruiker of app combinatie: met deze functie en een ander subnet met de functie configureerbare levensduur van tokens. Microsoft wil buiten gebruik stellen van de functie configureerbare levensduur van vernieuwingstoken op 15 oktober en vervang deze door de functie voor beheer van voorwaardelijke toegang verificatie-sessie.  

### <a name="policy-1-sign-in-frequency-control"></a>Beleid 1: Frequentie-aanmelden-besturingselement

1. Nieuw beleid maken
1. Kies alle vereiste voorwaarden voor de omgeving van de klant, met inbegrip van de doel-cloud-apps.

   > [!NOTE]
   > Het verdient aanbeveling om in te stellen van vragen frequentie gelijk verificatie voor belangrijke Microsoft Office-apps, zoals Exchange Online en SharePoint Online voor de beste gebruikerservaring.

1. Ga naar **toegangsbeheer** > **sessie** en klikt u op **aanmelden frequentie**
1. Voer de vereiste waarde van dagen en tijdstippen in het eerste tekstvak
1. Selecteer een waarde van **uur** of **dagen** in vervolgkeuzelijst
1. U beleid opslaan

![Beleid voor voorwaardelijke toegang is geconfigureerd voor aanmelding in frequentie](media/howto-conditional-access-session-lifetime/conditional-access-policy-session-sign-in-frequency.png)

In Azure AD ingeschreven Windows-apparaten aanmelden bij het apparaat wordt beschouwd als een prompt. Als u het teken in frequentie tot 24 uur voor Office-apps hebt geconfigureerd, geregistreerde gebruikers van Azure AD Windows-apparaten, voldoen aan de aanmeldingspagina in frequentie beleid door het aanmelden bij het apparaat en wordt niet gevraagd opnieuw bij het openen van Office-apps.

Als u andere aanmelden frequentie voor verschillende web-apps die worden uitgevoerd in dezelfde browsersessie hebt geconfigureerd, wordt de strengste beleid worden toegepast op beide apps omdat alle apps die worden uitgevoerd in dezelfde browsersessie een token tijdens één sessie delen.

### <a name="policy-2-persistent-browser-session"></a>Beleid 2: Permanente browsersessie

1. Nieuw beleid maken
1. Kies alle vereiste voorwaarden.

   > [!NOTE]
   > Houd er rekening mee dat dit besturingselement is vereist 'Alle Cloud-Apps' kiezen als een voorwaarde. Browser sessiepersistentie wordt bepaald door het verificatietoken voor de sessie. Alle tabbladen in een browsersessie delen een token tijdens één sessie en daarom alle persistentie status moeten delen.

1. Ga naar **toegangsbeheer** > **sessie** en klikt u op **permanente browsersessie**
1. Selecteer een waarde in de vervolgkeuzelijst
1. U beleid opslaan

![Beleid voor voorwaardelijke toegang is geconfigureerd voor permanente browser](media/howto-conditional-access-session-lifetime/conditional-access-policy-session-persistent-browser.png)

> [!NOTE]
> Permanente browsersessie configuratie in Azure AD voor voorwaardelijke toegang wordt overschreven de "blijven zich aangemeld?" instellen in de huisstijl van de deelvenster in de Azure-portal voor dezelfde gebruiker als u beide beleidsregels hebt geconfigureerd.

## <a name="validation"></a>Validatie

Het hulpprogramma What-If gebruiken voor het simuleren van een aanmelding van de gebruiker de doeltoepassing en andere voorwaarden op basis van hoe u uw beleid hebt geconfigureerd. De besturingselementen voor verificatie sessie beheer weergegeven in het resultaat van het hulpprogramma.

![Voorwaardelijke toegang, wat gebeurt er als hulpprogramma resultaten](media/howto-conditional-access-session-lifetime/conditional-access-what-if-tool-result.png)

## <a name="policy-deployment"></a>Implementatie van beleid

Om ervoor te zorgen dat uw beleid werkt zoals verwacht, is het aanbevolen om deze te testen voordat u implementeert in productie. In het ideale geval gebruik een testtenant om te controleren of het nieuwe beleid werkt zoals bedoeld. Zie voor meer informatie het artikel [aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](best-practices.md).

## <a name="next-steps"></a>Volgende stappen

* Als u weten hoe u een beleid voor voorwaardelijke toegang configureren wilt, Zie het artikel [MFA vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory](app-based-mfa.md).
* Als u klaar om te configureren van beleid voor voorwaardelijke toegang voor uw omgeving bent, Zie het artikel [aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](best-practices.md).
