---
title: Claimbewuste apps - Azure AD-toepassingsproxy | Microsoft Docs
description: Het publiceren van on-premises ASP.NET-toepassingen die AD FS-claims voor veilige externe toegang door uw gebruikers accepteren.
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
ms.date: 11/08/2018
ms.author: barbkess
ms.reviewer: japere
ms.openlocfilehash: bb04f1965764413c474c2e0d057d77d5d30edd07
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51623098"
---
# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Werken met claim-compatibele toepassingen in toepassingsproxy
[Claimbewuste apps](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) uitvoeren van een omleiding naar de Security Token Service (STS). De STS vraagt om de referenties van de gebruiker voor een token en vervolgens wordt de gebruiker omgeleid naar de toepassing. Er zijn een aantal manieren Application Proxy om te werken met deze omleidingen in te schakelen. Gebruik dit artikel om uw implementatie van claims-compatibele apps te configureren. 

## <a name="prerequisites"></a>Vereisten
Zorg ervoor dat de STS die de claims-compatibele app wordt omgeleid naar uw on-premises netwerk beschikbaar is. U kunt de STS beschikbaar maken door het beschikbaar te maken via een proxy of externe verbindingen. 

## <a name="publish-your-application"></a>Uw toepassing publiceren

1. Publiceer de toepassing op basis van de instructies die worden beschreven [publiceren van toepassingen met toepassingsproxy](application-proxy-publish-azure-portal.md).
2. Navigeer naar de pagina voor toepassingen in de portal en selecteer **eenmalige aanmelding**.
3. Als u ervoor hebt gekozen **Azure Active Directory** als uw **vooraf-verificatie methode**, selecteer **Azure AD eenmalige aanmelding uitgeschakeld** als uw **intern Methode voor netwerkverificatie**. Als u ervoor hebt gekozen **Passthrough** als uw **vooraf-verificatie methode**, moet u niet van belang.

## <a name="configure-adfs"></a>ADFS configureren

U kunt AD FS configureren voor de claim-compatibele apps op twee manieren. De eerste is met behulp van aangepaste domeinen. De tweede is met WS-Federation. 

### <a name="option-1-custom-domains"></a>Optie 1: Aangepaste domeinen

Als alle de interne URL's voor uw toepassingen volledig zijn gekwalificeerde domeinnamen (FQDN's), dan kunt u configureren [aangepaste domeinen](application-proxy-configure-custom-domain.md) voor uw toepassingen. Gebruik de aangepaste domeinen te maken van de externe URL's die hetzelfde als de interne URL's zijn. Als de externe URL's die overeenkomen met uw interne URL's, werken de STS-omleidingen of uw gebruikers zich on-premises of extern. 

### <a name="option-2-ws-federation"></a>Optie 2: WS-Federation

1. Open AD FS-beheer.
2. Ga naar **Relying Party-vertrouwensrelaties**, met de rechtermuisknop op de app die u met toepassingsproxy publiceren wilt en kies **eigenschappen**.  

   ![Relying Party-vertrouwensrelaties met de rechtermuisknop op de appnaam van de - schermafbeelding](./media/application-proxy-configure-for-claims-aware-applications/appproxyrelyingpartytrust.png)  

3. Op de **eindpunten** tabblad onder **eindpunttype**, selecteer **WS-Federation**.
4. Onder **vertrouwde URL**, voert u de URL die u hebt ingevoerd in de Application Proxy onder **externe URL** en klikt u op **OK**.  

   ![Toevoegen van een eindpunt - stelt de waarde van de vertrouwde URL - schermafbeelding](./media/application-proxy-configure-for-claims-aware-applications/appproxyendpointtrustedurl.png)  

## <a name="next-steps"></a>Volgende stappen
* [Schakel eenmalige aanmelding op](configure-single-sign-on-portal.md) voor toepassingen die niet claimbewuste
* [Systeemeigen client-apps om te communiceren met de proxy-toepassingen inschakelen](application-proxy-configure-native-client-application.md)


