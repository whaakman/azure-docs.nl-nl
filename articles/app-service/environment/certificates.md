---
title: Certificaten en de Azure App Service-omgeving
description: Groot aantal onderwerpen met betrekking tot certificaten op een as-omgeving uitgelegd
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 9e21a7e4-2436-4e81-bb05-4a6ba70eeaf7
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2018
ms.author: ccompy
ms.openlocfilehash: 3d417d560d8a88100f31def27c7db5f9b2493062
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47167919"
---
# <a name="certificates-and-the-app-service-environment"></a>Certificaten en de App Service-omgeving 

De App Service Environment (ase) is een implementatie van de Azure App Service die wordt uitgevoerd in uw Azure virtuele netwerk (vnet). Deze kan worden geïmplementeerd met een internet toegankelijk toepassingseindpunt of een toepassingseindpunt dat in uw VNet. Als u de as-omgeving met een eindpunt dat toegankelijk is internet implementeert, wordt deze implementatie een externe as-omgeving genoemd. Als u de as-omgeving met een eindpunt in uw VNet implementeert, wordt deze implementatie een ILB as-omgeving genoemd. U kunt meer informatie over de ILB as-omgeving van de [maken en gebruiken een ILB as-omgeving](https://docs.microsoft.com/azure/app-service/environment/create-ilb-ase) document.

De as-omgeving is een systeem met één tenant. Omdat het is één tenant, zijn er enkele functies alleen beschikbaar voor een as-omgeving die niet beschikbaar in de App Service met meerdere tenants. 

## <a name="ilb-ase-certificates"></a>Certificaten van de ILB as-omgeving 

Als u van een externe as-omgeving gebruikmaakt, worden uw apps bereikt op [toepassingsnaam]. [asename]. p.azurewebsites.net. Standaard worden alle as-omgevingen, zelfs ILB as-omgevingen, gemaakt met certificaten die volgt u deze indeling. Wanneer u een ILB as-omgeving hebt, zijn de apps op basis van de domeinnaam die u opgeeft bij het maken van de ILB as-omgeving bereikt. In de volgorde voor de apps ter ondersteuning van SSL, moet u certificaten uploaden. Een geldig SSL-certificaat met behulp van interne certificeringsinstanties, aanschaffen van een certificaat van een externe gebruiker of een zelfondertekend certificaat verkrijgen. 

Er zijn twee opties voor het configureren van certificaten met uw ILB as-omgeving.  U kunt een certificaat met jokertekens standaard ingesteld voor de ILB as-omgeving of certificaten instellen op de afzonderlijke web-apps in de as-omgeving.  Ongeacht de keuze die u maakt, moeten de volgende certificaatkenmerken juist zijn geconfigureerd:

- **Onderwerp:** dit kenmerk moet worden ingesteld op *. [ uw-root-domein-here] voor een certificaat met jokertekens ILB as-omgeving. Als u het certificaat voor uw app maakt, moet dit [toepassingsnaam] zijn. [uw-root-domein-here]
- **Alternatieve naam voor onderwerp:** dit kenmerk moet bevatten zowel *. [ uw-root-domein-here] en *.scm. [uw-root-domein-here] voor het certificaat met jokertekens ILB as-omgeving. Als u het certificaat voor uw app maakt, moet dit [toepassingsnaam] zijn. [uw-root-domein-here] en [toepassingsnaam] .scm. [uw-root-domein-here].

Als een derde variant, kunt u een ILB as-omgeving-certificaat dat al uw afzonderlijke app-namen in de SAN van het certificaat in plaats van met een jokertekenverwijzing bevat. Het probleem met deze methode is dat u wilt weten van tevoren de namen van de apps die u wilt opslaan in de as-omgeving of moet u het certificaat van de ILB as-omgeving telkens bijgewerkt.

### <a name="upload-certificate-to-ilb-ase"></a>Certificaat uploaden naar de ILB as-omgeving 

Nadat een ILB as-omgeving is gemaakt in de portal, kan het certificaat moet worden ingesteld voor de ILB as-omgeving. Totdat het certificaat is ingesteld, ziet de as-omgeving een banner dat het certificaat niet is ingesteld.  

Het certificaat dat u uploadt, moet een pfx-bestand. Nadat het certificaat is geüpload, wordt een schaalbewerking voor het instellen van het certificaat in de as-omgeving uitvoeren. 

U kunt geen de as-omgeving maken en uploaden van het certificaat als één actie in de portal of zelfs in één sjabloon. Als een aparte actie, kunt u uploaden van het certificaat met behulp van een sjabloon, zoals beschreven in de [een as-omgeving maken met een sjabloon](./create-from-template.md) document.  

Als u maken van een zelfondertekend certificaat wilt voor het snel testen, kunt u het volgende deel van de PowerShell gebruiken:

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     


## <a name="application-certificates"></a>Toepassingscertificaten 

Apps die worden gehost in een as-omgeving kunnen de app-georiënteerde certificaatfuncties die beschikbaar in de App Service met meerdere tenants zijn gebruiken. Deze functies zijn onder andere:  

- SNI-certificaten 
- IP-gebaseerd SSL, dit wordt alleen ondersteund met een externe as-omgeving.  Een ILB as-omgeving biedt geen ondersteuning voor SSL op basis van IP.
- Gehost KeyVault-certificaten 

De instructies voor het uploaden en beheren van deze certificaten zijn beschikbaar in de App Service SSL-zelfstudie https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl.  Als u gewoon certificaten zodat deze overeenkomt met een aangepaste domeinnaam die u hebt toegewezen aan uw web-app configureert, wordt deze instructies voldoende. Als u het certificaat voor een web-app van de ILB as-omgeving met de naam van het standaarddomein uploadt, geeft u de scm-site in de SAN van het certificaat als u eerder hebt genoteerd. 

## <a name="tls-settings"></a>TLS-instellingen 

U kunt de TLS-instelling configureren op het niveau van een app.  

## <a name="private-client-certificate"></a>Persoonlijke clientcertificaat 

Een gebruikelijk is uw app configureren als een client op een client-servermodel. Als u uw server met een persoonlijke CA-certificaat beveiligen, moet u het certificaat uploaden naar uw app.  De volgende instructies wordt geladen certificaten naar de truststore van de werknemers die uw app wordt uitgevoerd. Als u het certificaat voor één app laadt, kunt u deze gebruiken met uw andere apps in hetzelfde App Service-plan zonder dat het certificaat opnieuw te uploaden.

Het certificaat uploaden naar uw app in de as-omgeving:

1. Genereren van een *.cer* -bestand voor uw certificaat. 
2. Ga naar de app die u het certificaat in de Azure-portal moet
3. Ga naar de SSL-instellingen in de app. Klik op certificaat uploaden. Selecteer openbaar. Selecteer de lokale computer. Geef een naam op. Blader en selecteer uw *.cer* bestand. Selecteer uploaden. 
4. Kopieer de vingerafdruk.
5. Ga naar de toepassingsinstellingen. Maak een App-instelling WEBSITE_LOAD_ROOT_CERTIFICATES met de vingerafdruk van het als de waarde. Als u meerdere certificaten hebt, kunt u ze in dezelfde instelling gescheiden door komma's en geen witruimte zoals plaatsen 

    84EC242A4EC7957817B8E48913E50953552DAFA6, 6A5C65DC9247F762FE17BF8D4906E04FE6B31819

Het certificaat kan worden door alle apps in hetzelfde app service-abonnement als de app, die geconfigureerd die instelling. Als u deze alleen beschikbaar voor apps in een andere App Service-plan nodig hebt, moet u de bewerking van de App-instelling in een app in het App Service-plan te herhalen. Als u wilt controleren of het certificaat is ingesteld, gaat u naar de Kudu-console en uitgeven van deze opdracht dir cert: \localmachine\root in de PowerShell-console voor foutopsporing. 

Als u wilt testen, kunt u een zelfondertekend certificaat maken en genereren van een *.cer* bestand met de volgende PowerShell: 

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.cer"
    export-certificate -Cert $certThumbprint -FilePath $fileName -Type CERT

