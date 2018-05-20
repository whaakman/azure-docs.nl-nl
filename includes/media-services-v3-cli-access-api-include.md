---
title: bestand opnemen
description: bestand opnemen
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 04/13/2018
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: acb9bdf294dd66005df203f957c155540b658698
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
## <a name="access-the-media-services-api"></a>Toegang krijgt tot de Media Services API

Om verbinding te maken met Azure Media Services API's, gebruikt u de service-principal-verificatie van Azure Active Directory. Met de volgende opdracht wordt een Azure AD-toepassing gemaakt en een service-principal aan de account gekoppeld. U gaat de geretourneerde waarden gebruiken voor het configureren van uw .NET-app, zoals wordt weergegeven in de volgende stap.

Voordat u het script uitvoert, kunt u de `amsaccount` en `amsResourceGroup` vervangen door de namen die u hebt gekozen bij het maken van deze resources. `amsaccount` is de naam van de Azure Media Services-account waaraan de service-principal moet worden gekoppeld. <br/>De opdracht die volgt, gebruikt de optie `xml` waarmee een XML-bestand wordt geretourneerd dat u in uw app.config kunt plakken. Als u de optie `xml` weglaat, bevindt de respons zich in `json`.

```azurecli-interactive
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup --xml
```

Met deze opdracht wordt een respons geproduceerd die vergelijkbaar is met de volgende:

```xml
<add key="Region" value="West US 2" />
<add key="ResourceGroup" value="amsResourceGroup" />
<add key="AadEndpoint" value="https://login.microsoftonline.com" />
<add key="AccountName" value="amsaccount" />
<add key="SubscriptionId" value="111111111-0000-2222-3333-55555555555" />
<add key="ArmAadAudience" value="https://management.core.windows.net/" />
<add key="AadTenantId" value="2222222222-0000-2222-3333-6666666666666" />
<add key="AadSecret" value="33333333-0000-2222-3333-55555555555" />
<add key="AadClientId" value="44444444-0000-2222-3333-55555555555" />
<add key="ArmEndpoint" value="https://management.azure.com/" />
```

### <a name="configure-the-sample-app"></a>De voorbeeld-app configureren

Om de app uit te voeren en toegang te krijgen tot de Media Services API's, moet u de juiste toegangswaarden opgeven in App.config. 

1. Open Visual Studio.
2. Blader naar de oplossing die u hebt gekloond.
3. Vouw in Solution Explorer het project *EncodeAndStreamFiles* uit.
4. Stel dit project in als het opstartproject.
5. Open App.config.
6. Vervang de appSettings-waarden door de waarden die u in de vorige stap hebt gekregen.

 ```xml
 <add key="Region" value="value" />
 <add key="ResourceGroup" value="value" />
 <add key="AadEndpoint" value="value" />
 <add key="AccountName" value="value" />
 <add key="SubscriptionId" value="value" />
 <add key="ArmAadAudience" value="value" />
 <add key="AadTenantId" value="value" />
 <add key="AadSecret" value="value" />
 <add key="AadClientId" value="value" />
 <add key="ArmEndpoint" value="value" />
 ```    
 
7. Druk op CTRL + SHIFT + B om de oplossing te bouwen.
