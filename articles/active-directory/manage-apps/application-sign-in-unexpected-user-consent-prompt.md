---
title: Onverwachte toestemmingsprompt bij het aanmelden bij een toepassing | Microsoft Docs
description: Hoe u problemen kunt oplossen wanneer een gebruiker ziet een toestemmingsprompt voor een toepassing die u hebt geïntegreerd met Azure AD die u niet verwacht
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: celested
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 178a5e4e2a6ed520a6c58ce07a30684b2d1cbc09
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56217289"
---
# <a name="unexpected-consent-prompt-when-signing-in-to-an-application"></a>Onverwachte toestemmingsprompt bij het aanmelden bij een toepassing

Veel toepassingen die kunnen worden geïntegreerd met Azure Active Directory vereist machtigingen voor verschillende bronnen om uit te voeren. Wanneer deze resources ook worden geïntegreerd met Azure Active Directory, machtigingen voor toegang tot deze wordt aangevraagd met behulp van het toestemmingsframework Azure AD. 

Dit resulteert in een toestemming vragen wordt weergegeven de eerste keer dat een toepassing wordt gebruikt, dit is vaak een eenmalige bewerking. 

## <a name="scenarios-in-which-users-see-consent-prompts"></a>Scenario's waarin gebruikers zien toestemming vragen

Als u meer vragen kunnen worden verwacht in verschillende scenario's:

* De set machtigingen die vereist zijn door de toepassing hebt gewijzigd.

* De gebruiker die oorspronkelijk ingestemd met de toepassing is niet een beheerder en nu de toepassing voor de eerste keer wordt gebruikt door de gebruiker van een andere (niet-beheerders).

* De gebruiker die oorspronkelijk ingestemd met de toepassing is een beheerder, maar deze heeft geen toestemming gegeven namens de hele organisatie.

* De toepassing gebruikmaakt van [incrementele en dynamische toestemming](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) voor aanvullende machtigingen nadat deze is in eerste instantie toegestaan. Dit wordt vaak gebruikt als optionele functies van een extra toepassing machtigingen buiten degene die vereist zijn voor de functionaliteit van de basislijn vereist.

* Toestemming is ingetrokken na in eerste instantie wordt verleend.

* De toepassing om te vereisen een toestemmingsprompt telkens wanneer deze wordt gebruikt door de ontwikkelaar is geconfigureerd (Opmerking: dit is niet aanbevolen).

## <a name="next-steps"></a>Volgende stappen

-   [Apps, machtigingen en toestemming in Azure Active Directory (v1.0 eindpunt)](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)

-   [Bereiken, machtigingen en toestemming in Azure Active Directory (v2.0-eindpunt)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


