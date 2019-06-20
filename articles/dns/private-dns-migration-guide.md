---
title: Verouderde Azure DNS Private Zones migreren naar nieuwe Resource-Model
description: Deze handleiding bevat stapsgewijze instructies voor het migreren van oudere privé-DNS-zones naar het laatste resourcemodel
services: dns
author: rohinkoul
ms.service: dns
ms.topic: tutorial
ms.date: 06/18/2019
ms.author: rohink
ms.openlocfilehash: e7ebbf35cd572601f02a69930b58811686a92c86
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276095"
---
# <a name="migrating-legacy-azure-dns-private-zones-to-new-resource-model"></a>Verouderde Azure DNS private zones migreren naar nieuwe resourcemodel

We een nieuw model van de API-resource voor Azure DNS private zones als onderdeel van de preview-versie voor vernieuwen hebt verzonden. Vernieuwen van de Preview-versie biedt nieuwe functionaliteit en verwijdert u verschillende voorwaarden en beperkingen van de eerste openbare preview. Maar deze voordelen zijn niet beschikbaar op de persoonlijke DNS-zones die zijn gemaakt met behulp van verouderde API. Als u de voordelen van de nieuwe versie, moet u uw oude persoonlijke DNS-zone resources migreren naar nieuwe resourcemodel. Het migratieproces is eenvoudig en een PowerShell-script voor het automatiseren van dit proces is gesteld. Deze handleiding bevat stapsgewijze instructies voor het migreren van uw Azure DNS private zones met nieuwe resourcemodel.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u de meest recente versie van Azure PowerShell hebt geïnstalleerd. Voor meer informatie over Azure PowerShell (Az) en hoe ze deze willen installeren gaat u naar https://docs.microsoft.com/powershell/azure/new-azureps-module-az

Zorg ervoor dat u Az.PrivateDns-module voor de Azure PowerShell is geïnstalleerd hebt. Open een verhoogde PowerShell-venster (met beheerdersrechten modus) voor het installeren van deze module en voert u de volgende opdracht

```powershell
Install-Module -Name Az.PrivateDns -AllowPrerelease
```

>[!IMPORTANT]
>Het migratieproces is volledig geautomatiseerd en om te leiden tot uitvaltijd wordt niet verwacht. Echter, als u Azure DNS private zones (preview) in een productieomgeving u moet worden uitgevoerd onder het migratieproces tijdens een periode van gepland onderhoud. Zorg ervoor dat u de configuratie of recordsets van een privé-DNS-zones niet wijzigen terwijl u het migratiescript uitvoert.

## <a name="installing-the-script"></a>Het script installeren

Open een verhoogde PowerShell-venster (met beheerdersrechten modus) en voer de volgende opdracht

```powershell
install-script PrivateDnsMigrationScript
```

Voer 'A' wanneer u wordt gevraagd voor het installeren van het script

![Het script installeren](./media/private-dns-migration-guide/install-migration-script.png)

U kunt ook handmatig de meest recente versie van PowerShell-script aan https://www.powershellgallery.com/packages/PrivateDnsMigrationScript

## <a name="running-the-script"></a>Het script is uitgevoerd

De volgende opdracht uit te voeren van het script uitvoeren

```powershell
PrivateDnsMigrationScript.ps1
```

![Het script is uitgevoerd](./media/private-dns-migration-guide/running-migration-script.png)

### <a name="enter-the-subscription-id-and-sign-in-to-azure"></a>Voer de abonnements-ID en aanmelden bij Azure

U wordt gevraagd om in te voeren abonnements-ID met de persoonlijke DNS-zones die u wilt migreren. U wordt gevraagd om aan te melden bij uw Azure-account. Voltooi de aanmelding zodat script toegang heeft tot de persoonlijke DNS-zone-resources in het abonnement.

![Aanmelden bij Azure](./media/private-dns-migration-guide/login-migration-script.png)

### <a name="select-the-dns-zones-you-want-to-migrate"></a>Selecteer de DNS-zones die u wilt migreren

Het script met de lijst met alle persoonlijke DNS-zones in het abonnement en u wordt gevraagd te bevestigen welke u wilt migreren. Voer 'A' voor het migreren van alle privé-DNS-zones. Wanneer u deze stap hebt uitgevoerd, wordt het script maken van nieuwe persoonlijke DNS-zones met behulp van nieuwe resourcemodel en de gegevens kopiëren naar de nieuwe DSN-zone. Deze stap wordt de bestaande persoonlijke DNS-zones in niet toch wijzigen.

![DNS-zones selecteren](./media/private-dns-migration-guide/migratezone-migration-script.png)

