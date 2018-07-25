---
title: Azure SDK voor .NET 2.9 opmerkingen bij de Release
description: Azure SDK voor .NET 2.9 opmerkingen bij de Release
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: ''
ms.assetid: c83d815b-fc19-4260-821e-7d2a7206dffc
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 01b8ccc9fe6b5469408131bce68a903f15382998
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39222525"
---
# <a name="azure-sdk-for-net-29-release-notes"></a>Azure SDK voor .NET 2.9 opmerkingen bij de release

Dit onderwerp bevat de releaseopmerkingen voor versie 2.9 en 2.9.6 van Azure SDK voor .NET.

## <a name="azure-sdk-for-net-296-release-summary"></a>Overzicht van de Azure SDK voor .NET 2.9.6 release

Releasedatum: 16-11-2016
 
Er zijn geen belangrijke wijzigingen in de Azure SDK 2.9 gebruikt zijn geïntroduceerd in deze release. Er is ook geen upgradeproces nodig gebruikmaken van deze SDK met bestaande projecten voor de Service in de Cloud.

### <a name="visual-studio-2017-release-candidate"></a>Visual Studio 2017 Release Candidate

- Deze versie van de Azure SDK voor .NET is in Visual Studio 2017 RC, ingebouwde voor de Azure Workload. Alle hulpprogramma's die u moet doen van Azure-ontwikkeling zal onderdeel van Visual Studio 2017 RC voortaan zijn. Voor Visual Studio 2015 en Visual Studio 2013, wordt de SDK nog steeds beschikbaar zijn via WebPI. We zullen worden beëindigde Azure SDK voor .NET-versies voor Visual Studio 2013, wanneer Visual Studio 2017 is vrijgegeven als een uiteindelijke product. Volg deze koppeling voor Visual Studio 2017 RC downloaden: https://www.visualstudio.com/vs/visual-studio-2017-rc/

### <a name="azure-diagnostics"></a>Azure Diagnostics

- Het gedrag voor het opslaan van slechts een gedeeltelijke verbindingsreeks met de sleutel die is vervangen door een token voor de opslagverbindingsreeks van diagnostische gegevens van Cloud Services gewijzigd. De werkelijke opslagsleutel is nu opgeslagen in de map gebruiker profiel zodat de toegang kan worden beheerd. Visual Studio wordt de toegangssleutel gelezen uit de map gebruiker profiel voor lokale foutopsporing en publiceren. 
- In reactie op de wijziging die hierboven worden beschreven, Visual Studio Online team de sjabloon van Azure Cloud Services-implementatie taak uitgebreid zodat gebruikers kunnen opgeven aan welke toegangssleutel van het opslagaccount voor het instellen van de extensie voor diagnostische gegevens bij het publiceren naar Azure in doorlopende integratie en De implementatie.
- We hebben gemaakt voor het opslaan van beveiligde verbindingsreeks en tokenisering voor Azure Diagnostics (WAD), om u te helpen bij het oplossen van problemen met configuratie voor omgevingen vereisen mogelijk.
 
### <a name="windows-server-2016-virtual-machines"></a>Windows Server 2016 virtuele machines

- Visual Studio biedt nu ondersteuning voor het implementeren van Cloud Services met OS Family 5 (Windows Server 2016) virtuele machines. Voor bestaande cloudservices, kunt u uw instellingen wilt richten op het nieuwe type besturingssysteem. Bij het maken van nieuwe cloudservices, als u ervoor kiest om de service met behulp van .net 4.6 of hoger te maken, wordt standaard de service voor het gebruik van OS Family 5.  U kunt raadplegen voor meer informatie de [Gast OS-familie ondersteunt tabel](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/).

#### <a name="known-issues"></a>Bekende problemen

- Azure .NET SDK 2.9.6 geïntroduceerd een beperking waardoor de implementatie van projecten met behulp van niet-ondersteunde .NET frameworks (zoals .NET 4.6) naar een OS-familie < 5. Een tijdelijke oplossing wordt geboden [hier](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9).

 
### <a name="azure-in-role-cache"></a>Azure In-Role Cache 

- Ondersteuning voor Azure In-Role Cache eindigt op 30 November 2016. Voor meer informatie klikt u op [hier](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).

### <a name="azure-resource-manager-templates-for-azure-stack"></a>Azure Resource Manager-sjablonen voor Azure Stack

- Azure Stack hebben als implementatiedoel we geïntroduceerd voor uw Azure Resource Manager-sjablonen.


## <a name="azure-sdk-for-net-29-summary"></a>Azure SDK voor .NET 2.9 samenvatting

## <a name="overview"></a>Overzicht
Dit document bevat de releaseopmerkingen voor de Azure SDK voor .NET 2.9 release. 

Zie voor gedetailleerde informatie over de updates in deze release de [Azure SDK 2.9 gebruikt aankondiging post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a>Azure SDK 2.9 gebruikt voor Visual Studio 2015 Update 2 en Visual Studio '15' Preview
Deze update bevat de volgende oplossingen voor problemen:

* Probleem met betrekking tot de REST API-Client genereren waarin de tekenreeks "Onbekend Type" wordt weergegeven als de naam van de map van de code-generatie en/of de naam van de naamruimte dat in de gegenereerde code.
* Probleem met betrekking tot geplande webtaken waarin de verificatiegegevens moeten worden doorgegeven aan de Scheduler inrichtingsproces is mislukt.

Deze update bevat de volgende nieuwe functie:

* Ondersteuning voor secundaire App-Services op het tabblad 'Services' van het dialoogvenster voor het inrichten van App Service. 

## <a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a>Azure Data Lake Tools voor Visual Studio 2015 Update 2
Deze updates omvat het volgende:

* **Azure Data Lake Tools** voor Visual Studio nu samengevoegd met de Azure SDK voor .NET versie. Het hulpprogramma wordt automatisch geïnstalleerd wanneer u de Azure SDK installeert. 
  
    Het hulpprogramma regelmatig wordt bijgewerkt, gaat u [hier](http://aka.ms/datalaketool) om de updates te downloaden.
* **Server Explorer** nu kunt u Alles weergeven en maken van sommige entiteiten van de metagegevens van U-SQL. Zie voor meer informatie, [dit](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blog.

## <a name="hdinsight-tools"></a>HDInsight-hulpprogramma 's
**HDInsight-hulpprogramma's** voor Visual Studio nu ondersteunt HDInsight versie 3.3, met inbegrip van met Tez-grafieken en andere taal worden opgelost.

## <a name="azure-resource-manager"></a>Azure Resource Manager
Deze release voegt [KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) ondersteuning voor Resource Manager-sjablonen.

## <a name="see-also"></a>Zie ook
[Azure SDK 2.9 gebruikt aankondiging post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)

