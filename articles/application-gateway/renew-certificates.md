---
title: Een Azure Application Gateway-certificaat vernieuwen
description: Informatie over het vernieuwen van een certificaat dat is gekoppeld aan een application gateway-listener.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: article
ms.date: 05/17/2018
ms.author: victorh
ms.openlocfilehash: b125f707e8de17764701e981736a53492e5e756c
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="renew-application-gateway-certificates"></a>Vernieuwen van certificaten voor Application Gateway

Op een bepaald moment moet u uw certificaten vernieuwen, als u uw toepassingsgateway voor SSL-versleuteling geconfigureerd.

U kunt een certificaat dat is gekoppeld aan een listener met de Azure-portal of Azure PowerShell vernieuwen:

## <a name="azure-portal"></a>Azure Portal

Als u wilt een listener-certificaat van de portal wordt verlengd, gaat u naar uw application gateway-listeners. Klik op de listener met een certificaat dat moet worden vernieuwd en klik vervolgens op **vernieuwen of bewerken van de geselecteerde certificaat**.

![Certificaat vernieuwen](media/renew-certificate/ssl-cert.png)

Het nieuwe PFX-certificaat uploaden, een naam geven, typt u het wachtwoord en klik vervolgens op **opslaan**.

## <a name="azure-powershell"></a>Azure PowerShell

Uw om certificaat te vernieuwen met Azure PowerShell, gebruikt u de volgende cmdlet:

```PowerShell
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

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het configureren van SSL-Offloading met Azure Application Gateway, [SSL-Offload configureren](application-gateway-ssl-portal.md)
