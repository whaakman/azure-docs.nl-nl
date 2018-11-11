---
title: Cloud Services en beheercertificaten | Microsoft Docs
description: Meer informatie over het maken en gebruiken van certificaten met Microsoft Azure
services: cloud-services
documentationcenter: .net
author: jpconnock
manager: timlt
editor: ''
ms.assetid: fc70d00d-899b-4771-855f-44574dc4bfc6
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeconnoc
ms.openlocfilehash: 9cf37e611dce5705a4c866f25afa59e5c1602ec4
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51282200"
---
# <a name="certificates-overview-for-azure-cloud-services"></a>Overzicht van certificaten voor Azure Cloud Services
Certificaten worden gebruikt in Azure cloud Services ([service-certificaten](#what-are-service-certificates)) en om te verifiëren bij de beheer-API ([beheercertificaten](#what-are-management-certificates)). In dit onderwerp biedt een algemeen overzicht van beide typen certificaten hoe naar [maken](#create) en [implementeren](#deploy) ze naar Azure.

Certificaten die worden gebruikt in Azure zijn x.509 v3-certificaten en kunnen worden ondertekend door een andere vertrouwde certificaat of ze zijn zelf-ondertekend. Een zelfondertekend certificaat is ondertekend door de maker van een eigen, dus deze wordt niet vertrouwd standaard. De meeste browsers kunnen dit probleem te negeren. U moet alleen zelfondertekende certificaten bij het ontwikkelen en testen van uw cloudservices gebruiken. 

Certificaten die worden gebruikt door Azure kunnen bevatten een particuliere of openbare sleutel. Certificaten hebben een vingerafdruk die biedt een manier om ze te identificeren op ondubbelzinnige wijze. Deze vingerafdruk wordt gebruikt in de Azure [configuratiebestand](cloud-services-configure-ssl-certificate-portal.md) om te bepalen welk certificaat een cloudservice moeten gebruiken. 

>[!Note]
>Azure Cloud Services accepteert geen AES256 SHA256 gecodeerde certificaat.

## <a name="what-are-service-certificates"></a>Wat zijn service-certificaten?
Service-certificaten zijn gekoppeld aan cloud services en inschakelen van beveiligde communicatie naar en van de service. Als u een Webrol hebt geïmplementeerd, wilt u wilt opgeven van een certificaat met een daarvoor beschikbare HTTPS-eindpunt kan worden geverifieerd. Service-certificaten, gedefinieerd in de servicedefinitie van uw, worden automatisch geïmplementeerd op de virtuele machine waarop een exemplaar van uw rol is. 

U kunt service-certificaten te uploaden naar Azure met behulp van de Azure-portal of met behulp van het klassieke implementatiemodel. Service-certificaten zijn gekoppeld aan een bepaalde cloudservice. Ze worden toegewezen aan een implementatie in het servicedefinitiebestand.

Service-certificaten kunnen afzonderlijk worden beheerd vanaf uw services en kunnen worden beheerd door verschillende personen. Een ontwikkelaar kan bijvoorbeeld een servicepakket dat verwijst naar een certificaat dat door een IT-beheerder heeft eerder hebt geüpload naar Azure uploaden. Een IT-beheerder kan beheren en vernieuwen van dat certificaat (wijzigen van de configuratie van de service) zonder te hoeven uploaden van een nieuwe servicepakket. Bijwerken zonder een nieuwe servicepakket is mogelijk omdat de logische naam, de naam van het certificaatarchief en de locatie van het certificaat is in het servicedefinitiebestand en terwijl de certificaatvingerafdruk van het is opgegeven in het configuratiebestand van de service. Voor het bijwerken van het certificaat is alleen nodig voor het uploaden van een nieuw certificaat en de vingerafdrukwaarde in het configuratiebestand van de service te wijzigen.

>[!Note]
>De [Cloud Veelgestelde vragen over](cloud-services-faq.md) artikel vindt u enkele nuttige informatie over certificaten.

## <a name="what-are-management-certificates"></a>Wat zijn certificaten?
Van beheercertificaten kunnen u verifiëren met het klassieke implementatiemodel. Veel programma's en hulpprogramma's (zoals Visual Studio of de Azure SDK) kunt u deze certificaten gebruiken voor het automatiseren van configuratie en implementatie van verschillende Azure-services. Deze niet echt gerelateerd zijn aan cloud services. 

> [!WARNING]
> Wees voorzichtig! Deze typen certificaten toestaan iedereen die met hen verifieert voor het beheren van het abonnement dat ze zijn gekoppeld. 
> 
> 

### <a name="limitations"></a>Beperkingen
Er is een limiet van 100 beheercertificaten per abonnement. Er is ook een limiet van 100 beheercertificaten voor alle abonnementen voor een specifieke service-beheerder gebruikers-ID. Als de gebruikers-ID voor de accountbeheerder al gebruikt is om toe te voegen 100 beheercertificaten en er meer certificaten nodig, kunt u een CO-beheerder om toe te voegen van de extra certificaten toevoegen. 

<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a>Een nieuw zelfondertekend certificaat maken
U kunt een hulpprogramma dat beschikbaar is voor het maken van een zelfondertekend certificaat, zolang ze aan deze instellingen voldoen gebruiken:

* Een X.509-certificaat.
* Een persoonlijke sleutel bevat.
* Voor sleuteluitwisseling (PFX-bestand) gemaakt.
* Naam van het onderwerp moet overeenkomen met het domein dat wordt gebruikt voor toegang tot de service in de cloud.

    > U kunt een SSL-certificaat voor de cloudapp.net kan geen verkrijgen (of voor een Azure-gerelateerde)-domein. naam van het onderwerp van het certificaat moet overeenkomen met de aangepaste domeinnaam gebruikt voor toegang tot uw toepassing. Bijvoorbeeld, **contoso.net**, niet **contoso.cloudapp.net**.

* Minimaal 2048-bits-versleuteling.
* **Service-certificaat alleen**: Client-side-certificaat moet zich bevinden de *persoonlijke* certificaatarchief.

Er zijn twee eenvoudige manieren voor het maken van een certificaat op Windows, met de `makecert.exe` hulpprogramma of IIS.

### <a name="makecertexe"></a>Makecert.exe
Dit hulpprogramma is afgeschaft en wordt niet meer hier beschreven. Zie voor meer informatie, [dit MSDN-artikel](https://msdn.microsoft.com/library/windows/desktop/aa386968).

### <a name="powershell"></a>PowerShell
```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My" -KeyLength 2048 -KeySpec "KeyExchange"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

> [!NOTE]
> Als u gebruikmaken van het certificaat met een IP-adres in plaats van een domein wilt, gebruikt u het IP-adres in de parameter - DnsName.


Als u wilt gebruiken dit [certificaat met de beheerportal](../azure-api-management-certs.md), exporteren naar een **.cer** bestand:

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a>Internet informatieservices (IIS)
Er zijn veel pagina's op het internet die betrekking hebben op hoe u dit doet met IIS. [Hier](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) is een geweldige ik dat ik denk dat het wordt ook uitgelegd gevonden. 

### <a name="linux"></a>Linux
[Dit](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) artikel wordt beschreven hoe u certificaten kunt maken met SSH.

## <a name="next-steps"></a>Volgende stappen
[Uw servicecertificaat uploaden naar Azure portal](cloud-services-configure-ssl-certificate-portal.md).

Upload een [API-beheercertificaat](../azure-api-management-certs.md) naar de Azure-portal.

