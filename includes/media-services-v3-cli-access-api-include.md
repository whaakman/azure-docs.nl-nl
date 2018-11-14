---
title: bestand opnemen
description: bestand opnemen
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 11/11/2018
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: 4dde0a47f0452da2dd951df86ccb6e02a44521ed
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51616636"
---
## <a name="access-the-media-services-api"></a>Toegang kunt krijgen tot de Media Services API

Om verbinding te maken met Azure Media Services API's, gebruikt u de service-principal-verificatie van Azure Active Directory. Met de volgende opdracht wordt een Azure AD-toepassing gemaakt en een service-principal aan de account gekoppeld. De geretourneerde waarden moet u gebruiken om uw toepassing te configureren.

Voordat u het script uitvoert, kunt u de `amsaccount` en `amsResourceGroup` vervangen door de namen die u hebt gekozen bij het maken van deze resources. `amsaccount` is de naam van de Azure Media Services-account waaraan de service-principal moet worden gekoppeld.

De volgende opdracht retourneert een `json`-uitvoer:

```azurecli
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup
```

Met deze opdracht wordt een respons geproduceerd die vergelijkbaar is met de volgende:

```json
{
  "AadClientId": "00000000-0000-0000-0000-000000000000",
  "AadEndpoint": "https://login.microsoftonline.com",
  "AadSecret": "00000000-0000-0000-0000-000000000000",
  "AadTenantId": "00000000-0000-0000-0000-000000000000",
  "AccountName": "amsaccount",
  "ArmAadAudience": "https://management.core.windows.net/",
  "ArmEndpoint": "https://management.azure.com/",
  "Region": "West US 2",
  "ResourceGroup": "amsResourceGroup",
  "SubscriptionId": "00000000-0000-0000-0000-000000000000"
}
```

Als u een `xml` in het antwoord wilt hebben, gebruikt u de volgende opdracht:

```azurecli
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup --xml
```
