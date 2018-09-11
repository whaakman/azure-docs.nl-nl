---
title: 'Genereren en exporteren van certificaten voor punt-naar-Site: MakeCert: Azure | Microsoft Docs'
description: Een zelfondertekend basiscertificaat maken, de openbare sleutel exporteren en clientcertificaten genereren via MakeCert.
services: vpn-gateway
documentationcenter: na
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 09/05/2018
ms.author: cherylmc
ms.openlocfilehash: 3ff7e754a55e15a8fa8a32f846efbbbe5025e46e
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297856"
---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-makecert"></a>Genereren en exporteren van certificaten voor punt-naar-Site-verbindingen met MakeCert

Punt-naar-Site-verbindingen gebruiken certificaten om te verifiëren. Dit artikel leest u hoe te maken van een zelfondertekend basiscertificaat met MakeCert clientcertificaten genereren. Als u naar ander certificaat-instructies zoekt, Zie [certificaten - PowerShell](vpn-gateway-certificates-point-to-site.md) of [certificaten - Linux](vpn-gateway-certificates-point-to-site-linux.md).

Terwijl het wordt aangeraden de [Windows 10 PowerShell stappen](vpn-gateway-certificates-point-to-site.md) voor het maken van uw certificaten, bieden we deze MakeCert-instructies als optionele methode. De certificaten die u genereert met behulp van een van beide methoden kunnen worden geïnstalleerd op [een ondersteunde client-besturingssysteem](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq). MakeCert heeft echter de volgende beperkingen:

* MakeCert is afgeschaft. Dit betekent dat dit hulpprogramma op elk gewenst moment kan worden verwijderd. Alle certificaten die u al met MakeCert gegenereerd, worden niet beïnvloed wanneer MakeCert niet meer beschikbaar is. MakeCert wordt alleen gebruikt voor het genereren van de certificaten niet als een mechanisme voor het valideren.

## <a name="rootcert"></a>Een zelfondertekend basiscertificaat maken

De volgende stappen laten zien hoe u een zelfondertekend certificaat met MakeCert maken. Deze stappen zijn niet specifiek implementatiemodel. Ze zijn geldig voor zowel klassieke als Resource Manager.

