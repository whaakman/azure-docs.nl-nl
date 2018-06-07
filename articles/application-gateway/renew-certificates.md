---
title: Een Azure Application Gateway-certificaat vernieuwen
description: Informatie over het vernieuwen van een certificaat dat is gekoppeld aan een application gateway-listener.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: article
ms.date: 05/18/2018
ms.author: victorh
ms.openlocfilehash: b44a57fe8ebcc985d3ab66ea04936a1558d00863
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34598263"
---
# <a name="renew-application-gateway-certificates"></a>Vernieuwen van certificaten voor Application Gateway

Op een bepaald moment moet u uw certificaten vernieuwen, als u uw toepassingsgateway voor SSL-versleuteling geconfigureerd.

U kunt een certificaat dat is gekoppeld aan een listener met de Azure portal, Azure PowerShell of Azure CLI vernieuwen:

## <a name="azure-portal"></a>Azure Portal

Als u wilt een listener-certificaat van de portal wordt verlengd, gaat u naar uw application gateway-listeners. Klik op de listener met een certificaat dat moet worden vernieuwd en klik vervolgens op **vernieuwen of bewerken van de geselecteerde certificaat**.

![Certificaat vernieuwen](media/renew-certificate/ssl-cert.png)

Het nieuwe PFX-certificaat uploaden, een naam geven, typt u het wachtwoord en klik vervolgens op **opslaan**.

## <a name="azure-powershell"></a>Azure PowerShell

Uw om certificaat te vernieuwen met Azure PowerShell, gebruikt u de volgende cmdlet:

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
