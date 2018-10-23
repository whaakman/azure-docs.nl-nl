---
title: End-to-end SSL configureren met Azure Application Gateway
description: In dit artikel wordt beschreven hoe u end-to-end SSL configureren met Azure Application Gateway met behulp van PowerShell
services: application-gateway
documentationcenter: na
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/23/2018
ms.author: victorh
ms.openlocfilehash: f5b63c7448a502d681bbafb0ac80237a8e2c63c6
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49650091"
---
# <a name="configure-end-to-end-ssl-by-using-application-gateway-with-powershell"></a>End-to-end SSL configureren met behulp van Application Gateway met PowerShell

## <a name="overview"></a>Overzicht

Azure Application Gateway biedt ondersteuning voor end-to-end versleuteling van verkeer. Application Gateway beëindigt de SSL-verbinding bij de application gateway. De gateway vervolgens past de routeringsregels toe op het verkeer, het pakket reencrypts en stuurt het pakket naar de juiste back-end-server op basis van de gedefinieerde routeringsregels. Reacties van de webserver ondergaan hetzelfde proces terug naar de eindgebruiker.

Application Gateway biedt ondersteuning voor het definiëren van aangepaste SSL-opties. Het biedt ook ondersteuning voor het uitschakelen van de volgende protocolversies: **TLSv1.0**, **TLSv1.1**, en **ondersteuning voor TLSv1.2**, evenals definiëren welke coderingssuites te gebruiken en de volgorde van voorkeur . Zie voor meer informatie over configuratieopties voor SSL, de [overzicht van de SSL-beleid](application-gateway-SSL-policy-overview.md).

> [!NOTE]
> SSL 2.0 en SSL 3.0 zijn standaard uitgeschakeld en kan niet worden ingeschakeld. Ze worden beschouwd als niet-beveiligd en kunnen niet worden gebruikt met Application Gateway.

![scenario-afbeelding][scenario]

## <a name="scenario"></a>Scenario

In dit scenario leert u hoe u een toepassingsgateway maken met behulp van end-to-end SSL met PowerShell.

In dit scenario wordt:

* Maak een resourcegroep met de naam **appgw-rg**.
* Maak een virtueel netwerk met de naam **appgwvnet** met een adresruimte van **10.0.0.0/16**.
* Twee subnetten met de naam **appgwsubnet** en **appsubnet**.
* Maak een kleine toepassing gateway ondersteunende end-to-end SSL-versleuteling die limieten SSL-protocolversies en coderingssuites.

## <a name="before-you-begin"></a>Voordat u begint

Voor het end-to-end SSL configureren met een application gateway, een certificaat is vereist voor de gateway en certificaten zijn vereist voor de back-endservers. Het gatewaycertificaat wordt gebruikt voor het versleutelen en ontsleutelen van het verkeer dat wordt verzonden naar het via SSL. Het gatewaycertificaat moet zich in Personal Information Exchange (PFX)-indeling. De bestandsindeling kunt u exporteert de persoonlijke sleutel die is vereist voor de toepassingsgateway om uit te voeren van de versleuteling en ontsleuteling van verkeer.

Voor end-to-end SSL-versleuteling moet de back-end in de whitelist opgenomen met de application gateway. U moet het openbare certificaat van de back-endservers uploaden naar de application gateway. Het certificaat toe te voegen, zorgt u ervoor dat de application gateway communiceert alleen met bekende back-end-exemplaren. Dit verder beveiligt de end-to-end communicatie.

Het configuratieproces wordt beschreven in de volgende secties.

## <a name="create-the-resource-group"></a>De resourcegroep maken

In deze sectie helpt u bij het maken van een resourcegroep met de application gateway.


   1. Meld u aan bij uw Azure-account.

   ```powershell
   Connect-AzureRmAccount
   ```


   2. Selecteer het abonnement moet worden gebruikt voor dit scenario.

   ```powershell
   Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
   ```


   3. Maak een resourcegroep. (Sla deze stap over als u een bestaande resourcegroep gebruikt.)

   ```powershell
   New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
   ```

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Een virtueel netwerk en een subnet maken voor de toepassingsgateway

