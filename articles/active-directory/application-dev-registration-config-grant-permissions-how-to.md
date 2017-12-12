---
title: Het verlenen van machtigingen voor een toepassing ontwikkelde aangepaste | Microsoft Docs
description: Het verlenen van machtigingen voor uw toepassing aangepaste ontwikkeld met behulp van de Azure AD-portal of een URL-parameter
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 0f13efd5e629bde592a3ab3a3e2db4a22180b5bb
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-grant-permissions-to-a-custom-developed-application"></a>Het verlenen van machtigingen voor een toepassing ontwikkelde aangepaste

Als u wilt verlenen toestemming optie preventief van uw app of een app in een foutbericht weergegeven dat u geen toestemming hebt gegeven worden uitgevoerd, kunt u onderstaande stappen.

## <a name="how-to-perform-admin-consent-for-your-application"></a>Het uitvoeren van beheerder toestemming voor uw toepassing

Dit heeft het effect van verlenen tot de toepassing voor alle gebruikers in uw organisatie.

1. Navigeer naar de **App registraties** blade als een **hoofdbeheerder**, selecteer vervolgens de app.

2. Selecteer **Required Permissions**, en klik tot slot de **machtiging verlenen** knop aan de bovenkant van de blade.

U kunt ook een aanvraag voor het samenstellen *login.microsoftonline.com* met de configuraties van uw app en toe te voegen op *& vragen = admin\_toestemming*. Na het aanmelden met beheerdersreferenties, heeft de app gekregen toestemming voor alle gebruikers.

## <a name="how-to-force-user-consent-for-your-application"></a>Het afdwingen van de gebruiker toestemming geven voor uw toepassing

* Toevoegen op auth-aanvragen *& vragen toestemming =* waarvoor eindgebruikers toestemming geven telkens wanneer verificatie vereist.

## <a name="next-steps"></a>Volgende stappen

[Toestemming en het integreren van Apps AzureAD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

[Toestemming en rollen voor AzureAD v2.0 geconvergeerde Apps](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[AzureAD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
