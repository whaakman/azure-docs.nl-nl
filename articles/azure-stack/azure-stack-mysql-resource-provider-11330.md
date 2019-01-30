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
ms.date: 01/09/2019
ms.author: jeffgilb
ms.reviewer: jiahan
ms.lastreviewed: 01/09/2019
ms.openlocfilehash: 7f44e8c2c4587ecfdb3bd5eb4304789674da96f3
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55252002"
---
# <a name="mysql-resource-provider-11330--release-notes"></a>MySQL resource provider 1.1.33.0 release-opmerkingen

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Deze releaseopmerkingen beschrijven de verbeteringen en bekende problemen in MySQL resource provider-versie 1.1.33.0.

## <a name="build-reference"></a>Naslaginformatie over bouwen
De MySQL-resourceprovider binaire downloaden en voer vervolgens de zelfstandige extractor om de inhoud uitpakken naar een tijdelijke map. De resourceprovider heeft een minimale bijbehorende Azure Stack bouwen. De minimale Azure Stack-versie die is vereist voor het installeren van deze versie van de MySQL-resourceprovider wordt hieronder weergegeven:

> |Minimale versie van Azure Stack|MySQL resource provider-versie|
> |-----|-----|
> |Versie 1808 (1.1808.0.97)|[MySQL RP versie 1.1.33.0](https://aka.ms/azurestackmysqlrp11330)|  
> |     |     |

> [!IMPORTANT]
> De minimale ondersteunde Azure Stack-update van toepassing op uw geïntegreerde Azure Stack-systeem of de meest recente Azure Stack Development Kit (ASDK) implementeren voordat u de nieuwste versie van de MySQL-resourceprovider implementeert.

## <a name="new-features-and-fixes"></a>Nieuwe functies en oplossingen
Deze versie van de Azure Stack-MySQL-resourceprovider bevat de volgende verbeteringen en oplossingen:

### <a name="fixes"></a>Oplossingen
- **MySQL resource provider portalextensie mogelijk kiest u de verkeerde abonnement**. De MySQL-resourceprovider maakt gebruik van Azure Resource Manager-aanroepen om te bepalen van de eerste service admin-abonnement moet worden gebruikt, die misschien niet de *Providerabonnement standaard*. Als dit gebeurt, werkt de MySQL-resourceprovider niet normaal. 

- **MySQL-hostserver wordt niet vermeld die worden gehost databases.** Gebruikers gecreëerde databases kunnen niet worden weergegeven bij het weergeven van tenantbronnen voor MySQL-servers die als host fungeert.

- **Vorige MySQL resource provider (1.1.30.0)-implementatie kan mislukken als TLS 1.2 is niet ingeschakeld**. De MySQL-resourceprovider 1.1.33.0 om in te schakelen van TLS 1.2 bij het implementeren van de resourceprovider gedraaid geheimen of bijwerken van de resourceprovider bijgewerkt. 

- **MySQL resource provider geheime rotatie mislukt**. Dit resulteert in de volgende foutcode wanneer u geheimen probleem is opgelost: ` New-AzureRmResourceGroupDeployment - Error: Code=InvalidDeploymentParameterValue; Message=The value of deployment parameter 'StorageAccountBlobUri' is null.`

## <a name="known-issues"></a>Bekende problemen 

- **MySQL-SKU's kan een uur duren om te worden weergegeven in de portal**. Het kan een uur duren voor zojuist gemaakte SKU's zijn zichtbaar voor gebruik bij het maken van nieuwe MySQL-databases. 

    **Tijdelijke oplossing**: Geen.

- **MySQL-aanmeldingen hergebruikt**. Bij het maken van een nieuwe MySQL Meld u aan met de dezelfde gebruikersnaam als een bestaande aanmelding onder hetzelfde abonnement, leidt dat dezelfde aanmeldgegevens en het bestaande wachtwoord. 

    **Tijdelijke oplossing**: Gebruik verschillende gebruikersnamen op bij het maken van nieuwe aanmeldingen onder hetzelfde abonnement of aanmeldingen kan maken met de dezelfde gebruikersnaam onder verschillende abonnementen.

- **Gedeelde MySQL aanmeldingen gegevensinconsistentie veroorzaken**. Als een MySQL-aanmelding wordt gedeeld voor meerdere MySQL-databases onder hetzelfde abonnement, ontstaan het aanmeldingswachtwoord wijzigen gegevensinconsistenties.

    **Tijdelijke oplossing**: Gebruik altijd verschillende logins voor verschillende databases onder hetzelfde abonnement.


### <a name="known-issues-for-cloud-admins-operating-azure-stack"></a>Bekende problemen voor Cloud-beheerders die Azure Stack
Raadpleeg de documentatie in de [opmerkingen bij de Release van de Azure Stack](azure-stack-servicing-policy.md).

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over de MySQL-resourceprovider](azure-stack-mysql-resource-provider.md).

[Voorbereidingen voor het implementeren van de MySQL-resourceprovider](azure-stack-mysql-resource-provider-deploy.md#prerequisites).

[De MySQL-resourceprovider upgraden van een vorige versie](azure-stack-mysql-resource-provider-update.md). 