Het volgende voorbeeld wordt een virtueel netwerk en twee subnetten. Een subnet wordt gebruikt voor het opslaan van de toepassingsgateway. Het andere subnet wordt gebruikt voor de back-ends die als host de web-App fungeren.


   1. Een adresbereik voor het subnet moet worden gebruikt voor de toepassingsgateway toewijst.

   ```powershell
   $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24
   ```

   > [!NOTE]
   > Subnetten die zijn geconfigureerd voor een application gateway moeten correct worden aangepast. Een application gateway kan worden geconfigureerd voor maximaal 10 exemplaren. Elk exemplaar wordt één IP-adres van het subnet. Te klein van een subnet kan nadelige invloed heeft op een application gateway horizontaal schalen.
   > 
   > 

   2. Een adresbereik moet worden gebruikt voor de back-end-adresgroep toewijzen.

   ```powershell
   $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24
   ```

   3. Een virtueel netwerk maken met de subnetten die zijn gedefinieerd in de voorgaande stappen.

   ```powershell
   $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
   ```

   4. Het ophalen van de virtueelnetwerkresource en bronnen van het subnet moet worden gebruikt in de volgende stappen.

   ```powershell
   $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
   $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
   $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet
   ```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Een openbaar IP-adres maken voor de front-endconfiguratie

