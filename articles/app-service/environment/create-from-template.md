---
title: Een Azure App Service-omgeving maken met een Resource Manager-sjabloon
description: Wordt uitgelegd hoe u een externe of ILB Azure App Service-omgeving maken met een Resource Manager-sjabloon
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
ms.openlocfilehash: b3829f0e1b87451bf0706edc268359be5c4480bc
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2017
---
# <a name="create-an-ase-by-using-an-azure-resource-manager-template"></a>Een as-omgeving maken met een Azure Resource Manager-sjabloon

## <a name="overview"></a>Overzicht
Azure App Service-omgevingen (ASEs) kunnen worden gemaakt met een eindpunt toegankelijk is via het internet of een eindpunt op een intern adres in een Azure-netwerk (VNet). Wanneer dit wordt gemaakt met een interne eindpunt, wordt dat eindpunt wordt verstrekt door een Azure onderdeel een interne load balancer (ILB) genoemd. De as-omgeving op een interne IP-adres, heet een ILB-as-omgeving. De as-omgeving met een openbaar eindpunt heet een externe as-omgeving. 

Een as-omgeving kan worden gemaakt met behulp van de Azure-portal of een Azure Resource Manager-sjabloon. In dit artikel wordt uitgelegd welke stappen en de syntaxis die u moet een externe as-omgeving of ILB as-omgeving maken met Resource Manager-sjablonen. Zie voor meer informatie over het maken van een as-omgeving in de Azure portal, [maken van een externe as-omgeving] [ MakeExternalASE] of [maken van een as-omgeving voor de ILB][MakeILBASE].

Wanneer u een as-omgeving in de Azure portal maakt, kunt u uw VNet maken op hetzelfde moment of kies een bestaande VNet te implementeren in. Wanneer u een as-omgeving van een sjabloon maakt, moet u beginnen met: 

* Een Resource Manager VNet.
* Een subnet in dit VNet. De grootte van een as-omgeving van het is raadzaam `/25` met 128 adressen biedt bij toekomstige groei. Nadat de as-omgeving is gemaakt, kunt u de grootte niet wijzigen.
* De resource-ID van uw VNet. U kunt deze informatie vinden vanuit de Azure-portal onder de eigenschappen van uw virtuele netwerk.
* Het abonnement dat u implementeren wilt in.
* De locatie die u implementeren wilt in.

Voor het automatiseren van uw as-omgeving maken:

1. De as-omgeving met een sjabloon maken. Als u een externe as-omgeving maakt, bent u klaar na deze stap. Als u een ILB-as-omgeving maakt, zijn er enkele aanvullende zaken te doen.

2. Nadat u uw as ILB-omgeving is gemaakt, wordt een SSL-certificaat dat overeenkomt met uw domein ILB as-omgeving geüpload.

