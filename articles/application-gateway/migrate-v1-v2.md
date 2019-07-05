---
title: Azure Application Gateway migreren en Web Application Firewall van v1 in v2
description: Dit artikel leest u hoe u Azure Application Gateway en Web Application Firewall migreert van v1 in v2
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 6/18/2019
ms.author: victorh
ms.openlocfilehash: 0fd605d7d502970dccd37da1f3f70fdadb1094a1
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67550453"
---
# <a name="migrate-azure-application-gateway-and-web-application-firewall-from-v1-to-v2"></a>Azure Application Gateway migreren en Web Application Firewall van v1 in v2

[Azure Application Gateway en Web Application Firewall (WAF) v2](application-gateway-autoscaling-zone-redundant.md) is nu beschikbaar zijn, bieden aanvullende functies, zoals automatisch schalen en beschikbaarheidszone-redundantie. Bestaande v1-gateways niet worden echter automatisch omgezet in v2. Als u migreren van v1 in v2 wilt, volgt u de stappen in dit artikel.

Er zijn twee fasen van een migratie:

1. Migratie van de configuratie
2. Migreren van de clientverkeer

In dit artikel bevat informatie over migratie van de configuratie. Migratie van de client-verkeer, is afhankelijk van uw specifieke omgeving. Echter enkele aanbevelingen die op hoog niveau, algemene [vindt u](#migrate-client-traffic).

## <a name="migration-overview"></a>Overzicht migratie

Er is een Azure PowerShell-script beschikbaar die doet het volgende:

* Hiermee maakt u een nieuwe Standard_v2 of WAF_v2 gateway in een subnet van een virtueel netwerk die u opgeeft.
* De configuratie die is gekoppeld aan de v1-Standard- of WAF gateway naar de zojuist gemaakte Standard_V2 of WAF_V2 gateway naadloos worden gekopieerd.

### <a name="caveatslimitations"></a>Caveats\Limitations

* De nieuwe v2-gateway heeft nieuwe openbare en persoonlijke IP-adressen. Het is niet mogelijk om te verplaatsen van de IP-adressen die zijn gekoppeld aan de bestaande gateway v1 naadloos naar v2. U kunt echter een bestaande (niet-toegewezen) openbaar of privé IP-adres met de nieuwe v2-gateway toewijzen.
* U moet een IP-adresruimte opgeven voor een ander subnet binnen uw virtuele netwerk waar uw gateway v1 zich bevindt. Het script kan de v2-gateway maken in een bestaande subnetten waarvoor al een v1-gateway. Echter, als het bestaande subnet al is een v2-gateway, die mogelijk nog steeds werken opgegeven dat er is onvoldoende IP-adresruimte.
* Als u wilt migreren van een SSL-configuratie, moet u alle SSL-certificaten die worden gebruikt in uw v1-gateway.
* Als u FIPS-modus is ingeschakeld voor uw gateway V1 hebt, kunt u deze wordt niet gemigreerd naar uw nieuwe v2-gateway. FIPS-modus wordt niet ondersteund in v2.
* v2 biedt geen ondersteuning voor IPv6, zodat IPv6 is ingeschakeld v1-gateways worden niet gemigreerd. Als u het script uitvoert, kan het niet voltooien.
* Als de v1-gateway alleen een privé IP-adres heeft, het script maakt u een openbaar IP-adres en een privé IP-adres voor de nieuwe v2-gateway. v2-gateways ondersteund op dit moment alleen privé IP-adressen niet.

## <a name="download-the-script"></a>Het script downloaden

Download het migratiescript van de [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureAppGWMigration).

## <a name="use-the-script"></a>Gebruik het script

Er zijn twee opties voor u, afhankelijk van uw lokale instellingen voor PowerShell-omgeving en voorkeuren:

* Als u niet de Azure-Az-modules geïnstalleerd hebt, of geen rekening met het verwijderen van de Azure-Az-modules, de beste optie is met de `Install-Script` optie voor het uitvoeren van het script.
* Als u behouden van de Azure-Az-modules wilt, is de beste keuze voor het downloaden van het script en deze rechtstreeks worden uitgevoerd.

Als u wilt bepalen of u de Azure-Az-modules geïnstalleerd hebt, voer `Get-InstalledModule -Name az`. Als er geen een Az-modules hebt geïnstalleerd, en vervolgens kunt u de `Install-Script` methode.

### <a name="install-using-the-install-script-method"></a>Installeren met behulp van de methode Install-Script

Als u wilt deze optie gebruikt, moet u niet de Azure-Az-modules op uw computer geïnstalleerd hebben. Als ze worden geïnstalleerd, wordt er een fout weergegeven in de volgende opdracht uit. U kunt de modules Azure Az verwijderen, of gebruik de andere optie voor het script handmatig downloaden en uitvoeren.
  
Voer het script met de volgende opdracht:

`Install-Script -Name AzureAppGWMigration`

Met deze opdracht installeert ook de vereiste Az-modules.  

### <a name="install-using-the-script-directly"></a>Installeren met behulp van het script rechtstreeks

Als u beschikt over sommige Azure Az-modules geïnstalleerd en kan niet verwijderen (of niet wilt verwijderen), kunt u handmatig downloaden het script van de **handmatig downloaden** tabblad in de script-downloadkoppeling. Het script wordt gedownload als een onbewerkte nupkg-bestand. Zie voor informatie over het installeren van het script uit dit bestand nupkg [handmatig pakket downloaden](https://docs.microsoft.com/powershell/gallery/how-to/working-with-packages/manual-download).

Het script uitvoeren:

1. Gebruik `Connect-AzAccount` verbinding maken met Azure.

1. Gebruik `Import-Module Az` voor het importeren van de Az-modules.

1. Voer `Get-Help AzureAppGWMigration.ps1` om te controleren van de vereiste parameters:

   ```
   AzureAppGwMigration.ps1
    -resourceId <v1 application gateway Resource ID>
    -subnetAddressRange <subnet space you want to use>
    -appgwName <string to use to append>
    -sslCertificates <comma-separated SSLCert objects as above>
    -trustedRootCertificates <comma-separated Trusted Root Cert objects as above>
    -privateIpAddress <private IP string>
    -publicIpResourceName <public IP name string>
    -validateMigration -enableAutoScale
   ```

   Parameters voor het script:
   * **resourceId: [String]: Vereiste** -dit is de Azure-Resource-ID voor uw bestaande standaard v1- of v1-gateway WAF. Als u deze tekenreekswaarde zoekt, gaat u naar de Azure-portal, selecteert u uw toepassingsgateway of WAF-resource en klikt u op de **eigenschappen** koppeling voor de gateway. De Resource-ID bevindt zich op die pagina.

     U kunt ook de volgende Azure PowerShell-opdrachten voor het ophalen van de Resource-ID uitvoeren:

     ```azurepowershell
     $appgw = Get-AzApplicationGateway -Name <v1 gateway name> -ResourceGroupName <resource group Name> 
     $appgw.Id
     ```

   * **subnetAddressRange: [String]:  Vereiste** -dit is de IP-adresruimte die u hebt toegewezen (of u wilt toewijzen) voor een nieuw subnet met uw nieuwe v2-gateway. Dit moet worden opgegeven in de CIDR-notatie. Bijvoorbeeld: 10.0.0.0/24. U hoeft te maken van dit subnet vooraf. Het script maakt het voor u, als deze niet bestaat.
   * **appgwName: [String]: Optioneel**. Dit is een tekenreeks die u opgeeft als u wilt gebruiken als de naam voor de nieuwe Standard_v2 of WAF_v2-gateway. Als deze parameter niet is opgegeven, de naam van uw bestaande v1-gateway wordt gebruikt met het achtervoegsel *_v2* toegevoegd.
   * **sslCertificates: [PSApplicationGatewaySslCertificate]: Optioneel**.  Een door komma's gescheiden lijst PSApplicationGatewaySslCertificate-objecten die u maakt om weer te geven van de SSL-certificaten van uw v1-gateway moet worden geüpload naar de nieuwe gateway voor v2. Voor elk van uw SSL-certificaten geconfigureerd voor uw Standard v1- of v1-gateway WAF, kunt u een nieuw PSApplicationGatewaySslCertificate-object via de `New-AzApplicationGatewaySslCertificate` opdracht die hier worden weergegeven. U moet het pad naar het bestand een SSL-certificaat en het wachtwoord.

       Deze parameter is alleen optioneel als u geen HTTP-luisteraars geconfigureerd voor uw gateway v1 of WAF. Als u ten minste één HTTPS-listener-installatie hebt, moet u deze parameter.

      ```azurepowershell  
      $password = ConvertTo-SecureString <cert-password> -AsPlainText -Force
      $mySslCert1 = New-AzApplicationGatewaySslCertificate -Name "Cert01" `
        -CertificateFile <Cert-File-Path-1> `
        -Password $password 
      $mySslCert2 = New-AzApplicationGatewaySslCertificate -Name "Cert02" `
        -CertificateFile <Cert-File-Path-2> `
        -Password $password
      ```

      U kunt doorgeven `$mySslCert1, $mySslCert2` (door komma's gescheiden) in het vorige voorbeeld als waarden voor deze parameter in het script.
   * **trustedRootCertificates: [PSApplicationGatewayTrustedRootCertificate]: Optioneel**. Een door komma's gescheiden lijst PSApplicationGatewayTrustedRootCertificate-objecten die u maakt om weer te geven de [vertrouwde basiscertificaten](ssl-overview.md) voor de verificatie van uw back-end-exemplaren van uw v2-gateway.  

      Zie voor het maken van een lijst met objecten PSApplicationGatewayTrustedRootCertificate [New-AzApplicationGatewayTrustedRootCertificate](https://docs.microsoft.com/powershell/module/Az.Network/New-AzApplicationGatewayTrustedRootCertificate?view=azps-2.1.0&viewFallbackFrom=azps-2.0.0).
   * **privateIpAddress: [String]: Optioneel**. Een specifiek privé IP-adres dat u wilt koppelen aan uw nieuwe v2-gateway.  Dit moet zijn van het hetzelfde VNet dat u voor uw nieuwe v2-gateway toewijzen. Als dit niet is opgegeven, wordt het script een privé IP-adres toegewezen voor uw v2-gateway.
    * **publicIpResourceId: [String]: Optioneel**. De resourceId van een openbare IP-adres (standaard-SKU)-bron in uw abonnement die u wilt toewijzen aan de nieuwe v2-gateway. Als dit niet is opgegeven, wordt een nieuwe openbare IP-adres in dezelfde resourcegroep toegewezen door het script. De naam is van de v2-gateway met *- IP-* toegevoegd.
   * **validateMigration: [overschakelen]: Optioneel**. Gebruik deze parameter als u wilt dat het script moet een eenvoudige configuratie vergelijking validaties uitvoeren na het maken van de v2-gateway en de configuratiekopie. Standaard wordt geen validatie uitgevoerd.
   * **enableAutoScale: [overschakelen]: Optioneel**. Gebruik deze parameter als u wilt dat het script voor automatisch schalen op de nieuwe v2-gateway inschakelen nadat deze gemaakt. Automatisch schalen is standaard uitgeschakeld. U kunt altijd handmatig inschakelen dit later op de zojuist gemaakte v2-gateway.

1. Voer het script met de juiste parameters. Het duurt vijf tot zeven minuten om te voltooien.

    **Voorbeeld**

   ```azurepowershell
   AzureAppGWMigration.ps1 `
      -resourceId /subscriptions/8b1d0fea-8d57-4975-adfb-308f1f4d12aa/resourceGroups/MyResourceGroup/providers/Microsoft.Network/applicationGateways/myv1appgateway `
      -subnetAddressRange 10.0.0.0/24 `
      -appgwname "MynewV2gw" `
      -sslCertificates $Certs `
      -trustedRootCertificates $trustedCert `
      -privateIpAddress "10.0.0.1" `
      -publicIpResourceId "MyPublicIP" `
      -validateMigration -enableAutoScale
   ```

## <a name="migrate-client-traffic"></a>-Clientverkeer migreren

Dubbele Controleer eerst het script een nieuwe v2-gateway is gemaakt met de exacte configuratie van uw gateway v1 gemigreerd. U kunt dit controleren via de Azure-portal.

Bovendien een kleine hoeveelheid verkeer via de v2-gateway als een handmatige test verzenden.
  
Hier volgen enkele scenario's waar uw huidige application-gateway (standaard)-clientverkeer en onze aanbevelingen voor elke opdracht mogen ontvangen:

* **Een aangepaste DNS-zone (bijvoorbeeld contoso.com) die naar het front-end-IP-adres verwijst (met behulp van een A-record) die zijn gekoppeld aan uw Standard v1- of v1-gateway WAF**.

    U kunt uw DNS-record om te verwijzen naar het frontend-IP-adres of de DNS-label zijn die zijn gekoppeld aan uw Standard_v2 application-gateway bijwerken. Afhankelijk van de TTL is geconfigureerd op uw DNS-record, duurt het even voor uw clientverkeer om te migreren naar uw nieuwe v2-gateway.
* **Een aangepaste DNS-zone (bijvoorbeeld contoso.com) die naar de DNS-label verwijst (bijvoorbeeld: *myappgw.eastus.cloudapp.azure.com* met behulp van een CNAME-record) die zijn gekoppeld aan uw gateway v1**.

   U hebt twee opties:

  * Als u openbare IP-adressen in uw application gateway gebruikt, kunt u doen een gecontroleerde, gedetailleerde migratie met behulp van een Traffic Manager-profiel incrementeel om verkeer te routeren (gewogen verkeersrouteringsmethode) met de nieuwe v2-gateway.

    U kunt dit doen door toe te voegen van de DNS-labels van de v1- en v2 Toepassingsgateways op de [Traffic Manager-profiel](../traffic-manager/traffic-manager-routing-methods.md#weighted-traffic-routing-method), en uw aangepaste DNS-record (bijvoorbeeld www.contoso.com) aan het Traffic Manager-domein (bijvoorbeeld CNAMEing Contoso.trafficmanager.NET).
  * Of u kunt uw aangepaste domein-DNS-record om te verwijzen naar de DNS-label van de nieuwe gateway van de v2-toepassing bijwerken. Afhankelijk van de TTL is geconfigureerd op uw DNS-record, duurt het even voor uw clientverkeer om te migreren naar uw nieuwe v2-gateway.
* **Uw clients verbinding maken met de front-end IP-adres van uw toepassingsgateway**.

   Werk uw clients voor het gebruik van de IP-adressen die zijn gekoppeld aan de zojuist gemaakte v2 application-gateway. U wordt aangeraden dat u direct IP-adressen niet gebruikt. Overweeg het gebruik van de DNS-naamlabel (bijvoorbeeld yourgateway.eastus.cloudapp.azure.com) die zijn gekoppeld aan uw application gateway kunt u CNAME aan uw eigen aangepaste DNS-zone (bijvoorbeeld contoso.com).

## <a name="common-questions"></a>Veelgestelde vragen

### <a name="are-there-any-limitations-with-the-azure-powershell-script-to-migrate-the-configuration-from-v1-to-v2"></a>Zijn er beperkingen met de Azure PowerShell-script voor het migreren van de configuratie van v1 in v2?

Ja. Zie [onder voorbehoud/beperkingen](#caveatslimitations).

### <a name="is-this-article-and-the-azure-powershell-script-applicable-for-application-gateway-waf-product-as-well"></a>In dit artikel en de Azure PowerShell-script van toepassing is voor Application Gateway WAF product ook? 

Ja.

### <a name="does-the-azure-powershell-script-also-switch-over-the-traffic-from-my-v1-gateway-to-the-newly-created-v2-gateway"></a>De Azure PowerShell-script ook Schakel over het verkeer van mijn v1-gateway naar de zojuist gemaakte v2-gateway?

Nee. De Azure PowerShell-script wordt alleen de configuratie migreert. Migratie van de werkelijke hoeveelheid verkeer is uw verantwoordelijkheid en in uw beheer.

### <a name="is-the-new-v2-gateway-created-by-the-azure-powershell-script-sized-appropriately-to-handle-all-of-the-traffic-that-is-currently-served-by-my-v1-gateway"></a>Is de nieuwe v2-gateway die zijn gemaakt door de Azure PowerShell-script juiste grootte krijgen voor het afhandelen van al het verkeer dat momenteel wordt bediend door mijn v1-gateway?

De Azure PowerShell-script maakt een nieuwe v2-gateway met een geschikte grootte voor het afhandelen van het verkeer op uw bestaande v1-gateway. Automatisch schalen is standaard uitgeschakeld, maar u kunt automatisch schalen inschakelen wanneer u het script uitvoert.

### <a name="i-configured-my-v1-gateway--to-send-logs-to-azure-storage-does-the-script-replicate-this-configuration-for-v2-as-well"></a>Heb ik mijn v1-gateway om Logboeken te zenden naar Azure-opslag hebt geconfigureerd. Wordt deze configuratie voor v2 ook gerepliceerd door het script?

Nee. Het script repliceren niet van deze configuratie voor v2. U moet de configuratie van afzonderlijk toevoegen aan de gemigreerde v2-gateway.

### <a name="i-ran-into-some-issues-with-using-this-script-how-can-i-get-help"></a>Ik is bij enkele problemen met het gebruik van dit script. Hoe kan ik hulp krijgen?
  
U kunt een e-mailbericht te verzenden appgwmigrationsup@microsoft.com, een ondersteuningsaanvraag openen met ondersteuning voor Azure, of beide.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over Application Gateway v2](application-gateway-autoscaling-zone-redundant.md)
