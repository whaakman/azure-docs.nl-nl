---
title: Azure SDK voor .NET 2.9 Release-opmerkingen
description: Azure SDK voor .NET 2.9 Release-opmerkingen
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
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 199f0906f73d693d7cd4b73c928f23ae83b99596
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="azure-sdk-for-net-29-release-notes"></a>Azure SDK voor .NET 2.9 release-opmerkingen

Dit onderwerp bevat opmerkingen bij de release voor versies 2.9 en 2.9.6 van Azure SDK voor .NET.

##<a name="azure-sdk-for-net-296-release-summary"></a>Azure SDK voor .NET 2.9.6 release samenvatting

Releasedatum: 16-11-2016
 
Geen recente wijzigingen in de Azure SDK 2.9 zijn geïntroduceerd in deze release. Er is geen upgradeproces nodig gebruikmaken van deze SDK met bestaande Cloud Service-projecten.

### <a name="visual-studio-2017-release-candidate"></a>Visual Studio 2017 Release Candidate

- Deze versie van de Azure SDK voor .NET is Visual Studio 2017 RC ingebouwd in in de Azure-werkbelasting. Alle hulpprogramma's die u wilt ontwikkelen van Azure zal onderdeel zijn van Visual Studio 2017 RC voortaan. Voor Visual Studio 2015 en Visual Studio 2013 steeds de SDK nog beschikbaar is via WebPI. We zullen worden beëindigde Azure SDK voor .NET-versies voor Visual Studio 2013 wanneer Visual Studio 2017 als laatste product loslaat. Volg deze link om te downloaden van Visual Studio 2017 RC: https://www.visualstudio.com/vs/visual-studio-2017-rc/

### <a name="azure-diagnostics"></a>Azure Diagnostics

- Het gedrag voor het opslaan van slechts een gedeeltelijke verbindingsreeks met de sleutel vervangen door een token voor de verbindingsreeks voor opslag van Cloud Services-diagnostische gegevens wordt gewijzigd. De werkelijke opslagsleutel worden nu opgeslagen in de map van het gebruikersprofiel en kan dus de toegang kan worden beheerd. Visual Studio, leest van de opslagsleutel van gebruikersprofielmap voor lokale foutopsporing en publicatieproces. 
- Visual Studio Online-team verbeterd in reactie op de wijziging die hierboven worden beschreven, de Azure Cloud Services-implementatiesjabloon taak zodat gebruikers de opslagsleutel voor het instellen van de extensie voor diagnostische gegevens bij het publiceren naar Azure in continue integratie en implementatie kunnen opgeven.
- We hebben aangebracht voor het opslaan van beveiligde verbindingsreeks en tokeniseren voor Azure Diagnostics (af), om u te helpen bij het oplossen van problemen met configuratie over environements mogelijk.
 
### <a name="windows-server-2016-virtual-machines"></a>Windows Server 2016 virtuele machines

- Visual Studio biedt nu ondersteuning voor het implementeren van Cloud-Services op OS-familie 5 (Windows Server 2016) virtuele machines. Voor bestaande cloudservices, kunt u uw instellingen voor het doel van de nieuwe OS-familie. Bij het maken van nieuwe cloudservices als u wilt maken van de service met .net 4.6 of hoger, wordt standaard de service voor het gebruik van de OS-familie 5.  Raadpleeg voor meer informatie de [Gastbesturingssysteemgroep ondersteunen tabel](https://azure.microsoft.com/en-us/documentation/articles/cloud-services-guestos-update-matrix/).

#### <a name="known-issues"></a>Bekende problemen

- Azure .NET SDK 2.9.6 geïntroduceerd een beperking die implementatie van de projecten met behulp van niet-ondersteunde .NET frameworks (zoals .NET 4.6) naar een OS-familie blokkeert < 5. Een tijdelijke oplossing wordt aangeboden [hier](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9).

 
### <a name="azure-in-role-cache"></a>Azure In-Role Cache 

- Ondersteuning voor Azure In-Role Cache eindigt op 30 November 2016. Klik voor meer informatie [hier](https://azure.microsoft.com/en-us/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).

### <a name="azure-resource-manager-templates-for-azure-stack"></a>Azure Resource Manager-sjablonen voor Azure Stack

- Hebben wij de Stack Azure als het implementatiedoel van een voor uw Azure Resource Manager-sjablonen.


## <a name="azure-sdk-for-net-29-summary"></a>Azure SDK voor .NET 2.9 samenvatting

## <a name="overview"></a>Overzicht
Dit document bevat de releaseopmerkingen voor de Azure SDK voor .NET 2.9 release. 

Zie voor gedetailleerde informatie over de updates in deze release de [Azure SDK 2.9 aankondiging post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a>Azure SDK 2.9 voor Visual Studio 2015 Update 2 en Visual Studio "15" Preview
Deze update bevat de volgende oplossingen voor problemen:

* Probleem dat is gerelateerd aan de REST-API-Client generatie in waarin de tekenreeks 'Onbekend Type' wordt weergegeven als de naam van de map code gen en/of de naam van de naamruimte neergezet in de gegenereerde code.
* Het probleem is gerelateerd aan geplande webtaken waarin de verificatiegegevens konden niet worden doorgegeven aan de planner inrichtingsproces.

Deze update bevat de volgende nieuwe functie:

* Ondersteuning voor secundaire App-Services op het tabblad 'Services' van het dialoogvenster voor het inrichten van App Service. 

## <a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a>Azure Data Lake Tools voor Visual Studio 2015 Update 2
Deze update omvat het volgende:

* **Azure Data Lake Tools** voor Visual Studio nu samengevoegd met de Azure SDK voor .NET versie. Het hulpprogramma wordt automatisch geïnstalleerd wanneer u Azure SDK installeert. 
  
    Het hulpprogramma vaak wordt bijgewerkt, gaat u [hier](http://aka.ms/datalaketool) om de updates te downloaden.
* **Server Explorer** nu kunt u alle weergeven en sommige U-SQL-metagegevens-entiteiten maken. Zie voor meer informatie [dit](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blog.

## <a name="hdinsight-tools"></a>HDInsight-hulpprogramma 's
**HDInsight Tools** voor Visual Studio nu ondersteunt HDInsight versie 3.3, inclusief met Tez-grafieken en andere taal worden opgelost.

## <a name="azure-resource-manager"></a>Azure Resource Manager
Deze release voegt [KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) ondersteuning voor Resource Manager-sjablonen.

## <a name="see-also"></a>Zie ook
[Azure SDK 2.9 aankondiging post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)

