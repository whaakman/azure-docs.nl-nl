---
title: 'Genereren en exporteren van certificaten voor punt-naar-Site: PowerShell: Azure | Microsoft Docs'
description: Een zelfondertekend basiscertificaat maken, de openbare sleutel exporteren en clientcertificaten genereren via PowerShell in Windows 10 of Windows Server 2016.
services: vpn-gateway
documentationcenter: na
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: cherylmc
ms.openlocfilehash: e635eddabe239268829d3a907df10392c7e1b6a3
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52850955"
---
# <a name="generate-and-export-certificates-for-point-to-site-using-powershell"></a>Genereren en exporteren van certificaten voor punt-naar-Site met behulp van PowerShell

Punt-naar-Site-verbindingen gebruiken certificaten om te verifiëren. Dit artikel ziet u hoe u een zelfondertekend basiscertificaat maken en genereren van clientcertificaten met behulp van PowerShell in Windows 10 of Windows Server 2016. Als u naar ander certificaat-instructies zoekt, Zie [certificaten - Linux](vpn-gateway-certificates-point-to-site-linux.md) of [certificaten - MakeCert](vpn-gateway-certificates-point-to-site-makecert.md).

In dit artikel op een computer met Windows 10 of Windows Server 2016 moet u de stappen uitvoeren. De PowerShell-cmdlets die u gebruikt voor het genereren van certificaten maken deel uit van het besturingssysteem en werken niet op andere versies van Windows. De computer met Windows 10 of Windows Server 2016 is alleen nodig voor het genereren van de certificaten. Zodra de certificaten zijn gegenereerd, kunt u deze uploaden of installeren op een ondersteunde client-besturingssysteem. 

Als u geen toegang tot een computer met Windows 10 of Windows Server 2016, kunt u [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) certificaten te genereren. De certificaten die u genereert met behulp van een van beide methoden kunnen worden geïnstalleerd op een [ondersteund](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) client-besturingssysteem.

## <a name="rootcert"></a>1. Een zelfondertekend basiscertificaat maken

Gebruik de cmdlet New-SelfSignedCertificate om een zelfondertekend basiscertificaat te maken. Zie voor informatie over de extra parameters, [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).

1. Vanaf een computer met Windows 10 of Windows Server 2016, opent u een Windows PowerShell-console met verhoogde bevoegdheden. Deze voorbeelden werken niet in de Azure Cloud Shell 'Nu uitproberen'. U moet deze voorbeelden lokaal uitvoeren.
2. Gebruik het volgende voorbeeld om het zelfondertekend basiscertificaat te maken. Het volgende voorbeeld wordt een zelfondertekend basiscertificaat 'P2SRootCert', die automatisch wordt geïnstalleerd in 'Certificaten-Huidige gebruiker\Persoonlijk\Certificaten' met de naam. U kunt het certificaat weergeven door het openen van *certmgr.msc*, of *Gebruikerscertificaten beheren*.

  ```powershell
  $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
  ```

## <a name="clientcert"></a>2. Een clientcertificaat genereren

Op elke clientcomputer die via punt-naar-site verbinding maakt met een VNet, moet een clientcertificaat zijn geïnstalleerd. U een clientcertificaat genereren uit het zelfondertekende basiscertificaat, en vervolgens exporteren en het clientcertificaat installeren. Als het clientcertificaat niet is geïnstalleerd, mislukt de verificatie. 

De volgende stappen helpen u bij het genereren van een certificaat van een zelfondertekend basiscertificaat. U kunt meerdere clientcertificaten genereren uit het hetzelfde basiscertificaat. Wanneer u met behulp van de onderstaande stappen clientcertificaten genereert, wordt het certificaat wordt automatisch geïnstalleerd op de computer die u gebruikt voor het genereren van het certificaat. Als u een clientcertificaat installeren op een andere clientcomputer wilt, kunt u het certificaat exporteren.

De voorbeelden gebruiken de cmdlet New-SelfSignedCertificate voor het genereren van een clientcertificaat dat verloopt na één jaar. Zie voor meer informatie de extra parameter, zoals het instellen van een andere vervalwaarde voor het clientcertificaat [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).

### <a name="example-1"></a>Voorbeeld 1

