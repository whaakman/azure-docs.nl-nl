---
title: 'Azure AD Connect-synchronisatie: Directory uitbreidingen | Microsoft Docs'
description: In dit onderwerp beschrijft de functie van de extensies directory in Azure AD Connect.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 995ee876-4415-4bb0-a258-cca3cbb02193
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 9abd035b13a0d51c534eb8cac50c045012399a69
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="azure-ad-connect-sync-directory-extensions"></a>Azure AD Connect-synchronisatie: Directory-uitbreidingen
Directory-uitbreidingen kunt u het schema uitbreiden in Azure AD met uw eigen kenmerken van lokale Active Directory. Deze functie kunt u kenmerken die u kunt doorgaan met het beheren van lokale verbruikt LOB-apps bouwen. Deze kenmerken kunnen worden gebruikt via [directory-extensies voor Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) of [Microsoft Graph](https://graph.microsoft.io/). U kunt zien kenmerken beschikbaar via [explorer van Azure AD Graph](https://graphexplorer.azurewebsites.net/) en [Microsoft Graph explorer](https://developer.microsoft.com/en-us/graph/graph-explorer) respectievelijk.

Deze kenmerken worden op dit moment geen Office 365-werkbelasting verbruikt.

U configureren welke extra kenmerken die u wilt synchroniseren in het pad van de aangepaste instellingen in de installatiewizard.
![Wizard voor schema-uitbreiding](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)  
De installatie ziet u de volgende kenmerken, die kandidaten zijn geldig:

* Gebruikers en groepen objecttypen
* Kenmerken met één waarde: String, Boolean, Integer, binair
* Kenmerken met meerdere waarden: String, binair


>[!NOTE]
> Azure AD Connect ondersteunt meerdere waarden AD kenmerken naar Azure AD als extensies met meerdere waarden directory te synchroniseren, zijn er momenteel geen functies in Azure AD die ondersteuning bieden voor het gebruik van meerdere waarden directory-uitbreidingen.

De lijst met kenmerken is gelezen uit de schemacache gemaakt tijdens de installatie van Azure AD Connect. Als u extra kenmerken Active Directory-schema hebt uitgebreid en vervolgens de [schema moet worden vernieuwd](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) voordat deze nieuwe kenmerken zichtbaar zijn.

Een object in Azure AD kan maximaal 100 kenmerken voor Active directory-extensies hebben. De maximale lengte is 250 tekens. Als een kenmerkwaarde langer is, wordt deze afgekapt door de synchronisatie-engine.

Tijdens de installatie van Azure AD Connect is een toepassing geregistreerd waar deze kenmerken beschikbaar zijn. Hier ziet u deze toepassing in de Azure portal.  
![Schema-uitbreiding App](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3new.png)

De kenmerken worden voorafgegaan door uitbreiding\_{AppClientId}\_. De AppClientId heeft dezelfde waarde voor alle kenmerken in uw Azure AD-tenant.

Deze kenmerken zijn nu beschikbaar via de **Azure AD Graph**:

We kunnen query uitvoeren op de Azure AD Graph via de Azure AD Graph explorer: [https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/)

![Graph](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

Of via de **Microsoft Graph API**:

We kunnen de Microsoft Graph API via de Microsoft Graph explorer query: [https://developer.microsoft.com/en-us/graph/graph-explorer#](https://developer.microsoft.com/en-us/graph/graph-explorer#)

>[!NOTE]
> U moet expliciet vragen voor het kenmerk moet worden geretourneerd. Dit kan worden gedaan door expliciet selecteren van de kenmerken als volgt: https://graph.microsoft.com/beta/users/abbie.spencer@fabrikamonline.com? $select = extension_9d98ed114c4840d298fad781915f27e4_employeeID, extension_9d98ed114c4840d298fad781915f27e4_division voor meer informatie Controleer [Microsoft Graph: queryparameters gebruiken](https://developer.microsoft.com/en-us/graph/docs/concepts/query_parameters#select-parameter)

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [Azure AD Connect-synchronisatie](active-directory-aadconnectsync-whatis.md) configuratie.

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](active-directory-aadconnect.md).
