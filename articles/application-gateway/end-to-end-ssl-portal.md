---
title: Snelstartgids - end-to-end SSL-versleuteling configureren met Azure Application Gateway - Azure portal | Microsoft Docs
description: Informatie over het gebruik van de Azure-portal een Azure Application Gateway maken met end-to-end SSL-versleuteling.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 3/19/2019
ms.author: absha
ms.custom: mvc
ms.openlocfilehash: e47a3e1231701f3339057e25ee4388aff0c9fbd7
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58227595"
---
# <a name="configure-end-to-end-ssl-by-using-application-gateway-with-the-portal"></a>End-to-end SSL configureren met behulp van Application Gateway met de portal

In dit artikel leest u hoe de Azure-portal gebruiken om te configureren van end-to-end SSL-versleuteling met een toepassingsgateway v1-SKU.  

> [!NOTE]
> Application Gateway v2-SKU vereist vertrouwde basiscertificaten voor inschakelen end-to-end-configuratie. Portal ondersteuning voor het toevoegen van vertrouwde basiscertificaten is nog niet beschikbaar. Daarom in het geval van V2 SKU ziet [end-to-end SSL met behulp van PowerShell configureren](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="before-you-begin"></a>Voordat u begint

Voor het end-to-end SSL configureren met een application gateway, een certificaat is vereist voor de gateway en certificaten zijn vereist voor de back-endservers. Het gatewaycertificaat wordt gebruikt voor het afleiden van een symmetrische sleutel volgens de specificatie van de SSL-protocol. De symmetrische sleutel wordt vervolgens gebruikt voor het versleutelen en ontsleutelen van het verkeer dat wordt verzonden naar de gateway. Voor end-to-end SSL-versleuteling moet de back-end in de whitelist opgenomen met de application gateway. U doet dit door het openbare certificaat van de back-endservers, ook wel bekend als certificaten voor serververificatie te uploaden naar de toepassingsgateway. Het certificaat toe te voegen, zorgt u ervoor dat de application gateway communiceert alleen met bekende back-end-exemplaren. Dit verder beveiligt de end-to-end communicatie.

Zie voor meer informatie, [SSL-beëindiging en end-to-end SSL](https://docs.microsoft.com/azure/application-gateway/ssl-overview).

## <a name="create-a-new-application-gateway-with-end-to-end-ssl"></a>Een nieuwe toepassingsgateway maken met end-to-end SSL

Als u wilt een nieuwe toepassingsgateway maken met end-to-end SSL-versleuteling, moet u SSL-beëindiging tijdens het maken van een nieuwe toepassingsgateway voor het eerst inschakelt. Hiermee schakelt u SSL-versleuteling voor de communicatie tussen de gateway van de client en de toepassing. U wordt vervolgens goedgekeurde certificaten voor back-endservers in de HTTP-instellingen wilt SSL-versleuteling voor de communicatie tussen de gateway en back-end-toepassingsservers, waardoor het uitvoeren van end-to-end SSL-versleuteling inschakelen.

### <a name="enable-ssl-termination-while-creating-a-new-application-gateway"></a>SSL-beëindiging tijdens het maken van een nieuwe toepassingsgateway inschakelen

Raadpleeg dit artikel om te begrijpen hoe u [inschakelen van SSL-beëindiging tijdens het maken van een nieuwe toepassingsgateway](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal).

### <a name="whitelist-certificates-for-backend-servers"></a>Goedgekeurde certificaten voor back-endservers

1. Selecteer **Alle resources** en vervolgens **myAppGateway**.

2. Selecteer **HTTP-instellingen** in het menu links. Azure automatisch een standaard-HTTP-instelling, gemaakt **appGatewayBackendHttpSettings**, tijdens het maken van de toepassingsgateway. 

3. Select **appGatewayBackendHttpSettings**.

4. Onder **Protocol**, selecteer **HTTPS**. Een deelvenster voor **verificatiecertificaten back-end** wordt weergegeven. 

5. Onder **verificatiecertificaten back-end**, kiest u **nieuw**.

6. Voer geschikt **naam**.

7. Upload het certificaat met behulp van de **uploaden CER-certificaat** vak.![ addcert](./media/end-to-end-ssl-portal/addcert.png)

   > [!NOTE]
   > Het certificaat dat is opgegeven in deze stap moet de openbare sleutel van het pfx-certificaat aanwezig zijn op de back-end. Exporteer het certificaat (niet het basiscertificaat) geïnstalleerd op de server back-end in de Claim, bewijs en redeneren (CER)-indeling en worden gebruikt in deze stap. Deze stap accounttoewijzing de back-end met de application gateway.

8. Selecteer **Opslaan**.

## <a name="enable-end-to-end-ssl-for-existing-application-gateway"></a>End-to-end SSL voor bestaande toepassingsgateway inschakelen

Als u wilt een bestaande toepassingsgateway configureren met end-to-end SSL-versleuteling, moet u eerst inschakelen SSL-beëindiging in de listener. Hiermee schakelt u SSL-versleuteling voor de communicatie tussen de gateway van de client en de toepassing. U wordt vervolgens goedgekeurde certificaten voor back-endservers in de HTTP-instellingen wilt SSL-versleuteling voor de communicatie tussen de gateway en back-end-toepassingsservers, waardoor het uitvoeren van end-to-end SSL-versleuteling inschakelen.

U moet het gebruik van een listener met HTTPS-protocol en certificaat voor het inschakelen van SSL-beëindiging. U kunt het protocol van een bestaande listener niet wijzigen. Daarom kunt u ofwel een listener met HTTPS-protocol en het certificaat gebruikt of maak een nieuwe listener. Wanneer u de vorige kiest, kunt u negeren de hieronder vermelde stappen voor het **inschakelen SSL-beëindiging in bestaande toepassingsgateway** en verplaatst u rechtstreeks naar **goedgekeurde certificaten voor back-endservers** de sectie. Als u de laatste kiest, moet u deze stappen uitvoeren. 

### <a name="enable-ssl-termination-in-existing-application-gateway"></a>SSL-beëindiging in bestaande toepassingsgateway inschakelen

1. Selecteer **Alle resources** en vervolgens **myAppGateway**.

2. Selecteer **Listeners** in het menu links.

3. Kiezen tussen **Basic** en **multi-site** listener volgens uw behoeften.

4. Onder **Protocol**, selecteer **HTTPS**. Een deelvenster voor **certificaat** wordt weergegeven.

5. Upload het PFX-certificaat dat u wilt gebruiken voor SSL-beëindiging tussen de gateway van de client en de toepassing.

   > [!NOTE]
   > U kunt een zelfondertekend certificaat gebruiken voor testdoeleinden. U moet zelf-ondertekend certificaat niet gebruiken voor productieworkloads. Meer informatie over het [een zelfondertekend certificaat maken](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal#create-a-self-signed-certificate).

6. Toevoegen van andere vereiste instellingen voor de **Listener** volgens uw behoeften.

7. Selecteer **OK** om op te slaan.

### <a name="whitelist-certificates-for-backend-servers"></a>Goedgekeurde certificaten voor back-endservers

1. Selecteer **Alle resources** en vervolgens **myAppGateway**.

2. Selecteer **HTTP-instellingen** in het menu links. U kunt een van beide certificaten whitelist in een bestaande back-end HTTP instellen of maak een nieuwe HTTP-instelling. In de onderstaande stap, we whitelist certificaat voor de standaard-HTTP-instelling, wordt **appGatewayBackendHttpSettings**.

3. Select **appGatewayBackendHttpSettings**.

4. Onder **Protocol**, selecteer **HTTPS**. Een deelvenster voor **verificatiecertificaten back-end** wordt weergegeven. 

5. Onder **verificatiecertificaten back-end**, kiest u **nieuw**.

6. Voer geschikt **naam**.

7. Upload het certificaat met behulp van de **uploaden CER-certificaat** vak.![ addcert](./media/end-to-end-ssl-portal/addcert.png)

   > [!NOTE]
   > Het certificaat dat is opgegeven in deze stap moet de openbare sleutel van het pfx-certificaat aanwezig zijn op de back-end. Exporteer het certificaat (niet het basiscertificaat) geïnstalleerd op de server back-end in de Claim, bewijs en redeneren (CER)-indeling en worden gebruikt in deze stap. Deze stap accounttoewijzing de back-end met de application gateway.

8. Selecteer **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Webverkeer met een toepassingsgateway beheren met behulp van Azure CLI](./tutorial-manage-web-traffic-cli.md)
