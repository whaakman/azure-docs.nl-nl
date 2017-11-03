---
title: End-to-end SSL configureren met Azure Application Gateway | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een end-to-end SSL configureren met Azure Application Gateway met behulp van PowerShell
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: e6d80a33-4047-4538-8c83-e88876c8834e
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: davidmu
ms.openlocfilehash: df14d5c4572a250f9f8951ee3b86e87e6f652782
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-end-to-end-ssl-by-using-application-gateway-with-powershell"></a>End-to-end SSL configureren met behulp van Application Gateway met PowerShell

## <a name="overview"></a>Overzicht

Azure Application Gateway biedt ondersteuning voor end-to-end-versleuteling van verkeer. Toepassingsgateway beëindigt de SSL-verbinding op de toepassingsgateway. De gateway vervolgens de regels voor het doorsturen van toepassing is op het verkeer, reencrypts van het pakket en stuurt het pakket naar de juiste back-end-server op basis van de routeringsregels gedefinieerd. Reacties van de webserver ondergaan hetzelfde proces terug naar de eindgebruiker.

Application Gateway biedt ondersteuning voor het definiëren van aangepaste SSL-opties. Het ondersteunt ook het uitschakelen van de volgende versies van het protocol: **TLSv1.0**, **TLSv1.1**, en **TLSv1.2**, evenals definiëren welke coderingssuites te gebruiken en de volgorde van voorkeur . Zie voor meer informatie over de configureerbare opties voor SSL, de [SSL overzicht](application-gateway-SSL-policy-overview.md).

> [!NOTE]
> SSL 2.0 en SSL 3.0 zijn standaard uitgeschakeld en kan niet worden ingeschakeld. Deze worden beschouwd als niet-beveiligd en kunnen niet worden gebruikt met Application Gateway.

![afbeelding van scenario][scenario]

## <a name="scenario"></a>Scenario

In dit scenario leert u hoe u een toepassingsgateway maken met behulp van end-to-end SSL met PowerShell.

Dit scenario wordt:

* Maak een resourcegroep met de naam **appgw-rg**.
* Maak een virtueel netwerk met de naam **appgwvnet** met een adresruimte van **10.0.0.0/16**.
* Maken van twee subnetten aangeroepen **appgwsubnet** en **appsubnet**.
* Maak een kleine toepassing gateway ondersteunende end-to-end SSL-versleuteling die limieten SSL-protocol versie en coderingssuites.

## <a name="before-you-begin"></a>Voordat u begint

Voor end-to-end SSL met een application gateway configureert, is een certificaat vereist voor de gateway en certificaten zijn vereist voor de back-endservers. Het gatewaycertificaat wordt gebruikt voor het versleutelen en ontsleutelen van het verkeer dat wordt verzonden via SSL. Het gatewaycertificaat moet Personal Information Exchange (PFX)-indeling. Deze bestandsindeling kunt u exporteert de persoonlijke sleutel die is vereist voor de toepassingsgateway om uit te voeren voor de versleuteling en ontsleuteling van verkeer.

Voor end-to-end-SSL-codering moet de back-end goedgekeurde lijst met de toepassingsgateway. U moet het openbare certificaat van de back-endservers uploaden naar de toepassingsgateway. Het certificaat toe te voegen, zorgt u ervoor dat de toepassingsgateway alleen met bekende back-end-exemplaren communiceert. Dit verdere beveiligt de end-to-end communicatie.

Het configuratieproces wordt beschreven in de volgende secties.

## <a name="create-the-resource-group"></a>De resourcegroep maken

Deze sectie helpt u bij het maken van een resourcegroep met de toepassingsgateway.


   1. Aanmelden bij uw Azure-account.

   ```powershell
   Login-AzureRmAccount
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

Het volgende voorbeeld wordt een virtueel netwerk en twee subnets. Eén subnet wordt gebruikt om de toepassingsgateway. Het andere subnet wordt gebruikt voor de back-ends die als host fungeren van de webtoepassing.


   1. Wijs een adresbereik voor het subnet moet worden gebruikt voor de toepassingsgateway.

   ```powershell
   $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24
   ```

   > [!NOTE]
   > Subnetten geconfigureerd voor een application gateway moeten correct worden aangepast. Een application gateway kan worden geconfigureerd voor maximaal 10 exemplaren. Elk exemplaar duurt één IP-adres van het subnet. Te klein van een subnet, kan de uitbreiden van een toepassingsgateway nadelig beïnvloeden.
   > 
   > 

   2. Een adresbereik moet worden gebruikt voor de back-end-adresgroep toewijzen.

   ```powershell
   $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24
   ```

   3. Een virtueel netwerk maken met de subnetten die is gedefinieerd in de voorgaande stappen hebt uitgevoerd.

   ```powershell
   $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
   ```

   4. Het virtuele netwerk en bronnen van het subnet moet worden gebruikt in de stappen volgen ophalen.

   ```powershell
   $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
   $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
   $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet
   ```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Een openbaar IP-adres maken voor de front-endconfiguratie

Een openbaar IP-bron moet worden gebruikt voor de toepassingsgateway maken. Dit openbare IP-adres wordt gebruikt in een van de stappen volgen.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic
```

