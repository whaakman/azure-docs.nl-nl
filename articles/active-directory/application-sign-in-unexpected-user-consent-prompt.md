---
title: Onverwachte instemmingsprompt tijdens het aanmelden bij een toepassing | Microsoft Docs
description: "Wanneer een gebruiker ziet een toestemming wordt gevraagd een toepassing die hebt geïntegreerd met Azure AD dat u niet verwacht oplossen"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: e5b823e1251a7221f73efe6838d439f827f9665d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="unexpected-consent-prompt-when-signing-in-to-an-application"></a>Onverwachte instemmingsprompt tijdens het aanmelden bij een toepassing

Veel toepassingen die zijn geïntegreerd met Azure Active Directory vereisen machtigingen voor verschillende bronnen om te kunnen uitvoeren. Wanneer deze resources ook worden geïntegreerd met Azure Active Directory, toegang tot deze is aangevraagd met Azure AD toestemming framework. 

Dit resulteert in een toestemming vragen dat wordt weergegeven de eerste keer dat een toepassing wordt gebruikt, die vaak is een eenmalige bewerking. 

## <a name="scenarios-in-which-users-see-consent-prompts"></a>Scenario's waarin gebruikers zien toestemming vragen

Aanvullende prompts kunnen worden verwacht in verschillende scenario's:

* De reeks machtigingen die vereist zijn door de toepassing hebt gewijzigd.

* De gebruiker die oorspronkelijk wil de toepassing is niet een beheerder en nu de toepassing voor het eerst is gebruikt door de gebruiker van een andere (niet-beheerders).

* De gebruiker die oorspronkelijk wil de toepassing is een beheerder, maar zij geen toestemming geven namens de hele organisatie.

* De toepassing gebruikmaakt van [incrementele en dynamische toestemming](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) om aanvullende machtigingen nadat toestemming in eerste instantie is verleend. Dit wordt vaak gebruikt als optionele functies van een toepassing extra machtigingen die vereist is voor basisfunctionaliteit niet vereist.

* Toestemming is ingetrokken na in eerste instantie wordt verleend.

* De toepassing gevraagd om bevestiging toestemming telkens wanneer deze wordt gebruikt door de ontwikkelaar is geconfigureerd (Opmerking: dit is niet aanbevolen).

## <a name="next-steps"></a>Volgende stappen

-   [Apps, machtigingen en toestemming in Azure Active Directory (v1.0 eindpunt)](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)

-   [Scopes, machtigingen en toestemming in Azure Active Directory (v2.0-eindpunt)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


