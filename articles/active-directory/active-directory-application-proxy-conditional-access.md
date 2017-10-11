---
title: Voorwaardelijke toegang tot on-premises apps - Azure AD | Microsoft Docs
description: Bevat informatie over het instellen van voorwaardelijke toegang voor toepassingen die u publiceert toegankelijk extern met Azure AD-toepassingsproxy.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 2e97722b-eb4e-4078-b607-9fed210d8a0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 463946256f9e335fa6d98fc904835e5c3dc2725e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="working-with-conditional-access-in-azure-ad-application-proxy"></a>Werken met voorwaardelijke toegang in Azure AD-toepassingsproxy

>[!NOTE]
>In dit artikel is van toepassing op de klassieke Azure portal, die wordt buiten gebruik gesteld. Het is raadzaam dat u de [Azure-portal](https://portal.azure.com). In de Azure-portal hebt toepassingsproxy apps dezelfde voorwaardelijke toegangsfuncties als elke andere SaaS-app. Zie voor meer informatie over voorwaardelijke toegang, [aan de slag met voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

U kunt regels voor voorwaardelijke toegang verlenen tot toepassingen die zijn gepubliceerd met toepassingsproxy toegang configureren. Hiermee kunt u:

* Meervoudige authenticatie per toepassing
* Meervoudige verificatie vereisen alleen wanneer gebruikers zich niet op het werk
* Gebruikers toegang tot de toepassing niet op kantoor zijn blokkeren

Deze regels kunnen worden toegepast op alle gebruikers en groepen of alleen op specifieke gebruikers en groepen. Standaard wordt de regel geldt voor alle gebruikers die toegang tot de toepassing hebben. De regel kan echter ook worden beperkt tot gebruikers die lid van de opgegeven beveiligingsgroepen zijn.  

Toegangsregels worden geëvalueerd wanneer een gebruiker een federatieve toepassing die gebruikmaakt van OAuth 2.0, OpenID Connect, SAML of WS-Federation. Toegangsregels worden bovendien met OAuth 2.0 en OpenID Connect geëvalueerd wanneer een vernieuwingstoken wordt gebruikt voor het verkrijgen van een toegangstoken.

## <a name="conditional-access-prerequisites"></a>Vereisten voor voorwaardelijke toegang
* Abonnement op Azure Active Directory Premium
* Een federatieve of beheerde Azure Active Directory-tenant
* Federatieve tenants is vereist voor multi-factor authentication (MFA)  
    ![Toegangsregels configureren - meervoudige authenticatie](./media/active-directory-application-proxy-conditional-access/application-proxy-conditional-access.png)

## <a name="configure-per-application-multi-factor-authentication"></a>Per toepassing multi-factor authentication configureren
1. Meld u als beheerder aan in de klassieke Azure portal.
2. Ga naar Active Directory en selecteer de directory waarin u toepassingsproxy wilt inschakelen.
3. Klik op **toepassingen** en schuif omlaag naar de **toegangsregels** sectie. De sectie toegang regels wordt alleen weergegeven voor toepassingen die worden gepubliceerd met toepassingsproxy die gebruikmaken van federatieve verificatie.
4. De regel inschakelen door het selecteren **toegangsregels inschakelen** naar **op**.
5. Geef de gebruikers en groepen aan wie de regels van toepassing. Gebruik de **groep toevoegen** knop om te selecteren van een of meer groepen waarop de toegangsregel van toepassing is. Dit dialoogvenster kan ook worden gebruikt om te verwijderen van de geselecteerde groepen.  Wanneer de regels toepassen op groepen zijn geselecteerd, wordt de toegangsregels gelden alleen voor gebruikers die deel uitmaken van een van de opgegeven beveiligingsgroepen.  

   * Als u wilt uitsluiten expliciet beveiligingsgroepen van de regel, Controleer **behalve** en geef een of meer groepen. Gebruikers die lid van een groep in de lijst Except zijn zijn niet vereist voor multi-factor authentication uitvoeren.  
   * Als een gebruiker is geconfigureerd met de functie van de multi-factor authentication-server per gebruiker, heeft deze instelling voorrang op de regels van toepassing multi-factor authentication-server. Een gebruiker die is geconfigureerd voor de per gebruiker multi-factor authentication-server is vereist voor multi-factor authentication uitvoeren, zelfs als ze zijn vrijgesteld van regels voor meervoudige verificatie van de toepassing. Meer informatie over [meervoudige verificatie en per gebruiker instellingen](../multi-factor-authentication/multi-factor-authentication.md).
6. Selecteer de toegangsregel die u wilt instellen:

   * **Meervoudige authenticatie**: gebruikers aan wie toegangsregels zijn van toepassing zijn vereist voor volledige multi-factor authentication-server voordat de toegang tot de toepassing waarvoor de regel geldt.
   * **Meervoudige authenticatie niet op het werk**: moeten gebruikers toegang wilt krijgen tot de toepassing van een vertrouwde IP-adres niet naar de multi-factor authentication uitvoeren. De vertrouwde IP-adresbereiken kunnen worden geconfigureerd op de pagina voor meervoudige verificatie-instellingen.
   * **Blokkeert de toegang niet op het werk**: gebruikers toegang wilt krijgen tot de toepassing van buiten uw bedrijfsnetwerk niet mogelijk toegang tot de toepassing.

## <a name="configuring-mfa-for-federation-services"></a>Configureren van MFA voor federatieservices
Voor federatieve tenants multi-factor authentication (MFA) kan worden uitgevoerd met Azure Active Directory of met de on-premises AD FS-server. MFA wordt standaard op elke pagina die wordt gehost door Azure Active Directory. Voer Windows PowerShell en gebruik van de eigenschap – SupportsMFA om in te stellen van de Azure AD-module voor het configureren van MFA lokale.

Het volgende voorbeeld laat zien hoe lokale MFA inschakelen via de [cmdlet Set-MsolDomainFederationSettings](https://msdn.microsoft.com/library/azure/dn194088.aspx) op de tenant contoso.com:`Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true `

Naast deze vlag instelt, moet het federatieve tenant AD FS-exemplaar worden geconfigureerd voor multi-factor authentication uitvoeren. Volg de instructies voor [implementeren van Microsoft Azure multi-factor authentication-server op de lokale](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="see-also"></a>Zie ook
* [Werken met claim-compatibele toepassingen](active-directory-application-proxy-claims-aware-apps.md)
* [Publiceren van toepassingen met toepassingsproxy](active-directory-application-proxy-publish.md)
* [Eenmalige aanmelding inschakelen](active-directory-application-proxy-sso-using-kcd.md)
* [Toepassingen publiceren met uw eigen domeinnaam](active-directory-application-proxy-custom-domains.md)

Ga naar het [blog over toepassingsproxy](http://blogs.technet.com/b/applicationproxyblog/) voor nieuws en updates.
