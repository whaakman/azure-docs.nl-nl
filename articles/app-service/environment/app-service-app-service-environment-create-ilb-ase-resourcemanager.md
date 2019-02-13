---
title: App Service-ILB as-omgeving met behulp van Azure Resource Manager-sjablonen - maken | Microsoft Docs
description: Informatie over het maken van een interne load balancer as-omgeving met behulp van Azure Resource Manager-sjablonen.
services: app-service
documentationcenter: ''
author: stefsch
manager: nirma
editor: ''
ms.assetid: 091decb6-b0de-42a1-9f2f-c18d9b2e67df
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: stefsch
ms.custom: seodec18
ms.openlocfilehash: 35e0dc5dabaf1602b87ec6a8be86ed609f3ea12f
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56107375"
---
# <a name="how-to-create-an-ilb-ase-using-azure-resource-manager-templates"></a>Een ILB ASE maken met behulp van Azure Resource Manager-sjablonen

> [!NOTE] 
> In dit artikel gaat over de App Service Environment v1. Er is een nieuwere versie van de App Service Environment die gebruiksvriendelijker en wordt uitgevoerd op een krachtigere infrastructuur. Voor meer informatie over de nieuwe versie begin met het [Inleiding tot App Service Environment](intro.md).
>

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="overview"></a>Overzicht
App Service-omgevingen kunnen worden gemaakt met een intern virtueel netwerk-adres in plaats van een openbare VIP-adres.  Deze interne adres wordt geleverd door een Azure-onderdeel de interne load balancer (ILB) genoemd.  Een ILB as-omgeving kunnen worden gemaakt met behulp van de Azure portal.  Ook kunnen worden gemaakt met automation via Azure Resource Manager-sjablonen.  Dit artikel helpt bij de stappen en de syntaxis voor het maken van een ILB as-omgeving met Azure Resource Manager-sjablonen.

Er zijn drie stappen die betrokken zijn bij het maken van een ILB as-omgeving automatiseren:

1. Eerst wordt de base as-omgeving gemaakt in een virtueel netwerk met behulp van een interne load balancer-adres in plaats van een openbare VIP-adres.  Als onderdeel van deze stap wordt een hoofddomeinnaam op toegewezen aan de ILB as-omgeving.
2. Zodra de ILB as-omgeving is gemaakt, wordt een SSL-certificaat geüpload.  
3. Het geüploade SSL-certificaat is expliciet toegewezen aan de ILB as-omgeving als de 'standaard' SSL-certificaat.  Dit SSL-certificaat wordt gebruikt voor SSL-verkeer naar apps op de ILB as-omgeving wanneer de apps worden aangepakt met behulp van de algemene hoofddomein (bijvoorbeeld) toegewezen aan de as-omgeving https://someapp.mycustomrootcomain.com)

## <a name="creating-the-base-ilb-ase"></a>Het maken van de Base ILB as-omgeving
Een voorbeeld van Azure Resource Manager-sjabloon en de bijbehorende parameters-bestand zijn beschikbaar op GitHub [hier][quickstartilbasecreate].

De meeste van de parameters in de *azuredeploy.parameters.json* bestand gemeenschappelijk zijn voor het maken van zowel ILB as-omgevingen, evenals as-omgevingen gebonden aan een openbare VIP-adres.  De lijst hieronder aanroepen speciale Opmerking uitvoerparameters of die zijn uniek, bij het maken van een ILB as-omgeving:

* *internalLoadBalancingMode*:  In de meeste gevallen set wordt dit tot 3, wat betekent dat zowel HTTP/HTTPS-verkeer op poort 80/443 en het besturingselement/gegevenskanaal poorten hebben geluisterd naar door de FTP-service op de as-omgeving, worden gekoppeld aan een ILB intern virtueel netwerk-adres toegewezen.  Als deze eigenschap wordt in plaats daarvan ingesteld op 2, klikt u vervolgens gerelateerde alleen de FTP-service poorten (zowel controle en kanalen) wordt gebonden aan een ILB-adres, terwijl het HTTP/HTTPS-verkeer op het openbare VIP-adres blijft.
* *dnsSuffix*:  Deze parameter bepaalt de standaard-hoofddomein dat wordt toegewezen aan de as-omgeving.  In de openbare variatie van Azure App Service biedt het hoofddomein standaard voor alle web-apps is *azurewebsites.net*.  Maar omdat een ILB as-omgeving intern voor het virtuele netwerk van een klant is, het niet verstandig om te gebruiken van de openbare service standaard-hoofddomein.  Een ILB as-omgeving moet in plaats daarvan een standaard-hoofddomein die zinvol voor gebruik binnen een bedrijf intern virtueel netwerk hebben.  Bijvoorbeeld, een hypothetische Contoso Corporation gebruiken een standaard-hoofddomein van *interne contoso.com* voor apps die zijn bedoeld om alleen worden omgezet en toegankelijk is in het virtuele netwerk van Contoso. 
* *ipSslAddressCount*:  Deze parameter is automatisch standaard ingesteld op een waarde van 0 in de *azuredeploy.json* omdat ILB as-omgevingen hebben slechts één ILB-adres.  Er zijn geen expliciete IP-SSL-adressen voor een ILB as-omgeving, en kan daarom de IP-SSL-adresgroep voor een ILB as-omgeving moet worden ingesteld op nul, anders een inrichting fout wordt uitgevoerd. 

