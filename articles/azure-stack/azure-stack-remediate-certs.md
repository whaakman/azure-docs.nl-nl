---
title: Herstellen van problemen bij het certificaat voor Azure Stack | Microsoft Docs
description: Gebruik de Azure Stack-gereedheid van de vereisten om te controleren en oplossen van problemen bij het certificaat.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 11/19/2018
ms.openlocfilehash: 009eb56621f7cd395c3d2eefb29b9fa624af888b
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57778192"
---
# <a name="remediate-common-issues-for-azure-stack-pki-certificates"></a>Oplossen van veelvoorkomende problemen met de Azure Stack PKI-certificaten

De informatie in dit artikel kunt u te begrijpen en oplossen van veelvoorkomende problemen met de Azure Stack PKI-certificaten. U problemen kunt detecteren wanneer u de Azure Stack gereedheid van de Registercontrole [valideren van Azure Stack PKI-certificaten](azure-stack-validate-pki-certs.md). Het hulpprogramma controleert om ervoor te zorgen dat de certificaten te voldoen aan de vereisten voor PKI van een Azure Stack-implementatie en Azure Stack-geheim rotatie en registreert de resultaten in een [report.json bestand](azure-stack-validation-report.md).  

## <a name="pfx-encryption"></a>PFX-codering

**Fout** -PFX-versleuteling is niet TripleDES SHA1.

**Herstel** -exporteren pfx-bestanden met **TripleDES SHA1** versleuteling. Dit is de standaardinstelling voor alle Windows 10-clients bij het exporteren van het certificaat-module of met behulp van `Export-PFXCertificate`.

## <a name="read-pfx"></a>PFX lezen

**Waarschuwing** -wachtwoord beveiligt alleen de persoonlijke gegevens in het certificaat.  

**Herstel** -exporteren PFX-bestanden met de optionele instelling voor **inschakelen certificaat privacy**.  

**Fout** -PFX-bestand ongeldige.  

**Herstel** -opnieuw Exporteer het certificaat met behulp van de stappen in [voorbereiden Azure Stack PKI-certificaten voor de implementatie van](azure-stack-prepare-pki-certs.md).

## <a name="signature-algorithm"></a>Handtekeningalgoritme

**Fout** -handtekeningalgoritme SHA1 is.

**Herstel** -gebruik de stappen in Azure Stack-certificaten genereren van de aanvraag opnieuw genereren van het Certificate Signing Request (CSR) met de handtekeningalgoritme van SHA256-ondertekening. Voer vervolgens opnieuw de CSR voor de certificeringsinstantie het certificaat opnieuw.

## <a name="private-key"></a>Privésleutel

**Fout** -de persoonlijke sleutel ontbreekt of bevat niet de lokale Machine-kenmerk.  

**Herstel** - van de computer die de CSR gegenereerd opnieuw Exporteer het certificaat met behulp van de stappen in [voorbereiden Azure Stack PKI-certificaten voor de implementatie van](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment). Deze stappen omvatten exporteren uit het certificaatarchief van lokale computer.

## <a name="certificate-chain"></a>Certificaatketen

**Fout** -certificaatketen is niet voltooid.  

**Herstel** -certificaten moeten een volledige certificaatketen bevatten. Opnieuw Exporteer het certificaat met behulp van de stappen in [voorbereiden Azure Stack PKI-certificaten voor de implementatie van](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment) en selecteer de optie **indien mogelijk alle certificaten in het certificeringspad opnemen.**

## <a name="dns-names"></a>DNS-namen

**Fout** -de DNSNameList op het certificaat bevat geen naam van het Azure Stack-service-eindpunt of een geldig jokerteken. Komt overeen met jokerteken zijn alleen geldig voor de meest linkse naamruimte van de DNS-naam. Bijvoorbeeld, _*. region.domain.com_ is alleen geldig voor *portal.region.domain.com*, niet _*. table.region.domain.com_.

**Herstel** -gebruik van de stappen in Azure Stack-certificaten voor het ondertekenen van het genereren van de aanvraag opnieuw genereren van de certificaataanvraag met de juiste DNS-namen voor de ondersteuning van Azure Stack-eindpunten. Verzend vervolgens opnieuw de certificaataanvraag naar een certificeringsinstantie en volg de stappen in [voorbereiden Azure Stack PKI-certificaten voor de implementatie van](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment) voor het exporteren van het certificaat van de computer die de CSR gegenereerd.  

## <a name="key-usage"></a>Sleutelgebruik

**Fout** - Key usage heeft geen digitale handtekening of sleutelcodering of uitgebreid sleutelgebruik, server-verificatie of clientverificatie ontbreekt.  

**Herstel** -gebruik de stappen in [Azure Stack-ondertekening aanvraag genereren van certificaten](azure-stack-get-pki-certs.md) opnieuw genereren van de certificaataanvraag met de juiste Key Usage-kenmerken. De certificaataanvraag op de certificeringsinstantie opnieuw en controleer of dat een certificaatsjabloon het sleutelgebruik in de aanvraag niet worden overschreven.

