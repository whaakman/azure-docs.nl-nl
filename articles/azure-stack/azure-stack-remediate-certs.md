---
title: Certificaatproblemen oplossen voor Azure-Stack | Microsoft Docs
description: Gebruik de Azure-Stack gereedheid van de controle om te controleren en herstellen van certificaatproblemen.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 0d2c4d848f861e4e07dbd0de4609344955ca26f7
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="remediate-common-issues-for-azure-stack-pki-certificates"></a>Oplossen van veelvoorkomende problemen voor Azure Stack PKI-certificaten
De informatie in dit artikel kunt u begrijpen en oplossen van veelvoorkomende problemen voor Azure Stack PKI-certificaten. U kunt problemen detecteren wanneer u Azure Stack gereedheid van de Registercontrole [Azure Stack PKI-certificaten valideren](azure-stack-validate-pki-certs.md). Het hulpprogramma wordt gecontroleerd om ervoor te zorgen dat de certificaten voldoen aan de vereisten van de PKI van een Azure-Stack-implementatie en de Azure-Stack geheim rotatie en registreert de resultaten in een [report.json bestand](azure-stack-validation-report.md).  

## <a name="read-pfx"></a>PFX lezen
**Waarschuwing** -wachtwoord beveiligt alleen de persoonlijke gegevens in het certificaat.  
**Herstel** -het is raadzaam u PFX-bestanden met de optionele instelling voor exporteert **inschakelen certificaat privacy**.  

**Fout** -PFX-bestand ongeldig.  
**Herstel** -opnieuw exporteren van het certificaat met de stappen in [voorbereiden Azure Stack PKI-certificaten voor de implementatie van](azure-stack-prepare-pki-certs.md).

## <a name="signature-algorithm"></a>Algoritme voor handtekening
**Fout** -algoritme voor handtekening SHA1 is.    
**Herstel** -gebruik de stappen in de Azure-Stack certificaten genereren van de aanvraag opnieuw genereren van de aanvraag (Certificaatondertekening) met de handtekening algoritme van SHA256-ondertekening. Verzend de aanvraag voor Certificaatondertekening tot de certificeringsinstantie het certificaat opnieuw.

## <a name="private-key"></a>Privésleutel
**Fout** -de persoonlijke sleutel ontbreekt of bevat niet de lokale Machine-kenmerk.  
**Herstel** - vanaf de computer waarop de CSR gegenereerd opnieuw Exporteer het certificaat met de stappen in voorbereiden Azure Stack PKI-certificaten voor implementatie. Deze stappen omvatten exporteren uit het certificaatarchief van de lokale computer.

## <a name="certificate-chain"></a>Certificaatketen
**Fout** -certificaatketen is niet voltooid.  
**Herstel** -certificaten moeten een volledige certificaatketen bevatten.  Het certificaat met de stappen in opnieuw exporteren [voorbereiden Azure Stack PKI-certificaten voor de implementatie van](azure-stack-prepare-pki-certs.md) en selecteer de optie **indien mogelijk alle certificaten in het certificeringspad opnemen.**

## <a name="dns-names"></a>DNS-namen
**Fout** -DNSNameList op het certificaat bevat niet de naam van het Azure-Stack-service-eindpunt, of een geldig jokerteken.  Komt overeen met jokerteken zijn alleen geldig voor de meest linkse-naamruimte van de DNS-naam. Bijvoorbeeld: _*. region.domain.com_ is alleen geldig voor *portal.region.domain.com*, niet _*. table.region.domain.com_.  
**Herstel** -gebruik van de stappen in de Azure-Stack certificaten ondertekening generatie van de aanvraag opnieuw genereren van de CSR met de juiste DNS-namen voor de ondersteuning van Azure-Stack-eindpunten. Opnieuw indienen certificaataanvraag naar een certificeringsinstantie en volg de stappen in [voorbereiden Azure Stack PKI-certificaten voor de implementatie van](azure-stack-prepare-pki-certs.md) het certificaat te exporteren van de computer die de aanvraag voor Certificaatondertekening gegenereerd.  

## <a name="key-usage"></a>Sleutelgebruik
**Fout** - sleutelgebruik ontbreekt digitale handtekening of sleutelcodering, orEnhanced sleutelgebruik ontbreekt Server-verificatie of clientverificatie.  
**Herstel** -gebruik de stappen in [Azure Stack certificaten ondertekenen aanvraag generatie](azure-stack-get-pki-certs.md) opnieuw genereren van de CSR met de juiste kenmerken voor sleutelgebruik.  Verzend de aanvraag voor Certificaatondertekening tot de certificeringsinstantie opnieuw en Bevestig het sleutelgebruik in de aanvraag niet wordt een certificaatsjabloon is overschreven.

