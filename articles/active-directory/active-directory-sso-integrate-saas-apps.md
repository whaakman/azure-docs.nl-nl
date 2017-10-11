---
title: Eenmalige aanmelding Azure Active Directory integreren met SaaS-apps | Microsoft Docs
description: Eenmalige aanmelding, verificatie en gebruikers inrichten gecentraliseerd beheer van SaaS-apps in Azure Active Directory inschakelen. Een overzicht van hoe u Azure Active Directory integreren met SaaS-apps.
services: active-directory
keywords: Azure AD integreren met SaaS-apps
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 53b9d341-d1fc-4bbb-ac7c-3f4c68fcf00a
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/17/2017
ms.author: curtand
ms.reviewer: aaronsm
ms.openlocfilehash: fc0d297598c334ca8f6f8a2bd3ae948c87956342
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="integrate-azure-active-directory-single-sign-on-with-saas-apps"></a>Eenmalige aanmelding Azure Active Directory integreren met SaaS-apps
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-enterprise-apps-manage-sso.md)
> * [Klassieke Azure Portal](active-directory-sso-integrate-saas-apps.md)
>
>

[!INCLUDE [active-directory-sso-use-case-intro](../../includes/active-directory-sso-use-case-intro.md)]

Om te beginnen instellen van eenmalige aanmelding voor een app die u in uw organisatie bent gebracht, wordt u een bestaande map in Azure Active Directory (Azure AD). U kunt een Azure AD-directory die u via Microsoft Azure, Office 365 of Windows Intune aanschaft gebruiken. Als u twee of meer van deze hebt, raadpleegt u [uw Azure AD-directory beheren](active-directory-administer.md) om te bepalen welke een om te gebruiken.

> [!IMPORTANT]
> Microsoft raadt u aan Azure AD te beheren met het [Azure AD-beheercentrum](https://aad.portal.azure.com) in Azure Portal in plaats van de klassieke Azure portal waarnaar in dit artikel wordt verwezen. Voor informatie over het toewijzen van beheerdersrollen in het Azure AD-beheercentrum, Zie [beheerdersrollen toewijzen in Azure Active Directory](active-directory-enterprise-apps-manage-sso.md).

## <a name="authentication"></a>Authentication
Voor toepassingen die ondersteuning bieden voor de WS-Federation, SAML 2.0 of OpenID Connect protocollen, Azure Active Directory gebruikt handtekeningcertificaten vertrouwensrelaties vastleggen. Zie voor meer informatie over deze [beheren van certificaten voor federatieve eenmalige aanmelding](active-directory-sso-certs.md).

Voor toepassingen die ondersteuning bieden voor alleen HTML-formulieren gebaseerde aanmeldingspagina, gebruikt Azure Active Directory 'wachtwoordkluizen' vertrouwensrelaties vastleggen. Hierdoor kunnen de gebruikers in uw organisatie automatisch worden aangemeld bij een SaaS-toepassing door Azure AD met behulp van de gegevens van de gebruikersaccount van de SaaS-toepassing. Azure AD worden verzameld en veilig opgeslagen gegevens van de gebruikersaccount en het bijbehorende wachtwoord. Zie voor meer informatie [op basis van wachtwoorden eenmalige aanmelding](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

## <a name="authorization"></a>Autorisatie
Een ingerichte account kan een gebruiker worden geautoriseerd voor het gebruik van een toepassing nadat ze zijn geverifieerd via eenmalige aanmelding. Gebruikers inrichten kan worden gedaan handmatig of in sommige gevallen kunt u toevoegen en verwijderen van gebruikersgegevens van de SaaS-app op basis van wijzigingen in Azure Active Directory. Zie voor meer informatie over het gebruik van de bestaande Azure AD-connectors voor het automatisch inrichten [automatisch gebruikers inrichten en de inrichting voor SaaS-toepassingen](active-directory-saas-app-provisioning.md).

Anders, u kunt handmatig gebruikersgegevens toevoegen aan een app, of gebruik andere inrichting oplossingen die beschikbaar in de marketplace zijn.

## <a name="access"></a>Toegang
Azure AD levert aanpasbare op verschillende manieren voor het implementeren van toepassingen voor eindgebruikers in uw organisatie. U bent niet vergrendeld in een bepaalde implementatie of een oplossing voor toegang. U kunt [welke oplossing het beste past bij uw behoeften](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

## <a name="additional-considerations-for-applications-already-in-use"></a>Aanvullende overwegingen voor toepassingen al gebruik in
Instellen van eenmalige aanmelding op voor een toepassing die gebruikmaakt van uw organisatie al is een ander proces van het proces van het maken van nieuwe accounts voor een nieuwe toepassing. Er zijn een aantal voorbereidende stappen, met inbegrip van: toewijzing van gebruikers-id's in de toepassing aan Azure AD-identiteiten en kennis van hoe gebruikers krijgen met aanmelden bij een toepassing nadat deze is geïntegreerd.

> [!NOTE]
> Als u eenmalige aanmelding instelt voor een bestaande toepassing, moet u beschikken over rechten voor globale beheerder in beide Azure AD en de SaaS-toepassing.
>
>

### <a name="mapping-user-accounts"></a>Toewijzing van gebruikersaccounts
De identiteit van een gebruiker heeft doorgaans een unieke id die een e-mailadres of de UPN (user Principal name worden kan). Koppeling (kaart). u moet elke gebruiker toepassingsidentiteit naar hun respectieve Azure AD identity. Er zijn verschillende manieren om dit te bereiken, afhankelijk van hoe de vereisten van de verificatie van uw toepassing.

Zie voor meer informatie over het toewijzen van de toepassing identiteiten met Azure AD-identiteiten [uitgegeven claims in het SAML-token aanpassen](http://social.technet.microsoft.com/wiki/contents/articles/31257.azure-active-directory-customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps.aspx) en [aanpassen kenmerktoewijzingen voor het inrichten van](active-directory-saas-customizing-attribute-mappings.md).

### <a name="understanding-the-users-log-in-experience"></a>Kennis van de gebruiker aanmelden
Wanneer u eenmalige aanmelding voor een toepassing die al in gebruik integreert, is het Houd er rekening mee dat de gebruikerservaring worden beïnvloed. Voor alle toepassingen beginnen gebruikers met hun Azure AD-referenties aan te melden. Ook kan zijn dat ze een andere portal gebruiken moeten voor toegang tot de toepassingen.

Eenmalige aanmelding voor sommige toepassingen kan worden uitgevoerd op de aanmelding van de toepassing in de interface, maar voor andere toepassingen van de gebruiker zijn, zoals door een centrale portal gaan ([mijn Apps](http://myapps.microsoft.com) of [Office365](http://portal.office.com/myapps)) aan te melden. Meer informatie over de verschillende soorten eenmalige aanmelding en hun gebruikerservaringen in [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

Een andere waardevolle resource is *toestemming van de gebruiker het onderdrukken van* in de [Guiding ontwikkelaars](active-directory-applications-guiding-developers-for-lob-applications.md) artikel.

## <a name="next-steps"></a>Volgende stappen
Voor SaaS-apps die u in de App-galerie van vinden Azure AD levert een aantal [zelfstudies over het integreren van SaaS-apps](active-directory-saas-tutorial-list.md).

Als de app is niet in App-galerie, kunt u [toe te voegen aan de galerie van Azure AD-App als een aangepaste toepassing](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

Er is veel meer details op al deze problemen in de bibliotheek Azure.com vanaf [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).

Bovendien mis niet de [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md).
