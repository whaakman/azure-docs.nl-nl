---
title: Azure Stack SQL resource provider 1.1.30.0 Opmerkingen bij de release | Microsoft Docs
description: Meer informatie over wat er in de meest recente Azure Stack resource provider update van SQL, inclusief bekende problemen en waar om deze te downloaden.
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
ms.openlocfilehash: ae5a824fbd96a9a76eb18811a46bfc17afa15073
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58879349"
---
# <a name="sql-resource-provider-11330-release-notes"></a>Opmerkingen bij de SQL-resource provider 1.1.33.0 release

*Van toepassing op Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Deze releaseopmerkingen beschrijven de verbeteringen en bekende problemen in SQL resource provider-versie 1.1.33.0.

## <a name="build-reference"></a>Naslaginformatie over bouwen
Downloaden van de SQL-resourceprovider binaire en voer vervolgens de zelfstandige extractor om de inhoud uitpakken naar een tijdelijke map. De resourceprovider heeft een minimale bijbehorende Azure Stack bouwen. De minimale Azure Stack-versie die is vereist voor het installeren van deze versie van de SQL-resourceprovider wordt hieronder weergegeven:

> |Minimale versie van Azure Stack|Provider-versie van SQL-resource|
> |-----|-----|
> |Versie 1808 (1.1808.0.97)|[SQL RP versie 1.1.33.0](https://aka.ms/azurestacksqlrp11330)|  
> |     |     |

> [!IMPORTANT]
> De minimale ondersteunde Azure Stack-update van toepassing op uw geïntegreerde Azure Stack-systeem of de meest recente Azure Stack Development Kit (ASDK) implementeren voordat u de nieuwste versie van de SQL-resourceprovider implementeert.

## <a name="new-features-and-fixes"></a>Nieuwe functies en oplossingen
Deze versie van de resourceprovider van Azure Stack SQL omvat de volgende verbeteringen en oplossingen:

### <a name="fixes"></a>Oplossingen
- **SQL-resource provider portalextensie mogelijk kiest u het verkeerde abonnement**. De SQL-resourceprovider maakt gebruik van Azure Resource Manager-aanroepen om te bepalen van de eerste service admin-abonnement moet worden gebruikt, die misschien niet de *Providerabonnement standaard*. Als dit gebeurt, werkt het SQL-resourceprovider niet normaal. 

- **Gehoste databases niet wordt vermeld in SQL-hostserver.** Gebruikers gecreëerde databases kunnen niet worden weergegeven bij het weergeven van tenantbronnen voor SQL-servers die als host fungeert.

- **Vorige SQL resource provider (1.1.30.0)-implementatie kan mislukken als TLS 1.2 is niet ingeschakeld**. De SQL-resourceprovider 1.1.33.0 om in te schakelen van TLS 1.2 bij het implementeren van de resourceprovider gedraaid geheimen of bijwerken van de resourceprovider bijgewerkt. 

- **SQL-resource provider geheime rotatie mislukt**. Dit resulteert in de volgende foutcode wanneer u geheimen probleem is opgelost:
`New-AzureRmResourceGroupDeployment - Error: Code=InvalidDeploymentParameterValue; Message=The value of deployment parameter 'StorageAccountBlobUri' is null.`

## <a name="known-issues"></a>Bekende problemen 

- **SQL-SKU's kan een uur duren om te worden weergegeven in de portal**. Het kan een uur duren voor zojuist gemaakte SKU's zijn zichtbaar voor gebruik bij het maken van nieuwe SQL-databases. 

    **Tijdelijke oplossing**: Geen.

- **SQL-aanmeldingen hergebruikt**. Bij het maken van een nieuwe SQL Meld u aan met de dezelfde gebruikersnaam als een bestaande aanmelding onder hetzelfde abonnement, leidt dat dezelfde aanmeldgegevens en het bestaande wachtwoord. 

    **Tijdelijke oplossing**: Gebruik verschillende gebruikersnamen op bij het maken van nieuwe aanmeldingen onder hetzelfde abonnement of aanmeldingen kan maken met de dezelfde gebruikersnaam onder verschillende abonnementen.

- **Gedeelde SQL-aanmeldingen gegevensinconsistentie veroorzaken**. Als een SQL-aanmelding wordt gedeeld voor meerdere SQL-databases onder hetzelfde abonnement, ontstaan het aanmeldingswachtwoord wijzigen gegevensinconsistenties.

    **Tijdelijke oplossing**: Gebruik altijd verschillende logins voor verschillende databases onder hetzelfde abonnement.

- **SQL-resourceprovider niet kan worden toegevoegd van SQL Server Always On-listener**. Wanneer u de listener-IP-adres van de SQL Server Always On-Listener, kan de SQL-resourceprovider VM van de listener-hostnaam niet omzetten.

    **Tijdelijke oplossing**: Zorg ervoor dat DNS correct werkt voor het omzetten van het listener-IP-adres in hostnaam van de listener.

### <a name="known-issues-for-cloud-admins-operating-azure-stack"></a>Bekende problemen voor Cloud-beheerders die Azure Stack
Raadpleeg de documentatie in de [opmerkingen bij de Release van de Azure Stack](azure-stack-servicing-policy.md).

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over de SQL-resourceprovider](azure-stack-sql-resource-provider.md).

[Voorbereidingen voor het implementeren van de SQL-resourceprovider](azure-stack-sql-resource-provider-deploy.md#prerequisites).

[De SQL-resourceprovider upgraden van een vorige versie](azure-stack-sql-resource-provider-update.md). 