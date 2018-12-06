---
title: Een Azure App Service-omgeving maken met behulp van Resource Manager-sjabloon
description: Wordt uitgelegd hoe u een externe of ILB Azure App Service-omgeving maken met behulp van Resource Manager-sjabloon
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 6eb7d43d-e820-4a47-818c-80ff7d3b6f8e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: bdd8ac47f709153b17e2dcf44ff9a2c568e650cc
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52958744"
---
# <a name="create-an-ase-by-using-an-azure-resource-manager-template"></a>Een as-omgeving maken met behulp van een Azure Resource Manager-sjabloon

## <a name="overview"></a>Overzicht
Azure App Service-omgevingen (as-omgevingen) kunnen worden gemaakt met een via internet toegankelijke eindpunt of een eindpunt op een intern adres in een Azure-netwerk (VNet). Als met een intern eindpunt dat is gemaakt, wordt dit eindpunt wordt verstrekt door een Azure-onderdeel genaamd een interne load balancer (ILB). De as-omgeving op een intern IP-adres wordt een ILB as-omgeving genoemd. De as-omgeving met een openbaar eindpunt wordt een externe as-omgeving genoemd. 

Een as-omgeving kan worden gemaakt met behulp van de Azure-portal of een Azure Resource Manager-sjabloon. Dit artikel helpt bij de stappen en de syntaxis die u moet een externe as-omgeving of een ILB as-omgeving maken met Resource Manager-sjablonen. Zie voor meer informatie over het maken van een as-omgeving in Azure portal, [maken van een externe as-omgeving] [ MakeExternalASE] of [een ILB as-omgeving maken][MakeILBASE].

Wanneer u een as-omgeving in Azure portal maakt, kunt u uw VNet op hetzelfde moment maken of een bestaande VNet te implementeren in kiezen. Wanneer u een as-omgeving van een sjabloon maakt, moet u beginnen met: 

* Een Resource Manager-VNet.
* Een subnet in dat VNet. We raden aan de grootte van een ASE-subnet van `/24` met 256-adressen voor toekomstige groei en schaalbehoeften. Nadat de as-omgeving is gemaakt, kunt u de grootte niet wijzigen.
* De resource-ID van uw VNet. U kunt deze informatie krijgen vanuit Azure portal onder de eigenschappen van uw virtuele netwerk.
* Het abonnement dat u implementeren wilt in.
* De locatie die u implementeren wilt in.

Voor het automatiseren van het maken van de as-omgeving:

1. De as-omgeving maken met een sjabloon. Als u een externe as-omgeving maakt, bent u klaar na deze stap. Als u een ILB as-omgeving maakt, zijn er enkele meer dingen te doen.

2. Nadat uw ILB as-omgeving is gemaakt, wordt een SSL-certificaat dat overeenkomt met het domein van uw ILB as-omgeving is geüpload.