Zodra de *azuredeploy.parameters.json* bestand is ingevuld voor een ILB as-omgeving, de ILB as-omgeving kunnen vervolgens worden gemaakt met het volgende Powershell-codefragment.  Wijzigen van het bestand paden zodat deze overeenkomen met waar de Azure Resource Manager-sjabloonbestanden bevinden zich op uw computer.  Vergeet ook niet op te geven van uw eigen waarden voor de implementatienaam van Azure Resource Manager en de naam van de resourcegroep.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Nadat de Azure Resource Manager sjabloon wordt ingediend duurt een paar uur voor de ILB as-omgeving moet worden gemaakt.  Nadat het maken is voltooid, wordt de ILB as-omgeving weergegeven in de UX-portal in de lijst met App Service-omgevingen voor het abonnement waarmee de implementatie is geactiveerd.

## <a name="uploading-and-configuring-the-default-ssl-certificate"></a>Uploaden en het 'Standaard' SSL-certificaat configureren
Zodra de ILB as-omgeving is gemaakt, moet een SSL-certificaat worden gekoppeld aan de as-omgeving als de 'standaard' SSL-certificaat gebruiken voor het tot stand brengen van SSL-verbindingen met apps.  Verder met het voorbeeld hypothetische Contoso Corporation als van de as-omgeving standaard DNS-achtervoegsel is *interne contoso.com*, klikt u vervolgens een verbinding met *https://some-random-app.internal-contoso.com* een SSL-certificaat dat is vereist geldig voor **.internal contoso.com*. 

Er zijn tal van manieren om een geldig SSL-certificaat met inbegrip van de interne CA's, aanschaffen van een certificaat van een externe gebruiker en het gebruik van een zelfondertekend certificaat verkrijgen.  De volgende certificaatkenmerken moeten juist zijn geconfigureerd, ongeacht wat de bron van het SSL-certificaat is:

* *Onderwerp*:  Dit kenmerk moet worden ingesteld op **.uw-root-domein-here.com*
* *Alternatieve naam voor onderwerp*:  Dit kenmerk moet bevatten zowel **.uw-root-domein-here.com*, en **.Hier-root-domein-here.com*.  De reden voor de tweede vermelding is dat SSL-verbindingen met de SCM/Kudu-site die is gekoppeld aan elke app worden uitgevoerd met een adres van het formulier *your-app-name.scm.your-root-domain-here.com*.

Met een geldig SSL-certificaat in voorraad, zijn twee aanvullende voorbereidende stappen nodig.  Het SSL-certificaat moet worden geconverteerd/opgeslagen als een pfx-bestand.  Houd er rekening mee dat het pfx-bestand moet alle tussenliggende opnemen en basiscertificaten en moet ook zijn beveiligd met een wachtwoord.

Het resulterende pfx-bestand moet worden geconverteerd naar een Base 64-tekenreeks omdat het SSL-certificaat worden geüpload met behulp van een Azure Resource Manager-sjabloon.  Omdat Azure Resource Manager-sjablonen tekstbestanden zijn, wordt het pfx-bestand moet worden geconverteerd naar een Base 64-tekenreeks, zodat deze opgenomen als een parameter van de sjabloon worden kan.

Het onderstaande codefragment Powershell toont een voorbeeld van een zelfondertekend certificaat genereren en exporteren van het certificaat als een pfx-bestand converteren van het pfx-bestand in een met base64 gecodeerde tekenreeks en vervolgens opslaan van de met base64 gecodeerde tekenreeks in een afzonderlijk bestand.  De Powershell-code voor base64-codering is gebaseerd op de [Powershell-Scripts Blog][examplebase64encoding].

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     

    $fileContentBytes = get-content -encoding byte $fileName
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    $fileContentEncoded | set-content ($fileName + ".b64")

