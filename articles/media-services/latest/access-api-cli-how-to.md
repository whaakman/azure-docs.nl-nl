---
title: Toegang tot de API van Azure Media Services - CLI 2.0 | Microsoft Docs
description: Volg de stappen van deze instructies voor toegang tot de API van Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: cflower
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: mvc
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: a4a7c59e93b860245d67695de90fbae2becac3e9
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="access-azure-media-services-api-with-cli-20"></a>Toegang tot Azure mediaservices API met CLI 2.0
 
Verbinding maken met de API van Azure Media Services, moet u de verificatie van Azure AD-service-principal gebruiken. Uw toepassing moet om aan te vragen van een Azure AD-token met de volgende parameters:

* Azure AD-tenant-eindpunt
* Media Services resource-URI
* Resource-URI voor de REST mediaservices
* Waarden van Azure AD-toepassing: de client-ID en clientgeheim

In dit artikel leest u hoe CLI 2.0 gebruiken als u wilt een Azure AD-toepassing en service principal te maken en de waarden die nodig zijn voor toegang tot resources met Azure Media Services.

## <a name="prerequisites"></a>Vereisten 

Maak een nieuw Azure Media Services-account, zoals beschreven in [deze snelstartgids](create-account-cli-quickstart.md).

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij de [Azure-portal](http://portal.azure.com) en start **CloudShell** CLI-opdrachten uitvoeren, zoals wordt weergegeven in de volgende stappen.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om te zien welke versie u hebt. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een bestand Stream](stream-files-dotnet-quickstart.md)
