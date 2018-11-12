---
title: Veelvoorkomende oorzaken van cloudservicerollen | Microsoft Docs
description: Een cloud service-rol die plotseling wordt gerecycled kan leiden tot significante downtime. Hier volgen enkele veelvoorkomende problemen waardoor rollen worden gerecycled, waarmee u downtime verminderen.
services: cloud-services
documentationcenter: ''
author: simonxjx
manager: felixwu
editor: ''
tags: top-support-issue
ms.assetid: 533930d1-8035-4402-b16a-cf887b2c4f85
ms.service: cloud-services
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 06/15/2018
ms.author: v-six
ms.openlocfilehash: 2a9214b918883e493ebe5c93fc7f56e7ce9c77ec
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51234491"
---
# <a name="common-issues-that-cause-roles-to-recycle"></a>Algemene problemen waardoor rollen worden herhaald
In dit artikel worden enkele van de veelvoorkomende oorzaken van problemen met implementatie beschreven en vindt u tips voor probleemoplossing voor u deze problemen oplossen. Een indicatie dat er een probleem met een toepassing is is wanneer de rolinstantie niet kan worden gestart, of deze cycli tussen de statussen initialiseren, bezet en stoppen.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-runtime-dependencies"></a>Ontbrekende runtime-afhankelijkheden
Als een rol in uw toepassing afhankelijk van een is assembly die geen deel uitmaakt van het .NET Framework of de door Azure beheerde bibliotheek, moet u expliciet die assembly opnemen in het toepassingspakket. Houd er rekening mee dat andere Microsoft-frameworks niet beschikbaar op Azure standaard zijn. Als uw rol is afhankelijk van een dergelijke structuur, moet u deze assembly's toevoegen aan het toepassingspakket.

Voordat u bouwen en inpakken van uw toepassing, controleert u het volgende:

* Als u Visual studio gebruikt, controleert u of de **lokale kopie** eigenschap is ingesteld op **waar** voor elke assembly waarnaar wordt verwezen in uw project die geen deel uitmaakt van de Azure SDK of .NET Framework.
* Zorg ervoor dat het web.config-bestand verwijst niet naar een niet-gebruikte assembly's in het element compilatie.
* De **Buildactie** van elke .cshtml bestand is ingesteld op **inhoud**. Dit zorgt ervoor dat de bestanden correct wordt weergegeven in het pakket en kunnen andere waarnaar wordt verwezen, bestanden worden weergegeven in het pakket.

## <a name="assembly-targets-wrong-platform"></a>Assembly doelen onjuist platform
Azure is een 64-bits-omgeving. .NET-assembly's gecompileerd voor een 32-bits-doel werkt daarom niet op Azure.

## <a name="role-throws-unhandled-exceptions-while-initializing-or-stopping"></a>Rol onverwerkte uitzonderingen genereert tijdens initialiseren of stoppen
Eventuele uitzonderingen die zijn gegenereerd door de methoden van de [RoleEntryPoint] klasse, waaronder de [OnStart], [OnStop], en [uitvoeren] methoden, zijn niet-verwerkte uitzonderingen. Als er een niet-verwerkte uitzondering optreedt in een van deze methoden, wordt de rol gerecycled. Als het recyclen van de rol is herhaaldelijk, ArgumentOutOfRangeException het kan een niet-verwerkte uitzondering telkens wanneer die er wordt geprobeerd om te starten.

## <a name="role-returns-from-run-method"></a>Rol retourneert uit Run-methode
De [uitvoeren] methode is bedoeld voor onbepaalde tijd worden uitgevoerd. Als uw code overschrijft de [uitvoeren] methode, dat deze moet slapen voor onbepaalde tijd. Als de [uitvoeren] methode retourneert, de rol wordt gerecycled.

## <a name="incorrect-diagnosticsconnectionstring-setting"></a>Onjuiste DiagnosticsConnectionString-instelling
Als toepassing gebruikmaakt van Azure Diagnostics, uw serviceconfiguratiebestand moet opgeven de `DiagnosticsConnectionString` configuratie-instelling. Deze instelling moet een HTTPS-verbinding naar uw opslagaccount opgeven in Azure.

Om ervoor te zorgen dat uw `DiagnosticsConnectionString` instelling correct is voordat u het pakket van uw toepassing in Azure implementeert, controleert u het volgende:  

* De `DiagnosticsConnectionString` instelling verwijst naar een geldig opslagaccount in Azure.  
  Standaard worden deze instelling verwijst naar de geëmuleerde storage-account, zodat u deze instelling expliciet wijzigen moet voordat u uw toepassingspakket implementeren. Als u deze instelling niet wijzigt, wordt er een uitzondering opgetreden bij de rolinstantie probeert te starten van de diagnostische monitor. Dit kan leiden tot de rolinstantie worden voor onbepaalde tijd wordt gerecycled.
* De verbindingsreeks is opgegeven in de volgende [indeling](../storage/common/storage-configure-connection-string.md). (Het protocol moet worden opgegeven als HTTPS). Vervang *MyAccountName* met de naam van uw opslagaccount en *MyAccountKey* door uw toegangssleutel:    

        DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey

  Als u uw toepassing ontwikkelt met behulp van Azure-hulpprogramma's voor Microsoft Visual Studio, kunt u de eigenschappenpagina's gebruiken om in te stellen deze waarde.

## <a name="exported-certificate-does-not-include-private-key"></a>Geëxporteerde certificaat bevat geen persoonlijke sleutel
Als u wilt uitvoeren van een Webrol onder SSL, moet u ervoor zorgen dat het geëxporteerde management-certificaat de persoonlijke sleutel bevat. Als u de *Windows Certificate Manager* voor het exporteren van het certificaat, moet u selecteren **Ja** voor de **de persoonlijke sleutel exporteren** optie. Het certificaat moet worden geëxporteerd in de PFX-indeling, dit de enige indeling die momenteel worden ondersteund is.

## <a name="next-steps"></a>Volgende stappen
Meer weergeven [artikelen over probleemoplossing](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) voor cloudservices.

Nog een rol recyclen van scenario's op weergeven [van Kevin Williamson blogserie](https://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

[RoleEntryPoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx
[OnStart]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx
[OnStop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[Uitvoeren]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx
