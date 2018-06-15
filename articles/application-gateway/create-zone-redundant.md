---
title: De gateway van een zone-redundante Azure-toepassing maken
description: Informatie over het maken van een zone redundant application gateway waarvoor een gateway seperarte in elke zone niet.
services: application-gateway
author: amsriva
manager: jpconnock
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 5/8/2018
ms.author: victorh
ms.openlocfilehash: 03bc58db813534fdd17c9567f6425ab7357ed901
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33937754"
---
# <a name="create-a-zone-redundant-azure-application-gateway---private-preview"></a>Maak een zone-redundante Azure-toepassing-gateway - private preview

De gateway zone redundant toepassingsplatform is een nieuwe SKU die veel verbeteringen via de bestaande toepassing gateway SKU, waaronder biedt:
- **Zone tolerantie** - de implementatie van een toepassing gateway kan meerdere Zones van de beschikbaarheid, opheffen van de noodzaak om in te richten nu omspannen en de afzonderlijke toepassingsgateway pincode-exemplaren in elke zone met een traffic manager. U kunt een zone voor één of meerdere zones waarop de gateway toepassingsexemplaren zijn geïmplementeerd, dus zorgt zone fout tolerantie. De back-endpool voor toepassingen kan op dezelfde manier worden verdeeld over beschikbaarheid zones.
- **Verbeterde prestaties**
- **Statische VIP** -de toepassingsgateway VIP ondersteunt nu het statische VIP-type uitsluitend. Dit zorgt ervoor dat het VIP die zijn gekoppeld aan de toepassingsgateway niet na het opnieuw opstarten wijzigt.
- **Sleutelkluis-integratie voor SSL-klantcertificaten**
- **Snellere implementatie en bijwerken**

> [!NOTE]
> De zone redundant toepassingsgateway is momenteel in Preview-versie private. Dit voorbeeld wordt geleverd zonder een service level agreement, en wordt niet aanbevolen voor productieworkloads. Bepaalde functies mogelijk niet ondersteund of kunnen hebben beperkte mogelijkheden. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="supported-regions"></a>Ondersteunde regio’s

Zone-redundante Toepassingsgateways worden momenteel ondersteund in de regio VS-Oost 2. Meer regio's wordt binnenkort toegevoegd.

## <a name="topologies"></a>Topologieën

Met de huidige release moet u niet langer zone vastgemaakt Toepassingsgateways om zonal redundantie te maken. De implementatie van dezelfde gateway kan meerdere zones nu omspannen.

Ten minste drie exemplaren zijn vereist om ervoor te zorgen dat ze worden verdeeld over alle drie zones. Als u meer exemplaren worden toegevoegd, toepassingsgateway exemplaren verdeelt over zones.

Vorige zone redundant topologieën staande in het volgende diagram:

![Oude redundante topologie](media/create-zone-redundant/old-redundant.png)

De nieuwe zone redundant topologie ziet er in dit diagram:

![Nieuwe zone redundant topologie](media/create-zone-redundant/new-redundant.png)

## <a name="deployment"></a>Implementatie

### <a name="prerequisites"></a>Vereisten

Zone-redundante mogelijkheid is momenteel alleen beschikbaar als private preview. U moet de e- appgwxzone@microsoft.com naar wilt plaatsen. Nadat u een bevestiging ontvangen, kunt u doorgaan met de volgende stappen. De volgende informatie opnemen in uw e-mailadres voor whitelisting:

- Abonnements-id
- Regionaam
- Geschatte aantal Toepassingsgateways vereist

### <a name="resource-manager-template-deployment"></a>Sjabloonimplementatie van Resource Manager

1. Zorg ervoor dat het abonnement dat u wilt plaatsen, zoals eerder opgemerkt.
2. Nadat u een bevestiging ontvangen, meld u aan bij de Azure-account en selecteer het juiste abonnement als meer dan één abonnement aanwezig is. Zorg ervoor dat u het abonnement dat goedgekeurde lijst is selecteert.

   ```PowerShell
   Login-AzureRmAccount

   Select-AzureRmSubscription -Subscription "<whitelisted subscription name>”
   ```
3. Een nieuwe resourcegroep maken

   ``` PowerShell
   New-AzureRmResourceGroup -Name <resource group name> -Location "East US 2"
   ```
4. Downloaden van de sjablonen van [GitHub](https://github.com/amitsriva/CrossZonePreview) en noteer de map waar u opslaat.
5. Maak een nieuwe implementatie in de resourcegroep die u hebt gemaakt. De sjabloon en de parameters-bestand op de juiste wijze wijzigen voordat u implementeert. 

   Het volgende diagram laat zien waar u de tenant-ID in de Azure portal kunt ophalen:

   ![Tenant-ID van de portal](media/create-zone-redundant/tenant-id.png)

De sjabloon wordt gemaakt van de volgende bronnen:

- **Toegewezen gebruikersidentiteit** -dit wordt gebruikt voor het inschakelen van gateway toepassingsexemplaren toegang tot de sleutelkluis en ophalen van de certificaten die zijn opgeslagen door de gebruiker.
- **KeyVault** – de Sleutelkluis waar het certificaat van de gebruiker wordt opgeslagen. Dit kan ook een bestaande Sleutelkluis zijn.
- **Geheim** : de persoonlijke sleutel die is opgeslagen in de Sleutelkluis.
- **Toegangsbeleid** : een toegangsbeleid is toegepast op de Sleutelkluis die machtigingen met toegewezen gebruikers-id verleent zodat gateway toepassingsimplementaties gebruikerscertificaten kunnen ophalen.
- **Openbaar IP-adres** – een gereserveerd IP-adres dat wordt gebruikt voor toegang tot de toepassingsgateway. Dit IP-adres kan nooit worden gewijzigd voor de levenscyclus van de toepassingsgateway.
- **Netwerkbeveiligingsgroepen** – een NSG automatisch gemaakt op het subnet van de toepassingsgateway waarmee netwerkverkeer via de poort op de geconfigureerde listener wordt geopend. Dit is expliciet gemaakt en beheerd in de nieuwe SKU vergeleken met de vorige SKU waarop deze NSG impliciete is.
- **Virtueel netwerk** – de vnet waar de toepassingsgateway en toepassingen worden geïmplementeerd.
- **Toepassingsgateway** – Hiermee maakt u een toepassingsgateway met instanties in de vereiste zones. Standaard worden alle zones (1,2,3) geselecteerd. De SKU-naam is gewijzigd in *Standard_v2*. Deze SKU-naam kan worden gewijzigd. De configuratie voor automatisch schalen heeft momenteel, min en max ingesteld op het vereiste aantal exemplaren. Wanneer automatisch schalen is ingeschakeld, kan dit worden aangepast.

```PowerShell
New-AzureRmResourceGroupDeployment -Name Deployment1 -ResourceGroupName AmitVMSSLinuxTest9 -TemplateFile <complete path to template.json> -TemplateParameterFile <complete path to parameters.json>
```
### <a name="powershell-deployment"></a>PowerShell-implementatie

1. Zorg ervoor dat het abonnement dat u gebruikt wilt plaatsen, zoals eerder vermeld in de vereisten.
2. Download en installeer de persoonlijke PowerShell MSI van [GitHub](https://github.com/amitsriva/CrossZonePreview/blob/master/Azure-Cmdlets-5.7.0.19009-x64.msi).
3. Het persoonlijke PowerShell zip-bestand downloaden vanaf de locatie in het bevestigingsbericht voor preview-registratie genoemd. Pak het bestand naar de schijf en noteer de locatie.
4. Zodra de Preview-versie is ingeschakeld, worden de preview-modules eerst voordat aan te melden bij uw account geladen:

   ```PowerShell
   $azurePSPath = "<complete path to Azure-PowerShell folder>"
   import-module "$azurePSPath\AzureRM.Profile\AzureRM.Profile.psd1"
   import-module "$azurePSPath\Azure.Storage\Azure.Storage.psd1"
   import-module "$azurePSPath\AzureRM.Resources\AzureRM.Resources.psd1"
   import-module "$azurePSPath\AzureRM.Network\AzureRM.Network.psd1"
   import-module "$azurePSPath\AzureRM.KeyVault\AzureRM.KeyVault.psd1"
   ```

4. Meld u aan bij de Azure-account en selecteer het gewenste abonnement als meer dan één abonnement aanwezig is. Zorg ervoor dat u het juiste abonnement dat goedgekeurde lijst is selecteert.
5. Voer de volgende opdrachten tot stand brengen van algemene constanten die namen voor de meeste van de entiteiten die wordt gemaakt. 

   De vermeldingen zoals vereist voor uw voorkeur naming wijzigen.

   ```PowerShell
   $location = "eastus2"
   $version = "300"

   $rgname = "RG_A_$version"
   $appgwName = "AGW_A_$version"
   $vaultName = "KVA$version"
   $userAssignedIdentityName = "UI_A_$version"
   $certificateName = "KVCA$version"
   $nsgName = "NSG_A_$version"
   $vnetName = "VN_A_$version"
   $gwSubnetName = "SN_A_$version"
   $gipconfigname = "GC_A_$version"
   $publicIpName = "PIP_A_$version"
   $fipconfig01Name = "FC_A_$version"
   $poolName = "BP_A_$version"
   $frontendPort01Name = "FP1_A_$version"
   $frontendPort02Name = "FP2_A_$version"
   $poolSetting01Name = "BS_A_$version"
   $listener01Name = "HL1_A_$version"
   $listener02Name = "HL2_A_$version"
   $rule01Name = "RR1_A_$version"
   $rule02Name = "RR2_A_$version"
   $AddressPrefix = "111.111.222.0" 
   ```
6. De resourcegroep maken:

   ```PowerShell
   $resourceGroup = New-AzureRmResourceGroup -Name $rgname -Location $location -Force
   ```
7. Maak toegewezen identiteit van de gebruiker die toegang geven tot de toepassingsgateway wordt gebruikt om certificaten te ontvangen van de Sleutelkluis.

   ```PowerShell
   $userAssignedIdentity = New-AzureRmResource -ResourceGroupName $rgname -Location $location -ResourceName $userAssignedIdentityName -ResourceType Microsoft.ManagedIdentity/userAssignedIdentities -Force
   ```
8. De Sleutelkluis gebruikt voor het opslaan van uw certificaten maken:

   ```PowerShell
   $keyVault = New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $rgname -Location $location -EnableSoftDelete
   ```
9. Upload het certificaat naar de Sleutelkluis als een geheim:

   ```PowerShell
   $securepfxpwd = ConvertTo-SecureString -String "<password>" -AsPlainText -Force

   $cert = Import-AzureKeyVaultCertificate -VaultName $vaultName -Name         $certificateName -FilePath ‘<path to pfx file>'  -Password $securepfxpwd
   ```
10. Toegangsbeleid toewijzen aan de Sleutelkluis met behulp van de gebruikers-id toegewezen. Hiermee kunt de gateway toepassingsexemplaren voor toegang tot de Sleutelkluis geheime:

   ```PowerShell
   Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $rgname -PermissionsToSecrets get -ObjectId $userAssignedIdentity.Properties.principalId
   ```
11. Maken van de Netwerkbeveiligingsgroep (NSG) voor toegang tot het subnet van de gateway op poorten waar de nieuwe listeners worden gemaakt.

    Bijvoorbeeld voor HTTP/HTTPS op standaard poorten het NSG zou inkomende toegang toestaan tot 80, 443 en 65200-65535 voor beheertaken uit te voeren.

   ```PowerShell
   $srule01 = New-AzureRmNetworkSecurityRuleConfig -Name "listeners" -Direction Inbound -SourceAddressPrefix * -SourcePortRange * -Protocol * -DestinationAddressPrefix * -DestinationPortRange 22,80,443 -Access Allow -Priority 100

   $srule02 = New-AzureRmNetworkSecurityRuleConfig -Name "managementPorts" -Direction Inbound -SourceAddressPrefix * -SourcePortRange * -Protocol * -DestinationAddressPrefix * -DestinationPortRange "65200-65535" -Access Allow -Priority 101

   $nsg = New-AzureRmNetworkSecurityGroup -Name $nsgName -ResourceGroupName $rgname -Location $location -SecurityRules $srule01,$srule02 -Force
   ```

12. VNet en subnetten maken:

   ```PowerShell
   $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 
   $gwSubnetName -AddressPrefix "$AddressPrefix/24" -NetworkSecurityGroup $nsg

   $vnet = New-AzureRmvirtualNetwork -Name $vnetName -ResourceGroupName $rgname -Location $location -AddressPrefix "$AddressPrefix/24" -Subnet $gwSubnet -Force
   ```
13. Een openbaar IP-adres van type gereserveerd/statische maken:

   ```PowerShell
   $publicip = New-AzureRmPublicIpAddress -ResourceGroupName $rgname -name $publicIpName -location $location -AllocationMethod Static -Sku Standard -Force
   ```

14. De toepassingsgateway maken:

   ```PowerShell
   $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name $gipconfigname -Subnet $gwSubnet

   $fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name $fipconfig01Name -PublicIPAddress $publicip

   $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name $poolName -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com

   $fp01 = New-AzureRmApplicationGatewayFrontendPort -Name $frontendPort01Name -Port 443

   $fp02 = New-AzureRmApplicationGatewayFrontendPort -Name $frontendPort02Name -Port 80

   $sslCert01 = New-AzureRmApplicationGatewaySslCertificate -Name "SSLCert" -KeyVaultSecretId $secret.Id

   $listener01 = New-AzureRmApplicationGatewayHttpListener -Name $listener01Name -Protocol Https -FrontendIPConfiguration
 $fipconfig01 -FrontendPort $fp01 -SslCertificate $sslCert01

   $listener02 = New-AzureRmApplicationGatewayHttpListener -Name $listener02Name -Protocol Http -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp02

   $poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name $poolSetting01Name -Port 80 -Protocol Http -CookieBasedAffinity Disabled

   $rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name $rule01Name -RuleType basic -BackendHttpSettings $poolSetting01 -HttpListener $listener01 -BackendAddressPool $pool

   $rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name $rule02Name -RuleType basic -BackendHttpSettings $poolSetting01 -HttpListener $listener02 -BackendAddressPool $pool

   $sku = New-AzureRmApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2 -Capacity 2

   $listeners = @($listener02)

   $fps = @($fp01, $fp02)

   $fipconfigs = @($fipconfig01)

   $sslCerts = @($sslCert01)

   $rules = @($rule01, $rule02)

   $listeners = @($listener01, $listener02)

   $appgw = New-AzureRmApplicationGateway -Name $appgwName -ResourceGroupName $rgname -Location $location -UserAssignedIdentityId $userAssignedIdentity.ResourceId -Probes $probeHttps -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting01 -GatewayIpConfigurations $gipconfig -FrontendIpConfigurations $fipconfigs -FrontendPorts $fps -HttpListeners $listeners -RequestRoutingRules $rules -Sku $sku -SslPolicy $sslPolicy -sslCertificates $sslCerts -Force
   ```

15. De gemaakte toepassingsgateway openbare IP-adres ophalen:

   ```PowerShell
   $pip = Get-AzureRmPublicIpAddress -Name $publicIpName -ResourceGroupName $rgname $pip.IpAddress
   ```

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

-  Ik krijgt voor application gateway Preview-versie?

   Er is tijdens de preview, zonder kosten. U zult in rekening gebracht voor bronnen dan toepassingsgateway zoals Sleutelkluis, virtuele machines, enzovoort.
- Welke regio's is de Preview-versie beschikbaar is in?

   De evaluatieversie is momenteel beschikbaar in de regio VS-Oost 2. Meer regio's wordt binnenkort toegevoegd.
- Wordt in de portal in de Preview-versie ondersteund?

   Nee, is ondersteuning beperkt tot een persoonlijke PowerShell-module en Resource Manager-sjabloon tijdens de private preview.

- Wordt in productie werkbelasting tijdens de Preview-versie private ondersteund?

   Nee, er is geen SLA of ondersteuning tijdens de private preview. Het is niet raadzaam om productieworkloads tijdens previews. Ondersteuning is beperkt tot directe interactie met de productgroep met behulp van de e-mailalias voor preview.

- Hoe kan ik problemen melden?

   De private preview kan fouten bevatten en mogelijk frequente code-implementaties. Gebruik de ondersteuningsalias appgwxzone@microsoft.com voor het melden van problemen en hulp.

## <a name="known-issues-and-limitations"></a>Bekende problemen en beperkingen


|Probleem  |Details  |
|---------|---------|---------|
|Billing     |Op dit moment geen facturering|
|Logboeken met diagnostische gegevens (geen metrische gegevens)     |Prestaties en logboeken van de aanvraag/antwoord weergegeven momenteel niet|
|CLI-Portal-SDK     |Er is geen ondersteuning voor de portal, CLI of SDK. De portal moet niet worden gebruikt voor het uitgeven van updates te bekijken van gateways.|
|Tijd tot tijd bijwerken via de sjabloon is mislukt     |Dit wordt veroorzaakt door race condition KeyVault-beleid|Zodra de Sleutelkluis en de gebruiker toegewezen identiteit is gemaakt, wordt het kan worden verwijderd uit de sjabloon en alleen verwijzingen naar geheim en identiteit in de sjabloon zijn vereist.|
|Automatisch schalen     |Er is geen ondersteuning voor automatisch schalen momenteel|
|WAF     |WAF wordt momenteel niet ondersteund|
|Gebruiker opgegeven certificaten en dynamische VIP 's     |Deze worden niet ondersteund in het nieuwe model. Sleutelkluis gebruiken voor het opslaan van certificaten en statische VIP's.|
|Hetzelfde subnet voor de oude en de preview-versie van de toepassingsgateway     |Een subnet met een bestaande (oude model) voor application gateway kan niet worden gebruikt voor de private preview-versie.|
|HTTP/2, FIPS-modus WebSocket, Azure Web Apps als back-end     |Momenteel niet ondersteund |


## <a name="support-and-feedback"></a>Ondersteuning en feedback

Voor ondersteuning en feedback contact op met naar appgwxzone@microsoft.com. De productgroep voor application gateway is blij graag uw feedback voor verbeteringen en advies indien vereist.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over andere functies van application gateway:

- [Wat is Azure Application Gateway?](overview.md)