## <a name="key-size"></a>Sleutelgrootte

**Fout** -sleutel kleiner is dan 2048.

**Herstel** -gebruik de stappen in [Azure Stack-ondertekening aanvraag genereren van certificaten](azure-stack-get-pki-certs.md) opnieuw genereren van de certificaataanvraag met de juiste sleutellengte (2048) en voer vervolgens opnieuw de CSR voor de certificeringsinstantie.

## <a name="chain-order"></a>Ketenvolgorde

**Fout** -de volgorde van de certificaatketen is onjuist.  

**Herstel** -opnieuw Exporteer het certificaat met behulp van de stappen in [voorbereiden Azure Stack PKI-certificaten voor de implementatie van](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment) en selecteer de optie **indien mogelijk alle certificaten in het certificeringspad opnemen.** Zorg ervoor dat alleen het leaf-certificaat is geselecteerd voor het exporteren.

## <a name="other-certificates"></a>Andere certificaten

**Fout** -pakket van het pfx-certificaten die niet de leaf-certificaat of een deel van de certificaatketen bevat.  

**Herstel** -opnieuw Exporteer het certificaat met behulp van de stappen in [voorbereiden Azure Stack PKI-certificaten voor de implementatie van](azure-stack-prepare-pki-certs.md#prepare-certificates-for-deployment), en selecteer de optie **indien mogelijk alle certificaten in het certificeringspad opnemen.** Zorg ervoor dat alleen het leaf-certificaat is geselecteerd voor het exporteren.

## <a name="fix-common-packaging-issues"></a>Veelvoorkomende verpakking problemen oplossen

De **AzsReadinessChecker** bevat een helper-cmdlet met de naam `Repair-AzsPfxCertificate`, welke kunt importeren en vervolgens exporteren een PFX-bestand voor het oplossen van veelvoorkomende problemen van pakketten, met inbegrip van:

- *PFX-codering* is niet TripleDES SHA1.
- *Persoonlijke sleutel* lokale Machine-kenmerk ontbreekt.
- *Certificaatketen* is onvolledig of onjuist. De lokale computer moet de certificaatketen bevatten als het PFX-pakket niet bestaat.
- *Andere certificaten*

`Repair-AzsPfxCertificate` kan geen help als u wilt een nieuw CSR niet genereren en voer opnieuw een certificaat.

### <a name="prerequisites"></a>Vereisten

De volgende vereisten moeten worden voldaan op de computer waarop het hulpprogramma wordt uitgevoerd:

- Windows 10 of Windows Server 2016, met een internetverbinding.
- PowerShell 5.1 of hoger. De volgende PowerShell-cmdlet uitvoeren om te controleren of uw versie, en bekijk vervolgens de *belangrijke* en *kleine* versies:

   ```powershell
   $PSVersionTable.PSVersion
   ```

- Configureer [PowerShell voor Azure Stack](azure-stack-powershell-install.md).
- Download de nieuwste versie van de [Microsoft Azure Stack-gereedheid Checker](https://aka.ms/AzsReadinessChecker) hulpprogramma.

### <a name="import-and-export-an-existing-pfx-file"></a>Importeren en exporteren van een bestaand PFX-bestand

1. Open een PowerShell-prompt met beheerdersrechten op een computer die voldoet aan de vereisten, en voer de volgende opdracht voor het installeren van de AzsReadinessChecker:

   ```powershell
   Install-Module Microsoft.AzureStack.ReadinessChecker -Force
   ```

2. Voer de volgende cmdlet om in te stellen van het PFX-wachtwoord van de PowerShell-prompt. Vervang `PFXpassword` met het wachtwoord:

   ```powershell
   $password = Read-Host -Prompt PFXpassword -AsSecureString
   ```

3. Voer het volgende als u wilt een nieuw PFX-bestand exporteren vanuit de PowerShell-prompt:

   - Voor `-PfxPath`, geef het pad op naar het PFX-bestand dat u met werkt. In het volgende voorbeeld wordt het pad is `.\certificates\ssl.pfx`.
   - Voor `-ExportPFXPath`, geef de locatie en naam van het PFX-bestand voor uitvoer. In het volgende voorbeeld wordt het pad is `.\certificates\ssl_new.pfx`:

   ```powershell
   Repair-AzsPfxCertificate -PfxPassword $password -PfxPath .\certificates\ssl.pfx -ExportPFXPath .\certificates\ssl_new.pfx
   ```  

4. Nadat het hulpprogramma is voltooid, bekijkt u de uitvoer voor geslaagd:

   ```powershell
   Repair-AzsPfxCertificate v1.1809.1005.1 started.
   Starting Azure Stack Certificate Import/Export
   Importing PFX .\certificates\ssl.pfx into Local Machine Store
   Exporting certificate to .\certificates\ssl_new.pfx
   Export complete. Removing certificate from the local machine store.
   Removal complete.
   Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
   Repair-AzsPfxCertificate Completed
   ```

## <a name="next-steps"></a>Volgende stappen

- [Hier voor meer informatie over de beveiliging van Azure Stack](azure-stack-rotate-secrets.md).
