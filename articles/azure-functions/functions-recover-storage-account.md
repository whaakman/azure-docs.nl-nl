---
title: Problemen oplossen met Azure Functions-Runtime is niet bereikbaar.
description: Informatie over het oplossen van een ongeldige storage-account.
services: functions
documentationcenter: ''
author: alexkarcher-msft
manager: cfowler
editor: ''
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: alkarche
ms.openlocfilehash: 1902091978233ecaf80f04e3a08c70c20aee42c9
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "54000016"
---
# <a name="how-to-troubleshoot-functions-runtime-is-unreachable"></a>Problemen oplossen met "functions-runtime is niet bereikbaar"


## <a name="error-text"></a>Fouttekst
Dit document is bedoeld om problemen met de volgende fout weergegeven in de Functions-portal.

`Error: Azure Functions Runtime is unreachable. Click here for details on storage configuration`

### <a name="summary"></a>Samenvatting
Dit probleem treedt op wanneer de Azure Functions-Runtime kan niet worden gestart. De meest voorkomende reden voor deze fout kan optreden, is de functie-app toegang verliezen tot het opslagaccount. [Meer informatie over de vereisten voor een opslagaccount hier](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal#storage-account-requirements)

### <a name="troubleshooting"></a>Problemen oplossen
We behandelen de vier meest voorkomende foutgevallen, identificeren en over het oplossen van elke aanvraag.

1. Storage-Account is verwijderd
1. Toepassingsinstellingen voor Storage-Account verwijderd
1. Opslagaccountreferenties is ongeldig
1. Storage-Account is niet toegankelijk
1. Uitvoering van het quotum voor dagelijkse volledige

## <a name="storage-account-deleted"></a>Storage-account is verwijderd

Elke functie-app is vereist voor een opslagaccount om te werken. Als dit account wordt verwijderd. de functie werkt niet.

### <a name="how-to-find-your-storage-account"></a>Over het vinden van uw storage-account

Gestart door het opzoeken van de naam van uw opslagaccount in de instellingen van uw toepassing. De `AzureWebJobsStorage` of `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` bevat de naam van uw storage-account dat is opgenomen in een verbindingsreeks. Lees meer informatie op de [instellingenverwijzing hier toepassing](https://docs.microsoft.com/azure/azure-functions/functions-app-settings#azurewebjobsstorage)

Zoeken naar uw opslagaccount in Azure portal om te zien als deze nog steeds bestaat. Als deze is verwijderd, moet u opnieuw een storage-account maken en deze vervangen door uw storage-verbindingsreeksen. Uw functiecode verloren en moet u het opnieuw implementeren.

## <a name="storage-account-application-settings-deleted"></a>Toepassingsinstellingen voor Storage-account verwijderd

In de vorige stap, als u geen verbindingsreeks voor opslag account zijn ze waarschijnlijk verwijderd of overschreven. Verwijderen van app-instellingen wordt meestal gedaan bij het gebruik van implementatiesites of Azure Resource Manager-scripts om in te stellen toepassingsinstellingen.

### <a name="required-application-settings"></a>Instellingen van de vereiste toepassing

* Vereist
    * [`AzureWebJobsStorage`](https://docs.microsoft.com/azure/azure-functions/functions-app-settings#azurewebjobsstorage)
* Vereist voor verbruik Plan functies
    * [`WEBSITE_CONTENTAZUREFILECONNECTIONSTRING`](https://docs.microsoft.com/azure/azure-functions/functions-app-settings#websitecontentazurefileconnectionstring)
    * [`WEBSITE_CONTENTSHARE`](https://docs.microsoft.com/azure/azure-functions/functions-app-settings#websitecontentshare)

[Meer informatie over instellingen voor deze toepassing hier](https://docs.microsoft.com/azure/azure-functions/functions-app-settings)

### <a name="guidance"></a>Richtlijnen

* 'Site-instelling' niet controleren voor een van deze instellingen. Wanneer u sleuven implementatie van de functie wordt verbroken.
* Stel deze instellingen niet bij het gebruik van geautomatiseerde implementaties.
* Deze instellingen moeten zijn opgegeven en geldig tijdens de aanmaak. Een geautomatiseerde implementatie die deze instellingen niet bevat resulteert in een niet-functionele App, zelfs als de instellingen worden toegevoegd na de gebeurtenis.

## <a name="storage-account-credentials-invalid"></a>Opslagaccountreferenties is ongeldig

De bovenstaande verbindingsreeksen voor Storage-Account moeten worden bijgewerkt als u opslagsleutels opnieuw genereert. [Meer informatie over storage Sleutelbeheer hier](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account#manage-your-storage-account)

## <a name="storage-account-inaccessible"></a>Storage-account is niet toegankelijk

Uw functie-App moet toegang hebben tot het opslagaccount. Veelvoorkomende problemen dat blok een functies toegang tot een opslagaccount zijn:

* Functie-Apps geïmplementeerd naar App Service-omgevingen zonder de juiste netwerk-regels waarmee verkeer van en naar het storage-account
* De storage-account firewall is ingeschakeld en niet worden geconfigureerd zodat verkeer naar en van functies. [Lees meer over de storage-account firewall-configuratie hier](https://docs.microsoft.com/azure/storage/common/storage-network-security?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)

## <a name="daily-execution-quota-full"></a>Uitvoering van het quotum voor dagelijkse volledige

Als u het quotum voor een dagelijkse uitvoering geconfigureerd hebt, uw functie-App tijdelijk uitgeschakeld en veel van de portal besturingselementen wordt niet beschikbaar. 

* Als u wilt controleren, controle platformfuncties openen > functie-App-instellingen in de portal. U ziet het volgende bericht weergegeven als u overschreden
    * `The Function App has reached daily usage quota and has been stopped until the next 24 hours time frame.`
* Het quotum verwijderen en opnieuw starten van uw app om het probleem te verhelpen.

## <a name="next-steps"></a>Volgende stappen

Nu dat uw functie-App terug en operationele is Bekijk onze snelstartgidsen en developer-verwijzingen aan de slag kunt en nogmaals uit te voeren!

* [Uw eerste Azure-functie maken](functions-create-first-azure-function.md)  
  Ga meteen aan de slag en maak uw eerste functie met de Azure Functions-snelstartgids. 
* [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)  
  Biedt meer technische informatie over de Azure Functions-runtime en bevat gedetailleerde informatie over het coderen van functies en het definiëren van triggers en bindingen.
* [Azure Functions testen](functions-test-a-function.md)  
  Beschrijft de verschillende hulpprogramma’s en technieken voor het testen van uw functies.
* [Azure Functions schalen](functions-scale.md)  
  Beschrijft de serviceabonnementen die beschikbaar zijn voor Azure Functions, zoals het hostingabonnement Consumption, en helpt u bij het kiezen van het juiste abonnement. 
* [Meer informatie over Azure App Service](../app-service/overview.md)  
  Azure Functions maakt gebruik van Azure App Service voor kernfunctionaliteit zoals implementaties, omgevingsvariabelen en diagnostische procedures. 
