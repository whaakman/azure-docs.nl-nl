---
title: Verificatiemethoden voor Azure Security Center voor IoT-Preview | Microsoft Docs
description: Meer informatie over de diverse verificatiemethoden die beschikbaar is bij het gebruik van de Azure Security Center voor IoT-service.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 10b38f20-b755-48cc-8a88-69828c17a108
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: d1f9d77d1f87e8d201e2cb034401bb3cae14f41b
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58862910"
---
# <a name="security-agent-authentication-methods"></a>Security agent verificatiemethoden 

> [!IMPORTANT]
> Azure Security Center voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt geleverd zonder een service level agreement, en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

In dit artikel wordt uitgelegd dat de diverse verificatiemethoden die u met de agent AzureIoTSecurity gebruiken kunt om te verifiëren met de IoT-Hub.

Een beveiligingsmodule is vereist voor de onboarding voor elk apparaat naar Azure Security Center (ASC) voor IoT in de IoT-Hub. Voor de verificatie van het apparaat heeft kunt ASC voor IoT gebruiken een van twee methoden. Kies de methode die geschikt is voor uw bestaande IoT-oplossing. 

> [!div class="checklist"]
> * Module-optie voor beveiliging
> * Apparaat-optie

## <a name="authentication-methods"></a>Verificatiemethoden

De twee methoden voor de agent AzureIoTSecurity verificatie uitvoeren:

 - **Module** -verificatiemodus<br>
   De Module wordt geverifieerd onafhankelijk van het dubbele apparaat.
   Gebruik dit verificatietype als u de beveiligingsagent op een toegewezen verificatiemethode via security module (alleen symmetrische sleutel) te gebruiken.
        
 - **Apparaat** -verificatiemodus<br>
    Bij deze methode is de security-agent eerst worden geverifieerd aan de apparaat-id. Na de initiële verificatie de ASC voor IoT-agent wordt uitgevoerd een **REST** aanroepen naar de IoT-Hub met behulp van de REST-API met de verificatiegegevens van het apparaat. De ASC voor IoT-agent vervolgens vraagt u de verificatiemethode voor netwerkbeveiliging module en de gegevens van de IoT-Hub. In de laatste stap voert de ASC voor IoT-agent een verificatie op basis van de ASC voor IoT-module.
    
    Gebruik dit verificatietype als u de beveiligingsagent als u een bestaande verificatiemethode voor het apparaat opnieuw wilt (zelfondertekend certificaat of de symmetrische sleutel). 

Zie [Security agent installatieparameters](#security-agent-installation-parameters) voor meer informatie over het configureren van.
                                
## <a name="authentication-methods-known-limitations"></a>Bekende beperkingen verificatiemethoden

- **Module** verificatiemodus biedt alleen ondersteuning voor verificatie met een symmetrische sleutel.
- Door CA ondertekend certificaat wordt niet ondersteund door **apparaat** -verificatiemodus.  

## <a name="security-agent-installation-parameters"></a>Installatieparameters Security-agent

Wanneer [implementeren van een beveiligingsagent](how-to-deploy-agent.md), details van de verificatie moeten worden opgegeven als argumenten.
Deze argumenten worden beschreven in de volgende tabel.


|Parameter|Description|Opties|
|---------|---------------|---------------|
|**identity**|Verificatiemodus| **Module** of **apparaat**|
|**type**|Verificatietype|**SymmetricKey** of **SelfSignedCertificate**|
|**filePath**|Absolute volledige pad naar het bestand met het certificaat of de symmetrische sleutel| |
|**gatewayHostname**|FQDN-naam van de IoT-Hub|Voorbeeld: ContosoIotHub.azure-devices.net|
|**deviceId**|Apparaat-id|Voorbeeld: MyDevice1|
|**certificateLocationKind**|Opslaglocatie van certificaat|**LokaalBestand** of **Store**|


Wanneer u het installatiescript van de security-agent gebruikt, wordt de volgende configuratie wordt automatisch uitgevoerd.

Als u wilt de beveiligingsverificatie van de agent handmatig bewerken, bewerken de config-bestand. 

## <a name="change-authentication-method-after-deployment"></a>Verificatiemethode wijzigen na de implementatie

Bij het implementeren van een beveiligingsagent met een script voor installatie, wordt er automatisch een configuratiebestand gemaakt.

Als u wilt wijzigen verificatiemethoden na de implementatie, is bij handmatig bewerken van het configuratiebestand vereist.


### <a name="c-based-security-agent"></a>C#-op basis van de security-agent

Bewerken _Authentication.config_ met de volgende parameters:

```xml
<Authentication>
  <add key="deviceId" value=""/>
  <add key="gatewayHostname" value=""/>
  <add key="filePath" value=""/>
  <add key="type" value=""/>
  <add key="identity" value=""/>
  <add key="certificateLocationKind" value="" />
</Authentication>
```

### <a name="c-based-security-agent"></a>Beveiliging op basis van de C agent

Bewerken _LocalConfiguration.json_ met de volgende parameters:

```json
"Authentication" : {
    "Identity" : "",
    "AuthenticationMethod" : "",
    "FilePath" : "",
    "DeviceId" : "",
    "HostName" : ""
}
```

## <a name="see-also"></a>Zie ook
- [Beveiligingsoverzicht van agents](security-agent-architecture.md)
- [Security-agent implementeren](how-to-deploy-agent.md)
- [Toegang tot onbewerkte beveiligingsgegevens](how-to-security-data-access.md)
