---
title: Het verlenen van machtigingen voor een aangepaste toepassing | Microsoft Docs
description: Het verlenen van machtigingen voor uw aangepaste toepassing met behulp van de Azure AD-portal of een URL-parameter
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: celested
ms.openlocfilehash: 10cf04fca8379f41587b1ea680c5b52a26193b53
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44724054"
---
# <a name="how-to-grant-permissions-to-a-custom-developed-application"></a>Het verlenen van machtigingen voor een aangepaste toepassing

Als u wilt verlenen van toestemming preventief in uw app of aan een app worden uitgevoerd in een foutbericht weergegeven dat u geen toestemming hebt gegeven, kunt u onderstaande stappen.

## <a name="how-to-perform-admin-consent-for-your-application"></a>Toestemming van een beheerder voor uw toepassing uitvoeren

Dit is het effect van het verlenen van toestemming voor de toepassing voor alle gebruikers in uw organisatie.

1. Navigeer naar de **App-registraties** blade als een **hoofdbeheerder**, selecteer vervolgens de app.

2. Selecteer **vereiste machtigingen**, en ten slotte klikt u op de **machtigingen verlenen** knop aan de bovenkant van de blade.

U kunt ook kunt u een aanvraag voor het samenstellen *login.microsoftonline.com* met uw app-configuraties en toe te voegen op *& vragen = admin\_toestemming geven*. Na het aanmelden met beheerdersreferenties, heeft toestemming voor alle gebruikers op de app zijn verleend.

## <a name="how-to-force-user-consent-for-your-application"></a>Het afdwingen van de gebruiker toestemming geven voor uw toepassing

* Append naar verificatieaanvragen *& vragen toestemming =* die vereisen dat eindgebruikers toestemming geven telkens wanneer ze worden geverifieerd.

## <a name="next-steps"></a>Volgende stappen

[Toestemming en het integreren van Apps aan AzureAD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

[De toestemming en rollen voor AzureAD v2.0 geconvergeerde Apps](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[AzureAD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
