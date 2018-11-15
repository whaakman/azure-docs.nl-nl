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
ms.date: 11/14/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: 6354212eb95fbefb217dd5339613d050da55f4ba
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688131"
---
# <a name="mysql-resource-provider-11300--release-notes"></a>MySQL resource provider 1.1.30.0 release-opmerkingen

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

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

    **Tijdelijke oplossing**: geen.

- **MySQL-aanmeldingen hergebruikt**. Bij het maken van een nieuwe MySQL Meld u aan met de dezelfde gebruikersnaam als een bestaande aanmelding onder hetzelfde abonnement, leidt dat dezelfde aanmeldgegevens en het bestaande wachtwoord. 

    **Tijdelijke oplossing**: gebruik van verschillende gebruikersnamen op bij het maken van nieuwe aanmeldingen onder hetzelfde abonnement of aanmeldingen kan maken met de dezelfde gebruikersnaam onder verschillende abonnementen.


### <a name="known-issues-for-cloud-admins-operating-azure-stack"></a>Bekende problemen voor Cloud-beheerders die Azure Stack
Raadpleeg de documentatie in de [opmerkingen bij de Release van de Azure Stack](azure-stack-servicing-policy.md).

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over de MySQL-resourceprovider](azure-stack-mysql-resource-provider.md).

[Voorbereidingen voor het implementeren van de MySQL-resourceprovider](azure-stack-mysql-resource-provider-deploy.md#prerequisites).

[De MySQL-resourceprovider upgraden van een vorige versie](azure-stack-mysql-resource-provider-update.md). 