---
title: Azure CLI gebruiken voor een Azure AD-app maken en configureren voor toegang tot Azure Media Services API | Microsoft Docs
description: In dit onderwerp laat zien hoe de Azure CLI gebruiken voor een Azure AD-app maken en configureren voor toegang tot de API van Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: fcd0ea10bd39f9e7252e114e8d6401a4fe0ecadb
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35905475"
---
# <a name="use-azure-cli-to-create-an-aad-app-and-configure-it-to-access-azure-media-services-api"></a>Azure CLI gebruiken voor een AAD-app maken en configureren voor toegang tot Azure Media Services API

Dit onderwerp ziet u hoe de Azure CLI gebruiken om te maken van een Azure Active Directory (Azure AD)-toepassing en service-principal toegang krijgen tot bronnen van Azure Media Services. 

## <a name="prerequisites"></a>Vereisten

- Een Azure-account. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie. 
- Een Media Services-account. Zie voor meer informatie, [een Azure Media Services-account maken met de Azure-portal](media-services-portal-create-account.md).

## <a name="use-the-azure-cloud-shell"></a>De Azure Cloudshell gebruiken

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Open Cloud Shell in het bovenste navigatiedeelvenster van de portal.

    ![Cloud Shell](./media/media-services-cli-create-and-configure-aad-app/media-services-cli-create-and-configure-aad-app01.png) 

Zie voor meer informatie, [overzicht van Azure Cloud Shell](../../cloud-shell/overview.md).

## <a name="create-an-azure-ad-app-and-configure-access-to-the-media-account-with-azure-cli"></a>Een Azure AD-app maken en configureren van toegang tot het media-account met Azure CLI
 
```azurecli
az login
az ad sp create-for-rbac --name <appName> --password <strong password>
az role assignment create --assignee < user/app id> --role Contributor --scope <subscription/subscription id>
```

Bijvoorbeeld:

```azurecli
az role assignment create --assignee a3e068fa-f739-44e5-ba4d-ad57866e25a1 --role Contributor --scope /subscriptions/0b65e280-7917-4874-9fed-1307f2615ea2/resourceGroups/Default-AzureBatch-SouthCentralUS/providers/microsoft.media/mediaservices/sbbash
```

In dit voorbeeld wordt de **bereik** is de volledige resource-pad voor de media services-account. Echter, de **bereik** kan zijn op elk niveau.

Bijvoorbeeld, wordt een van de volgende niveaus:
 
* De **abonnement** niveau.
* De **resourcegroep** niveau.
* De **resource** niveau (bijvoorbeeld een Media-account).

Zie voor meer informatie, [een Azure-service-principal maken met de Azure CLI](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)

Zie ook [Manage Role-Based toegangsbeheer met de Azure-opdrachtregelinterface](../../role-based-access-control/role-assignments-cli.md). 

## <a name="next-steps"></a>Volgende stappen

Aan de slag met [bestanden uploaden naar uw account](media-services-portal-upload-files.md).
