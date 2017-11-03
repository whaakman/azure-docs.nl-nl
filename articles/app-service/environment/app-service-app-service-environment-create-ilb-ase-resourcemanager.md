---
title: Het maken van een as ILB-omgeving met behulp van Azure Resource Manager-sjablonen | Microsoft Docs
description: Informatie over het maken van een interne load balancer as-omgeving met behulp van Azure Resource Manager-sjablonen.
services: app-service
documentationcenter: 
author: stefsch
manager: nirma
editor: 
ms.assetid: 091decb6-b0de-42a1-9f2f-c18d9b2e67df
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: stefsch
ms.openlocfilehash: ea9407208f1bf555cf1a6d166825896dec89ec29
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-an-ilb-ase-using-azure-resource-manager-templates"></a>Een ILB ASE maken met behulp van Azure Resource Manager-sjablonen

> [!NOTE] 
> In dit artikel gaat over de v1 App Service-omgeving. Er is een nieuwere versie van de App Service-omgeving die eenvoudiger te gebruiken en wordt uitgevoerd op krachtiger infrastructuur. Voor meer informatie over de nieuwe versie begin met het [Inleiding tot de App-serviceomgeving](intro.md).
>

## <a name="overview"></a>Overzicht
App Service-omgevingen kunnen worden gemaakt met de interne adres van een virtueel netwerk in plaats van een openbare VIP.  Deze interne adres is verstrekt door een Azure onderdeel met de naam van de interne load balancer (ILB).  Een as ILB-omgeving kunnen worden gemaakt met de Azure portal.  Ook kunnen worden gemaakt met behulp van automatisering in Azure Resource Manager-sjablonen.  In dit artikel wordt uitgelegd welke stappen en de syntaxis voor het maken van een as ILB-omgeving met Azure Resource Manager-sjablonen.

Er zijn drie stappen die betrokken zijn bij de automatisering van het maken van een as ILB-omgeving:

