---
title: Met behulp van b2clogin.com| Microsoft Docs
description: Meer informatie over het gebruik van b2clogin.com in plaats van login.microsoftonline.com.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 04/29/2018
ms.author: davidmu
ms.openlocfilehash: d2737e995b91caa2dcc55027baa2b552e64af23e
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
#<a name="using-b2clogincom"></a>Met b2clogin.com

>[!IMPORTANT]
>Deze functie is openbare preview 
>

U hebt nu de optie voor het gebruik van de service Azure AD B2C met `<YourTenantName>.b2clogin.com` in plaats van `login.microsoftonline.com`.  Dit biedt veel voordelen:
* De maximale grootte van dezelfde cookie-header wordt niet langer worden gedeeld met andere Microsoft-producten
* U kunt alle verwijzingen naar Microsoft in de URL verwijderen (u kunt vervangen `<YourTenantName>.onmicrosoft.com` met uw tenant-ID)

 Om te profiteren van b2clogin.com, moet u het volgende instellen:

1. Voor uw **uitvoeren nu endpoint** Zorg ervoor dat u gebruikt `<YourTenantName>.b2clogin.com` in plaats van `login.microsoftonline.com`.
2. Als u MSAL gebruikt, moet u instellen `validateauthority=false`.  Dit komt doordat de uitgever van het beveiligingstoken wordt`<YourTenantName>.b2clogin.com`.