## <a name="key-size"></a>Sleutelgrootte
**Fout** -sleutel kleiner is dan 2048    
**Herstel** -gebruik de stappen in [Azure Stack certificaten ondertekenen aanvraag generatie](azure-stack-get-pki-certs.md) opnieuw genereren van de CSR met de juiste sleutel-lengte (2048) en verzend de aanvraag voor Certificaatondertekening tot de certificeringsinstantie.

## <a name="chain-order"></a>Ketenvolgorde
**Fout** -de volgorde van de certificaatketen is onjuist.  
**Herstel** -opnieuw exporteren van het certificaat met de stappen in [voorbereiden Azure Stack PKI-certificaten voor de implementatie van](azure-stack-prepare-pki-certs.md) en selecteer de optie **indien mogelijk alle certificaten in het certificeringspad opnemen.** Zorg ervoor dat alleen het leaf-certificaat is geselecteerd om te exporteren. 

## <a name="other-certificates"></a>Andere certificaten
**Fout** -certificaten die niet het certificaat of een deel van de certificaatketen voor de PFX-pakket bevat.  
**Herstel** -opnieuw exporteren van het certificaat met de stappen in [voorbereiden Azure Stack PKI-certificaten voor de implementatie van](azure-stack-prepare-pki-certs.md), en selecteer de optie **indien mogelijk alle certificaten in het certificeringspad opnemen.** Zorg ervoor dat alleen het leaf-certificaat is geselecteerd om te exporteren.

## <a name="fix-common-packaging-issues"></a>Algemene verpakking problemen oplossen
De AzsReadinessChecker kunt importeren en exporteren van een PFX-bestand voor het oplossen van veelvoorkomende problemen verpakking, met inbegrip van: 
 - *Persoonlijke sleutel* lokale Machine-kenmerk ontbreekt.
 - *Certificaatketen* is onvolledig of is onjuist. (De lokale computer moet bevatten de certificaatketen als het PFX-pakket niet.) 
 - *Andere certificaten*.
De AzsReadinessChecker kan echter niet als u een nieuwe CSR niet genereren en een certificaat opnieuw moet helpen. 

### <a name="prerequisites"></a>Vereisten
De volgende vereisten moeten worden voldaan op de computer waarop het hulpprogramma wordt uitgevoerd: 
 - Windows 10 of Windows Server 2016 met een internetverbinding.
 - PowerShell 5.1 of hoger. Voer de volgende PowerShell-cmd en bekijk vervolgens te controleren of uw versie, de *belangrijke* versie en *secundaire* versies.

   > `$PSVersionTable.PSVersion`
 - Configureer [PowerShell voor Azure Stack](azure-stack-powershell-install.md). 
 - Download de nieuwste versie van [Microsoft Azure-Stack gereedheid Checker](https://aka.ms/AzsReadinessChecker) hulpprogramma.

### <a name="import-and-export-an-existing-pfx-file"></a>Importeren en exporteren van een bestaande PFX-bestand
1. Op een computer die voldoet aan de vereisten, open een administratieve PowerShell een opdrachtprompt en voer de volgende opdracht voor het installeren van de AzsReadinessChecker  
   > `Install-Module Microsoft.AzureStack.ReadinessChecker- Force`

2. Voer de volgende voor het instellen van het PFX-wachtwoord van de PowerShell-prompt. Vervang *PFXpassword* met het wachtwoord. 
   > `$password = Read-Host -Prompt PFXpassword -AsSecureString`

3. Voer de volgende om een nieuwe PFX-bestand te exporteren uit de PowerShell-prompt.
   - Voor *- PfxPath*, het pad naar het PFX-bestand dat u met werkt opgeven.  In het volgende voorbeeld wordt het pad is *.\certificates\ssl.pfx*.
   - Voor *- ExportPFXPath*, geef de locatie en naam van het PFX-bestand voor uitvoer.  In het volgende voorbeeld wordt het pad is *.\certificates\ssl_new.pfx*

   > `Start-AzsReadinessChecker -PfxPassword $password -PfxPath .\certificates\ssl.pfx -ExportPFXPath .\certificates\ssl_new.pfx`  

4. Nadat het hulpprogramma is voltooid, Controleer de uitvoer voor een correcte werking: ![resultaten](./media/azure-stack-remediate-certs/remediate-results.png)

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over Azure-Stack-beveiliging](azure-stack-rotate-secrets.md)