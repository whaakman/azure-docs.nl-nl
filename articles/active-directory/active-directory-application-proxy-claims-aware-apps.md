---
title: Claims-compatibele apps - Azure AD-toepassingsproxy | Microsoft Docs
description: Het publiceren van on-premises ASP.NET-toepassingen die AD FS-claims voor veilige externe toegang door uw gebruikers te accepteren.
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: harshja
ms.assetid: 91e6211b-fe6a-42c6-bdb3-1fff0312db15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: daveba
ms.openlocfilehash: 9cf6c5aedbf1062922e755c962449253e49f8013
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Werken met claimbewuste toepassingen in de toepassingsproxy
[Claims-compatibele apps](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) een omleiding naar de Security Token Service (STS) uitvoeren. De STS vraagt om de referenties van de gebruiker voor een token en stuurt vervolgens door de gebruiker naar de toepassing. Er zijn een aantal manieren Application Proxy voor gebruik met deze omleidingen in te schakelen. Gebruik dit artikel voor het configureren van uw implementatie van claims-compatibele apps. 

## <a name="prerequisites"></a>Vereisten
Zorg ervoor dat de STS die de claimbewuste app wordt omgeleid naar uw on-premises netwerk beschikbaar is. U kunt de STS beschikbaar maken door het beschikbaar te maken via een proxy of door externe verbindingen toestaan. 

## <a name="publish-your-application"></a>Uw toepassing publiceren

1. Uw toepassing volgens de instructies die worden beschreven publiceren [publiceren van toepassingen met toepassingsproxy](application-proxy-publish-azure-portal.md).
2. Navigeer naar de toepassingspagina in de portal en selecteer **eenmalige aanmelding**.
3. Als u hebt gekozen **Azure Active Directory** als uw **pre-authenticatie methode**, selecteer **Azure AD eenmalige aanmelding uitgeschakeld** als uw **intern Verificatiemethode**. Als u hebt gekozen **Passthrough** als uw **pre-authenticatie methode**, u hoeft niet te wijzigen.

## <a name="configure-adfs"></a>AD FS configureren

U kunt AD FS voor claims-compatibele apps op twee manieren configureren. De eerste is met behulp van aangepaste domeinen. De tweede is met WS-Federation. 

### <a name="option-1-custom-domains"></a>Optie 1: Aangepaste domeinen

Als alle de interne URL's voor uw toepassingen volledig zijn gekwalificeerde domeinnamen (FQDN's), dan kunt u configureren [aangepaste domeinen](active-directory-application-proxy-custom-domains.md) voor uw toepassingen. Gebruik de aangepaste domeinen te maken van de externe URL's die hetzelfde als de interne URL's zijn. Als de externe URL's die overeenkomen met uw interne URL's, werken de STS-omleidingen of uw gebruikers on-premises of extern zijn. 

### <a name="option-2-ws-federation"></a>Optie 2: WS-Federation

1. Open AD FS-beheer.
2. Ga naar **Relying Party-vertrouwensrelaties**, met de rechtermuisknop op de app die u met toepassingsproxy publiceren wilt en kiest u **eigenschappen**.  

   ![Relying Party-vertrouwensrelaties met de rechtermuisknop op de naam van de app - schermafbeelding](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  

3. Op de **eindpunten** tabblad onder **eindpunttype**, selecteer **WS-Federation**.
4. Onder **vertrouwde URL**, voert u de URL die u hebt ingevoerd in de toepassingsproxy onder **externe URL** en klik op **OK**.  

   ![Toevoegen van een eindpunt - waarde vertrouwde URL - schermafbeelding](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="next-steps"></a>Volgende stappen
* [Schakel eenmalige aanmelding op](application-proxy-sso-overview.md) voor toepassingen die niet claimbewuste
* [Native ClientApps om te communiceren met de proxy-toepassingen inschakelen](active-directory-application-proxy-native-client.md)


