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
ms.openlocfilehash: 90200f7be6c71346441922365fc4439111dd8701
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57314514"
---
# <a name="renew-application-gateway-certificates"></a>Application Gateway-certificaten vernieuwen

Op een bepaald moment moet u uw certificaten vernieuwen als u uw toepassingsgateway voor SSL-versleuteling hebt geconfigureerd.

U kunt een certificaat dat is gekoppeld aan een listener met de Azure portal, Azure PowerShell of Azure CLI vernieuwen:

## <a name="azure-portal"></a>Azure Portal

Een listener om certificaat te vernieuwen vanuit de portal, gaat u naar uw application gateway-listeners. Klik op de listener met een certificaat dat moet worden vernieuwd en klik vervolgens op **vernieuwen of bewerken van de geselecteerde certificaat**.

![Certificaat vernieuwen](media/renew-certificate/ssl-cert.png)

Het nieuwe PFX-certificaat uploaden, een naam geven, typt u het wachtwoord en klik vervolgens op **opslaan**.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Uw om certificaat te vernieuwen met behulp van Azure PowerShell, gebruikt u het volgende script:

```azurepowershell-interactive
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName <ResourceGroup> `
  -Name <AppGatewayName>

$password = ConvertTo-SecureString `
  -String "<password>" `
  -Force `
  -AsPlainText

set-AzApplicationGatewaySSLCertificate -Name <oldcertname> `
-ApplicationGateway $appgw -CertificateFile <newcertPath> -Password $password

Set-AzApplicationGateway -ApplicationGateway $appgw
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
