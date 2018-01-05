---
title: 'Genereren en exporteren van certificaten voor punt-naar-Site: MakeCert: Azure | Microsoft Docs'
description: In dit artikel bevat stappen om een zelfondertekend basiscertificaat maken, de openbare sleutel niet exporteren en met MakeCert clientcertificaten te genereren.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: 2beacc461370f268e54e1eedcb32939f7c606b14
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-makecert"></a>Genereren en exporteren van certificaten voor punt-naar-Site-verbindingen met het MakeCert

Punt-naar-Site-verbindingen kunt u certificaten voor verificatie gebruiken. In dit artikel laat zien hoe een zelfondertekend basiscertificaat maken en met MakeCert clientcertificaten te genereren. Als u zoekt de configuratiestappen punt-naar-Site, zoals het uploaden van basiscertificaten, selecteert u een van de artikelen ' configureren punt-naar-Site' in de volgende lijst:

> [!div class="op_single_selector"]
> * [Zelfondertekende certificaten - PowerShell maken](vpn-gateway-certificates-point-to-site.md)
> * [Zelfondertekende certificaten - MakeCert maken](vpn-gateway-certificates-point-to-site-makecert.md)
> * [Punt-naar-Site - Resource Manager - Azure-portal configureren](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Punt-naar-Site - Resource Manager - PowerShell configureren](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Punt-naar-Site - Classic - Azure-portal configureren](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Terwijl wordt u aangeraden de [Windows 10 PowerShell stappen](vpn-gateway-certificates-point-to-site.md) voor het maken van uw certificaten, bieden we deze instructies MakeCert als een optionele methode. De certificaten die u genereren met behulp van beide methoden kunnen worden geïnstalleerd op [een ondersteunde client-besturingssysteem](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq). MakeCert heeft echter de volgende beperking:

* MakeCert is afgeschaft. Dit betekent dat dit hulpprogramma op elk moment kan worden verwijderd. Certificaten die u al hebt gegenereerd met MakeCert worden niet beïnvloed wanneer MakeCert niet meer beschikbaar is. MakeCert wordt alleen gebruikt voor het genereren van de certificaten niet als een mechanisme voor het valideren.

## <a name="rootcert"></a>Een zelfondertekend basiscertificaat maken

De volgende stappen ziet u hoe u een zelfondertekend certificaat met MakeCert maakt. Deze stappen zijn niet specifiek implementatiemodel. Ze zijn geldig voor zowel Resource Manager en classic.

1. Download en installeer [MakeCert](https://msdn.microsoft.com/library/windows/desktop/aa386968(v=vs.85).aspx).
2. Na de installatie vindt u doorgaans het hulpprogramma makecert.exe onder dit pad: ' C:\Program Files (x86) \Windows Kits\10\bin\<arch >'. Hoewel het is mogelijk dat deze is geïnstalleerd op een andere locatie. Open een opdrachtprompt als beheerder en navigeer naar de locatie van het hulpprogramma MakeCert. U kunt het volgende voorbeeld aanpassen voor de juiste locatie:

  ```cmd
  cd C:\Program Files (x86)\Windows Kits\10\bin\x64
  ```
3. Maak en installeer een certificaat in het persoonlijke certificaatarchief op uw computer. Het volgende voorbeeld wordt een bijbehorende *.cer* -bestand dat u bij het configureren van P2S uploaden naar Azure. Vervang 'P2SRootCert' en 'P2SRootCert.cer' met de naam die u wilt gebruiken voor het certificaat. Het certificaat bevindt zich in uw 'Certificaten - Huidige gebruiker\Persoonlijk\Certificaten'.

  ```cmd
  makecert -sky exchange -r -n "CN=P2SRootCert" -pe -a sha256 -len 2048 -ss My
  ```

## <a name="cer"></a>Exporteer de openbare sleutel (.cer)

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

Het bestand exported.cer moet worden geüpload naar Azure. Zie voor instructies [een punt-naar-Site-verbinding configureren](vpn-gateway-howto-point-to-site-resource-manager-portal.md#uploadfile). Zie het toevoegen van een extra vertrouwd basiscertificaat [in deze sectie](vpn-gateway-howto-point-to-site-resource-manager-portal.md#add) van het artikel.

### <a name="export-the-self-signed-certificate-and-private-key-to-store-it-optional"></a>Exporteren van het zelfondertekend certificaat en de persoonlijke sleutel voor het opslaan van het (optioneel)

U kunt het zelfondertekende basiscertificaat exporteren en veilig opslaan. Indien nodig zijn, u kunt later op een andere computer installeren en meer clientcertificaten te genereren of een andere cer-bestand exporteren. Als u wilt het zelfondertekende basiscertificaat exporteren als een .pfx-bestand, selecteer het basiscertificaat en gebruik dezelfde stappen zoals beschreven in [exporteren van een clientcertificaat](#clientexport).

## <a name="create-and-install-client-certificates"></a>Maken en clientcertificaten te installeren

U kunt het zelfondertekende certificaat niet installeren rechtstreeks op de clientcomputer. U moet een clientcertificaat genereren uit het zelfondertekende certificaat. U en vervolgens exporteren en installeren van het clientcertificaat voor de clientcomputer. De volgende stappen zijn niet specifiek implementatiemodel. Ze zijn geldig voor zowel Resource Manager en classic.

### <a name="clientcert"></a>Een clientcertificaat genereren

Op elke clientcomputer die via punt-naar-site verbinding maakt met een VNet, moet een clientcertificaat zijn geïnstalleerd. U een clientcertificaat genereren uit het zelfondertekende basiscertificaat en vervolgens exporteren en installeren van het clientcertificaat. Als het clientcertificaat niet is geïnstalleerd, mislukt de verificatie. 

De volgende stappen maakt u een clientcertificaat van een zelfondertekend basiscertificaat genereren. U kunt meerdere clientcertificaten uit hetzelfde basiscertificaat genereren. Als u clientcertificaten met de onderstaande stappen genereert, wordt het certificaat wordt automatisch geïnstalleerd op de computer die u gebruikt voor het genereren van het certificaat. Als u een clientcertificaat installeren op een andere clientcomputer wilt, kunt u het certificaat exporteren.
 
1. Open een opdrachtprompt als beheerder op dezelfde computer die u gebruikt voor het zelfondertekend certificaat maken.
2. Wijzigen en uitvoeren van het voorbeeld voor het genereren van een clientcertificaat.
  * Wijziging *'P2SRootCert'* op de naam van de zelf-ondertekend basis dat u het clientcertificaat van genereert. Zorg ervoor dat u de naam van het basiscertificaat dat alles wat is de "CN =' waarde is die u hebt opgegeven toen u het zelfondertekende basiscertificaat hebt gemaakt.
  * Wijziging *P2SChildCert* op de naam die u een clientcertificaat wilt te genereren.

  Als u het volgende voorbeeld uitvoert zonder het te wijzigen, is het resultaat een certificaat met de naam P2SChildcert in uw persoonlijke certificaatarchief die is gegenereerd op basis van het basiscertificaat P2SRootCert.

  ```cmd
  makecert.exe -n "CN=P2SChildCert" -pe -sky exchange -m 96 -ss My -in "P2SRootCert" -is my -a sha256
  ```

### <a name="clientexport"></a>Een clientcertificaat exporteren

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]

### <a name="install"></a>Een geëxporteerde certificaat installeren

Als u wilt installeren een clientcertificaat, Zie [een clientcertificaat installeren](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="next-steps"></a>Volgende stappen

Ga door met de punt-naar-Site-configuratie. 

* Voor **Resource Manager** model implementatiestappen Zie [P2S configureren met behulp van systeemeigen Azure certificaatverificatie](vpn-gateway-howto-point-to-site-resource-manager-portal.md).
* Voor **klassieke** model implementatiestappen Zie [punt-naar-Site VPN-verbinding geconfigureerd met een VNet (klassiek)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).
