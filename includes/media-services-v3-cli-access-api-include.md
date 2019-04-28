---
title: bestand opnemen
description: bestand opnemen
services: media-services
author: WenJason
ms.service: media-services
ms.topic: include
origin.date: 05/29/2018
ms.date: 06/25/2018
ms.author: v-nany
ms.custom: include file
ms.openlocfilehash: 4dde0a47f0452da2dd951df86ccb6e02a44521ed
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60309030"
---
## <a name="access-the-media-services-api"></a>Toegang kunt krijgen tot de Media Services API

Om verbinding te maken met Azure Media Services API's, gebruikt u de service-principal-verificatie van Azure Active Directory. Met de volgende opdracht wordt een Azure AD-toepassing gemaakt en een service-principal aan de account gekoppeld. De geretourneerde waarden moet u gebruiken om uw toepassing te configureren.

Voordat u het script uitvoert, kunt u de `amsaccount` en `amsResourceGroup` vervangen door de namen die u hebt gekozen bij het maken van deze resources. `amsaccount` is de naam van de Azure Media Services-account waaraan de service-principal moet worden gekoppeld.

De volgende opdracht retourneert een `json`-uitvoer:

```cli
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup
```

Met deze opdracht wordt een respons geproduceerd die vergelijkbaar is met de volgende:

```json
{
  "AadClientId": "00000000-4cdd-418a-8a72-0755ace03de5",
  "AadEndpoint": "https://login.partner.microsoftonline.cn",
  "AadSecret": "00000000-02f5-4bf2-9057-1c4f7baff155",
  "AadTenantId": "00000000-86f1-41af-91ab-2d7cd011db47",
  "AccountName": "amsaccount22",
  "ArmAadAudience": "https://management.core.chinacloudapi.cn/",
  "ArmEndpoint": "https://management.chinacloudapi.cn/",
  "Region": "chinaeast",
  "ResourceGroup": "amsResourceGroup2",
  "SubscriptionId": "00000000-6753-4ca2-b1ae-193798e2c9d8"
}
```

Als u een `xml` in het antwoord wilt hebben, gebruikt u de volgende opdracht:

```cli
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup --xml
```