1. Eerst wordt de base as-omgeving gemaakt in een virtueel netwerk met een interne load balancer-adres in plaats van een openbare VIP.  Als onderdeel van deze stap wordt een hoofddomeinnaam toegewezen aan de as ILB-omgeving.
2. Zodra de as ILB-omgeving is gemaakt, wordt een SSL-certificaat geüpload.  
3. Het geüploade SSL-certificaat als het SSL-certificaat 'standaard' expliciet toegewezen aan de as ILB-omgeving.  Dit SSL-certificaat wordt gebruikt voor SSL-verkeer voor apps op de as ILB-omgeving wanneer de apps worden aangepakt met behulp van het algemene hoofddomein toegewezen aan de as-omgeving (bijvoorbeeld https://someapp.mycustomrootcomain.com)

## <a name="creating-the-base-ilb-ase"></a>De Base ILB as-omgeving maken
Een voorbeeld van Azure Resource Manager-sjabloon en de bijbehorende parameterbestand zijn beschikbaar op GitHub [hier][quickstartilbasecreate].

De meeste van de parameters in de *azuredeploy.parameters.json* bestand voor het maken van zowel ILB ASEs, evenals ASEs gebonden aan een openbare VIP gelden.  De lijst hieronder aanroepen van speciale Opmerking uitvoerparameters of die zijn uniek, bij het maken van een as ILB-omgeving:

* *interalLoadBalancingMode*: Stel deze optie om 3, wat betekent dat zowel HTTPS-verkeer op poort 80/443 en de controlegegevens channel-poorten te hebben geluisterd naar door de FTP-service op de as-omgeving In de meeste gevallen wordt gebonden aan een ILB toegewezen virtueel netwerk interne adres.  Als deze eigenschap wordt in plaats daarvan ingesteld op 2, gerelateerd alleen de FTP-service poorten (zowel besturings- als kanalen) wordt gebonden aan een ILB-adres, terwijl het HTTP/HTTPS-verkeer op het openbare VIP blijft.
* *dnsSuffix*: deze parameter bepaalt de standaard-hoofddomein dat wordt toegewezen aan de as-omgeving.  In de openbare variatie van Azure App Service, het hoofddomein standaard voor alle web-apps is *azurewebsites.net*.  Maar omdat een ILB-as-omgeving voor het virtuele netwerk van een klant interne, niet dat u de openbare dienst standaard hoofddomein gebruiken.  Een as ILB-omgeving moet in plaats daarvan een standaard-hoofddomein die zinvol voor gebruik binnen een bedrijf intern virtueel netwerk hebben.  Bijvoorbeeld, een hypothetische Contoso Corporation gebruiken een hoofddomein standaard van *interne contoso.com* voor apps die zijn bedoeld om alleen worden omgezet en toegankelijk zijn in het virtuele netwerk van Contoso. 
* *ipSslAddressCount*: deze parameter is automatisch standaard ingesteld op een waarde van 0 in de *azuredeploy.json* bestand omdat ILB ASEs slechts één ILB-adres hebben.  Er zijn geen expliciete SSL IP-adressen voor een as ILB-omgeving, en daarom de IP-SSL-adresgroep voor een as ILB-omgeving moet worden ingesteld op nul, anders een inrichting fout zal optreden. 

Eenmaal de *azuredeploy.parameters.json* bestand voor een as ILB-omgeving is ingevuld, de as ILB-omgeving kunnen vervolgens worden gemaakt met het volgende Powershell-codefragment.  Het bestand paden overeen waar de Azure Resource Manager-sjabloon-bestanden bevinden zich op uw computer te wijzigen.  Ook moet u uw eigen waarden voor de naam van de Azure Resource Manager deployment en de naam van resourcegroep.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Nadat de Azure Resource Manager sjabloon wordt ingediend duurt het enkele uren voor de ILB as-omgeving moet worden gemaakt.  Nadat het maken is voltooid, wordt de as ILB-omgeving in de portal UX in de lijst met App Service-omgevingen voor het abonnement dat de implementatie geactiveerd weergegeven.

## <a name="uploading-and-configuring-the-default-ssl-certificate"></a>Uploaden en configureren van het SSL-certificaat 'Standaard'
Zodra de as ILB-omgeving is gemaakt, moet een SSL-certificaat worden gekoppeld aan het as-omgeving zoals 'standaard' SSL-certificaat voor SSL-verbindingen naar apps.  U doorgaat met het voorbeeld hypothetische Contoso Corporation als van de as-omgeving standaard DNS-achtervoegsel is *interne contoso.com*, klikt u vervolgens een verbinding met *https://some-random-app.internal-contoso.com* vereist een SSL-certificaat is geldig voor **.internal contoso.com*. 

Er zijn tal van manieren om een geldig SSL-certificaat waaronder interne CA's, het aanschaffen van een certificaat van een externe verlener en met behulp van een zelfondertekend certificaat verkrijgen.  Ongeacht de bron van het SSL-certificaat moeten de volgende kenmerken van de certificaten correct worden geconfigureerd:

* *Onderwerp*: dit kenmerk moet worden ingesteld op **.your-hoofdmap-domain-here.com*
* *Alternatieve onderwerpnaam*: dit kenmerk moet bevatten beide **.your-hoofdmap-domain-here.com*, en **.scm.your-hoofdmap-domain-here.com*.  De reden voor de tweede vermelding is dat SSL-verbindingen naar de SCM/Kudu-site die is gekoppeld aan elke app worden gemaakt met een adres van het formulier *your-app-name.scm.your-root-domain-here.com*.

Met een geldig SSL-certificaat in de hand, worden twee aanvullende voorbereidende stappen uitgevoerd.  Het SSL-certificaat moet worden geconverteerd en opgeslagen als een .pfx-bestand.  Houd er rekening mee dat het .pfx-bestand moet alle tussenliggende bevatten en de hoofd-certificaten en ook moet worden beveiligd met een wachtwoord.

Het resulterende pfx-bestand moet worden geconverteerd naar een base64-tekenreeks omdat het SSL-certificaat worden geüpload met een Azure Resource Manager-sjabloon.  Aangezien Azure Resource Manager-sjablonen tekstbestanden zijn, wordt het .pfx-bestand moet worden geconverteerd naar een base64-tekenreeks zodat deze opgenomen als een parameter van de sjabloon worden kan.

Het onderstaande codefragment Powershell toont een voorbeeld van een zelfondertekend certificaat genereren, exporteren van het certificaat als een .pfx-bestand voor het converteren van het pfx-bestand in een met base64-gecodeerde tekenreeks en vervolgens opslaan van het base64-gecodeerde tekenreeks naar een afzonderlijk bestand.  De Powershell-code voor base64-codering is gebaseerd op de [Powershell-Scripts Blog][examplebase64encoding].

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     

    $fileContentBytes = get-content -encoding byte $fileName
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    $fileContentEncoded | set-content ($fileName + ".b64")

Zodra het SSL-certificaat is is gegenereerd en geconverteerd naar een base64-gecodeerde tekenreeks, de sjabloon van de Azure Resource Manager voorbeeld op GitHub voor [het standaard SSL-certificaat configureren] [ configuringDefaultSSLCertificate] kan worden gebruikt.

De parameters in de *azuredeploy.parameters.json* bestand worden hieronder weergegeven:

* *appServiceEnvironmentName*: de naam van de as ILB-omgeving wordt geconfigureerd.
* *existingAseLocation*: tekenreeks met de Azure-regio waar de as ILB-omgeving is geïmplementeerd.  Bijvoorbeeld: 'Zuid-centraal VS'.
* *pfxBlobString*: de based64 gecodeerd tekenreeksweergave van het pfx-bestand.  Met behulp van het bovenstaande codefragment, zou u de tekenreeks in 'exportedcert.pfx.b64' kopieert en plakt u deze in als de waarde van de *pfxBlobString* kenmerk.
* *wachtwoord*: het wachtwoord dat wordt gebruikt voor het beveiligen van het pfx-bestand.
* *certificateThumbprint*: vingerafdruk van het certificaat.  Als u deze waarde wordt opgehaald uit Powershell (bijvoorbeeld *$certificate. Vingerafdruk* van het vorige codefragment), kunt u de waarde-is.  Echter als u de waarde van het dialoogvenster Windows-certificaat kopieert, moet u het verwijderen van overbodige spaties.  De *certificateThumbprint* moet als volgt uitzien: AF3143EB61D43F6727842115BB7F17BBCECAECAE
* *Certificaatnaam*: een beschrijvende tekenreeks-id van uw eigen keuze die worden gebruikt voor de identiteit van het certificaat.  De naam wordt gebruikt als onderdeel van de unieke id van de Azure Resource Manager voor de *Microsoft.Web/certificates* entiteit geeft het SSL-certificaat.  De naam van de **moet** eindigen met het volgende achtervoegsel: \_yourASENameHere_InternalLoadBalancingASE.  Dit achtervoegsel wordt gebruikt door de portal als een indicatie dat het certificaat wordt gebruikt voor het beveiligen van een as ingeschakeld met een ILB-omgeving.

Een afgekorte voorbeeld van *azuredeploy.parameters.json* worden hieronder weergegeven:

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

Eenmaal de *azuredeploy.parameters.json* bestand is ingevuld, wordt het standaard SSL-certificaat kan worden geconfigureerd met het volgende Powershell-codefragment.  Het bestand paden overeen waar de Azure Resource Manager-sjabloon-bestanden bevinden zich op uw computer te wijzigen.  Ook moet u uw eigen waarden voor de naam van de Azure Resource Manager deployment en de naam van resourcegroep.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Nadat de Azure Resource Manager-sjabloon is ingediend dat het duurt ongeveer 40 minuten minuten per as-front-omgeving de wijziging toepassen.  Bijvoorbeeld, met een standaard aangepast as-omgeving met behulp van twee front-ends, duurt de sjabloon ongeveer één uur en 20 minuten om te voltooien.  Terwijl de sjabloon die wordt uitgevoerd de as-omgeving kan niet worden geschaald.  

Nadat de sjabloon is voltooid, apps op de as ILB-omgeving kunnen worden geopend via HTTPS en de verbindingen worden beveiligd met het standaard SSL-certificaat.  Het standaard SSL-certificaat wordt gebruikt wanneer de apps op de as ILB-omgeving met een combinatie van naam van de toepassing plus de hostnaam van de standaard worden aangepakt.  Bijvoorbeeld *https://mycustomapp.internal-contoso.com* zou gebruiken de standaard SSL-certificaat voor **.internal contoso.com*.

Echter, net als bij apps die worden uitgevoerd op de openbare multitenant-service, ontwikkelaars kunnen ook aangepaste hostnamen voor afzonderlijke apps configureren, en configureer vervolgens unieke SNI-SSL-certificaatbindingen voor afzonderlijke apps.  

## <a name="getting-started"></a>Aan de slag
Om aan de slag met App Service-omgevingen, Zie [Inleiding tot de App Service-omgeving](app-service-app-service-environment-intro.md)

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-create/
[examplebase64encoding]: http://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/ 

