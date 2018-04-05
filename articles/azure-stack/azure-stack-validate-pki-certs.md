---
title: Azure-Stack Public Key Infrastructure-certificaten voor de implementatie van Azure-Stack geïntegreerd systemen valideren | Microsoft Docs
description: Beschrijft hoe de Azure-Stack PKI-certificaten voor Azure-Stack geïntegreerd systemen valideren.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 0bdadadb1f4ee5f76cde9d05b11e8d57b99ac191
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="validate-azure-stack-pki-certificates"></a>Valideren van Azure Stack PKI-certificaten

Azure-Stack certificaat Registercontrole beschreven in dit artikel wordt geleverd door de OEM opgenomen in het bestand deploymentdata.json om te valideren dat de [gegenereerd PKI-certificaten](azure-stack-get-pki-certs.md) geschikt zijn voor de voorafgaand aan de implementatie. Certificaten worden gevalideerd met voldoende tijd om te testen en ophalen van de certificaten opnieuw uitgegeven indien nodig.

Het certificaat Registercontrole (Certchecker) worden de volgende controles uitgevoerd:

- **Lezen van PFX**. Controleert op geldig PFX-bestand, wachtwoord en een waarschuwing als openbare informatie niet wordt beveiligd door het wachtwoord. 
- **Handtekeningalgoritme**. Controleert de algoritme voor handtekening niet SHA1.
- **Persoonlijke sleutel**. Controleert de persoonlijke sleutel aanwezig is en is geëxporteerd met het kenmerk van de lokale computer. 
- **Certificaatketen**. Controleert certificaatketen in intact inclusief voor zelfondertekende certificaten. 
- **DNS-namen**. Controleert de SAN bevat relevante DNS-namen voor elk eindpunt of als een ondersteunende jokerteken aanwezig is. 
- **Sleutelgebruik**. Sleutelgebruik bevat digitale handtekening en sleutelcodering en Enhanced Key Usage bevat serververificatie en clientverificatie controleert.
- **Sleutelgrootte**. Controleert de dat sleutelgrootte is 2048 of groter.
- **Volgorde keten**. Controleert de volgorde van de andere certificaten, waardoor de keten juist is.
- **Andere certificaten**. Zorg ervoor dat er geen andere certificaten worden verpakt door PFX dan het relevante leaf-certificaat en de keten.
- **Er is geen profiel**. Controleert of dat een nieuwe gebruiker kan het PFX-gegevens laden zonder een gebruikersprofiel geladen, het gedrag van gMSA accounts mimicking tijdens het onderhoud van het certificaat.

> [!IMPORTANT]  
> De PKI-certificaatbestand PFX en het wachtwoord moeten worden behandeld als gevoelige informatie.

## <a name="prerequisites"></a>Vereisten
Uw systeem aan de volgende vereisten voldoen voordat de PKI-certificaten voor de implementatie van Azure-Stack valideren:
- CertChecker (in **PartnerToolKit** onder **\utils\certchecker**)
- SSL certificaten geëxporteerd na de [voorbereiding-instructies](azure-stack-prepare-pki-certs.md)
- DeploymentData.json
- Windows 10 of Windows Server 2016

## <a name="perform-certificate-validation"></a>Validatie van het servercertificaat uitvoeren

Volg deze stappen voor het voorbereiden en valideren van de Azure-Stack PKI-certificaten: 

1. Pak de inhoud van <partnerToolkit>\utils\certchecker naar een nieuwe map, bijvoorbeeld **c:\certchecker**.

2. Open PowerShell als beheerder en wijzig de directory in de map certchecker:

  ```powershell
  cd c:\certchecker
  ```
 
3. Maak een mapstructuur voor certificaten met de volgende PowerShell-opdrachten:

  ```powershell 
  $directories = "ACS","ADFS","Admin Portal","ARM Admin","ARM Public","Graph","KeyVault","KeyVaultInternal","Public Portal" 
  $destination = '.\certs' 
  $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}  
  ```

  >  [!NOTE]
  >  Als de id-provider voor de implementatie van Azure-Stack Azure AD is, verwijdert u de **ADFS** en **grafiek** mappen. 

4. Plaats uw certificaten in de juiste mappen die zijn gemaakt in de vorige stap, bijvoorbeeld: 
  - c:\certchecker\Certs\ACS\CustomerCertificate.pfx,  
  - c:\certchecker\Certs\Admin Portal\CustomerCertificate.pfx  
  - c:\certchecker\Certs\ARM Admin\CustomerCertificate.pfx  
  - enzovoort... 

5. Kopiëren **deploymentdata.json** in de **c:\certchecker** directory.

