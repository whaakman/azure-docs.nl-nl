---
title: 'Genereren en exporteren van certificaten voor punt-naar-Site: PowerShell: Azure | Microsoft Docs'
description: In dit artikel bevat stappen om een zelfondertekend basiscertificaat maken, de openbare sleutel niet exporteren en clientcertificaten op Windows 10 met behulp van PowerShell te genereren.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 27b99f7c-50dc-4f88-8a6e-d60080819a43
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: be2e8fe12dee88ccf81faaa114056a29e03881bd
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-powershell-on-windows-10"></a>Genereren en exporteren van certificaten voor punt-naar-Site-verbindingen op Windows 10 met behulp van PowerShell

Punt-naar-Site-verbindingen kunt u certificaten voor verificatie gebruiken. In dit artikel laat zien hoe een zelfondertekend basiscertificaat maken en clientcertificaten op Windows 10 met behulp van PowerShell te genereren. Als u zoekt de configuratiestappen punt-naar-Site, zoals het uploaden van basiscertificaten, selecteert u een van de artikelen ' configureren punt-naar-Site' in de volgende lijst:

> [!div class="op_single_selector"]
> * [Zelfondertekende certificaten - PowerShell maken](vpn-gateway-certificates-point-to-site.md)
> * [Zelfondertekende certificaten - MakeCert maken](vpn-gateway-certificates-point-to-site-makecert.md)
> * [Punt-naar-Site - Resource Manager - Azure-portal configureren](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Punt-naar-Site - Resource Manager - PowerShell configureren](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Punt-naar-Site - Classic - Azure-portal configureren](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 


In dit artikel op een computer met Windows 10, moet u de stappen uitvoeren. De PowerShell-cmdlets die u gebruikt voor het genereren van certificaten deel uitmaken van het besturingssysteem Windows 10 en werken niet op andere versies van Windows. De Windows 10-computer is alleen nodig voor het genereren van de certificaten. Zodra de certificaten zijn gegenereerd, kunt u deze uploaden of installeren op een ondersteunde client-besturingssysteem. 

Als u geen toegang tot een Windows 10-computer, kunt u [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) voor het genereren van certificaten. De certificaten die u genereren met behulp van beide methoden kunnen worden geïnstalleerd op een [ondersteund](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) clientbesturingssysteem.

## <a name="rootcert"></a>Een zelfondertekend basiscertificaat maken

Gebruik de cmdlet New-SelfSignedCertificate voor het maken van een zelfondertekend basiscertificaat. Zie voor informatie over de extra parameters, [nieuw SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).

1. Open vanaf een computer met Windows 10, Windows PowerShell-console met verhoogde bevoegdheden.
2. Gebruik het volgende voorbeeld voor het maken van het zelfondertekende basiscertificaat. Het volgende voorbeeld maakt een zelfondertekend basiscertificaat met de naam 'P2SRootCert', die automatisch wordt geïnstalleerd in 'Certificaten-Huidige gebruiker\Persoonlijk\Certificaten'. U kunt het certificaat weergeven door het openen van *certmgr.msc*, of *Gebruikerscertificaten beheren*.

  ```powershell
  $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
  ```

### <a name="cer"></a>Exporteer de openbare sleutel (.cer)

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

Het bestand exported.cer moet worden geüpload naar Azure. Zie voor instructies [een punt-naar-Site-verbinding configureren](vpn-gateway-howto-point-to-site-rm-ps.md#upload). Toevoegen van een vertrouwd basiscertificaat aanvullende [in deze sectie](vpn-gateway-howto-point-to-site-rm-ps.md#addremovecert) van het artikel.

### <a name="export-the-self-signed-root-certificate-and-public-key-to-store-it-optional"></a>Exporteer het zelfondertekende basiscertificaat en openbare sleutel voor het opslaan van het (optioneel)

U kunt het zelfondertekende basiscertificaat exporteren en veilig opslaan. Indien nodig zijn, u kunt later op een andere computer installeren en meer clientcertificaten te genereren of een andere cer-bestand exporteren. Als u wilt het zelfondertekende basiscertificaat exporteren als een .pfx-bestand, selecteer het basiscertificaat en gebruik dezelfde stappen zoals beschreven in [exporteren van een clientcertificaat](#clientexport).

## <a name="clientcert"></a>Een clientcertificaat genereren

Op elke clientcomputer die via punt-naar-site verbinding maakt met een VNet, moet een clientcertificaat zijn geïnstalleerd. U een clientcertificaat genereren uit het zelfondertekende basiscertificaat en vervolgens exporteren en installeren van het clientcertificaat. Als het clientcertificaat niet is geïnstalleerd, mislukt de verificatie. 

De volgende stappen maakt u een clientcertificaat van een zelfondertekend basiscertificaat genereren. U kunt meerdere clientcertificaten uit hetzelfde basiscertificaat genereren. Als u clientcertificaten met de onderstaande stappen genereert, wordt het certificaat wordt automatisch geïnstalleerd op de computer die u gebruikt voor het genereren van het certificaat. Als u een clientcertificaat installeren op een andere clientcomputer wilt, kunt u het certificaat exporteren.

De voorbeelden gebruikt de cmdlet New-SelfSignedCertificate voor het genereren van een clientcertificaat dat na één jaar verloopt. Zie voor aanvullende parameterinformatie, zoals het instellen van een andere vervaldatum waarde voor het clientcertificaat [nieuw SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).

### <a name="example-1"></a>Voorbeeld 1

In dit voorbeeld wordt de gedeclareerde '$cert' variabele uit de vorige sectie. Als u de PowerShell-console gesloten na het maken van het zelfondertekende basiscertificaat of extra client certificaten in een nieuwe PowerShell-console-sessie maakt, gebruikt u de stappen in [voorbeeld 2](#ex2).

Wijzigen en voer in het voorbeeld voor het genereren van een clientcertificaat. Als u het volgende voorbeeld uitvoert zonder het te wijzigen, is het resultaat een certificaat met de naam 'P2SChildCert'.  Als u wilt de naam van het certificaat van de onderliggende iets anders, wijzigt u de CN-waarde. Wijzig de TextExtension niet bij het uitvoeren van dit voorbeeld. Het clientcertificaat dat u genereren wordt automatisch geïnstalleerd in 'Certificaten - Huidige gebruiker\Persoonlijk\Certificaten' op uw computer.

```powershell
New-SelfSignedCertificate -Type Custom -KeySpec Signature `
-Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" `
-Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
```

### <a name="ex2"></a>Voorbeeld 2

Als u extra clientcertificaten maakt of dezelfde PowerShell-sessie die u gebruikt voor het maken van uw zelfondertekende basiscertificaat niet gebruikt, gebruikt u de volgende stappen uit:

1. Identificeer het zelfondertekende basiscertificaat dat is geïnstalleerd op de computer. Deze cmdlet retourneert een lijst met certificaten die zijn geïnstalleerd op uw computer.

  ```powershell
  Get-ChildItem -Path “Cert:\CurrentUser\My”
  ```
2. Zoek de onderwerpnaam in de geretourneerde lijst en kopieer de vingerafdruk die ernaast bevindt zich in een tekstbestand. Er zijn twee certificaten in het volgende voorbeeld. De CN-naam is de naam van het zelfondertekende basiscertificaat van waaruit u wilt voor het genereren van een onderliggende-certificaat. In dit geval 'P2SRootCert'.

  ```
  Thumbprint                                Subject
  
  AED812AD883826FF76B4D1D5A77B3C08EFA79F3F  CN=P2SChildCert4
  7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655  CN=P2SRootCert
  ```
3. Een variabele voor het basiscertificaat met de vingerafdruk van de vorige stap declareren. Vervang de VINGERAFDRUK met de vingerafdruk van het basiscertificaat van waaruit u wilt voor het genereren van een onderliggende-certificaat.

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\THUMBPRINT"
  ```

  Met de vingerafdruk voor P2SRootCert in de vorige stap, de variabele, ziet er bijvoorbeeld als volgt:

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655"
  ```
4.  Wijzigen en voer in het voorbeeld voor het genereren van een clientcertificaat. Als u het volgende voorbeeld uitvoert zonder het te wijzigen, is het resultaat een certificaat met de naam 'P2SChildCert'. Als u wilt de naam van het certificaat van de onderliggende iets anders, wijzigt u de CN-waarde. Wijzig de TextExtension niet bij het uitvoeren van dit voorbeeld. Het clientcertificaat dat u genereren wordt automatisch geïnstalleerd in 'Certificaten - Huidige gebruiker\Persoonlijk\Certificaten' op uw computer.

  ```powershell
  New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" `
  -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
  ```

## <a name="clientexport"></a>Een clientcertificaat exporteren   

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]

## <a name="install"></a>Een geëxporteerde certificaat installeren

Als u wilt installeren een clientcertificaat, Zie [installeren van een clientcertificaat voor punt-naar-Site-verbindingen](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="next-steps"></a>Volgende stappen

Ga door met de punt-naar-Site-configuratie.

* Voor **Resource Manager** model implementatiestappen Zie [P2S configureren met behulp van systeemeigen Azure certificaatverificatie](vpn-gateway-howto-point-to-site-resource-manager-portal.md). 
* Voor **klassieke** model implementatiestappen Zie [punt-naar-Site VPN-verbinding geconfigureerd met een VNet (klassiek)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).