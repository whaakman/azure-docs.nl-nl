---
title: SharePoint Online en Exchange Online instellen voor voorwaardelijke toegang van Azure Active Directory | Microsoft Docs
description: Informatie over het instellen van SharePoint Online en Exchange Online voor voorwaardelijke toegang van Azure Active Directory.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 62349fba-3cc0-4ab5-babe-372b3389eff6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: c527d4511ab2dd3bb349c10c1defb24723fef59a
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="set-up-sharepoint-online-and-exchange-online-for-azure-active-directory-conditional-access"></a>SharePoint Online en Exchange Online instellen voor voorwaardelijke toegang van Azure Active Directory 

Met [voorwaardelijke toegang van Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), kunt u bepalen hoe gebruikers toegang tot uw cloud-apps. Als u het gebruik van voorwaardelijke toegang om toegang voor SharePoint en Exchange online te beheren wilt, moet u naar:

- Controleer of uw scenario voorwaardelijke toegang wordt ondersteund
- Voorkomen dat de client-apps zonder het afdwingen van uw beleid voor voorwaardelijke toegang.   

Dit artikel wordt uitgelegd hoe u beide gevallen kunt oplossen.


## <a name="what-you-need-to-know"></a>Wat u moet weten

U kunt voorwaardelijke toegang van Azure AD cloud om apps te beveiligen als een verificatiepoging afkomstig van is:

- Een webbrowser

- Een client-app die gebruikmaakt van [moderne verificatie](https://support.office.com/article/Using-Office-365-modern-authentication-with-Office-clients-776c0036-66fd-41cb-8928-5495c0f9168a)

- Exchange ActiveSync 

Sommige cloud-apps bieden ook ondersteuning voor legacy-verificatieprotocollen. Dit geldt, bijvoorbeeld voor SharePoint Online en Exchange Online. Wanneer een client-app u een verouderde verificatieprotocol gebruiken kunt voor toegang tot een cloud-app, kan geen beleid voor voorwaardelijke toegang op deze toegangspoging door Azure AD afdwingen. Om te voorkomen dat een client-app voor het omzeilen van het afdwingen van beleid, moet u controleren of het is mogelijk alleen moderne verificatie op de desbetreffende cloud-apps inschakelen. 

Voorbeelden voor voorwaardelijke toegang van apps niet van toepassing op client zijn:

- Office 2010 en eerder

- Office 2013 wanneer moderne verificatie niet is ingeschakeld



 
## <a name="control-access-to-sharepoint-online"></a>Toegang tot SharePoint Online beheren

Naast moderne verificatie ondersteunt SharePoint Online ook verouderde verificatieprotocollen. Als de oude verificatieprotocollen zijn ingeschakeld, worden uw beleid voor voorwaardelijke toegang voor SharePoint niet afgedwongen voor clients die gebruikmaken van moderne verificatie niet.

U kunt de verouderde verificatieprotocollen voor toegang tot SharePoint uitschakelen met behulp van de  **[Set SPOTenant](https://technet.microsoft.com/library/fp161390.aspx)**  cmdlet: 

    Set-SPOTenant -LegacyAuthProtocolsEnabled $false

## <a name="control-access-to-exchange-online"></a>Toegang tot Exchange Online beheren

Bij het instellen van beleidsregels voor voorwaardelijke toegang voor Exchange Online, moet u controleert u het volgende:

- Exchange ActiveSync

- Verouderde verificatieprotocollen



### <a name="exchange-activesync"></a>Exchange ActiveSync

Exchange Active Sync ondersteunt moderne verificatie, maar er zijn enkele beperkingen met betrekking tot de ondersteuning voor scenario's voor voorwaardelijke toegang:

- U kunt alleen de voorwaarde van de platforms apparaat configureren  

    ![Apparaatplatformen](./media/active-directory-conditional-access-no-modern-authentication/05.png)

- Instellen van de vereiste voor meervoudige verificatie wordt niet ondersteund  

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-no-modern-authentication/01.png)

Om toegang tot Exchange Online effectief beveiligen van Exchange ActiveSync, kunt u het volgende doen:

- Een ondersteunde voorwaardelijk toegangsbeleid configureren met de volgende stappen:

    a. Selecteer het zojuist **Office 365 Exchange Online** als cloud-app.  

    ![Voorwaardelijke toegang](./media/active-directory-conditional-access-no-modern-authentication/04.png)

    b. Selecteer **Exchange Active Sync** als **clientapp**, en selecteer vervolgens **beleid van toepassing alleen op ondersteunde platforms**.  

    ![Apparaatplatformen](./media/active-directory-conditional-access-no-modern-authentication/03.png)

- Exchange ActiveSync geblokkeerd via regels voor Active Directory Federation Services (AD FS).

        @RuleName = "Block Exchange ActiveSync"
        c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
        => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "false");




### <a name="legacy-authentication-protocols"></a>Verouderde verificatieprotocollen

Naast moderne verificatie ondersteunt Exchange Online ook verouderde verificatieprotocollen. Als oudere verificatieprotocollen zijn ingeschakeld, worden uw beleid voor voorwaardelijke toegang voor Exchange Online voor clients die geen van moderne verificatie gebruikmaken niet afgedwongen.

Voor Exchange Online kunt u verouderde verificatieprotocollen uitschakelen door het instellen van regels voor AD FS. Deze toegang vanuit geblokkeerd:

- Oudere Office-clients, zoals Office 2013 waarvoor geen moderne verificatie is ingeschakeld 

- Eerdere versies van Office


## <a name="set-up-ad-fs-rules"></a>AD FS-regels instellen

U kunt de volgende regels voor uitgifteautorisatie inschakelen of blokkeren van verkeer op het niveau van de AD FS. 

### <a name="block-legacy-traffic-from-the-extranet"></a>Verouderde verkeer blokkeren van het extranet

Door het toepassen van de volgende drie regels: 

- U inschakelen toegang voor:
    - Exchange ActiveSync-verkeer
    - Browserverkeer
    - Verkeer van moderne verificatie
- U blokkeren toegang voor: 
    - Verouderde client-apps van het extranet

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

Door het toepassen van de volgende drie regels:

- U inschakelen toegang voor: 
    - Exchange ActiveSync-verkeer
    - Browserverkeer
    - Verkeer van moderne verificatie
- U blokkeren toegang voor: 
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

Zie voor meer informatie [voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal.md)




