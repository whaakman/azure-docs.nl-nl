---
title: Veelvoorkomende oorzaken van Cloud Service-rollen recyclen | Microsoft Docs
description: De functie van een cloud-service die plotseling wordt gerecycled kan leiden tot aanzienlijke downtime. Hier volgen enkele veelvoorkomende problemen die ertoe leiden rollen dat worden gerecycled, waarmee u de uitvaltijd beperken.
services: cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 533930d1-8035-4402-b16a-cf887b2c4f85
ms.service: cloud-services
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 11/03/2017
ms.author: v-six
ms.openlocfilehash: 7f513a7d3fe44cbbf06855059c87c10ddceef7c9
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="common-issues-that-cause-roles-to-recycle"></a>Algemene problemen waardoor rollen worden herhaald
Dit artikel worden enkele van de algemene oorzaken van problemen met implementatie beschreven en biedt tips voor probleemoplossing waarmee u deze problemen oplossen. Geeft aan dat er een probleem met een toepassing is is wanneer de rolinstantie niet kan worden gestart, of deze replicatiecycli tussen de statussen initialiseren bezet en stoppen.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-runtime-dependencies"></a>Ontbrekende runtime-afhankelijkheden
Als een rol in uw toepassing afhankelijk van een is assembly die geen deel uitmaakt van de .NET Framework of de Azure beheerde bibliotheek, moet u die assembly expliciet opnemen in het toepassingspakket. Houd er rekening mee dat andere Microsoft-frameworks niet standaard beschikbaar in Azure. Als uw rol is afhankelijk van een dergelijke framework, moet u deze assembly's toevoegen aan het toepassingspakket.

Voordat u bouwen en de toepassing van het pakket, Controleer het volgende:

* Als u Visual studio gebruikt, controleert u of de **lokale kopie** eigenschap is ingesteld op **True** voor elke assembly waarnaar wordt verwezen in uw project die geen deel uitmaakt van de Azure SDK of .NET Framework.
* Zorg ervoor dat het bestand web.config verwijst niet naar alle ongebruikte-assembly's in het element compilatie.
* De **Build Action** van elke .cshtml bestand is ingesteld op **inhoud**. Dit zorgt ervoor dat de bestanden correct wordt weergegeven in het pakket en hiermee andere bestanden waarnaar wordt verwezen in het pakket wordt weergegeven.

## <a name="assembly-targets-wrong-platform"></a>Assembly doelen onjuist platform
Azure is een 64-bits-omgeving. .NET-assembly's die voor een 32-bits-doel is gecompileerd werkt daarom niet op Azure.

## <a name="role-throws-unhandled-exceptions-while-initializing-or-stopping"></a>Rol van niet-verwerkte uitzonderingen genereert tijdens het initialiseren van of stoppen
Eventuele uitzonderingen die zijn gegenereerd door de methoden van de [RoleEntryPoint] klasse, waaronder de [OnStart], [OnStop], en [uitvoeren] methoden, zijn niet-verwerkte uitzonderingen. Als een niet-verwerkte uitzondering in een van deze methoden optreedt, wordt de rol gerecycled. Als de rol herhaaldelijk recyclen is, er het mogelijk een onverwerkte uitzondering telkens wanneer die deze probeert te starten.

## <a name="role-returns-from-run-method"></a>Rol wordt geactiveerd vanuit Run-methode
De [uitvoeren] methode is bedoeld voor onbepaalde tijd worden uitgevoerd. Als uw code overschrijft de [uitvoeren] methode, dat deze moet slapen voor onbepaalde tijd. Als de [uitvoeren] methode retourneert de rol wordt gerecycled.

## <a name="incorrect-diagnosticsconnectionstring-setting"></a>Onjuiste DiagnosticsConnectionString-instelling
Als toepassing gebruikmaakt van Azure Diagnostics, uw serviceconfiguratiebestand moet opgeven de `DiagnosticsConnectionString` configuratie-instelling. Deze instelling moet een HTTPS-verbinding naar uw opslagaccount in Azure opgeven.

Om ervoor te zorgen dat uw `DiagnosticsConnectionString` instelling juist is voordat u uw toepassingspakket in Azure implementeert, controleert u het volgende:  

* De `DiagnosticsConnectionString` instelling verwijst naar een geldig opslagaccount in Azure.  
  Standaard wijst deze instelling naar het geëmuleerde storage-account zodat u deze instelling expliciet wijzigen moet voordat u uw toepassingspakket implementeren. Als u deze instelling niet wijzigt, wordt een uitzondering opgetreden bij de rolinstantie wordt de diagnostische monitor gestart. Hierdoor kunnen de rolinstantie wilt recyclen voor onbepaalde tijd.
* De verbindingsreeks is opgegeven in de volgende [indeling](../storage/common/storage-configure-connection-string.md). (Het protocol moet worden opgegeven als HTTPS). Vervang *MyAccountName* met de naam van uw opslagaccount en *MyAccountKey* door uw toegangssleutel:    

        DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey

  Als u uw toepassing ontwikkelt met behulp van Azure-hulpprogramma's voor Microsoft Visual Studio, kunt u de eigenschappenpagina's kunt u deze waarde instellen.

## <a name="exported-certificate-does-not-include-private-key"></a>Geëxporteerde certificaat bevat geen persoonlijke sleutel
Voor het uitvoeren van een Webrol onder SSL, moet u ervoor zorgen dat het geëxporteerde management-certificaat de persoonlijke sleutel bevat. Als u de *Windows Certificate Manager* om het certificaat exporteren, moet u selecteren **Ja** voor de **de persoonlijke sleutel exporteren** optie. Het certificaat moet worden geëxporteerd in het PFX-indeling, dit de enige indeling die momenteel worden ondersteund is.

## <a name="next-steps"></a>Volgende stappen
Meer [probleemoplossing artikelen](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) voor cloudservices.

Bekijk meer rollen recyclen van scenario's op [blogreeks van Kevin Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

[RoleEntryPoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx
[OnStart]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx
[OnStop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[uitvoeren]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx
