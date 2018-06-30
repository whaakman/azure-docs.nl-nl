---
title: Met behulp van b2clogin.com | Microsoft Docs
description: Meer informatie over het gebruik van b2clogin.com in plaats van login.microsoftonline.com.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/29/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: c4b3122984cdcb324f7b86e44a62e111d6ca0a29
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37131589"
---
# <a name="using-b2clogincom"></a>Met b2clogin.com

>[!IMPORTANT]
>Deze functie is openbare preview 
>

U hebt nu de optie voor het gebruik van de service Azure AD B2C met `<YourTenantName>.b2clogin.com` in plaats van `login.microsoftonline.com`.  Dit biedt veel voordelen:
* U de maximale grootte van dezelfde cookie-header niet langer worden gedeeld met andere Microsoft-producten.
* U kunt alle verwijzingen naar Microsoft in de URL verwijderen (u kunt vervangen `<YourTenantName>.onmicrosoft.com` met uw tenant-ID). Bijvoorbeeld: `https://<tenantname>.b2clogin.com/tfp/<tenantname>/<policyname>/v2.0/.well-known/openid-configuration`.

 Om te profiteren van b2clogin.com, moet u het volgende instellen:

1. Voor uw **uitvoeren nu endpoint** Zorg ervoor dat u gebruikt `<YourTenantName>.b2clogin.com` in plaats van `login.microsoftonline.com`.
2. Als u MSAL gebruikt, moet u instellen `validateauthority=false`.  Dit komt doordat de uitgever van het beveiligingstoken wordt`<YourTenantName>.b2clogin.com`.