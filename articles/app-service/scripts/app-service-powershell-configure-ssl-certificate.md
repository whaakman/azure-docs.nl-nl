---
title: Azure PowerShell-Script voorbeeld - Bind een aangepaste SSL-certificaat voor een web-app | Microsoft Docs
description: Azure PowerShell-Script voorbeeld - Bind een aangepaste SSL-certificaat voor een web-app
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 23e83b74-614a-49a0-bc08-7542120eeec5
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 851b172cd9218c9ade692e4c9e50a59b4b677ac5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="bind-a-custom-ssl-certificate-to-a-web-app"></a>Een aangepaste SSL-certificaat binden aan een web-app

Dit voorbeeldscript wordt een web-app in App Service gemaakt met de bijbehorende resources en vervolgens koppelt het SSL-certificaat van een aangepaste domeinnaam aan deze. 

Installeer zo nodig de Azure PowerShell met de instructie gevonden in de [Azure PowerShell handleiding](/powershell/azure/overview), en voer vervolgens `Login-AzureRmAccount` geen verbinding maken met Azure. Controleer ook of:

- Een verbinding met Azure is gemaakt met behulp van de `az login` opdracht.
- U hebt toegang tot uw domeinregistrar DNS-configuratiepagina.
- U hebt een geldig. PFX-bestand en het bijbehorende wachtwoord voor het SSL-certificaat dat u wilt uploaden en binden.

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om de resourcegroep, web-app en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Nieuwe AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Maakt een resourcegroep waarin alle resources worden opgeslagen. |
| [Nieuwe AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | Hiermee maakt u een App Service-abonnement. |
| [Nieuwe AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Hiermee maakt u een web-app. |
| [Set-AzureRmAppServicePlan](/powershell/module/azurerm.websites/set-azurermappserviceplan) | Hiermee wijzigt u een App Service-abonnement als de prijscategorie wilt wijzigen. |
| [Set-AzureRmWebApp](/powershell/module/azurerm.websites/set-azurermwebapp) | Een web-app-configuratie wijzigen. |
| [Nieuwe AzureRmWebAppSSLBinding](/powershell/module/azurerm.websites/new-azurermwebappsslbinding) | Hiermee maakt u een SSL-certificaat-binding voor een web-app. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).

Aanvullende voorbeelden van de Azure Powershell voor Azure App Service Web Apps kunnen worden gevonden in de [voorbeelden van Azure PowerShell](../app-service-powershell-samples.md).