Een openbare IP-resource moet worden gebruikt voor de toepassingsgateway maakt. Dit openbare IP-adres wordt gebruikt in een van de volgende stappen.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic
```

> [!IMPORTANT]
> Application Gateway biedt geen ondersteuning voor het gebruik van een openbaar IP-adres dat is gemaakt met een gedefinieerde domeinlabel. Alleen een openbaar IP-adres met een dynamisch gemaakte domeinlabel wordt ondersteund. Als u een aangepaste DNS-naam voor de application gateway nodig hebt, raden wij dat u een CNAME-record gebruiken als een alias.

## <a name="create-an-application-gateway-configuration-object"></a>Een configuratieobject voor de toepassingsgateway maken

Alle configuratie-items zijn ingesteld voordat u de toepassingsgateway maakt. Volg de onderstaande stappen om de configuratie-items te maken die nodig zijn voor een toepassingsgatewayresource.

1. Maak een toepassingsgateway IP-configuratie. Deze instelling configureert u welke van de subnetten maakt gebruik van de toepassingsgateway. Wanneer de toepassingsgateway wordt geopend, het neemt een IP-adres van het geconfigureerde subnet en routeert het netwerkverkeer naar het IP-adressen in de back-end-IP-adresgroep. Onthoud dat elk exemplaar één IP-adres gebruikt.

   ```powershell
   $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet
   ```


2. Maak een front-end-IP-configuratie. Deze instelling wordt een particuliere of openbare IP-adres toegewezen aan de front-end van de toepassingsgateway. De volgende stap koppelt het openbare IP-adres in de vorige stap aan de front-end-IP-configuratie.

   ```powershell
   $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip
   ```

3. Configureer de back-end IP-adresgroep met de IP-adressen van de back-end-webservers. Deze IP-adressen zijn de IP-adressen die het netwerkverkeer ontvangen dat afkomstig is van het front-end-IP-eindpunt. De IP-adressen in het voorbeeld vervangen door uw eigen IP-adreseindpunten van toepassing.

   ```powershell
   $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3
   ```

   > [!NOTE]
   > Een volledig gekwalificeerde domeinnaam (FQDN) is ook een geldige waarde moet worden gebruikt in plaats van een IP-adres voor de back-endservers. U dit inschakelen met behulp van de **- BackendFqdns** overschakelen. 


4. Configureer de front-end-IP-poort voor het openbare IP-eindpunt. Dit is de poort die eindgebruikers kunnen verbinding met maken.

   ```powershell
   $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443
   ```

5. Configureer het certificaat voor de toepassingsgateway. Dit certificaat wordt gebruikt om te ontsleutelen en u het verkeer op de application gateway.

   ```powershell
   $passwd = ConvertTo-SecureString  <certificate file password> -AsPlainText -Force 
   $cert = New-AzureRmApplicationGatewaySSLCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password $passwd 
   ```

   > [!NOTE]
   > Hiermee configureert u het certificaat voor de SSL-verbinding. Het certificaat moet zich in een pfx-indeling en het wachtwoord moet 4 tot en met 12 tekens.

6. Maken van de HTTP-listener voor de toepassingsgateway. Toewijzen aan de front-end-IP-configuratie, poort en SSL-certificaat te gebruiken.

   ```powershell
   $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SSLCertificate $cert
   ```

7. Upload het certificaat moet worden gebruikt voor de resources van de back-end-adrespool SSL is ingeschakeld.

   > [!NOTE]
   > De standaard-test haalt de openbare sleutel van de *standaard* SSL-binding op de back-end IP-adres en vergelijkt de waarde voor openbare sleutel wordt ontvangen op de waarde van de openbare sleutel u hier opgeeft. 
   
   > Als u van hostheaders en Servernaamindicatie (SNI) op de back-end gebruikmaakt, is de opgehaalde openbare sleutel mogelijk niet de gewenste site aan welke verkeersstromen. Als u niet zeker bent, gaat u naar https://127.0.0.1/ op de back-endservers om te bevestigen welk certificaat wordt gebruikt voor de *standaard* SSL-binding. Gebruik de openbare sleutel van de aanvraag die in deze sectie. Als u van host-headers en SNI op HTTPS-bindingen gebruikmaakt en u geen ontvangt een reactie en een certificaat van de aanvraag van een handmatige browser aan https://127.0.0.1/ op de back-end-servers, moet u een standaard SSL-binding op deze instellen. Als u niet doet dit, mislukt de tests en de back-end is niet opgenomen in de whitelist.

   ```powershell
   $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer
   ```

   > [!NOTE]
   > Het certificaat dat is opgegeven in deze stap moet de openbare sleutel van het pfx-certificaat aanwezig zijn op de back-end. Exporteer het certificaat (niet het basiscertificaat) geïnstalleerd op de server back-end in de Claim, bewijs en redeneren (CER)-indeling en worden gebruikt in deze stap. Deze stap accounttoewijzing de back-end met de application gateway.

   Als u van de v2-SKU van Application Gateway gebruikmaakt, maakt u een vertrouwd basiscertificaat in plaats van een certificaat voor clientverificatie. Zie voor meer informatie, [overzicht van end-to-end SSL met Application Gateway](ssl-overview.md#end-to-end-ssl-with-the-v2-sku):

   ```powershell
   $trustedRootCert01 = New-AzureRmApplicationGatewayTrustedRootCertificate -Name "test1" -CertificateFile  <path to root cert file>
   ```

8. Configureer de HTTP-instellingen voor de application gateway back-end. Het certificaat is geüpload in de vorige stap in de HTTP-instellingen toewijzen.

   ```powershell
   $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert
   ```

   Gebruik de volgende opdracht uit voor de v2-SKU van Application Gateway:

   ```powershell
   $poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name “setting01” -Port 443 -Protocol Https -CookieBasedAffinity Disabled -TrustedRootCertificate $trustedRootCert01 -HostName "test1"
   ```

9. Maak een load balancer-routeringsregel waarmee het gedrag van de load balancer worden geconfigureerd. In dit voorbeeld wordt een eenvoudige round robin-regel gemaakt.

   ```powershell
   $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   ```

10. Configureer de exemplaargrootte van de toepassingsgateway. De beschikbare grootten zijn **Standard\_kleine**, **Standard\_gemiddeld**, en **Standard\_grote**.  Voor de capaciteit, de beschikbare waarden zijn **1** via **10**.

    ```powershell
    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
    ```

    > [!NOTE]
    > Het aantal instanties van 1 kan worden gekozen voor testdoeleinden. Het is belangrijk te weten dat een aantal instanties onder twee exemplaren niet wordt gedekt door de SLA en wordt daarom niet aanbevolen. Kleine gateways die moeten worden gebruikt voor dev-test en niet voor productiedoeleinden.

11. Het SSL-beleid moet worden gebruikt voor de toepassingsgateway configureren. Application Gateway ondersteunt de mogelijkheid om in te stellen van een minimumversie voor SSL-protocolversies.

   De volgende waarden zijn een lijst met protocolversies die kunnen worden gedefinieerd:

    - **TLSV1_0**
    - **TLSV1_1**
    - **TLSV1_2**
    
   Het volgende voorbeeld wordt de minimale protocolversie ingesteld op **TLSv1_2** en kunnen **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256**, **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, en **TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** alleen.

   ```powershell
   $SSLPolicy = New-AzureRmApplicationGatewaySSLPolicy -MinProtocolVersion TLSv1_2 -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256"
   ```

## <a name="create-the-application-gateway"></a>De toepassingsgateway maken

Met de voorgaande stappen maakt u de toepassingsgateway. Het maken van de gateway is een proces dat kan lang duren om uit te voeren.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgateway -SSLCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SSLPolicy $SSLPolicy -AuthenticationCertificates $authcert -Verbose
```