> [!IMPORTANT]
> Toepassingsgateway biedt geen ondersteuning voor het gebruik van een openbaar IP-adres dat is gemaakt met een domeinlabel gedefinieerde. Alleen een openbaar IP-adres met een domeinlabel dynamisch gemaakte wordt ondersteund. Als u een beschrijvende DNS-naam voor de toepassingsgateway nodig hebt, raden wij dat u een CNAME-record gebruiken als een alias.

## <a name="create-an-application-gateway-configuration-object"></a>Een configuratieobject voor de toepassingsgateway maken

Alle configuratie-items zijn ingesteld voordat u de toepassingsgateway maakt. Volg de onderstaande stappen om de configuratie-items te maken die nodig zijn voor een toepassingsgatewayresource.

   1. Toepassingsgateway een IP-configuratie maken. Deze instelling configureert welke van de subnetten maakt gebruik van de toepassingsgateway. Wanneer de toepassingsgateway wordt geopend, wordt deze een IP-adres van het geconfigureerde subnet opgehaald en routes netwerkverkeer naar de IP-adressen in de back-end-IP-adresgroep. Onthoud dat elk exemplaar één IP-adres gebruikt.

   ```powershell
   $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet
   ```


   2. Maak een front-end-IP-configuratie. Deze instelling wordt een persoonlijke of openbare IP-adres toegewezen aan de front-end van de toepassingsgateway. De volgende stap koppelt het openbare IP-adres in de vorige stap aan de front-end-IP-configuratie.

   ```powershell
   $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip
   ```

   3. De backend-IP-adresgroep configureren met de IP-adressen van de back-end-webservers. Deze IP-adressen zijn de IP-adressen die het netwerkverkeer ontvangen dat afkomstig is van het front-end-IP-eindpunt. De IP-adressen in het voorbeeld vervangen door uw eigen toepassing IP-adres-eindpunten.

   ```powershell
   $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3
   ```

   > [!NOTE]
   > Een volledig gekwalificeerde domeinnaam (FQDN) is ook een geldige waarde voor gebruik in plaats van een IP-adres voor de back-endservers. U het inschakelen met behulp van de **- BackendFqdns** overschakelen. 


   4. Configureer de front-end-IP-poort voor het openbare IP-eindpunt. Dit is de poort waarmee gebruikers verbinding maken.

   ```powershell
   $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443
   ```

   5. Configureer het certificaat voor de toepassingsgateway. Dit certificaat wordt gebruikt om te ontsleutelen en u het verkeer in de toepassingsgateway.

   ```powershell
   $cert = New-AzureRmApplicationGatewaySSLCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>
   ```

   > [!NOTE]
   > Dit voorbeeld configureert u het certificaat voor de SSL-verbinding. Het certificaat moet zich in een pfx-indeling en het wachtwoord moet 4 tot en met 12 tekens.

   6. De HTTP-listener voor de toepassingsgateway maken. Toewijzen aan de front-end-IP-configuratie, de poort en de SSL-certificaat te gebruiken.

   ```powershell
   $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SSLCertificate $cert
   ```

   7. Upload het certificaat moet worden gebruikt op de bronnen van de back-end-pool SSL zijn ingeschakeld.

   > [!NOTE]
   > De standaard-test haalt de openbare sleutel van de *standaard* SSL-binding op de back-end-IP-adres en vergelijkt de waarde voor openbare sleutel wordt ontvangen aan de waarde van de openbare sleutel u hier opgeeft. 
   
   > Als u van hostheaders en Server Name vermelding (SNI) op de back-end gebruikmaakt, is de opgehaalde openbare sleutel mogelijk niet de gewenste site naar welke verkeersstromen. Als u niet zeker bent, gaat u naar https://127.0.0.1/ op de back-endservers om te controleren welk certificaat wordt gebruikt voor de *standaard* SSL-binding. Gebruik de openbare sleutel van de aanvraag die in deze sectie. Als u van hostheaders en SNI op HTTPS-bindingen gebruikmaakt en niet u een antwoord en het certificaat van een aanvraag voor een handmatige browser naar https://127.0.0.1/ op de back-endservers ontvangt, moet u een standaard SSL-binding op de ze instellen. Als u niet doet dit, mislukt de tests en de back-end is niet wilt plaatsen.

   ```powershell
   $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer
   ```

   > [!NOTE]
   > Het certificaat dat is opgegeven in deze stap moet de openbare sleutel van het pfx-certificaat aanwezig is op de back-end. Exporteer het certificaat (niet het basiscertificaat) geïnstalleerd op de back-end-server in de Claim, bewijs en redeneren (CER)-indeling en worden gebruikt in deze stap. Deze stap whitelists de back-end met de toepassingsgateway.

   8. Configureer de HTTP-instellingen voor de toepassing gateway back-end. Toewijzen van het certificaat dat is geüpload in de vorige stap in de HTTP-instellingen.

   ```powershell
   $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert
   ```
   9. Maak een load balancer-routeringsregel waarmee het gedrag van de load balancer worden geconfigureerd. In dit voorbeeld wordt wordt een eenvoudige round robin-regel gemaakt.

   ```powershell
   $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   ```

   10. Configureer de exemplaargrootte van de toepassingsgateway. De beschikbare grootten zijn **standaard\_kleine**, **standaard\_gemiddeld**, en **standaard\_grote**.  De capaciteit, de beschikbare waarden zijn **1** via **10**.

   ```powershell
   $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
   ```

   > [!NOTE]
   > Een aantal exemplaren van 1 worden gekozen voor testdoeleinden. Het is belangrijk te weten dat alle exemplaren onder twee exemplaren niet wordt gedekt door de SLA en wordt daarom niet aanbevolen. Er zijn kleine gateways moet worden gebruikt voor het ontwikkelen testen en niet voor productiedoeleinden.

   11. Configureer het SSL-beleid moet worden gebruikt in de toepassingsgateway. Application Gateway ondersteunt de mogelijkheid om in te stellen de minimumversie voor SSL-protocol versie.

   De volgende waarden zijn een lijst met protocolversies die kunnen worden gedefinieerd:

    - **TLSV1_0**
    - **TLSV1_1**
    - **TLSV1_2**
    
   Het volgende voorbeeld wordt de minimale protocolversie ingesteld op **TLSv1_2** en schakelt **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256**, **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, en **TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** alleen.

   ```powershell
   $SSLPolicy = New-AzureRmApplicationGatewaySSLPolicy -MinProtocolVersion TLSv1_2 -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256"
   ```

