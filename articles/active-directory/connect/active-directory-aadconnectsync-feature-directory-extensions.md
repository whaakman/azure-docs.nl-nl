---
title: 'Azure AD Connect-synchronisatie: Directory uitbreidingen | Microsoft Docs'
description: In dit onderwerp beschrijft de functie van de extensies directory in Azure AD Connect.
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 995ee876-4415-4bb0-a258-cca3cbb02193
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 16ce7da9321a4a36e892bad75819d1c30ab5197a
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
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

Deze kenmerken zijn nu beschikbaar via de grafiek:  
![Graph](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

De kenmerken worden voorafgegaan door uitbreiding\_{AppClientId}\_. De AppClientId heeft dezelfde waarde voor alle kenmerken in uw Azure AD-tenant.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [Azure AD Connect-synchronisatie](active-directory-aadconnectsync-whatis.md) configuratie.

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](active-directory-aadconnect.md).
