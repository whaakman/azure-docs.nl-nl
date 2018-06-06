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
ms.openlocfilehash: c41c02acaeffa170d55f3c59f34a4b1ecae1c523
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34712450"
---
# <a name="using-b2clogincom"></a>Met b2clogin.com

>[!IMPORTANT]
>Deze functie is openbare preview 
>

U hebt nu de optie voor het gebruik van de service Azure AD B2C met `<YourTenantName>.b2clogin.com` in plaats van `login.microsoftonline.com`.  Dit biedt veel voordelen:
* De maximale grootte van dezelfde cookie-header wordt niet langer worden gedeeld met andere Microsoft-producten
* U kunt alle verwijzingen naar Microsoft in de URL verwijderen (u kunt vervangen `<YourTenantName>.onmicrosoft.com` met uw tenant-ID)

 Om te profiteren van b2clogin.com, moet u het volgende instellen:

1. Voor uw **uitvoeren nu endpoint** Zorg ervoor dat u gebruikt `<YourTenantName>.b2clogin.com` in plaats van `login.microsoftonline.com`.
2. Als u MSAL gebruikt, moet u instellen `validateauthority=false`.  Dit komt doordat de uitgever van het beveiligingstoken wordt`<YourTenantName>.b2clogin.com`.