## <a name="create-the-application-gateway"></a>De toepassingsgateway maken

Met de bovenstaande stappen maakt u de toepassingsgateway. Het maken van de gateway is een proces dat kan lang duren om uit te voeren.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgateway -SSLCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SSLPolicy $SSLPolicy -AuthenticationCertificates $authcert -Verbose
```

## <a name="limit-ssl-protocol-versions-on-an-existing-application-gateway"></a>SSL-protocol versie op een bestaande toepassingsgateway beperken

De voorgaande stappen duurde u bij het maken van een toepassing met end-to-end-SSL- en uitschakelen van bepaalde versies van SSL-protocol. Bepaalde beleidsregels SSL op een bestaande toepassingsgateway Hiermee schakelt u het volgende voorbeeld.

   1. Ophalen van de toepassingsgateway om bij te werken.

   ```powershell
   $gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG
   ```

   2. Definieer een SSL-beleid. In het volgende voorbeeld **TLSv1.0** en **TLSv1.1** zijn uitgeschakeld en de coderingssuites **TLS\_ECDHE\_ECDSA\_WITH\_ AES\_128\_GCM\_SHA256**, **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, en **TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** zijn de enige zijn toegestaan.

   ```powershell
   Set-AzureRmApplicationGatewaySSLPolicy -MinProtocolVersion -PolicyType Custom -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256" -ApplicationGateway $gw

   ```

   3. Tot slot werkt de gateway. Houd er rekening mee dat deze laatste stap is het een langlopende taak. Wanneer deze is voltooid, wordt end-to-end-SSL geconfigureerd in de toepassingsgateway.

   ```powershell
   $gw | Set-AzureRmApplicationGateway
   ```

## <a name="get-an-application-gateway-dns-name"></a>Ophalen van een application gateway DNS-naam

Nadat de gateway is gemaakt, wordt de volgende stap is het front-end voor de communicatie te configureren. Application Gateway is een dynamisch toegewezen DNS-naam vereist wanneer u een openbaar IP-adres, dit is geen beschrijvende. U kunt een CNAME-record gebruiken om te verwijzen naar het openbare eindpunt van de toepassingsgateway zodat eindgebruikers kunnen de toepassingsgateway bereikt. Zie voor meer informatie [een aangepaste domeinnaam configureren in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Voor het configureren van een alias ophalen van gegevens van de toepassingsgateway en de bijbehorende IP-en DNS-naam met behulp van de **PublicIPAddress** element gekoppeld aan de toepassingsgateway. Gebruik DNS-naam van de toepassingsgateway te maken van een CNAME-record die de twee webtoepassingen aan deze DNS-naam verwijst. We niet raden het gebruik van A-records, omdat het VIP kunt wijzigen op opnieuw starten van de toepassingsgateway.

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

Zie voor meer informatie over het beperken van de beveiliging van uw webtoepassingen met Web Application Firewall via Application Gateway, de [Web application firewall overzicht](application-gateway-webapplicationfirewall-overview.md).

[scenario]: ./media/application-gateway-end-to-end-SSL-powershell/scenario.png
