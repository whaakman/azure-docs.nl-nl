---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: f3048daa7d597d92041733cdf5c6f299cebc2f73
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142386"
---
## <a name="configure-your-aspnet-web-app-with-the-applications-registration-information"></a>Uw ASP.NET-Web-App configureren met de registratiegegevens van de toepassing

In deze stap maakt u configureren van uw project om SSL te gebruiken, en vervolgens de SSL-URL gebruiken voor het configureren van uw toepassing registratie-informatie. Vervolgens kunt toevoegen van de toepassing ' registratiegegevens aan uw oplossing via *web.config*.

1. Selecteer het project in Solution Explorer en bekijk de `Properties` venster (als er geen een venster met eigenschappen, druk op F4)
2. Wijziging `SSL Enabled` naar `True`
3. Kopieer de waarde van `SSL URL` hierboven en plak deze in de `Redirect URL` veld boven aan deze pagina en klik vervolgens op *Update*:<br/><br/>![Projecteigenschappen](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
4. Voeg de volgende in `web.config` bestand zich in de hoofdmap van de map, onder de sectie `configuration\appSettings`:

```xml
<add key="ClientId" value="[Enter the application Id here]" />
<add key="RedirectUri" value="[Enter the Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