Zodra het SSL-certificaat is is gegenereerd en geconverteerd naar een met base64 gecodeerde tekenreeks, het voorbeeld van Azure Resource Manager-sjabloon op GitHub voor [configureren van het standaard SSL-certificaat] [ configuringDefaultSSLCertificate] kan worden gebruikt.

De parameters in de *azuredeploy.parameters.json* bestand worden hieronder vermeld:

* *appServiceEnvironmentName*:  De naam van de ILB as-omgeving wordt geconfigureerd.
* *existingAseLocation*:  De tekenreeks met de Azure-regio waar de ILB as-omgeving is geïmplementeerd.  Bijvoorbeeld:  'Zuid-centraal VS'.
* *pfxBlobString*:  De based64 gecodeerd tekenreeksweergave van het pfx-bestand.  Met behulp van het bovenstaande codefragment, zou u de tekenreeks in 'exportedcert.pfx.b64' kopiëren en plak deze in als de waarde van de *pfxBlobString* kenmerk.
* *wachtwoord*:  Het wachtwoord dat wordt gebruikt voor het beveiligen van het pfx-bestand.
* *certificateThumbprint*:  De vingerafdruk voor het certificaat.  Als u deze waarde wordt opgehaald vanuit Powershell (bijvoorbeeld *$certificate. Vingerafdruk* uit het vorige codefragment), kunt u de waarde-is.  Echter als u de waarde in het dialoogvenster Windows-certificaat kopieert, vergeet niet het verwijderen van de overbodige spaties.  De *certificateThumbprint* moet er ongeveer zo uitzien:  AF3143EB61D43F6727842115BB7F17BBCECAECAE
* *certificateName*:  Een beschrijvende tekenreeks-id van uw eigen kiezen die aan identiteit het certificaat wordt gebruikt.  De naam wordt gebruikt als onderdeel van de unieke id van de Azure Resource Manager voor de *Microsoft.Web/certificates* entiteit voor het SSL-certificaat.  De naam van de **moet** eindigen met het volgende achtervoegsel: \_yourASENameHere_InternalLoadBalancingASE.  Dit achtervoegsel wordt gebruikt door de portal als een indicator dat het certificaat wordt gebruikt voor het beveiligen van een ASE met ILB ingeschakeld.

Een verkorte voorbeeld van *azuredeploy.parameters.json* wordt hieronder weergegeven:

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

Zodra de *azuredeploy.parameters.json* bestand zijn ingevuld, wordt het standaard SSL-certificaat kan worden geconfigureerd met behulp van de volgende Powershell-codefragment.  Wijzigen van het bestand paden zodat deze overeenkomen met waar de Azure Resource Manager-sjabloonbestanden bevinden zich op uw computer.  Vergeet ook niet op te geven van uw eigen waarden voor de implementatienaam van Azure Resource Manager en de naam van de resourcegroep.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Nadat de Azure Resource Manager sjabloon wordt ingediend duurt het ongeveer 40 minuten per ASE front-end de wijziging toepassen.  Bijvoorbeeld, met een standaard aangepast as-omgeving met behulp van twee front-ends, duurt de sjabloon ongeveer één uur en 20 minuten om te voltooien.  Terwijl de sjabloon wordt uitgevoerd kunnen de as-omgeving worden niet geschaald.  

Nadat de sjabloon is voltooid, apps van de ILB as-omgeving is toegankelijk via HTTPS en de verbindingen worden beveiligd met behulp van het standaard SSL-certificaat.  Het standaard SSL-certificaat wordt gebruikt wanneer de apps op de ILB as-omgeving worden aangepakt met behulp van een combinatie van naam van de toepassing, plus de standaardhostnaam.  Bijvoorbeeld *https://mycustomapp.internal-contoso.com* zou gebruiken de standaard-SSL-certificaat voor **.internal contoso.com*.

Echter, net als bij apps die worden uitgevoerd op de openbare multitenant-service, ontwikkelaars kunnen ook zo configureren dat aangepaste hostnamen voor afzonderlijke apps, en configureer vervolgens unieke SNI SSL-certificaatbindingen voor afzonderlijke apps.  

## <a name="getting-started"></a>Aan de slag
Als u wilt aan de slag met App Service-omgevingen, Zie [Inleiding tot App Service-omgeving](app-service-app-service-environment-intro.md)

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-create/
[examplebase64encoding]: https://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/ 

