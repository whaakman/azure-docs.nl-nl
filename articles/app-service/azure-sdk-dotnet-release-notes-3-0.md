---
title: Azure SDK voor .NET 3.0 Release-opmerkingen | Microsoft Docs
description: Azure SDK voor .NET 3.0 Release-opmerkingen
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: c83d815b-fc19-4260-821e-7d2a7206dffc
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 03/07/2017
ms.author: juliako
ms.openlocfilehash: eea4e569ac2d0192ed7872d2fcb9bed03614832b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-sdk-for-net-30-release-notes"></a>Azure SDK voor .NET 3.0 release-opmerkingen

Dit onderwerp bevat de releaseopmerkingen voor versie 3.0 van de Azure SDK voor .NET.

##<a name="azure-sdk-for-net-30-release-summary"></a>Azure SDK voor .NET 3.0 release samenvatting

Releasedatum: 07-03/2017
 
Geen recente wijzigingen in de Azure SDK 3.0 zijn geïntroduceerd in deze release. Er is geen upgradeproces nodig gebruikmaken van deze SDK met bestaande Cloud Service-projecten. Als u gebruik van de Azure SDK 3.0 zonder een upgradeproces, installeert Azure SDK 3.0 in dezelfde directory als de Azure SDK 2.9. Meest voor de onderdelen is niet veranderd met de primaire versie van 2.9 maar in plaats daarvan het build-nummer zojuist hebt bijgewerkt.

## <a name="visual-studio-2017-rtw"></a>Visual Studio 2017 RTW

- In Visual Studio-2017 is deze versie van de Azure SDK voor .NET ingebouwd in de Azure-werkbelasting. Alle hulpprogramma's die u wilt ontwikkelen van Azure zal onderdeel zijn van Visual Studio 2017 voortaan. Voor Visual Studio 2015 steeds de SDK nog beschikbaar is via WebPI. We hebben Azure SDK voor .NET-versies voor Visual Studio 2013 stopgezet nu dat Visual Studio 2017 is vrijgegeven.

### <a name="azure-diagnostics"></a>Azure Diagnostics

- Het gedrag voor het opslaan van slechts een gedeeltelijke verbindingsreeks met de sleutel vervangen door een token voor de verbindingsreeks voor opslag van Cloud Services-diagnostische gegevens wordt gewijzigd. De werkelijke opslagsleutel worden nu opgeslagen in de map van het gebruikersprofiel en kan dus de toegang kan worden beheerd. Visual Studio, leest van de opslagsleutel van gebruikersprofielmap voor lokale foutopsporing en publicatieproces. 
- Visual Studio Online-team verbeterd in reactie op de wijziging die hierboven worden beschreven, de Azure Cloud Services-implementatiesjabloon taak zodat gebruikers de opslagsleutel voor het instellen van de extensie voor diagnostische gegevens bij het publiceren naar Azure in continue integratie en implementatie kunnen opgeven.
- We hebben aangebracht voor het opslaan van beveiligde verbindingsreeks en tokeniseren voor Azure Diagnostics (af), om u te helpen bij het oplossen van problemen met configuratie over environements mogelijk.
 
### <a name="windows-server-2016-virtual-machines"></a>Windows Server 2016 virtuele machines

- Visual Studio biedt nu ondersteuning voor het implementeren van Cloud-Services op OS-familie 5 (Windows Server 2016) virtuele machines. Voor bestaande cloudservices, kunt u uw instellingen voor het doel van de nieuwe OS-familie. Bij het maken van nieuwe cloudservices als u wilt maken van de service met .net 4.6 of hoger, wordt standaard de service voor het gebruik van de OS-familie 5.  Raadpleeg voor meer informatie de [Gastbesturingssysteemgroep ondersteunen tabel](../cloud-services/cloud-services-guestos-update-matrix.md).

### <a name="known-issues"></a>Bekende problemen

- Azure .NET SDK 3.0 geïntroduceerd om een probleem bij het verwijderen van Visual Studio 2017 in de configuratie van een gelijktijdige met Visual Studio 2015.  Als u de Azure SDK voor Visual Studio 2015 geïnstalleerd hebt, worden de Microsoft Azure-Opslagemulator en Microsoft Azure Compute-Emulator verwijderd als u Visual Studio 2017 verwijdert.  Het resultaat is een fout bij het maken en foutopsporing van nieuwe Cloud services-projecten in Visual Studio 2015. Om dit probleem oplossen, installeer de Azure-SDK van de Web Platform Installer.  Het probleem worden opgelost in een toekomstige update voor Visual Studio 2017.  .

 
### <a name="azure-in-role-cache"></a>Azure In-Role Cache 

- Ondersteuning voor Azure In-Role Cache is gestopt op 30 November 2016. Klik voor meer informatie [hier](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).




