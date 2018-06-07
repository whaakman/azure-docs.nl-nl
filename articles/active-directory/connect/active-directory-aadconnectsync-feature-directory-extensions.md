---
title: 'Azure AD Connect-synchronisatie: Directory uitbreidingen | Microsoft Docs'
description: In dit onderwerp beschrijft de functie van de extensies directory in Azure AD Connect.
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
ms.openlocfilehash: dda35e63c209951547a667c46639dc0f37c87b43
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34593629"
---
# <a name="azure-ad-connect-sync-directory-extensions"></a>Azure AD Connect-synchronisatie: Directory-uitbreidingen
Directory-uitbreidingen kunt u het schema in Azure Active Directory (Azure AD) uit te breiden met uw eigen kenmerken van lokale Active Directory. Deze functie kunt u LOB-apps bouwen door kenmerken die u voor het beheren van lokale blijven gebruiken. Deze kenmerken kunnen worden gebruikt via [directory-Azure AD Graph API-uitbreidingen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) of [Microsoft Graph](https://graph.microsoft.io/). U kunt de beschikbare kenmerken zien met behulp van [Explorer van Azure AD Graph](https://graphexplorer.azurewebsites.net/) en [Microsoft Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer)respectievelijk.

Op dit moment verbruikt geen Office 365-werkbelasting deze kenmerken.

U configureren welke extra kenmerken die u wilt synchroniseren in het pad van de aangepaste instellingen in de installatiewizard.

![Wizard voor schema-uitbreiding](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)  

De installatie ziet u de volgende kenmerken, die kandidaten zijn geldig:

* Gebruikers en groepen objecttypen
* Kenmerken met één waarde: String, Boolean, Integer, binair
* Kenmerken met meerdere waarden: String, binair


>[!NOTE]
> Azure AD Connect ondersteunt synchroniseren met meerdere waarden Active Directory-kenmerken naar Azure AD als meerdere waarden directory-uitbreidingen. Maar er zijn geen functies in Azure AD momenteel ondersteuning voor het gebruik van meerdere waarden directory-uitbreidingen.

De lijst met kenmerken is gelezen uit de schemacache dat gemaakt tijdens de installatie van Azure AD Connect. Als u extra kenmerken Active Directory-schema hebt uitgebreid, moet u [vernieuwen van het schema](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) voordat deze nieuwe kenmerken zichtbaar zijn.

Een object in Azure AD kan maximaal 100 kenmerken voor directory-extensies hebben. De maximale lengte is 250 tekens. Als een kenmerkwaarde langer is, wordt deze door de synchronisatie-engine afgekapt.

Tijdens de installatie van Azure AD Connect is een toepassing geregistreerd waar deze kenmerken beschikbaar zijn. Hier ziet u deze toepassing in de Azure portal.

![Schema-uitbreiding app](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3new.png)

De kenmerken worden voorafgegaan door de uitbreiding \_{AppClientId}\_. AppClientId heeft dezelfde waarde voor alle kenmerken in uw Azure AD-tenant.

Deze kenmerken zijn nu beschikbaar via de Azure AD Graph API. U kunt deze query met behulp van [Explorer van Azure AD Graph](https://graphexplorer.azurewebsites.net/).

![Azure AD Graph Explorer](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

Of u kunt de kenmerken via de Microsoft Graph-API met behulp van een query [Microsoft Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer#).

>[!NOTE]
> U moet vragen van de kenmerken die moeten worden geretourneerd. Selecteer expliciet de kenmerken die u als volgt: https://graph.microsoft.com/beta/users/abbie.spencer@fabrikamonline.com?$select = extension_9d98ed114c4840d298fad781915f27e4_employeeID, extension_9d98ed114c4840d298fad781915f27e4_division. 
>
> Zie voor meer informatie [Microsoft Graph: gebruik queryparameters](https://developer.microsoft.com/en-us/graph/docs/concepts/query_parameters#select-parameter).

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [Azure AD Connect-synchronisatie](active-directory-aadconnectsync-whatis.md) configuratie.

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](active-directory-aadconnect.md).
