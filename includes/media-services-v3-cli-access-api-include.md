---
title: bestand opnemen
description: bestand opnemen
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 05/29/2018
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: f4600413e05950446db71f988c4c4302f0dcacb3
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39063118"
---
## <a name="access-the-media-services-api"></a>Toegang kunt krijgen tot de Media Services API

Om verbinding te maken met Azure Media Services API's, gebruikt u de service-principal-verificatie van Azure Active Directory. Met de volgende opdracht wordt een Azure AD-toepassing gemaakt en een service-principal aan de account gekoppeld. De geretourneerde waarden moet u gebruiken om uw toepassing te configureren.

Voordat u het script uitvoert, kunt u de `amsaccount` en `amsResourceGroup` vervangen door de namen die u hebt gekozen bij het maken van deze resources. `amsaccount` is de naam van de Azure Media Services-account waaraan de service-principal moet worden gekoppeld.

De volgende opdracht retourneert een `json`-uitvoer:

```azurecli-interactive
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

```azurecli-interactive
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup --xml
```