### <a name="switching-dns-resolution-to-the-new-dns-zones"></a>DNS-omzetting overschakelen naar de nieuwe DNS-zones

Zodra de zones en -records zijn gekopieerd naar de nieuwe resourcemodel, vraagt het script u de DNS-omzetting overschakelen naar de nieuwe DNS-zones. Deze stap verwijdert u de koppeling tussen verouderde privé-DNS-zones en uw virtuele netwerken. Als de bestaande zone losgekoppeld van de virtuele netwerken is, zou de DNS-omzetting voor deze virtuele netwerken automatisch de nieuwe DNS-zones in de bovenstaande stap gemaakt overnemen.

Selecteer 'A' om over te schakelen van de DNS-omzetting voor alle virtuele netwerken.

![Switch-naamomzetting](./media/private-dns-migration-guide/switchresolution-migration-script.png)

### <a name="verify-the-dns-resolution"></a>Controleer of de DNS-omzetting

Voordat u doorgaat, moet u controleren of DNS-omzetting op de DNS-zones werkt zoals verwacht. U kunt aanmelden bij uw azure-VM's en probleem nslookup query op de gemigreerde zones om te controleren of deze DNS-omzetting werkt.

![Controleren of de naamomzetting](./media/private-dns-migration-guide/verifyresolution-migration-script.png)

Als u merkt dat de DNS-query's worden niet oplossen, wacht een paar minuten en probeer de query's. Als de DNS-query's werkt zoals verwacht, voert u 'Y' wanneer het script vraagt u het virtuele netwerk verwijderen uit de privé-DNS-zone.

![Controleer of de naamomzetting](./media/private-dns-migration-guide/confirmresolution-migration-script.png)

>[!IMPORTANT]
>Als de oplossing op basis van de gemigreerde zones niet vanwege een bepaalde reden DNS werkt zoals verwacht, voert u n in hierboven genoemde stap en het script wordt de DNS-omzetting gaat u terug naar de oude zones. Maak een ondersteuningsticket en we u kunnen helpen bij de migratie van uw DNS-zones.

## <a name="cleanup"></a>Opschonen

Deze stap verwijdert u de oude DNS-zones en moet worden uitgevoerd nadat u hebt gecontroleerd of DNS-omzetting werkt zoals verwacht. U wordt gevraagd elke privé-DNS-zone verwijderen. Voer 'Y' om een opdrachtprompt nadat u hebt gecontroleerd of DNS-omzetting voor deze zones correct werkt.

![Opruimen](./media/private-dns-migration-guide/cleanup-migration-script.png)

## <a name="update-your-automation"></a>Bijwerken van uw automation

Als u van automatisering gebruikmaakt, zoals sjablonen, PowerShell-scripts of aangepaste code die is ontwikkeld met behulp van SDK, moet u uw automation voor het gebruik van de nieuwe resourcemodel voor het privé-DNS-zones bijwerken. Hieronder vindt u de koppeling naar nieuwe persoonlijke DNS CLI/PS/SDK-documentatie.
* [Azure DNS private zones REST-API](https://docs.microsoft.com/rest/api/dns/privatedns/privatezones)
* [Azure DNS private zones CLI](https://docs.microsoft.com/cli/azure/ext/privatedns/network/private-dns?view=azure-cli-latest)
* [Azure DNS private zones PowerShell](https://docs.microsoft.com/powershell/module/az.privatedns/?view=azps-2.3.2)
* [Azure DNS private zones SDK](https://docs.microsoft.com/dotnet/api/overview/azure/privatedns/management?view=azure-dotnet-preview)

## <a name="need-further-help"></a>Meer hulp nodig hebt

Maak een ondersteuningsticket als u meer hulp nodig met het migratieproces of vanwege een of andere reden de hierboven vermelde stappen niet voor u werken. Neem het transcript-bestand gegenereerd door de PowerShell-script met uw ondersteuningsticket.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het maken van een privé-zone in Azure DNS met behulp [Azure PowerShell](./private-dns-getstarted-powershell.md) of [Azure CLI](./private-dns-getstarted-cli.md).

* Meer informatie over sommige algemene [privézone-scenario's](./private-dns-scenarios.md) die u kunt realiseren met privézones in Azure DNS.

* Voor algemene vragen en antwoorden over privézones in Azure DNS, met inbegrip van specifiek gedrag kunt u verwachten voor bepaalde soorten bewerkingen, Zie [Veelgestelde vragen over privé-DNS](./dns-faq-private.md).

* Meer informatie over DNS-zones en records, recentst [DNS-zones en records overzicht](dns-zones-records.md).

* Informatie over enkele van de andere belangrijke [netwerkmogelijkheden](../networking/networking-overview.md) van Azure.