## <a name="limit-ssl-protocol-versions-on-an-existing-application-gateway"></a>SSL-protocolversies op een bestaande toepassingsgateway beperken

De voorgaande stappen vond u bij het maken van een toepassing met end-to-end SSL en bepaalde versies van SSL-protocol uitschakelen. Het volgende voorbeeld wordt bepaald SSL-beleid op een bestaande toepassingsgateway uitgeschakeld.

   1. Ophalen van de toepassingsgateway om bij te werken.

   ```powershell
   $gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG
   ```

   2. Definieer een SSL-beleid. In het volgende voorbeeld **TLSv1.0** en **TLSv1.1** zijn uitgeschakeld en de coderingssuites **TLS\_ECDHE\_ECDSA\_WITH\_ AES\_128\_GCM\_SHA256**, **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, en **TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** wordt alleen zijn toegestaan.

   ```powershell
   Set-AzureRmApplicationGatewaySSLPolicy -MinProtocolVersion TLSv1_2 -PolicyType Custom -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256" -ApplicationGateway $gw

   ```

   3. Werk tot slot de gateway. Houd er rekening mee dat deze laatste stap is het een langlopende taak. Wanneer dit is voltooid, is op de application gateway end-to-end-SSL geconfigureerd.

   ```powershell
   $gw | Set-AzureRmApplicationGateway
   ```

## <a name="get-an-application-gateway-dns-name"></a>Ophalen van een DNS-naam van application gateway

Nadat de gateway is gemaakt, wordt de volgende stap is het configureren van de front-end voor communicatie. Application Gateway is een dynamisch toegewezen DNS-naam vereist bij het gebruik van een openbaar IP-adres, die is niet gebruiksvriendelijk. Om te zorgen dat eindgebruikers de toepassingsgateway kunnen bereiken, kunt u een CNAME-record gebruiken om te verwijzen naar het openbare eindpunt van de toepassingsgateway. Zie voor meer informatie, [een aangepaste domeinnaam configureren in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Als u wilt configureren een alias, haalt u details van de toepassingsgateway en de bijbehorende IP-/ DNS-naam met behulp van de **PublicIPAddress** element gekoppeld aan de toepassingsgateway. Gebruik DNS-naam van de toepassingsgateway te maken van een CNAME-record die de twee webtoepassingen naar deze DNS-naam wijst. We geen raden het gebruik van A-records, omdat het VIP kan veranderen op opnieuw opstarten van de toepassingsgateway.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : appgw-RG
Location                 : westus
Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het beperken van de beveiliging van uw webtoepassingen met Web Application Firewall via de Application Gateway, de [overzicht van Web application firewall](application-gateway-webapplicationfirewall-overview.md).

[scenario]: ./media/application-gateway-end-to-end-SSL-powershell/scenario.png
