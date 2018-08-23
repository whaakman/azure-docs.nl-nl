---
title: Een Azure Application Gateway-certificaat vernieuwen
description: Leer hoe u een certificaat dat is gekoppeld aan een listener van een application gateway te vernieuwen.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: article
ms.date: 8/15/2018
ms.author: victorh
ms.openlocfilehash: 48bd548ec977d2dc4dd3b5b2f34df04562a6e918
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "42058070"
---
# <a name="renew-application-gateway-certificates"></a>Application Gateway-certificaten vernieuwen

Op een bepaald moment moet u uw certificaten vernieuwen als u uw toepassingsgateway voor SSL-versleuteling hebt geconfigureerd.

U kunt een certificaat dat is gekoppeld aan een listener met de Azure portal, Azure PowerShell of Azure CLI vernieuwen:

## <a name="azure-portal"></a>Azure Portal

Een listener om certificaat te vernieuwen vanuit de portal, gaat u naar uw application gateway-listeners. Klik op de listener met een certificaat dat moet worden vernieuwd en klik vervolgens op **vernieuwen of bewerken van de geselecteerde certificaat**.

![Certificaat vernieuwen](media/renew-certificate/ssl-cert.png)

Het nieuwe PFX-certificaat uploaden, een naam geven, typt u het wachtwoord en klik vervolgens op **opslaan**.

## <a name="azure-powershell"></a>Azure PowerShell

Uw om certificaat te vernieuwen met behulp van Azure PowerShell, gebruikt u het volgende script:

```azurepowershell-interactive
$appgw = Get-AzureRmApplicationGateway `
  -ResourceGroupName <ResourceGroup> `
  -Name <AppGatewayName>

$password = ConvertTo-SecureString `
  -String "<password>" `
  -Force `
  -AsPlainText

set-azureRmApplicationGatewaySSLCertificate -Name <oldcertname> `
-ApplicationGateway $appgw -CertificateFile <newcertPath> -Password $password

Set-AzureRmApplicationGateway -ApplicationGateway $appgw
```
## <a name="azure-cli"></a>Azure-CLI

```azurecli-interactive
az network application-gateway ssl-cert update \
  -n "<CertName>" \
  --gateway-name "<AppGatewayName>" \
  -g "ResourceGroupName>" \
  --cert-file <PathToCerFile> \
  --cert-password "<password>"
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het configureren van SSL-Offloading met Azure Application Gateway, [SSL-Offload configureren](application-gateway-ssl-portal.md)