3. Het geüploade SSL-certificaat is toegewezen aan de ILB as-omgeving als de 'standaard' SSL-certificaat.  Dit certificaat voor SSL-verkeer naar apps op de ILB as-omgeving wordt gebruikt wanneer ze het algemene hoofddomein die toegewezen aan de as-omgeving gebruiken (bijvoorbeeld https://someapp.mycustomrootdomain.com).


## <a name="create-the-ase"></a>De as-omgeving maken
Resource Manager-sjabloon waarmee u een as-omgeving en de bijbehorende parameters-bestand maakt, is beschikbaar [in een voorbeeld] [ quickstartasev2create] op GitHub.

Als u een ILB as-omgeving maken wilt, gebruikt u deze Resource Manager-sjabloon [voorbeelden][quickstartilbasecreate]. Ze geschikt zijn voor die toepassing. De meeste van de parameters in de *azuredeploy.parameters.json* bestand gemeenschappelijk zijn voor het maken van de ILB as-omgevingen en externe as-omgevingen. De volgende lijst worden parameters van speciale Opmerking aangeroepen of die uniek is, zijn bij het maken van een ILB as-omgeving:

* *internalLoadBalancingMode*: Stel deze optie om 3, wat betekent dat zowel HTTP/HTTPS-verkeer op poort 80/443 en de controlegegevens channel-poorten te hebben geluisterd naar door de FTP-service op de as-omgeving, In de meeste gevallen wordt gebonden aan een ILB-toegewezen virtueel netwerk interne adres. Als deze eigenschap is ingesteld op 2, worden alleen de FTP-service met betrekking tot poorten (zowel controle en kanalen) zijn gebonden aan een ILB-adres. Het HTTP/HTTPS-verkeer blijft op het openbare VIP-adres.
* *dnsSuffix*: deze parameter bepaalt de standaard-hoofddomein dat toegewezen aan de as-omgeving. In de openbare variatie van Azure App Service biedt het hoofddomein standaard voor alle web-apps is *azurewebsites.net*. Omdat een ILB as-omgeving intern voor het virtuele netwerk van een klant is, verstandig niet het om te gebruiken van de openbare service standaard-hoofddomein. Een ILB as-omgeving moet in plaats daarvan een standaard-hoofddomein die zinvol voor gebruik binnen een bedrijf intern virtueel netwerk hebben. Bijvoorbeeld, Contoso Corporation gebruiken een standaard-hoofddomein van *interne contoso.com* voor apps die zijn bedoeld om te worden omgezet en alleen toegankelijk binnen het virtuele netwerk van Contoso. 
* *ipSslAddressCount*: deze parameter wordt automatisch standaard ingesteld op een waarde van 0 in de *azuredeploy.json* omdat ILB as-omgevingen hebben slechts één ILB-adres. Er zijn geen expliciete IP-SSL-adressen voor een ILB as-omgeving. Daarom kan moet de IP-SSL-adresgroep voor een ILB as-omgeving worden ingesteld op nul. Anders treedt er een inrichten fout op. 

Na de *azuredeploy.parameters.json* bestand is ingevuld, wordt de as-omgeving maken met behulp van de PowerShell-codefragment. Wijzig de bestandspaden zodat deze overeenkomt met de Resource Manager-sjabloonbestand locaties op uw computer. Vergeet niet uw eigen waarden voor de implementatienaam van de Resource Manager-en de naam van de resourcegroep opgeven:

```powershell
$templatePath="PATH\azuredeploy.json"
$parameterPath="PATH\azuredeploy.parameters.json"

New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath
```

Het duurt ongeveer een uur voor de as-omgeving moet worden gemaakt. De as-omgeving wordt vervolgens weergegeven in de portal in de lijst van de as-omgevingen voor het abonnement waarmee de implementatie is geactiveerd.

## <a name="upload-and-configure-the-default-ssl-certificate"></a>Uploaden en de 'standaard' SSL-certificaat configureren
Een SSL-certificaat moet worden gekoppeld aan de as-omgeving als de 'standaard' SSL-certificaat dat wordt gebruikt voor SSL-verbindingen met apps. Als de standaard DNS-achtervoegsel van de as-omgeving is *interne contoso.com*, een verbinding met https://some-random-app.internal-contoso.com vereist een SSL-certificaat is geldig voor **.internal contoso.com*. 

Een geldig SSL-certificaat met behulp van interne certificeringsinstanties, aanschaffen van een certificaat van een externe gebruiker of een zelfondertekend certificaat verkrijgen. De volgende certificaatkenmerken moeten correct worden geconfigureerd, ongeacht de bron van het SSL-certificaat:

* **Onderwerp**: dit kenmerk moet worden ingesteld op **.uw-root-domein-here.com*.
* **Alternatieve onderwerpnaam**: dit kenmerk moet bevatten zowel **.uw-root-domein-here.com* en **.Hier-root-domein-here.com*. SSL-verbindingen met de SCM/Kudu-site die is gekoppeld aan elke app gebruiken een adres van het formulier *your-app-name.scm.your-root-domain-here.com*.

Met een geldig SSL-certificaat in voorraad, zijn twee aanvullende voorbereidende stappen nodig. Converteer het SSL-certificaat naar een .pfx-bestand of sla het certificaat in deze indeling op. Houd er rekening mee dat het pfx-bestand moet alle tussenliggende opnemen en basiscertificaten. Beveilig het bestand met een wachtwoord.

Het pfx-bestand moet worden geconverteerd naar een Base 64-tekenreeks omdat het SSL-certificaat is geüpload met behulp van Resource Manager-sjabloon. Omdat de Resource Manager-sjablonen zijn tekstbestanden, kan het pfx-bestand moet worden geconverteerd naar een Base 64-tekenreeks. Deze manier opgenomen als een parameter van de sjabloon zijn kan.

Het volgende PowerShell-codefragment om te gebruiken:

* Een zelfondertekend certificaat genereren.
* Het certificaat exporteren als een pfx-bestand.
* Het pfx-bestand converteren naar een base64-gecodeerde tekenreeks.
* Sla de base64-gecodeerde tekenreeks in een afzonderlijk bestand. 

Deze PowerShell-code voor base64-codering is gebaseerd op de [PowerShell-scripts blog][examplebase64encoding]:

```powershell
$certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

$certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
$password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

$fileName = "exportedcert.pfx"
Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     

$fileContentBytes = get-content -encoding byte $fileName
$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
$fileContentEncoded | set-content ($fileName + ".b64")
```

Nadat het SSL-certificaat is is gegenereerd en geconverteerd naar een base64-gecodeerde tekenreeks, gebruikt u het voorbeeld van de Resource Manager-sjabloon [de standaard-SSL-certificaat configureren] [ quickstartconfiguressl] op GitHub. 

De parameters in de *azuredeploy.parameters.json* bestand worden hier weergegeven:

* *appServiceEnvironmentName*: de naam van de ILB as-omgeving wordt geconfigureerd.
* *existingAseLocation*: tekenreeks met de Azure-regio waar de ILB as-omgeving is geïmplementeerd.  Bijvoorbeeld: 'Zuid-centraal VS'.
* *pfxBlobString*: de weergave based64-gecodeerde tekenreeks van het pfx-bestand. Gebruik het bovenstaande codefragment en kopieer de tekenreeks in 'exportedcert.pfx.b64'. Plak deze in als de waarde van de *pfxBlobString* kenmerk.
* *wachtwoord*: het wachtwoord dat wordt gebruikt voor het beveiligen van het pfx-bestand.
* *certificateThumbprint*: vingerafdruk van het certificaat. Als u deze waarde wordt opgehaald vanuit PowerShell (bijvoorbeeld *$certificate. Vingerafdruk* uit het vorige codefragment), kunt u de waarde is. Als u de waarde in het dialoogvenster Windows-certificaat kopieert, vergeet niet het verwijderen van de overbodige spaties. De *certificateThumbprint* ziet er ongeveer als AF3143EB61D43F6727842115BB7F17BBCECAECAE.
* *certificateName*: een beschrijvende tekenreeks-id van uw eigen keuze aan identiteit gebruikt met het certificaat. De naam wordt gebruikt als onderdeel van de unieke id van de Resource Manager voor de *Microsoft.Web/certificates* entiteit die het SSL-certificaat aangeeft. De naam van de *moet* eindigen met het volgende achtervoegsel: \_yourASENameHere_InternalLoadBalancingASE. De Azure portal maakt gebruik van dit achtervoegsel als een indicator dat het certificaat wordt gebruikt voor het beveiligen van een ASE met ILB ingeschakeld.

Een verkorte voorbeeld van *azuredeploy.parameters.json* wordt hier weergegeven:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "appServiceEnvironmentName": {
      "value": "yourASENameHere"
    },
    "existingAseLocation": {
      "value": "East US 2"
    },
    "pfxBlobString": {
      "value": "MIIKcAIBAz...snip...snip...pkCAgfQ"
    },
    "password": {
      "value": "PASSWORDGOESHERE"
    },
    "certificateThumbprint": {
      "value": "AF3143EB61D43F6727842115BB7F17BBCECAECAE"
    },
    "certificateName": {
      "value": "DefaultCertificateFor_yourASENameHere_InternalLoadBalancingASE"
    }
  }
}
```

Na de *azuredeploy.parameters.json* bestand is ingevuld, wordt het standaard SSL-certificaat configureren met behulp van de PowerShell-codefragment. Wijzig de bestandspaden aan waar de Resource Manager-sjabloonbestanden bevinden zich op uw computer. Vergeet niet uw eigen waarden voor de implementatienaam van de Resource Manager-en de naam van de resourcegroep opgeven:

```powershell
$templatePath="PATH\azuredeploy.json"
$parameterPath="PATH\azuredeploy.parameters.json"

