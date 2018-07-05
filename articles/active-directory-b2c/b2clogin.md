---
title: Met b2clogin.com | Microsoft Docs
description: Meer informatie over het gebruik van b2clogin.com in plaats van login.microsoftonline.com.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/29/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 1d42d9a97244eeff501b9d02b0f143d6ef0c91b2
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37440626"
---
# <a name="using-b2clogincom"></a>Met b2clogin.com

>[!IMPORTANT]
>Deze functie is de openbare preview 
>

U hebt nu de optie voor het gebruik van de Azure AD B2C-service met `<YourTenantName>.b2clogin.com` in plaats van `login.microsoftonline.com`.  Dit biedt veel voordelen:
* U delen de maximale grootte van dezelfde cookie-header niet langer met andere Microsoft-producten.
* U kunt alle verwijzingen naar Microsoft in uw URL verwijderen (u kunt vervangen `<YourTenantName>.onmicrosoft.com` door uw tenant-ID). Bijvoorbeeld: `https://<tenantname>.b2clogin.com/tfp/<tenantname>/<policyname>/v2.0/.well-known/openid-configuration`.

 Als u wilt profiteren van b2clogin.com, moet u enkele van de volgende instellen:

1. Voor uw **eindpunt nu uitvoeren** Zorg ervoor dat u gebruikmaakt van `<YourTenantName>.b2clogin.com` in plaats van `login.microsoftonline.com`.
2. Als u MSAL gebruikt, moet u instellen `validateauthority=false`.  Dit komt doordat de uitgever van het token wordt`<YourTenantName>.b2clogin.com`.