---
title: Identiteit van object-id's voor de verificatie - API van Azure voor FHIR zoeken
description: In dit artikel wordt uitgelegd hoe u de id-object-id's die nodig zijn voor verificatie configureren voor Azure-API voor FHIR zoeken
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: b120b21a89a7105a25ba610402da99f9dce4b7e1
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824101"
---
# <a name="find-identity-object-ids-for-authentication-configuration"></a>Identiteit object-id's vinden voor de verificatieconfiguratie

In dit artikel leert u over het vinden van identiteit object-id's die nodig zijn voor het configureren van de lijst met toegestane identiteit object-id's voor de API van Azure voor FHIR.

De volledig beheerde Azure-API voor FHIR&reg; service is geconfigureerd voor toegang voor alleen een vooraf gedefinieerde lijst met identiteit object-id's. Wanneer een toepassing of een gebruiker probeert te krijgen tot de FHIR-API, kan een bearer-token moet worden weergegeven. Deze bearer-token heeft bepaalde claims (velden). Als u wilt verlenen toegang tot de FHIR-API, mag het token de juiste verlener (`iss`), doelgroep (`aud`), en een object-ID (`oid`) uit een lijst met toegestane object-id's. Een identiteit object-ID is de object-ID van een gebruiker of een service-principal in Azure Active Directory.

## <a name="configure-list-of-allowed-object-ids"></a>Lijst met toegestane object-id's configureren

Wanneer u een nieuwe Azure-API voor FHIR-exemplaar maakt, kunt u een lijst met toegestane object-id's configureren:

![Toegestane object-id's configureren](media/quickstart-paas-portal/configure-allowed-oids.png)

Deze object-id's kunnen id's zijn voor specifieke gebruikers of service-principals in uw Azure Active Directory.

## <a name="find-user-object-id-using-powershell"></a>Object-ID van een gebruiker met behulp van PowerShell zoeken

Als u een gebruiker met de gebruikersnaam van de hebt `myuser@consoso.com`, kunt u de gebruikers vinden `ObjectId` met behulp van de volgende PowerShell-opdracht:

```azurepowershell-interactive
$(Get-AzureADUser -Filter "UserPrincipalName eq 'myuser@consoso.com'").ObjectId
```

of u de Azure CLI kunt gebruiken:

```azurecli-interactive
az ad user show --upn-or-object-id myuser@consoso.com | jq -r .objectId
```

## <a name="find-service-principal-object-id-using-powershell"></a>Service-principal-object-ID met behulp van PowerShell vinden

Stel dat u zich hebt geregistreerd een [service client-app](register-service-azure-ad-client-app.md) en u wilt toestaan dat deze serviceclient toegang heeft tot de API van Azure voor FHIR, vindt u de object-ID voor de client service-principal met de volgende PowerShell-opdracht:

```azurepowershell-interactive
$(Get-AzureADServicePrincipal -Filter "AppId eq 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX'").ObjectId
```

waar `XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX` is de service-client toepassings-ID. U kunt ook kunt u de `DisplayName` van de service-client:

```azurepowershell-interactive
$(Get-AzureADServicePrincipal -Filter "DisplayName eq 'testapp'").ObjectId
```

Als u de Azure CLI gebruikt, kunt u het volgende gebruiken:

```azurecli-interactive
az ad sp show --id XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX | jq -r .objectId
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over het vinden van identiteit object-id's die nodig zijn voor het configureren van de identiteiten die zijn toegestaan voor toegang tot een Azure-API voor FHIR-instantie. Naast een volledig beheerde Azure-API voor FHIR implementeren:
 
>[!div class="nextstepaction"]
>[Open Source-FHIR-server implementeren](fhir-paas-portal-quickstart.md)