New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath
```

Het duurt ongeveer 40 minuten per ASE-front-end de wijziging toepassen. Bijvoorbeeld, voor een standaardformaat as-omgeving die gebruikmaakt van twee front-ends, duurt het sjabloon ongeveer één uur en 20 minuten om te voltooien. Terwijl de sjabloon wordt uitgevoerd, wordt de as-omgeving kan niet schalen.  

Nadat de sjabloon is voltooid, zijn de apps op de ILB as-omgeving toegankelijk via HTTPS. De verbindingen zijn beveiligd met behulp van het standaard SSL-certificaat. Het standaard SSL-certificaat wordt gebruikt wanneer de apps op de ILB as-omgeving met behulp van een combinatie van naam van de toepassing, plus de standaardnaam van de host worden behandeld. Bijvoorbeeld, https://mycustomapp.internal-contoso.com maakt gebruik van de standaard-SSL-certificaat voor **.internal contoso.com*.

Echter, net als bij apps die worden uitgevoerd op de openbare multitenant-service, kunnen configureren ontwikkelaars aangepaste hostnamen voor afzonderlijke apps. Ze kunnen ook unieke SNI SSL-certificaatbindingen voor afzonderlijke apps configureren.

## <a name="app-service-environment-v1"></a>App Service-omgeving v1 ##
App Service Environment heeft twee versies: ASEv1 en ASEv2. De voorgaande informatie is gebaseerd op ASEv2. In deze sectie leest u wat de verschillen zijn tussen ASEv1 en ASEv2.

In ASEv1 beheren u alle resources handmatig. Dit geldt ook voor de front-ends, werkrollen en IP-adressen die worden gebruikt voor op IP gebaseerd SSL. Voordat u uw App Service-plan uitschalen kunt, moet u de groep met werkrollen die u wilt hosten van de App uitschalen.

ASEv1 maakt gebruik van een ander prijsmodel dan ASEv2. In ASEv1 betaalt u voor elke toegewezen vCPU. Dit is inclusief vcpu's die worden gebruikt voor front-ends of werkrollen die geen werkbelastingen hosten. In ASEv1 is de maximale schaalgrootte voor een AS-omgeving standaard 55 hosts (in totaal). Dit is inclusief werkrollen en front-ends. Een voordeel van ASEv1 is dat deze versie kan worden geïmplementeerd in een klassiek virtueel netwerk en een virtueel netwerk van Resource Manager. Zie [Inleiding tot App Service Environment v1][ASEv1Intro] voor meer informatie over ASEv1.

Zie voor informatie over het maken van een ASEv1 met behulp van Resource Manager-sjabloon [een ILB as-omgeving v1 maken met een Resource Manager-sjabloon][ILBASEv1Template].


<!--Links-->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-asev2-ilb-create
[quickstartasev2create]: https://azure.microsoft.com/documentation/templates/201-web-app-asev2-create
[quickstartconfiguressl]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl
[quickstartwebapponasev2create]: https://azure.microsoft.com/documentation/templates/201-web-app-asp-app-on-asev2-create
[examplebase64encoding]: https://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/security-overview.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: https://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../../app-service/web-sites-purchase-ssl-web-site.md
[Kudu]: https://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[ILBASEv1Template]: app-service-app-service-environment-create-ilb-ase-resourcemanager.md
