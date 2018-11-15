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
ms.date: 11/14/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: d2dda25c63a6e4a73205b9e4a830a211d025b3ed
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688144"
---
# <a name="sql-resource-provider-11300-release-notes"></a>Opmerkingen bij de SQL-resource provider 1.1.30.0 release

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Deze releaseopmerkingen beschrijven de verbeteringen en bekende problemen in SQL resource provider-versie 1.1.30.0.

## <a name="build-reference"></a>Naslaginformatie over bouwen
Downloaden van de SQL-resourceprovider binaire en voer vervolgens de zelfstandige extractor om de inhoud uitpakken naar een tijdelijke map. De resourceprovider heeft een minimale bijbehorende Azure Stack bouwen. De minimale Azure Stack-versie die is vereist voor het installeren van deze versie van de SQL-resourceprovider wordt hieronder weergegeven:

> |Minimale versie van Azure Stack|Provider-versie van SQL-resource|
> |-----|-----|
> |Versie 1808 (1.1808.0.97)|[1.1.30.0](https://aka.ms/azurestacksqlrp11300)|
> |     |     |

> [!IMPORTANT]
> De minimale ondersteunde Azure Stack-update van toepassing op uw geïntegreerde Azure Stack-systeem of de meest recente Azure Stack Development Kit (ASDK) implementeren voordat u de nieuwste versie van de SQL-resourceprovider implementeert.

## <a name="new-features-and-fixes"></a>Nieuwe functies en oplossingen
Deze versie van de resourceprovider van Azure Stack SQL omvat de volgende verbeteringen en oplossingen:

- **Telemetrie zijn ingeschakeld voor SQL-resource provider implementaties**. Verzamelen van telemetriegegevens is ingeschakeld voor SQL-resource provider-implementaties. Telemetrie die is verzameld resource provider-implementatie bevat, starten en stoptijden, status, afsluiten, berichten en foutdetails afsluiten (indien van toepassing).

- **Update van TLS 1.2-versleuteling**. Ingeschakelde TLS 1.2-alleen ondersteuning voor de resource provider communicatie met de interne Azure Stack-onderdelen. 

### <a name="fixes"></a>Oplossingen

- **SQL-resource provider PowerShell voor Azure Stack compatibiliteit**. De SQL-resourceprovider is bijgewerkt naar werk met de Azure Stack-2018-03-01-hybride PowerShell-profiel en voor compatibiliteit met AzureRM 1.3.0 en hoger.

- **Blade SQL-aanmelding wijzigen-wachtwoord**. Een probleem opgelost waarbij het wachtwoord kan niet worden gewijzigd op de blade van het wachtwoord wijzigen. Verwijderde koppelingen van wachtwoord wijzigingsmeldingen.

- **SQL server-instellingen-blade update die als host fungeert**. Een probleem opgelost waarbij de instellingenblade is niet goed met de titel als 'Wachtwoord'.

## <a name="known-issues"></a>Bekende problemen 

- **SQL-SKU's kan een uur duren om te worden weergegeven in de portal**. Het kan een uur duren voor zojuist gemaakte SKU's zijn zichtbaar voor gebruik bij het maken van nieuwe SQL-databases. 

    **Tijdelijke oplossing**: geen.

- **SQL-aanmeldingen hergebruikt**. Bij het maken van een nieuwe SQL Meld u aan met de dezelfde gebruikersnaam als een bestaande aanmelding onder hetzelfde abonnement, leidt dat dezelfde aanmeldgegevens en het bestaande wachtwoord. 

    **Tijdelijke oplossing**: gebruik van verschillende gebruikersnamen op bij het maken van nieuwe aanmeldingen onder hetzelfde abonnement of aanmeldingen kan maken met de dezelfde gebruikersnaam onder verschillende abonnementen.

- **Gedeelde SQL-aanmeldingen gegevensinconsistentie veroorzaken**. Als een SQL-aanmelding wordt gedeeld voor meerdere SQL-databases onder hetzelfde abonnement, ontstaan het aanmeldingswachtwoord wijzigen gegevensinconsistenties.

    **Tijdelijke oplossing**: gebruik altijd verschillende logins voor verschillende databases onder hetzelfde abonnement.

### <a name="known-issues-for-cloud-admins-operating-azure-stack"></a>Bekende problemen voor Cloud-beheerders die Azure Stack
Raadpleeg de documentatie in de [opmerkingen bij de Release van de Azure Stack](azure-stack-servicing-policy.md).

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over de SQL-resourceprovider](azure-stack-sql-resource-provider.md).

[Voorbereidingen voor het implementeren van de SQL-resourceprovider](azure-stack-sql-resource-provider-deploy.md#prerequisites).

[De SQL-resourceprovider upgraden van een vorige versie](azure-stack-sql-resource-provider-update.md). 