Dit voorbeeld wordt de variabele gedeclareerde '$cert' uit de vorige sectie. Als u de PowerShell-console gesloten nadat het zelfondertekende basiscertificaat hebt gemaakt, of zijn het maken van aanvullende certificaten in een nieuwe PowerShell-console-sessie, gebruikt u de stappen in [voorbeeld 2](#ex2).

Wijzigen en voer het voorbeeld voor het genereren van een clientcertificaat. Als u het volgende voorbeeld uitvoert zonder het te wijzigen, is het resultaat een clientcertificaat met de naam 'P2SChildCert'.  Als u wilt de naam van het certificaat van de onderliggende iets anders, wijzigt u de CN-waarde. Wijzig de TextExtension niet bij het uitvoeren van dit voorbeeld. Het clientcertificaat dat u wordt automatisch geïnstalleerd in 'Certificaten - Huidige gebruiker\Persoonlijk\Certificaten' op uw computer.

```powershell
New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
-Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" `
-Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
```

### <a name="ex2"></a>Voorbeeld 2

Als u het maken van aanvullende clientcertificaten of niet dezelfde PowerShell-sessie die u gebruikt voor het maken van uw zelfondertekend basiscertificaat gebruikt, gebruikt u de volgende stappen uit:

1. Identificeer het zelfondertekende basiscertificaat dat is geïnstalleerd op de computer. Deze cmdlet retourneert een lijst met certificaten die zijn geïnstalleerd op uw computer.

  ```powershell
  Get-ChildItem -Path “Cert:\CurrentUser\My”
  ```
2. Zoek de naam van het onderwerp in de geretourneerde lijst en kopieer de vingerafdruk die naast het bevindt zich in een tekstbestand. Er zijn twee certificaten in het volgende voorbeeld. De CN-naam is de naam van het zelfondertekende basiscertificaat van waaruit u wilt een onderliggende certificaat genereren. In dit geval 'P2SRootCert'.

  ```
  Thumbprint                                Subject
  
  AED812AD883826FF76B4D1D5A77B3C08EFA79F3F  CN=P2SChildCert4
  7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655  CN=P2SRootCert
  ```
3. Declareer een variabele voor het basiscertificaat met behulp van de vingerafdruk van de vorige stap. VINGERAFDRUK vervangen door de vingerafdruk van het basiscertificaat van waaruit u wilt een onderliggende certificaat genereren.

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\THUMBPRINT"
  ```

  Met de vingerafdruk voor P2SRootCert in de vorige stap, de variabele, ziet er bijvoorbeeld als volgt:

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655"
  ```
4.  Wijzigen en voer het voorbeeld voor het genereren van een clientcertificaat. Als u het volgende voorbeeld uitvoert zonder het te wijzigen, is het resultaat een clientcertificaat met de naam 'P2SChildCert'. Als u wilt de naam van het certificaat van de onderliggende iets anders, wijzigt u de CN-waarde. Wijzig de TextExtension niet bij het uitvoeren van dit voorbeeld. Het clientcertificaat dat u wordt automatisch geïnstalleerd in 'Certificaten - Huidige gebruiker\Persoonlijk\Certificaten' op uw computer.

  ```powershell
  New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
  -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" `
  -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
  ```

## <a name="cer"></a>3. Exporteer de openbare sleutel voor basis-certificaat (.cer)

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]


### <a name="export-the-self-signed-root-certificate-and-private-key-to-store-it-optional"></a>Exporteer het zelfondertekende basiscertificaat en de persoonlijke sleutel om op te slaan (optioneel)

Kunt u het zelfondertekend basiscertificaat exporteren en bewaar veilig als back-up. Als moet worden, kunt u later kunt installeren op een andere computer en meer clientcertificaten genereren. Als u wilt het zelfondertekend basiscertificaat exporteren als een pfx-bestand, selecteer het basiscertificaat en gebruik dezelfde stappen zoals beschreven in [een clientcertificaat exporteren](#clientexport).

## <a name="clientexport"></a>4. Het clientcertificaat exporteren

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]


## <a name="install"></a>5. Een geëxporteerd clientcertificaat installeren

Elke client die verbinding met het VNet via een P2S-verbinding maakt vereist een clientcertificaat dat moet lokaal worden geïnstalleerd.

Voor het installeren van een clientcertificaat, Zie [installeren van een certificaat voor punt-naar-Site-verbindingen](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="install"></a>6. Ga door met de configuratiestappen voor P2S

Ga door met de punt-naar-Site-configuratie.

* Voor **Resource Manager** implementatiestappen voor model, Zie [P2S configureren met behulp van systeemeigen Azure certificaatverificatie](vpn-gateway-howto-point-to-site-resource-manager-portal.md). 
* Voor **klassieke** implementatiestappen voor model, Zie [configureren van een punt-naar-Site VPN-verbinding met een VNet (klassiek)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).
* Voor P2S probleemoplossingsinformatie, Zie [punt-naar-site-verbindingen van Azure voor het oplossen van](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
