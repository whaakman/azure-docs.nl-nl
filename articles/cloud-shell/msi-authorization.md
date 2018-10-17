---
title: Gebruik van beheerde identiteiten voor Azure-resources in Azure Cloud Shell | Microsoft Docs
description: Verifiëren van code met MSI-bestand in Azure Cloud Shell
services: azure
documentationcenter: ''
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/14/2018
ms.author: juluk
ms.openlocfilehash: fe77deeedc34bf769065e34ac2f81d631b0004d6
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49352937"
---
# <a name="use-managed-identities-for-azure-resources-in-azure-cloud-shell"></a>Gebruik van beheerde identiteiten voor Azure-resources in Azure Cloud Shell

Azure Cloud Shell biedt ondersteuning voor autorisatie met beheerde identiteiten voor Azure-resources. Gebruik dit om op te halen toegangstokens om veilig te communiceren met Azure-services.

## <a name="about-managed-identities-for-azure-resources"></a>Informatie over beheerde identiteiten voor Azure-resources
Een algemene vraag bij het bouwen van cloudtoepassingen is het veilig beheren de referenties die moeten worden in de code voor verificatie bij cloudservices. In Cloud Shell moet u mogelijk om te verifiëren voor het ophalen van Key Vault voor een referentie die een script wellicht.

Beheerde identiteiten voor Azure-resources maakt het oplossen van dit probleem eenvoudiger door middel van Azure-services een automatisch beheerde identiteit in Azure Active Directory (Azure AD). U kunt deze identiteit gebruiken voor verificatie bij alle services die ondersteuning bieden voor Azure AD-verificatie, inclusief Key Vault, zonder dat u referenties in uw code hoeft te hebben.

## <a name="acquire-access-token-in-cloud-shell"></a>Toegangstoken in de Cloud Shell verkrijgen

Voer de volgende opdrachten voor het instellen van uw toegangstoken MSI-bestand als een omgevingsvariabele `access_token`.
```
response=$(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s)
access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
echo The MSI access token is $access_token
```

## <a name="handling-token-expiration"></a>Geldigheidsduur van het token verwerken

Het lokale MSI-subsysteem caches tokens. Daarom kunt u deze aanroepen zo vaak als u wilt en een in-the-wire-aanroep naar Azure AD-resultaten alleen als:
- een cache ontbreekt, treedt op omdat er geen token in de cache
- het token is verlopen

Als u het token in uw code in de cache, moet u zijn voorbereid voor het afhandelen van scenario's waarin de resource geeft aan dat het token is verlopen.

Voor het afhandelen van token fouten, gaat u naar de [MSI-pagina over MSI-toegangstokens curling](https://docs.microsoft.com/azure/active-directory/managed-service-identity/how-to-use-vm-token#error-handling).

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over MSI](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview)  
[Verkrijgen van toegangstokens van MSI-VM 's](https://docs.microsoft.com/azure/active-directory/managed-service-identity/how-to-use-vm-token)
