---
title: Cloudservices en -beheercertificaten | Microsoft Docs
description: Meer informatie over het maken en gebruiken van certificaten met Microsoft Azure
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: fc70d00d-899b-4771-855f-44574dc4bfc6
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: 4032a429901c675436cb5e7fb04aa5645925fa30
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="certificates-overview-for-azure-cloud-services"></a>Certificaten voor Azure Cloud Services-overzicht
Certificaten worden gebruikt in Azure voor cloudservices ([service certificaten](#what-are-service-certificates)) en voor het verifiëren van de beheer-API ([beheercertificaten](#what-are-management-certificates)). In dit onderwerp biedt een algemeen overzicht van beide typen certificaten hoe naar [maken](#create) en [implementeren](#deploy) ze naar Azure.

Certificaten worden gebruikt in Azure worden x.509 v3-certificaten en kunnen worden ondertekend door een andere vertrouwd certificaat of deze kunnen zelfondertekend zijn. Een zelfondertekend certificaat is ondertekend door de maker van een eigen, dus niet vertrouwd standaard. De meeste browsers kunnen dit probleem negeren. Gebruik alleen zelfondertekende certificaten bij het ontwikkelen en testen van uw cloudservices. 

Certificaten die door Azure kunnen een persoonlijke of een openbare sleutel bevatten. Certificaten hebben een vingerafdruk die biedt een manier om ze te identificeren in een niet-ambigue manier. Deze vingerafdruk wordt gebruikt in de Azure [configuratiebestand](cloud-services-configure-ssl-certificate-portal.md) om te bepalen welk certificaat een cloudservice moeten gebruiken. 

## <a name="what-are-service-certificates"></a>Wat zijn servicecertificaten?
Certificaten van de service zijn gekoppeld aan cloud services en inschakelen van beveiligde communicatie van en naar de service. Als u een Webrol hebt geïmplementeerd, wilt u een certificaat dat kan worden geverifieerd een blootgestelde HTTPS-eindpunt opgeven. Service-certificaten, gedefinieerd in de servicedefinitie van de, worden automatisch geïmplementeerd op de virtuele machine waarop een exemplaar van uw rol. 

U kunt certificaten uploaden naar Azure met behulp van de Azure-portal of met behulp van het klassieke implementatiemodel. Certificaten van de service zijn gekoppeld aan een specifieke cloud-service. Ze worden toegewezen aan een implementatie in het servicedefinitiebestand.

Certificaten kunnen afzonderlijk worden beheerd vanaf uw services en kunnen worden beheerd door andere personen. Een ontwikkelaar kan bijvoorbeeld uploaden van een servicepakket dat verwijst naar een certificaat dat door een IT-beheerder eerder heeft geüpload naar Azure. Een IT-beheerder kan beheren en vernieuwen van dat certificaat (het wijzigen van de configuratie van de service) zonder dat u nodig hebt om een nieuwe servicepakket te uploaden. Bijwerken zonder een nieuw servicepakket is mogelijk omdat de logische naam, de naam van de store en de locatie van het certificaat in het servicedefinitiebestand en terwijl de vingerafdruk van het certificaat is opgegeven in het configuratiebestand van de service. Voor het bijwerken van het certificaat is alleen nodig voor het uploaden van een nieuw certificaat en de vingerafdrukwaarde in het configuratiebestand van de service wijzigen.

>[!Note]
>De [Cloud Services-Veelgestelde vragen over](cloud-services-faq.md) artikel bevat meer informatie over certificaten.

## <a name="what-are-management-certificates"></a>Wat zijn certificaten?
Van beheercertificaten kunnen u verifiëren met het klassieke implementatiemodel. Veel programma's en hulpprogramma's (zoals Visual Studio of de Azure SDK) gebruik van deze certificaten in configuratie en implementatie van verschillende Azure-services te automatiseren. Deze echt niet gerelateerd aan cloud services. 

> [!WARNING]
> Pas op! Deze typen certificaten dat alle gebruikers worden geverifieerd met hen voor het beheren van het abonnement dat ze zijn gekoppeld. 
> 
> 

### <a name="limitations"></a>Beperkingen
Er is een limiet van 100 beheercertificaten per abonnement. Er is ook een limiet van 100 beheercertificaten voor alle abonnementen onder een bepaalde service-beheerder gebruikers-ID. Als de gebruikers-ID voor de accountbeheerder al gebruikt is om toe te voegen 100 beheercertificaten en er meer certificaten nodig, kunt u een CO-beheerder om toe te voegen extra certificaten toevoegen. 

Zie voordat u meer dan 100 certificaten toe te voegen, als u een bestaand certificaat kunt hergebruiken. Met behulp van CO-beheerders wordt mogelijk onnodige complexiteit toegevoegd aan het beheerproces van uw certificaat.

<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a>Een nieuw zelfondertekend certificaat maken
U kunt een hulpmiddel beschikbaar voor het maken van een zelfondertekend certificaat, zolang ze aan deze instellingen voldoen gebruiken:

* Een X.509-certificaat.
* Een persoonlijke sleutel bevat.
* Voor sleuteluitwisseling (PFX-bestand) gemaakt.
* Naam van het onderwerp moet overeenkomen met het domein dat wordt gebruikt voor toegang tot de cloudservice.

    > U kunt een SSL-certificaat voor de cloudapp.net kan geen verkrijgen (of voor een Azure-gerelateerde)-domein. de onderwerpnaam van het certificaat moet overeenkomen met de naam van het aangepaste domein gebruikt voor toegang tot uw toepassing. Bijvoorbeeld: **contoso.net**, niet **contoso.cloudapp.net**.

* Minimaal 2048-bits versleuteling.
* **Service-certificaat alleen**: Client-side-certificaat moet zich bevinden de *persoonlijke* certificaatarchief.

Er zijn twee manieren om te maken van een certificaat op Windows, met de `makecert.exe` hulpprogramma of IIS.

### <a name="makecertexe"></a>MakeCert.exe
Dit hulpprogramma is afgeschaft en wordt niet meer hier beschreven. Zie voor meer informatie [deze MSDN-artikel](https://msdn.microsoft.com/library/windows/desktop/aa386968).

### <a name="powershell"></a>PowerShell
```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My" -KeyLength 2048 -KeySpec "KeyExchange"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

> [!NOTE]
> Als u gebruiken van het certificaat met een IP-adres in plaats van een domein wilt, gebruikt u het IP-adres in de parameter - DnsName.


Als u wilt gebruiken deze [certificaat met de beheerportal](../azure-api-management-certs.md), exporteren naar een **.cer** bestand:

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a>Internet informatieservices (IIS)
Er zijn meerdere pagina's op internet die betrekking hebben op hoe u dit doet met IIS. [Hier](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) is een uitstekende ik heb ik denk dat deze wordt ook uitgelegd. 

### <a name="linux"></a>Linux
[Dit](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) artikel wordt beschreven hoe om certificaten te maken met SSH.

## <a name="next-steps"></a>Volgende stappen
[Uw servicecertificaat uploaden naar de Azure-portal](cloud-services-configure-ssl-certificate-portal.md).

Upload een [beheercertificaat API](../azure-api-management-certs.md) bij de Azure portal.