1. Download en installeer [MakeCert](https://msdn.microsoft.com/library/windows/desktop/aa386968(v=vs.85).aspx).
2. Na de installatie kunt u doorgaans het hulpprogramma makecert.exe onder dit pad kunt vinden: "C:\Program Files (x86) \Windows Kits\10\bin\<arch >'. Hoewel het is mogelijk dat deze is geïnstalleerd op een andere locatie. Open een opdrachtprompt als beheerder en navigeer naar de locatie van het hulpprogramma MakeCert. U kunt het volgende voorbeeld gebruiken aanpassen voor de juiste locatie:

  ```cmd
  cd C:\Program Files (x86)\Windows Kits\10\bin\x64
  ```
3. Maak en installeer een certificaat in het persoonlijke certificaatarchief op uw computer. Het volgende voorbeeld wordt een bijbehorende *.cer* -bestand dat u naar Azure uploadt bij het configureren van P2S. Vervang 'P2SRootCert' en 'P2SRootCert.cer' met de naam die u wilt gebruiken voor het certificaat. Het certificaat bevindt zich in uw 'Certificaten - Huidige gebruiker\Persoonlijk\Certificaten'.

  ```cmd
  makecert -sky exchange -r -n "CN=P2SRootCert" -pe -a sha256 -len 2048 -ss My
  ```

## <a name="cer"></a>Exporteer de openbare sleutel (.cer)

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

Het bestand exported.cer moet worden geüpload naar Azure. Zie voor instructies [een punt-naar-Site-verbinding configureren](vpn-gateway-howto-point-to-site-resource-manager-portal.md#uploadfile). Als u wilt een extra vertrouwd basiscertificaat toevoegen, Zie [in deze sectie](vpn-gateway-howto-point-to-site-resource-manager-portal.md#add) van het artikel.

### <a name="export-the-self-signed-certificate-and-private-key-to-store-it-optional"></a>Exporteer de zelf-ondertekend certificaat en de persoonlijke sleutel voor het opslaan van het (optioneel)

U kunt het zelfondertekende basiscertificaat exporteren en API veilig opslaat. Indien nodig zijn, u kunt later installeren op een andere computer en meer clientcertificaten genereren of een andere cer-bestand exporteren. Als u wilt het zelfondertekend basiscertificaat exporteren als een pfx-bestand, selecteer het basiscertificaat en gebruik dezelfde stappen zoals beschreven in [een clientcertificaat exporteren](#clientexport).

## <a name="create-and-install-client-certificates"></a>Maken en clientcertificaten te installeren

U kunt het zelfondertekende certificaat niet installeren rechtstreeks op de clientcomputer. U moet een clientcertificaat genereren uit het zelfondertekende certificaat. U vervolgens exporteren en het clientcertificaat installeren op de clientcomputer. De volgende stappen zijn niet specifiek implementatiemodel. Ze zijn geldig voor zowel klassieke als Resource Manager.

### <a name="clientcert"></a>Een clientcertificaat genereren

Op elke clientcomputer die via punt-naar-site verbinding maakt met een VNet, moet een clientcertificaat zijn geïnstalleerd. U een clientcertificaat genereren uit het zelfondertekende basiscertificaat, en vervolgens exporteren en het clientcertificaat installeren. Als het clientcertificaat niet is geïnstalleerd, mislukt de verificatie. 

De volgende stappen helpen u bij het genereren van een certificaat van een zelfondertekend basiscertificaat. U kunt meerdere clientcertificaten genereren uit het hetzelfde basiscertificaat. Wanneer u met behulp van de onderstaande stappen clientcertificaten genereert, wordt het certificaat wordt automatisch geïnstalleerd op de computer die u gebruikt voor het genereren van het certificaat. Als u een clientcertificaat installeren op een andere clientcomputer wilt, kunt u het certificaat exporteren.
 
1. Open een opdrachtprompt als beheerder op dezelfde computer die u gebruikt voor het maken van de zelf-ondertekend certificaat.
2. Wijzig en voer het voorbeeld om een clientcertificaat genereren uit.
  * Wijziging *'P2SRootCert'* op de naam van het zelfondertekend basiscertificaat dat u het clientcertificaat van genereert. Zorg ervoor dat u de naam van het basiscertificaat dat alles wat is de ' CN =' de waarde is die u hebt opgegeven tijdens het maken van het zelfondertekend basiscertificaat.
  * Wijziging *P2SChildCert* op de naam die u een clientcertificaat wilt te genereren.

  Als u het volgende voorbeeld uitvoert zonder het te wijzigen, is het resultaat een clientcertificaat met de naam P2SChildcert in het persoonlijke certificaatarchief bevinden die is gegenereerd op basis van basiscertificaat P2SRootCert.

  ```cmd
  makecert.exe -n "CN=P2SChildCert" -pe -sky exchange -m 96 -ss My -in "P2SRootCert" -is my -a sha256
  ```

### <a name="clientexport"></a>Een clientcertificaat exporteren

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]

### <a name="install"></a>Een geëxporteerde certificaat installeren

Voor het installeren van een clientcertificaat, Zie [een clientcertificaat installeren](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="next-steps"></a>Volgende stappen

Ga door met de punt-naar-Site-configuratie. 

* Voor **Resource Manager** implementatiestappen voor model, Zie [P2S configureren met behulp van systeemeigen Azure certificaatverificatie](vpn-gateway-howto-point-to-site-resource-manager-portal.md).
* Voor **klassieke** implementatiestappen voor model, Zie [configureren van een punt-naar-Site VPN-verbinding met een VNet (klassiek)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).

Voor informatie over probleemoplossing voor P2S bekijkt u [Troubleshooting Azure point-to-site connections](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md) (Problemen met punt-naar-site-verbindingen in Azure oplossen).