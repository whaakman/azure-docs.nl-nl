---
title: Gebruik van MSI in Azure-Cloud-Shell | Microsoft Docs
description: Code die een MSI in Azure Cloud-Shell verifiëren
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
ms.openlocfilehash: 99577faf7328dc773a9da5f7c1227aa63600aa0a
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="use-msi-in-azure-cloud-shell"></a>MSI in de Azure-Cloud-Shell gebruiken

Azure Cloud-Shell ondersteunt autorisatie met beheerde Service identiteiten (MSI). Gebruik dit om op te halen toegangstokens beveiligde communicatie met Azure-services.

## <a name="about-managed-service-identity-msi"></a>Over beheerde Service-identiteit (MSI)
Een algemene uitdaging wanneer cloud-toepassingen bouwen veilig beheren de referenties die moeten worden in uw code voor verificatie bij de cloud-services. In de Cloud-Shell wellicht moet u voor het ophalen van de Sleutelkluis voor een referentie die een script wellicht te verifiëren.

Beheerde Service-identiteit (MSI) maakt het oplossen van dit probleem eenvoudiger door middel van een identiteit automatisch beheerde Azure-services in Azure Active Directory (Azure AD). U kunt deze identiteit gebruiken om alle services die ondersteuning biedt voor Azure AD-verificatie, met inbegrip van de Sleutelkluis, zonder dat u geen referenties hoeft in uw code te verifiëren.

## <a name="acquire-access-token-in-cloud-shell"></a>Toegangstoken in de Cloud-Shell verkrijgen

Voer de volgende opdrachten in te stellen uw toegangstoken MSI als een omgevingsvariabele `access_token`.
```
response=$(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s)
access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
echo The MSI access token is $access_token
```

## <a name="handling-token-expiration"></a>Verlopen van het token verwerken

Het lokale MSI-subsysteem caches tokens. Daarom kunt u deze aanroepen zo vaak als u wilt en een aanroep van de kabel naar Azure AD resulteert alleen als:
- een cache ontbreekt deze gebeurtenis treedt op omdat het geen token in de cache
- het token is verlopen

Als u het token in uw code cache, moet u zijn voorbereid voor het afhandelen van scenario's waar de resource geeft aan dat het token is verlopen.

Voor het afhandelen van token fouten, gaat u naar de [MSI pagina over MSI-toegangstokens curling](https://docs.microsoft.com/azure/active-directory/managed-service-identity/how-to-use-vm-token#error-handling).

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over MSI](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview)  
[Toegangstokens van MSI VM's ophalen](https://docs.microsoft.com/azure/active-directory/managed-service-identity/how-to-use-vm-token)
