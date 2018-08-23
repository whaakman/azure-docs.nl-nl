---
title: 'Azure AD Connect-synchronisatie: mapextensies | Microsoft Docs'
description: Dit onderwerp beschrijft de functie voor directory-extensies in Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 995ee876-4415-4bb0-a258-cca3cbb02193
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: a19fa124396bf9c9db4a60591b74f5425ce9670b
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42061341"
---
# <a name="azure-ad-connect-sync-directory-extensions"></a>Azure AD Connect-synchronisatie: Directory-extensies
Directory-extensies kunt u het schema in Azure Active Directory (Azure AD) uit te breiden met uw eigen kenmerken van on-premises Active Directory. Deze functie kunt u LOB-apps kunt maken met kenmerken die u doorgaan met het on-premises beheren verbruikt. Deze kenmerken kunnen worden gebruikt via [directory-Azure AD Graph API-uitbreidingen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) of [Microsoft Graph](https://graph.microsoft.io/). U kunt de beschikbare kenmerken zien met behulp van [Azure AD Graph Explorer](https://graphexplorer.azurewebsites.net/) en [Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer), respectievelijk.

Deze kenmerken worden op dit moment geen Office 365-werkbelasting verbruikt.

U configureren welke aanvullende kenmerken die u wilt synchroniseren in het pad van de aangepaste instellingen in de installatiewizard.

![Wizard voor schema-uitbreiding](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)  

De installatie ziet u de volgende kenmerken die geldige kandidaten zijn:

* Gebruikers en groepen objecttypen
* Kenmerken met één waarde: String, Booleaanse waarde, geheel getal, binaire bestanden
* Kenmerken met meerdere waarden: String, binaire bestanden


>[!NOTE]
> Synchronisatie met meerdere waarden Active Directory-kenmerken naar Azure AD als meerdere waarden mapextensies biedt ondersteuning voor Azure AD Connect. Maar er zijn geen functies in Azure AD, dat momenteel ondersteuning voor het gebruik van meerdere waarden mapextensies.

De lijst met kenmerken uit de cache van het schema dat gemaakt tijdens de installatie van Azure AD Connect gelezen. Als u extra kenmerken Active Directory-schema hebt uitgebreid, moet u [het schema vernieuwen](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) voordat deze nieuwe kenmerken weergegeven worden.

Een object in Azure AD kan maximaal 100 kenmerken voor directory-extensies hebben. De maximale lengte is 250 tekens. Als een kenmerkwaarde langer is, wordt deze door de synchronisatie-engine afgekapt.

Een toepassing geregistreerd tijdens de installatie van Azure AD Connect, waar deze kenmerken beschikbaar zijn. Hier ziet u deze toepassing in Azure portal.

![Schema-uitbreiding app](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3new.png)

De kenmerken worden voorafgegaan door de extensie \_{AppClientId}\_. AppClientId heeft dezelfde waarde voor alle kenmerken in uw Azure AD-tenant.

Deze kenmerken zijn nu beschikbaar via de Azure AD Graph API. U kunt query's voor uitvoert met behulp van [Azure AD Graph Explorer](https://graphexplorer.azurewebsites.net/).

![Azure AD Graph Explorer](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

Of u kunt de kenmerken via de Microsoft Graph API opvragen met behulp van [Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer#).

>[!NOTE]
> U moet vragen om de kenmerken die moeten worden geretourneerd. Selecteer expliciet de kenmerken die u als volgt: https://graph.microsoft.com/beta/users/abbie.spencer@fabrikamonline.com?$select = extension_9d98ed114c4840d298fad781915f27e4_employeeID, extension_9d98ed114c4840d298fad781915f27e4_division. 
>
> Zie voor meer informatie, [Microsoft Graph: gebruik queryparameters](https://developer.microsoft.com/graph/docs/concepts/query_parameters#select-parameter).

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [Azure AD Connect-synchronisatie](active-directory-aadconnectsync-whatis.md) configuratie.

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](active-directory-aadconnect.md).
