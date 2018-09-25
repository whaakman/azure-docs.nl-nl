---
title: SharePoint Online en Exchange Online instellen voor voorwaardelijke toegang van Azure Active Directory | Microsoft Docs
description: Meer informatie over het instellen van SharePoint Online en Exchange Online voor voorwaardelijke toegang van Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 62349fba-3cc0-4ab5-babe-372b3389eff6
ms.service: active-directory
ms.component: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/15/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: a5f0a17682f3481ad9a2dff5f144aa3d97f29920
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47063843"
---
# <a name="how-to-set-up-sharepoint-online-and-exchange-online-for-azure-active-directory-conditional-access"></a>Procedure: Voor het instellen van SharePoint Online en Exchange Online voor voorwaardelijke toegang van Azure Active Directory 

Met [voorwaardelijke toegang van Azure Active Directory (Azure AD)](overview.md), kunt u bepalen hoe gebruikers toegang tot uw cloud-apps. Als u gebruiken voor voorwaardelijke toegang voor het beheren van toegang tot SharePoint en Exchange online wilt, moet u naar:

- Controleer of uw scenario voor voorwaardelijke toegang wordt ondersteund
- Client-apps voorkomen dat het afdwingen van beleid voor voorwaardelijke toegang overslaan.   

In dit artikel wordt uitgelegd hoe u kan voorzien in beide gevallen.


## <a name="what-you-need-to-know"></a>Wat u moet weten

Voorwaardelijke toegang voor Azure AD kunt u cloud-apps beveiligen als een poging tot verificatie afkomstig is uit:

- Een webbrowser

- Een client-app die gebruikmaakt van [moderne verificatie](https://support.office.com/article/Using-Office-365-modern-authentication-with-Office-clients-776c0036-66fd-41cb-8928-5495c0f9168a)

- Exchange ActiveSync 

Sommige cloudapps biedt ook ondersteuning voor verouderde verificatieprotocollen. Dit geldt, bijvoorbeeld voor SharePoint Online en Exchange Online. Wanneer een client-app u een oudere verificatieprotocol gebruiken kunt voor toegang tot een cloud-app, kan geen Azure AD een beleid voor voorwaardelijke toegang op deze poging tot toegang afdwingen. Om te voorkomen dat een client-app zonder het afdwingen van beleid, moet u controleren of het is mogelijk om alleen moderne verificatie inschakelen in de betreffende cloud-apps. 

Er zijn voorbeelden voor client-apps voor voorwaardelijke toegang niet van toepassing op:

- Office 2010 en lager

- Office 2013 wanneer moderne verificatie is niet ingeschakeld



 
## <a name="control-access-to-sharepoint-online"></a>Toegang tot SharePoint Online beheren

Naast het moderne verificatie ondersteunt SharePoint Online ook verouderde verificatieprotocollen. Als de verouderde verificatieprotocollen zijn ingeschakeld, worden uw beleid voor voorwaardelijke toegang voor SharePoint niet afgedwongen voor clients die geen moderne verificatie wordt gebruikt.

U kunt verouderde verificatieprotocollen voor toegang tot SharePoint uitschakelen met behulp van de **[Set-SPOTenant](https://technet.microsoft.com/library/fp161390.aspx)** cmdlet: 

    Set-SPOTenant -LegacyAuthProtocolsEnabled $false

## <a name="control-access-to-exchange-online"></a>Toegang tot Exchange Online beheren

Bij het instellen van beleid voor voorwaardelijke toegang voor Exchange Online, moet u controleert u het volgende:

- Exchange ActiveSync

- Verouderde verificatieprotocollen



### <a name="exchange-activesync"></a>Exchange ActiveSync

Terwijl Exchange Active Sync biedt ondersteuning voor moderne verificatie, zijn er enkele beperkingen met betrekking tot de ondersteuning voor scenario's voor voorwaardelijke toegang:

- U kunt alleen de voorwaarde van de platforms apparaat configureren  

    ![Apparaatplatformen](./media/conditional-access-for-exo-and-spo/05.png)

- Instellen van de vereiste voor meervoudige verificatie wordt niet ondersteund  

    ![Voorwaardelijke toegang](./media/conditional-access-for-exo-and-spo/01.png)

Als u wilt effectief toegang tot Exchange Online beveiligen van Exchange ActiveSync, kunt u het volgende doen:

- Configureer beleid voor voorwaardelijke toegang ondersteund door de volgende stappen:

    a. Selecteer het zojuist **Office 365 Exchange Online** als cloud-app.  

    ![Voorwaardelijke toegang](./media/conditional-access-for-exo-and-spo/04.png)

    b. Selecteer **Exchange Active Sync** als **client-app**, en selecteer vervolgens **beleid toepassen op ondersteunde platformen**.  

    ![Apparaatplatformen](./media/conditional-access-for-exo-and-spo/03.png)

- Blokkeren Exchange ActiveSync op basis van regels voor Active Directory Federation Services (AD FS).

        @RuleName = "Block Exchange ActiveSync"
        c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
        => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "false");




### <a name="legacy-authentication-protocols"></a>Verouderde verificatieprotocollen

Naast het moderne verificatie ondersteunt Exchange Online ook verouderde verificatieprotocollen. Als verouderde verificatieprotocollen zijn ingeschakeld, wordt beleid voor voorwaardelijke toegang voor Exchange Online worden niet afgedwongen voor clients die geen moderne verificatie wordt gebruikt.

Voor Exchange Online kunt u verouderde verificatieprotocollen uitschakelen door in te stellen van AD FS-regels. Deze blokkeert de toegang van:

- Oudere Office-clients, zoals Office 2013 waarvoor geen moderne verificatie ingeschakeld 

- Eerdere versies van Office


## <a name="set-up-ad-fs-rules"></a>Regels voor AD FS instellen

U kunt de volgende regels voor uitgifteautorisatie inschakelen of blokkeren van verkeer op het niveau van de AD FS. 

### <a name="block-legacy-traffic-from-the-extranet"></a>Verouderde verkeer blokkeren vanaf het extranet

Door de volgende drie regels worden toegepast: 

- U inschakelen toegang voor:
    - Exchange ActiveSync-verkeer
    - Browserverkeer
    - Verkeer van moderne verificatie
- U de toegang voor blokkeren: 
    - Verouderde client-apps vanaf het extranet

**Regel 1:**

    @RuleName = "Allow all intranet traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

**Regel 2:**

    @RuleName = "Allow Exchange ActiveSync"
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

**Regel 3:**

    @RuleName = "Allow extranet browser and browser dialog traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

### <a name="block-legacy-traffic-from-anywhere"></a>Verouderde verkeer blokkeren vanaf elke locatie

Door de volgende drie regels worden toegepast:

- U inschakelen toegang voor: 
    - Exchange ActiveSync-verkeer
    - Browserverkeer
    - Verkeer van moderne verificatie
- U de toegang voor blokkeren: 
    - Verouderde apps vanaf elke locatie

##### <a name="rule-1"></a>Regel 1
    @RuleName = "Allow all intranet traffic only for browser and modern authentication clients"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>Regel 2
    @RuleName = "Allow Exchange ActiveSync"
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>Regel 3
    @RuleName = "Allow extranet browser and browser dialog traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie, [wat is voorwaardelijke toegang in Azure Active Directory](overview.md).

Zie voor instructies over het configureren van claimregels, [Claimregels configureren](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-claim-rules). 






