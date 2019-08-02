---
title: 'Azure AD Connect synchronisatie: Directory-extensies | Microsoft Docs'
description: In dit onderwerp wordt de functie Directory-extensies in Azure AD Connect beschreven.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 995ee876-4415-4bb0-a258-cca3cbb02193
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/05/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 88fdfce58bdd8e13637e77d01d4b6c0ab21f696a
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68607651"
---
# <a name="azure-ad-connect-sync-directory-extensions"></a>Azure AD Connect synchronisatie: Directory-extensies
U kunt Directory-extensies gebruiken om het schema uit te breiden in Azure Active Directory (Azure AD) met uw eigen kenmerken van on-premises Active Directory. Met deze functie kunt u LOB-apps bouwen door gebruik te maken van kenmerken die u on-premises blijft beheren. Deze kenmerken kunnen worden gebruikt via [Azure AD Graph API Directory-extensies](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) of [Microsoft Graph](https://developer.microsoft.com/graph/). U kunt de beschik bare kenmerken weer geven met behulp van respectievelijk [Azure AD Graph Explorer](https://graphexplorer.azurewebsites.net/) en [Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer).

Op dit moment gebruikt geen Office 365-werk belasting deze kenmerken.

U kunt configureren welke extra kenmerken u wilt synchroniseren in het pad voor aangepaste instellingen in de installatie wizard.

>[!NOTE]
>Het vak beschik bare kenmerken is hoofdletter gevoelig.

![Wizard schema-uitbrei ding](./media/how-to-connect-sync-feature-directory-extensions/extension2.png)  

In de installatie worden de volgende kenmerken weer gegeven. Dit zijn geldige kandidaten:

* Object typen voor gebruikers en groepen
* Kenmerken met één waarde: Teken reeks, Booleaanse waarde, geheel getal, binair
* Kenmerken met meerdere waarden: Teken reeks, binair


>[!NOTE]
> Hoewel Azure AD Connect ondersteuning biedt voor het synchroniseren van kenmerken met meerdere waarden Active Directory naar Azure AD als Directory-extensies met meerdere waarden, is er momenteel geen manier om de gegevens op te halen/te gebruiken die zijn geüpload in kenmerken van Directory-extensies met meerdere waarden.

De lijst met kenmerken wordt gelezen uit de schema cache die is gemaakt tijdens de installatie van Azure AD Connect. Als u het Active Directory schema met aanvullende kenmerken hebt uitgebreid, moet u [het schema vernieuwen](how-to-connect-installation-wizard.md#refresh-directory-schema) voordat deze nieuwe kenmerken zichtbaar zijn.

Een object in azure AD kan Maxi maal 100 kenmerken voor Directory-extensies hebben. De maximale lengte is 250 tekens. Als een kenmerk waarde langer is, wordt deze door de synchronisatie-engine afgekapt.

Tijdens de installatie van Azure AD Connect wordt een toepassing geregistreerd waar deze kenmerken beschikbaar zijn. U kunt deze toepassing weer geven in de Azure Portal.

![App voor schema-uitbrei ding](./media/how-to-connect-sync-feature-directory-extensions/extension3new.png)

De kenmerken worden voorafgegaan door de extensie \_{AppClientId}.\_ AppClientId heeft dezelfde waarde voor alle kenmerken in uw Azure AD-Tenant.

Deze kenmerken zijn nu beschikbaar via de Azure AD-Graph API. U kunt query's uitvoeren met behulp van [Azure AD Graph Explorer](https://graphexplorer.azurewebsites.net/).

![Azure AD Graph Explorer](./media/how-to-connect-sync-feature-directory-extensions/extension4.png)

U kunt ook de kenmerken opvragen via de Microsoft Graph-API met behulp van [Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer#).

>[!NOTE]
> U moet vragen om de kenmerken te retour neren. Selecteer de kenmerken als volgt expliciet: https\://graph.microsoft.com/beta/users/abbie.spencer@fabrikamonline.com? $Select = extension_9d98ed114c4840d298fad781915f27e4_employeeID, extension_9d98ed114c4840d298fad781915f27e4_division. 
>
> Zie [Microsoft Graph voor meer informatie: Query parameters](https://developer.microsoft.com/graph/docs/concepts/query_parameters#select-parameter)gebruiken.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [Azure AD Connect synchronisatie](how-to-connect-sync-whatis.md) configuratie.

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](whatis-hybrid-identity.md).