3. Het geüploade SSL-certificaat is toegewezen aan de as-omgeving voor de ILB als de 'standaard' SSL-certificaat.  Dit certificaat wordt gebruikt voor SSL-verkeer voor apps op de as ILB-omgeving wanneer ze het algemene hoofddomein dat toegewezen aan de as-omgeving (bijvoorbeeld https://someapp.mycustomrootcomain.com) gebruiken.


## <a name="create-the-ase"></a>De as-omgeving maken
Resource Manager-sjabloon die u een as-omgeving en de bijbehorende parameters-bestand maakt is beschikbaar [in een voorbeeld] [ quickstartasev2create] op GitHub.

Als u maken van een as ILB-omgeving wilt, gebruikt u deze Resource Manager-sjabloon [voorbeelden][quickstartilbasecreate]. Ze voorzien in dat geval gebruiken. De meeste van de parameters in de *azuredeploy.parameters.json* bestand voor het maken van ILB ASEs en externe ASEs gelden. De volgende lijst illustreert parameters van speciale opmerking of die uniek zijn, zijn bij het maken van een as ILB-omgeving:

* *internalLoadBalancingMode*: Stel deze optie om 3, wat betekent dat zowel HTTPS-verkeer op poort 80/443 en de controlegegevens channel-poorten te hebben geluisterd naar door de FTP-service op de as-omgeving In de meeste gevallen wordt gebonden aan een ILB-toegewezen virtueel netwerk interne adres. Als deze eigenschap is ingesteld op 2, worden alleen de FTP-service verwante poorten (zowel besturings- als kanalen) gekoppeld aan een ILB-adres. Het HTTP/HTTPS-verkeer blijft op het openbare VIP.
* *dnsSuffix*: deze parameter bepaalt het hoofddomein standaard die toegewezen aan de as-omgeving. In de openbare variatie van Azure App Service, het hoofddomein standaard voor alle web-apps is *azurewebsites.net*. Omdat een ILB-as-omgeving is voor intern gebruik voor het virtuele netwerk van een klant, niet dat u de openbare dienst standaard hoofddomein gebruiken. Een as ILB-omgeving moet in plaats daarvan een standaard-hoofddomein die zinvol voor gebruik binnen een bedrijf intern virtueel netwerk hebben. Bijvoorbeeld, Contoso Corporation gebruiken een hoofddomein standaard van *interne contoso.com* voor apps die zijn bedoeld om te worden omgezet en toegankelijk zijn alleen binnen het virtuele netwerk van Contoso. 
* *ipSslAddressCount*: deze parameter automatisch wordt standaard ingesteld op een waarde van 0 in de *azuredeploy.json* bestand omdat ILB ASEs slechts één ILB-adres hebben. Er zijn geen expliciete SSL IP-adressen voor een as ILB-omgeving. Daarom kan de IP-SSL-adresgroep voor een as ILB-omgeving moet worden ingesteld op nul. Anders treedt er een inrichten fout op. 

Na de *azuredeploy.parameters.json* bestand is ingevuld, de as-omgeving maken met behulp van het PowerShell-codefragment. Wijzig de bestandspaden zodat deze overeenkomen met de Resource Manager-sjabloonbestand locaties op uw computer. Vergeet niet uw eigen waarden voor de implementatienaam van Resource Manager en de naam van de resourcegroep opgeven:

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Het duurt ongeveer een uur voor de as-omgeving moet worden gemaakt. Vervolgens de as-omgeving wordt weergegeven in de portal in de lijst met ASEs voor het abonnement waarmee de implementatie is geactiveerd.

## <a name="upload-and-configure-the-default-ssl-certificate"></a>Uploaden en configureren van het SSL-certificaat voor "default"
Een SSL-certificaat moet worden gekoppeld aan het as-omgeving als het 'standaard' SSL-certificaat dat wordt gebruikt voor SSL-verbindingen met apps. Als van de as-omgeving standaard DNS-achtervoegsel *interne contoso.com*, een verbinding met https://some-random-app.internal-contoso.com vereist een SSL-certificaat is geldig voor **.internal contoso.com*. 

Een geldig SSL-certificaat ophalen met behulp van de interne CA's, een certificaat aanschaffen van een externe verlener of met behulp van een zelfondertekend certificaat. Ongeacht de bron van het SSL-certificaat, moeten de volgende certificaat-kenmerken juist zijn geconfigureerd:

* **Onderwerp**: dit kenmerk moet worden ingesteld op **.your-hoofdmap-domain-here.com*.
* **Alternatieve onderwerpnaam**: dit kenmerk moet bevatten beide **.your-hoofdmap-domain-here.com* en **.scm.your-hoofdmap-domain-here.com*. SSL-verbindingen met de SCM/Kudu-site die is gekoppeld aan elke app een adres van het formulier gebruiken *your-app-name.scm.your-root-domain-here.com*.

Met een geldig SSL-certificaat in de hand, worden twee aanvullende voorbereidende stappen uitgevoerd. Het SSL-certificaat, converteren/opslaan als een .pfx-bestand. Houd er rekening mee dat het .pfx-bestand moet alle tussenliggende bevatten en de hoofd-certificaten. Beveilig deze met een wachtwoord.

Het pfx-bestand moet worden geconverteerd naar een base64-tekenreeks omdat het SSL-certificaat wordt geüpload met behulp van een Resource Manager-sjabloon. Omdat Resource Manager-sjablonen tekstbestanden zijn, kan het pfx-bestand moet worden geconverteerd naar een base64-tekenreeks. Op deze manier kan zijn opgenomen als een parameter van de sjabloon.

Gebruik de volgende PowerShell-codefragment aan:

* Een zelfondertekend certificaat genereren.
* Het certificaat exporteren als een .pfx-bestand.
* Het pfx-bestand worden geconverteerd naar een base64-gecodeerde tekenreeks.
* Sla de base64-gecodeerde tekenreeks naar een afzonderlijk bestand. 

Deze PowerShell-code voor base64-codering is gebaseerd op de [PowerShell scripts blog][examplebase64encoding]:

        $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

        $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
        $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

        $fileName = "exportedcert.pfx"
        Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     

        $fileContentBytes = get-content -encoding byte $fileName
        $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
        $fileContentEncoded | set-content ($fileName + ".b64")

Nadat het SSL-certificaat is gegenereerd en geconverteerd naar een base64-gecodeerde tekenreeks, gebruikt u de voorbeeld-Resource Manager-sjabloon [de standaard SSL-certificaat configureren] [ quickstartconfiguressl] op GitHub. 

De parameters in de *azuredeploy.parameters.json* bestand worden hier vermeld:

* *appServiceEnvironmentName*: de naam van de as ILB-omgeving wordt geconfigureerd.
* *existingAseLocation*: tekenreeks met de Azure-regio waar de as ILB-omgeving is geïmplementeerd.  Bijvoorbeeld: 'Zuid-centraal VS'.
* *pfxBlobString*: de based64-gecodeerde tekenreeksrepresentatie van het pfx-bestand. Gebruik het bovenstaande codefragment en kopieer de tekenreeks in 'exportedcert.pfx.b64'. Plak deze in als de waarde van de *pfxBlobString* kenmerk.
* *wachtwoord*: het wachtwoord dat wordt gebruikt voor het beveiligen van het pfx-bestand.
* *certificateThumbprint*: vingerafdruk van het certificaat. Als u deze waarde wordt opgehaald uit PowerShell (bijvoorbeeld *$certificate. Vingerafdruk* van het vorige codefragment), kunt u de waarde is. Als u de waarde van het dialoogvenster Windows-certificaat kopieert, moet u het verwijderen van overbodige spaties. De *certificateThumbprint* ziet er ongeveer als AF3143EB61D43F6727842115BB7F17BBCECAECAE.
* *Certificaatnaam*: een beschrijvende tekenreeks-id van uw eigen keuze die worden gebruikt voor de identiteit van het certificaat. De naam wordt gebruikt als onderdeel van de unieke id van de Resource Manager voor de *Microsoft.Web/certificates* entiteit die staat voor het SSL-certificaat. De naam van de *moet* eindigen met het volgende achtervoegsel: \_yourASENameHere_InternalLoadBalancingASE. De Azure-portal gebruikt dit achtervoegsel als een indicatie dat het certificaat wordt gebruikt voor het beveiligen van een as ingeschakeld met een ILB-omgeving.

Een afgekorte voorbeeld van *azuredeploy.parameters.json* Hier wordt weergegeven:

    {
         "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json",
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

Na de *azuredeploy.parameters.json* bestand is ingevuld, wordt het standaard SSL-certificaat configureren met behulp van het PowerShell-codefragment. Wijzig de bestandspaden aan waar de Resource Manager-sjabloon-bestanden bevinden zich op uw computer. Vergeet niet uw eigen waarden voor de implementatienaam van Resource Manager en de naam van de resourcegroep opgeven:

     $templatePath="PATH\azuredeploy.json"
     $parameterPath="PATH\azuredeploy.parameters.json"

     New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Het duurt ongeveer 40 minuten per as-omgeving front-end de wijziging toepassen. Voor een standaardformaat as-omgeving die gebruikmaakt van twee front-ends, duurt de sjabloon bijvoorbeeld ongeveer één uur en 20 minuten om te voltooien. Terwijl de sjabloon die wordt uitgevoerd, kan niet de schaal van de as-omgeving.  

Nadat de sjabloon is voltooid, kan apps op de as ILB-omgeving kunnen worden benaderd via HTTPS. De verbindingen zijn beveiligd met behulp van het standaard SSL-certificaat. Het standaard SSL-certificaat wordt gebruikt wanneer de apps op de as ILB-omgeving worden aangepakt met behulp van een combinatie van naam van de toepassing plus de standaardnaam van de host. Https://mycustomapp.internal-contoso.com gebruikt bijvoorbeeld de standaard SSL-certificaat voor **.internal contoso.com*.

Ontwikkelaars kunnen echter net als bij apps die worden uitgevoerd op de openbare multitenant-service, configureren aangepaste hostnamen voor afzonderlijke apps. Ze kunnen ook unieke SNI-SSL-certificaatbindingen voor afzonderlijke apps configureren.

## <a name="app-service-environment-v1"></a>App Service-omgeving v1 ##
App Service-omgeving zijn er twee versies: ASEv1 en ASEv2. De voorgaande informatie is gebaseerd op ASEv2. Deze sectie leest u de verschillen tussen ASEv1 en ASEv2.

In ASEv1 beheert u alle resources handmatig. Dat betekent onder meer de front-ends, werknemers en IP-adressen gebruikt voor SSL op basis van IP. Voordat u uw App Service-abonnement uitbreiden kunt, moet u de worker-groep die u wilt deze hosten uitschalen.

ASEv1 maakt gebruik van een andere prijscategorie model uit ASEv2. In ASEv1 betaalt u voor elke vCPU toegewezen. Dat betekent onder meer vcpu's die worden gebruikt voor de front-ends of workers die werkbelastingen worden niet als host. Grootte van de maximale schaal van een as-omgeving is in ASEv1, 55 totale hosts. Die bevat werknemers en -front-ends. Een voordeel ASEv1 is dat deze kan worden geïmplementeerd in een klassiek virtueel netwerk en een virtueel netwerk van Resource Manager. Zie voor meer informatie over ASEv1, [App Service-omgeving v1 inleiding][ASEv1Intro].

Zie voor informatie over het maken van een ASEv1 met behulp van een Resource Manager-sjabloon [een v1 ILB as-omgeving maken met Resource Manager-sjabloon][ILBASEv1Template].


<!--Links-->
[quickstartilbasecreate]: http://azure.microsoft.com/documentation/templates/201-web-app-asev2-ilb-create
[quickstartasev2create]: http://azure.microsoft.com/documentation/templates/201-web-app-asev2-create
[quickstartconfiguressl]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl
[quickstartwebapponasev2create]: http://azure.microsoft.com/documentation/templates/201-web-app-asp-app-on-asev2-create
[examplebase64encoding]: http://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../../app-service/web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[ILBASEv1Template]: app-service-app-service-environment-create-ilb-ase-resourcemanager.md
