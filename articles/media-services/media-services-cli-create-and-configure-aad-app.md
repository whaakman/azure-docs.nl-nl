---
title: CLI 2.0 gebruikt in een Azure AD-app maken en configureren voor toegang tot Azure Media Services-API | Microsoft Docs
description: Dit onderwerp leest hoe u met CLI 2.0 een Azure AD-app maken en configureren voor toegang tot API voor Azure Media Services.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: 01a2bb6d99776feec936315bc882c3097ce832d4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-cli-20-to-create-an-aad-app-and-configure-it-to-access-azure-media-services-api"></a>CLI 2.0 gebruiken ter een AAD-app maken en configureren voor toegang tot Azure Media Services-API

Dit onderwerp leest u hoe een Azure Active Directory (Azure AD)-toepassing en service-principal te krijgen tot bronnen in Azure Media Services maken met CLI 2.0. 

## <a name="prerequisites"></a>Vereisten

- Een Azure-account. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie. 
- Een Media Services-account. Zie voor meer informatie [een Azure Media Services-account maken met de Azure-portal](media-services-portal-create-account.md).

## <a name="use-the-azure-cloud-shell"></a>De Azure-Cloud-Shell gebruiken

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Start de Cloud-Shell van het bovenste navigatievenster van de portal.

    ![Cloud Shell](./media/media-services-cli-create-and-configure-aad-app/media-services-cli-create-and-configure-aad-app01.png) 

Zie voor meer informatie [overzicht van Azure Cloud Shell](../cloud-shell/overview.md).

## <a name="create-an-azure-ad-app-and-configure-access-to-the-media-account-with-cli-20"></a>Een Azure AD-app maken en configureren van toegang tot het account van de media met CLI 2.0
 
```azurecli
az login
az ad sp create-for-rbac --name <appName> --password <strong password>
az role assignment create -- assignee < user/app id> --role Contributor --scope <subscription/subscription id>
```

Bijvoorbeeld:

```azurecli
az role assignment create --assignee a3e068fa-f739-44e5-ba4d-ad57866e25a1 --role Contributor --scope /subscriptions/0b65e280-7917-4874-9fed-1307f2615ea2/resourceGroups/Default-AzureBatch-SouthCentralUS/providers/microsoft.media/mediaservices/sbbash
```

In dit voorbeeld wordt de **bereik** is het pad van de volledige resource voor de media services-account. Echter, de **bereik** kan zijn op elk niveau.

Bijvoorbeeld, wordt een van de volgende niveaus:
 
* De **abonnement** niveau.
* De **resourcegroep** niveau.
* De **resource** niveau (bijvoorbeeld een account Media).

Zie voor meer informatie [een Azure-service-principal maken met Azure CLI 2.0](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)

Zie ook [Manage Role-Based toegangsbeheer met de Azure-opdrachtregelinterface](../active-directory/role-based-access-control-manage-access-azure-cli.md). 

## <a name="next-steps"></a>Volgende stappen

Aan de slag met [bestanden uploaden naar uw account](media-services-portal-upload-files.md).
