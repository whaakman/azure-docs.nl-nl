---
title: Azure Stack MySQL resource provider 1.1.30.0 Opmerkingen bij de release | Microsoft Docs
description: Meer informatie over wat er in de meest recente Azure Stack MySQL resource provider update, inclusief bekende problemen, en waar om deze te downloaden.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/10/2018
ms.author: jeffgilb
ms.reviewer: georgel
ms.openlocfilehash: 2f300e496873c0b048ccc1acc078bf1650e6bd9c
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53166282"
---
# <a name="mysql-resource-provider-11300--release-notes"></a>MySQL resource provider 1.1.30.0 release-opmerkingen

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Deze releaseopmerkingen beschrijven de verbeteringen en bekende problemen in MySQL resource provider-versie 1.1.30.0.

## <a name="build-reference"></a>Naslaginformatie over bouwen
De MySQL-resourceprovider binaire downloaden en voer vervolgens de zelfstandige extractor om de inhoud uitpakken naar een tijdelijke map. De resourceprovider heeft een minimale bijbehorende Azure Stack bouwen. De minimale Azure Stack-versie die is vereist voor het installeren van deze versie van de MySQL-resourceprovider wordt hieronder weergegeven:

> |Minimale versie van Azure Stack|MySQL resource provider-versie|
> |-----|-----|
> |Azure Stack 1808 update (1.1808.0.97)|[1.1.30.0](https://aka.ms/azurestackmysqlrp11300)|
> |     |     |

> [!IMPORTANT]
> De minimale ondersteunde Azure Stack-update van toepassing op uw geïntegreerde Azure Stack-systeem of de meest recente Azure Stack Development Kit (ASDK) implementeren voordat u de nieuwste versie van de MySQL-resourceprovider implementeert.

## <a name="new-features-and-fixes"></a>Nieuwe functies en oplossingen
Deze versie van de Azure Stack-MySQL-resourceprovider bevat de volgende verbeteringen en oplossingen:

- **Telemetrie zijn ingeschakeld voor MySQL resource provider implementaties**. Verzamelen van telemetriegegevens is ingeschakeld voor MySQL resource provider implementaties. Telemetrie die is verzameld resource provider-implementatie bevat, starten en stoptijden, status, afsluiten, berichten en foutdetails afsluiten (indien van toepassing).

- **Update van TLS 1.2-versleuteling**. Ingeschakelde TLS 1.2-alleen ondersteuning voor de resource provider communicatie met de interne Azure Stack-onderdelen. 

### <a name="fixes"></a>Oplossingen

- **MySQL resource provider PowerShell voor Azure Stack compatibiliteit**. De MySQL-resourceprovider is bijgewerkt naar werk met de Azure Stack-2018-03-01-hybride PowerShell-profiel en voor compatibiliteit met AzureRM 1.3.0 en hoger.

- **MySQL-aanmelding wijzigen wachtwoord blade**. Een probleem opgelost waarbij het wachtwoord kan niet worden gewijzigd op de blade van het wachtwoord wijzigen. Verwijderde koppelingen van wachtwoord wijzigingsmeldingen.

## <a name="known-issues"></a>Bekende problemen 

- **MySQL-SKU's kan een uur duren om te worden weergegeven in de portal**. Het kan een uur duren voor zojuist gemaakte SKU's zijn zichtbaar voor gebruik bij het maken van nieuwe MySQL-databases. 

    **Tijdelijke oplossing**: Geen.

- **MySQL-aanmeldingen hergebruikt**. Bij het maken van een nieuwe MySQL Meld u aan met de dezelfde gebruikersnaam als een bestaande aanmelding onder hetzelfde abonnement, leidt dat dezelfde aanmeldgegevens en het bestaande wachtwoord. 

    **Tijdelijke oplossing**: Gebruik verschillende gebruikersnamen op bij het maken van nieuwe aanmeldingen onder hetzelfde abonnement of aanmeldingen kan maken met de dezelfde gebruikersnaam onder verschillende abonnementen.

- **Vereiste voor ondersteuning van TLS 1.2**. Als u probeert te implementeren of bijwerken van de MySQL-resourceprovider vanaf een computer waarop TLS 1.2 niet is ingeschakeld, kan de bewerking mislukken. Voer de volgende PowerShell-opdracht op de computer die wordt gebruikt om te implementeren of bijwerken van de resource-provider om te controleren dat TLS 1.2 wordt geretourneerd als ondersteund:

  ```powershell
  [System.Net.ServicePointManager]::SecurityProtocol
  ```

  Als **Tls12** is niet opgenomen in de uitvoer van de opdracht, TLS 1.2 is niet ingeschakeld op de computer.

    **Tijdelijke oplossing**: Voer de volgende PowerShell-opdracht uit te schakelen van TLS 1.2 en start vervolgens de implementatie van resource provider of werk script vanuit dezelfde PowerShell-sessie:

    ```powershell
    [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12
    ```
 
### <a name="known-issues-for-cloud-admins-operating-azure-stack"></a>Bekende problemen voor Cloud-beheerders die Azure Stack
Raadpleeg de documentatie in de [opmerkingen bij de Release van de Azure Stack](azure-stack-servicing-policy.md).

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over de MySQL-resourceprovider](azure-stack-mysql-resource-provider.md).

[Voorbereidingen voor het implementeren van de MySQL-resourceprovider](azure-stack-mysql-resource-provider-deploy.md#prerequisites).

[De MySQL-resourceprovider upgraden van een vorige versie](azure-stack-mysql-resource-provider-update.md). 