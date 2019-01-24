---
title: Inhoudspakket voor Azure AD Power BI installeren | Microsoft Docs
description: Het inhoudspakket voor Microsoft Azure AD Power BI installeren
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
ms.assetid: fd5604eb-1334-4bd8-bfb5-41280883e2b5
ms.service: active-directory
ms.workload: identity
ms.component: report-monitor
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 9fe7d95d489b4959ce45f85180a4279dfef5f2e4
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54819166"
---
# <a name="quickstart-install-azure-active-directory-power-bi-content-pack"></a>Snelstart: Inhoudspakket voor Azure Active Directory Power BI installeren

|  |
|--|
|Op dit moment maakt het inhoudspakket voor Microsoft Azure AD Power BI gebruik van de Azure AD Graph-API's om gegevens op te halen uit uw Azure AD-tenant. Hierdoor ziet u mogelijk enkele verschillen tussen de gegevens die beschikbaar zijn in het inhoudspakket en de gegevens die worden opgehaald met de [Microsoft Graph-API's voor rapportage](concept-reporting-api.md). |
|  |

Het Power BI-inhoudspakket voor Azure AD (Azure Active Directory) biedt u de mogelijkheid om de rapportgegevens van uw omgeving te visualiseren. U kunt het vooraf gemaakte inhoudspakket downloaden en dit gebruiken om alle activiteiten in de map te rapporteren met behulp van de krachtige visualisatie-ervaring van Power BI. U kunt ook uw eigen dashboard maken en dit dashboard vervolgens eenvoudig delen met andere personen binnen uw organisatie. 

In deze snelstartgids leert u hoe u het inhoudspakket installeert.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze quickstart te voltooien:

* Een Power BI-account. Dit is hetzelfde account als uw Office 365- of Microsoft Azure AD-account. 
* De id van de Microsoft Azure AD-tenant. Dit is de **map-id** van de map op de [eigenschappenpagina](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Properties) van Azure Portal.
* Een licentie voor Microsoft Azure AD Premium (P1/P2). Zie [Aan de slag met Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) om uw versie van Azure Active Directory te upgraden.

## <a name="install-azure-ad-power-bi-content-pack"></a>Het inhoudspakket voor Microsoft Azure AD Power BI installeren 

1. Meld u aan bij [Power BI](https://app.powerbi.com/groups/me/getdata/services) met uw Power BI-account. Dit is hetzelfde account als uw Office 365- of Microsoft Azure AD-account.

2. Zoek de **Azure Active Directory-activiteitenlogboeken** op de pagina **Apps** en selecteer **Nu downloaden**. 

   ![Azure Active Directory Power BI Content Pack](./media/quickstart-install-power-bi-content-pack/getitnow.png) 
    
3. Typ de id van uw Microsoft Azure AD-tenant in het pop-upvenster, voer **7** in voor het aantal dagen om te zoeken en selecteer vervolgens **Volgende**.
    
   ![Azure Active Directory Power BI Content Pack](./media/quickstart-install-power-bi-content-pack/connect.png) 

4. Wanneer het dashboard Activiteitenlogboeken van Azure Active Directory is gemaakt, selecteert u dit.

   ![Azure Active Directory Power BI Content Pack](./media/quickstart-install-power-bi-content-pack/dashboard.png) 
    
## <a name="next-steps"></a>Volgende stappen

* [Power BI-inhoudspakket gebruiken](howto-power-bi-content-pack.md).
* [Fouten met inhoudspakketten oplossen](troubleshoot-content-pack.md).
