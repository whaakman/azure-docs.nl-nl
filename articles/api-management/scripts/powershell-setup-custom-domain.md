---
title: Voorbeeld van PowerShell-script - aangepast domein instellen | Microsoft Docs
description: Voorbeeld van PowerShell-script - aangepast domein instellen
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.devlang: na
ms.topic: sample
ms.date: 12/14/2017
ms.author: apimpm
ms.custom: mvc
ms.openlocfilehash: 70e0b7ad86d1b75003ac5a62297dd85d4731d4f9
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56726123"
---
# <a name="set-up-custom-domain"></a>Aangepast domein instellen

Met dit voorbeeldscript stelt u het aangepaste domein in op proxy- en portal-eindpunt van de API Management-service.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze zelfstudie versie 1.0 of hoger van de Azure PowerShell-module vereist. Voer `Get-Module -ListAvailable Az` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-Az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/api-management/setup-custom-domain/setup_custom_domain.ps1 "Set up custom domain")]

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) gebruiken om de resourcegroep en alle gerelateerde resources te verwijderen wanneer u ze niet meer nodig hebt.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

[!INCLUDE [api-management-custom-domain](../../../includes/api-management-custom-domain.md)]

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Meer voorbeelden voor Azure PowerShell voor Azure API Management vindt u in [Voorbeelden van PowerShell](../powershell-samples.md).