6. Voer de volgende opdrachten in het venster PowerShell: 

  ```powershell
  $password = Read-Host -Prompt "Enter PFX Password" -AsSecureString 
  .\CertChecker.ps1 -CertificatePath .\Certs\ -pfxPassword $password -deploymentDataJSONPath .\DeploymentData.json  
  ```

7. De uitvoer moet OK bevatten voor alle certificaten en alle kenmerken die zijn ingeschakeld: 

  ```powershell
  Starting Azure Stack Certificate Validation 1.1802.221.1
  Testing: ADFS\ContosoSSL.pfx
    Read PFX: OK
    Signature Algorithm: OK
    Private Key: OK
    Cert Chain: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Chain Order: OK
    Other Certificates: OK
    No Profile: OK
  Testing: KeyVaultInternal\ContosoSSL.pfx
    Read PFX: OK
    Signature Algorithm: OK
    Private Key: OK
    Cert Chain: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Chain Order: OK
    Other Certificates: OK
    No Profile: OK
  Testing: ACS\ContosoSSL.pfx
  WARNING: Pre-1803 certificate structure. The folder structure for Azure Stack 1803 and above is: ACSBlob, ACSQueue, ACSTable instead of ACS folder. Refer to deployment documentation for further informat
  ion.
    Read PFX: OK
    Signature Algorithm: OK
    Private Key: OK
    Cert Chain: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Chain Order: OK
    Other Certificates: OK
    No Profile: OK
  Detailed log can be found C:\CertChecker\CertChecker.log 
  ```

### <a name="known-issues"></a>Bekende problemen 
**Symptoom**: Certchecker voortijdig wordt afgesloten en u het volgende foutbericht: 
> Mislukt

> Details: Met deze opdracht kan niet worden uitgevoerd vanwege de fout: naam van de map is ongeldig. 

**Oorzaak**: certchecker.ps1 bijvoorbeeld uit een beperkende map actief is, c:\temp of % temp % 

**Resolutie**: het hulpprogramma certchecker naar de nieuwe map, bijvoorbeeld C:\CertChecker verplaatsen 


**Symptoom**: Certchecker wordt een waarschuwing weergegeven over het gebruik van Pre-1803 (zoals in het bovenstaande voorbeeld van stap 7):

> [!WARNING]
> De structuur van de pre-1803 certificaat. De map voor Azure Stack 1803 structuur en luidt: ACSTable ACSBlob, ACSQueue, in plaats van ACS-map. Zie voor meer informatie de documentatie van de implementatie.

**Oorzaak**: CertChecker gedetecteerd het gebruik van één ACS-map dit juist is voor implementaties voordat 1803. Voor Azure-Stack versie 1803 en boven-implementaties wijzigingen van de mapstructuur ACSTable, ACSQueue, ACSBlob. Certchecker is al bijgewerkt om deze functionaliteit.

**Resolutie**: als 1802 implementeert, is geen actie vereist. Als 1803 implementeert en boven ACS vervangen door ACSTable, ACSQueue, ACSBlob en kopieer de ACS-certificaten naar deze mappen.

**Symptoom**: Tests worden overgeslagen

**Oorzaak**: CertChecker bepaalde tests worden overgeslagen als een afhankelijkheid niet is voldaan:
- Andere certificaten worden overgeslagen als de certificaatketen is mislukt.
- Er is geen profiel wordt overgeslagen als:
  - Er is een beveiligingsbeleid voor de mogelijkheid een tijdelijke gebruiker maken en uitvoeren van powershell als die gebruiker beperken.
  - Controle van de persoonlijke sleutel is mislukt.

**Resolutie**: Volg de instructies van de hulpprogramma's in de detailsectie onder elk certificaat reeks tests uit.


## <a name="prepare-deployment-script-certificates"></a>Implementatie script certificaten voorbereiden 
Alle certificaten die u hebt voorbereid moeten als laatste stap in de juiste mappen op de implementatie-host worden geplaatst. Maak een map met de naam op uw host implementatie. Certificaten ** en plaats het geëxporteerde certificaat bestanden in de bijbehorende submappen die zijn opgegeven in de [verplichte certificaten](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs#mandatory-certificates) sectie:

```
\Certificates
\ACS\ssl.pfx
\Admin Portal\ssl.pfx
\ARM Admin\ssl.pfx
\ARM Public\ssl.pfx
\KeyVault\ssl.pfx
\KeyVaultInternal\ssl.pfx
\Public Portal\ssl.pfx
\ADFS\ssl.pfx*
\Graph\ssl.pfx*
```

<sup>*</sup> De certificaten die zijn gemarkeerd met een sterretje * zijn alleen nodig als AD FS wordt gebruikt als de identity-store.


## <a name="next-steps"></a>Volgende stappen
[Identiteitintegratie Datacenter](azure-stack-integrate-identity.md)
