---
title: Aangepaste metrische gegevens in Azure Monitor
description: Meer informatie over aangepaste metrische gegevens in Azure Monitor en hoe ze zijn gemodelleerd.
author: ancav
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: 2b3e8877713fde088be2147bbfa8969e351a3f06
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53326090"
---
# <a name="custom-metrics-in-azure-monitor"></a>Aangepaste metrische gegevens in Azure Monitor

Als u bronnen en toepassingen in Azure implementeert, moet u beginnen met het verzamelen van telemetriegegevens om inzicht te verkrijgen over de prestaties en status. Azure maakt sommige metrische gegevens beschikbaar voor u hieraan. Deze metrische gegevens worden genoemd, standard- of -platform. Ze zijn echter beperkt in aard. Het is raadzaam voor het verzamelen van sommige aangepaste prestatie-indicatoren of -business-specifieke metrische gegevens voor meer inzicht te krijgen.
Deze **aangepaste** metrische gegevens kunnen worden verzameld via de telemetriegegevens van uw toepassing, een agent die wordt uitgevoerd op uw Azure-resources of zelfs een buiten-in bewakingssysteem en rechtstreeks verzonden naar Azure Monitor. Nadat ze zijn gepubliceerd naar Azure Monitor, kunt u bladeren, query, en waarschuwing op basis van aangepaste metrische gegevens voor uw Azure-resources en toepassingen naast de standaard metrische gegevens die door Azure worden verzonden.

## <a name="send-custom-metrics"></a>Aangepaste metrische gegevens verzenden
Aangepaste metrische gegevens kunnen worden verzonden naar Azure Monitor via verschillende methoden:
- Uw toepassing instrumenteren met behulp van de Azure Application Insights-SDK en aangepaste telemetrie verzenden naar Azure Monitor. 
- De uitbreiding voor de Windows Azure Diagnostics (WAD) installeren op uw [virtuele Azure-machine](collect-custom-metrics-guestos-resource-manager-vm.md), [virtuele-machineschaalset](collect-custom-metrics-guestos-resource-manager-vmss.md), [klassieke VM](collect-custom-metrics-guestos-vm-classic.md), of [klassieke Cloudservices](collect-custom-metrics-guestos-vm-cloud-service-classic.md) en prestatiemeteritems verzenden naar Azure Monitor. 
- Installeer de [InfluxData Telegraf agent](collect-custom-metrics-linux-telegraf.md) uitvoer invoegtoepassing op uw virtuele Azure Linux-machine en de metrische gegevens verzenden met behulp van de Azure Monitor.
- Aangepaste metrische gegevens verzenden [rechtstreeks naar de Azure Monitor REST API](../../monitoring-and-diagnostics/metrics-store-custom-rest-api.md), `https://<azureregion>.monitoring.azure.com/<AzureResourceID>/metrics`.

Wanneer u aangepaste metrische gegevens verzenden naar Azure Monitor, elk gegevenspunt of waarde, die wordt gerapporteerd, moeten de volgende informatie bevatten.

### <a name="authentication"></a>Verificatie
Om in te dienen aangepaste metrische gegevens naar Azure Monitor, de entiteit die is ingediend door de metrische gegevens moet een geldig Azure Active Directory (Azure AD)-token in de **Bearer** -header van de aanvraag. Er zijn enkele ondersteunde manieren om een geldige bearer-token verkrijgen:
1. [Identiteiten voor een Azure-resources beheerd](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview). Geeft een identiteit die tot een Azure-resource zelf, zoals een virtuele machine. Beheerde Service Identity (MSI) is ontworpen om u te machtigen voor resources bepaalde bewerkingen uitvoeren. Een voorbeeld toe dat er een resource om te verzenden van metrische gegevens over zelf. Een resource of het MSI-bestand, kan worden verleend **bewaking metrische gegevens Publisher** machtigingen op een andere resource. Met deze machtiging beschikt, kan het MSI-bestand metrische gegevens voor andere resources ook verzenden.
2. [Azure AD-Service-Principal](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals). In dit scenario, een Azure AD-toepassing of service, kunnen machtigingen worden toegewezen om te verzenden van metrische gegevens over een Azure-resource.
Azure Monitor wordt het toepassingstoken voor de verificatie van de aanvraag heeft gevalideerd met behulp van openbare sleutels van Azure AD. De bestaande **bewaking metrische gegevens Publisher** rol al over deze machtiging beschikt. Het is beschikbaar in Azure portal. De service-principal, afhankelijk van welke resources er aangepaste metrische gegevens voor, verzendt kan worden opgegeven die de **Publisher van metrische gegevens controleren** rol bij het bereik dat is vereist. Voorbeelden zijn een abonnement, resourcegroep of specifieke resource.

> [!NOTE]  
> Wanneer u een Azure AD-token voor het verzenden van aangepaste metrische gegevens aanvraagt, zorg ervoor dat de doelgroep of resource voor het token is aangevraagd https://monitoring.azure.com/. Zorg ervoor dat u de afsluitende slash (/).

### <a name="subject"></a>Onderwerp
Deze eigenschap wordt vastgelegd welke Azure-resource-ID voor de aangepaste metrische gegevens wordt gerapporteerd. Deze gegevens worden gecodeerd in de URL van de API-aanroep wordt gemaakt. Elke API kan alleen indienen voor metrische waarden voor een enkel Azure-resource.

> [!NOTE]  
> U kunt aangepaste metrische gegevens op basis van de resource-ID van een resourcegroep of abonnement kan niet verzenden.
>
>

### <a name="region"></a>Regio
Deze eigenschap bevat wat de resource die u bij het verzenden van metrische gegevens voor is geïmplementeerd in Azure-regio. Metrische gegevens moeten worden verzonden naar de dezelfde Azure-Monitor regionale eindpunt als de regio van de resource is geïmplementeerd in. Aangepaste metrische gegevens voor een virtuele machine geïmplementeerd in VS-West moeten bijvoorbeeld worden verzonden naar het eindpunt van WestUS regionale Azure Monitor. De regio-informatie is ook gecodeerd in de URL van de API-aanroep.

> [!NOTE]  
> Aangepaste metrische gegevens zijn alleen beschikbaar in een subset van de Azure-regio's tijdens de openbare preview. Een lijst met ondersteunde regio's wordt in een volgende sectie van dit artikel beschreven.
>
>

### <a name="timestamp"></a>Tijdstempel
Elk gegevenspunt verzonden naar Azure Monitor moet worden gemarkeerd met een tijdstempel. Deze timestamp bevat de datum/tijd waarop de metrische waarde wordt gemeten of verzameld. Azure Monitor worden metrische gegevens met tijdstempels die tot 20 minuten in het verleden en 5 minuten in de toekomst geaccepteerd.

### <a name="namespace"></a>Naamruimte
Naamruimten zijn een manier voor het categoriseren of vergelijkbare metrische gegevens te groeperen. U kunt met behulp van naamruimten, isolatie tussen groepen van metrische gegevens die verschillende insights of prestatie-indicatoren verzamelen mogelijk bereiken. Bijvoorbeeld, u mogelijk een naamruimte met de naam **ContosoMemoryMetrics** die gebruik van geheugen metrische gegevens die uw app-profiel wordt bijgehouden. Een andere naamruimte met de naam **ContosoAppTransaction** alle metrische gegevens over gebruikerstransacties in uw toepassing kunt bijhouden.

### <a name="name"></a>Name
**Naam** is de naam van de metrische gegevens die wordt gerapporteerd. Meestal is de naam van de beschrijving voor het identificeren van wat wordt gemeten. Een voorbeeld is een metrische waarde die het aantal bytes dat geheugen wordt gebruikt op een bepaalde virtuele machine meet. Dat er een metrische naam als **geheugen Bytes In gebruik**.

### <a name="dimension-keys"></a>Dimensiesleutels
Een dimensie is een sleutel of de waarde-paar waarmee u kunt aanvullende kenmerken Geef een beschrijving van de metrische gegevens worden verzameld. U kunt meer informatie over de metrische gegevens, waardoor meer inzicht te krijgen verzamelen met behulp van de aanvullende kenmerken. Bijvoorbeeld, de **geheugen Bytes In gebruik** metrische gegevens mogelijk een dimensie-sleutel met de naam **proces** die worden vastgelegd met het aantal bytes van het geheugen elk proces op een virtuele machine wordt gebruikt. U kunt de metrische gegevens om te zien hoeveel geheugen specifieke processen gebruiken of voor het identificeren van de top vijf processen door geheugengebruik filteren met behulp van deze sleutel.
Elke aangepaste metrische gegevens kan maximaal 10 dimensies hebben.

### <a name="dimension-values"></a>Dimensiewaarden
Tijdens het rapporteren van een metrische gegevenspunt voor elke dimensie-sleutel op de metrische gegevens worden gerapporteerd, is er een overeenkomende dimensiewaarde. Bijvoorbeeld, als u wilt rapporteren van het geheugen dat wordt gebruikt door de ContosoApp op de virtuele machine:

* De naam van de meetwaarde zou worden **geheugen Bytes in gebruik**.
* De dimensie-sleutel zou worden **proces**.
* De waarde van de dimensie **ContosoApp.exe**.

Bij het publiceren van een metrische waarde, kunt u alleen een waarde van één dimensie per dimensie sleutel opgeven. Als u de dezelfde geheugengebruik voor meerdere processen op de virtuele machine hebt verzameld, kunt u meerdere metrische waarden voor tijdstempel rapporteren. Elke metrische waarde geeft de waarde van een andere dimensie voor de **proces** dimensie-sleutel.

### <a name="metric-values"></a>Metrische waarden
Azure Monitor slaat alle metrische gegevens met granulariteit van één minuut intervallen. We begrijpen dat gedurende een bepaalde minuut een metrische waarde moet mogelijk meerdere keren worden verzameld. Een voorbeeld is de CPU-gebruik. Of misschien nodig zijn voor veel afzonderlijke gebeurtenissen worden gemeten. Een voorbeeld is aanmelden transactie latenties. Als u wilt beperken het nummer van ruwe waarden die u moet verzenden en te betalen in Azure Monitor, kunt u lokaal vooraf aggregeren en hoe de waarden:

* **Min**: De minimale waargenomen waarde van de voorbeelden en metingen in de minuut.
* **Maximale**: De maximale waargenomen waarde van de voorbeelden en metingen in de minuut.
* **Som**: De som van alle waarden in de voorbeelden en metingen waargenomen in de minuut.
* **Aantal**: Het aantal voorbeelden en metingen in de minuut.

Bijvoorbeeld, als er zijn 4 aanmelden transacties aan uw app gedurende een opgegeven per minuut, de resulterende gemeten, is de latentie voor elk mogelijk als volgt:

|Transactie 1|Transactie 2|Transactie 3|Transactie 4|
|---|---|---|---|
|7 ms|4 ms|13 ms|16 ms|
|

Vervolgens zou de resulterende metrische publicatie naar Azure Monitor als volgt zijn:
* Min.: 4
* Max.: 16
* Som: 40
* Aantal: 4

Als uw toepassing kan niet worden vooraf aggregeren lokaal en moet elke afzonderlijke voorbeeld of gebeurtenis onmiddellijk na verzameling verzenden, kunt u de onbewerkte meetwaarden kan verzenden. Bijvoorbeeld, telkens wanneer een transactie aanmelding wordt uitgevoerd op uw app u metrische gegevens publiceren naar Azure Monitor met alleen een meting. Dus voor een aanmelding transactie die 12 ms duurde, zou de metrische publicatie zijn als volgt:
* Min.: 12
* Max.: 12
* Som: 12
* Aantal: 1

Met dit proces kunt u meerdere waarden voor de dezelfde metrische waarde plus de combinatie van de dimensie tijdens een opgegeven minuut verzenden. Azure Monitor worden vervolgens neemt de ruwe waarden verzonden voor een bepaalde minuut en verzamelt deze samen.

### <a name="sample-custom-metric-publication"></a>Voorbeeld-publicatie voor aangepaste metrische gegevens
In het volgende voorbeeld maakt u een aangepaste metrische gegevens met de naam **geheugen Bytes in gebruik** onder de metrische naamruimte **geheugen profiel** voor een virtuele machine. De metrische gegevens heeft één dimensie met de naam **proces**. Voor de opgegeven timestamp verzenden we metrische waarden voor twee verschillende processen:

```json
{
    "time": "2018-08-20T11:25:20-7:00",
    "data": {

      "baseData": {

        "metric": "Memory Bytes in Use",
        "namespace": "Memory Profile",
        "dimNames": [
          "Process"        ],
        "series": [
          {
            "dimValues": [
              "ContosoApp.exe"
            ],
            "min": 10,
            "max": 89,
            "sum": 190,
            "count": 4
          },
          {
            "dimValues": [
              "SalesApp.exe"
            ],
            "min": 10,
            "max": 23,
            "sum": 86,
            "count": 4
          }
        ]
      }
    }
  }
```
> [!NOTE]  
> Application Insights, de extensie voor diagnostische gegevens en de agent InfluxData Telegraf zijn al geconfigureerd voor metrische waarden voor het juiste regionale eindpunt verzenden en uitvoeren van de voorgaande eigenschappen in elke uitstoot.
>
>

## <a name="custom-metric-definitions"></a>Aangepaste metrische definities
Er is niet nodig om te definiëren van een aangepaste metrische gegevens in Azure Monitor voordat deze wordt verzonden. Elk punt metrische gegevens is gepubliceerd bevat naamruimte, naam en dimensie-informatie. Zodat de eerste keer dat een aangepaste meetwaarde is verzonden naar Azure Monitor, wordt er automatisch een metrische definitie gemaakt. Deze metrische definitie is vervolgens kunnen worden gedetecteerd voor de resources die de metrische gegevens tegen wordt verzonden via de metrische definities.

> [!NOTE]  
> Azure Monitor biedt geen nog ondersteuning voor het definiëren van **eenheden** voor een aangepaste metrische gegevens.

## <a name="using-custom-metrics"></a>Met aangepaste metrische gegevens
Nadat de aangepaste metrische gegevens worden verzonden naar Azure Monitor, kunt u ze via de Azure-portal bladeren en query's via de Azure Monitor REST API's voor uitvoert. U kunt ook waarschuwingen maken op deze om u te waarschuwen wanneer aan bepaalde voorwaarden wordt voldaan.
### <a name="browse-your-custom-metrics-via-the-azure-portal"></a>Uw aangepaste metrische gegevens via de Azure-portal bladeren
1.  Ga naar de [Azure Portal](https://portal.azure.com).
2.  Selecteer de **Monitor** deelvenster.
3.  Selecteer **Metrische gegevens**.
4.  Selecteer een resource die u kunt aangepaste metrische gegevens tegen hebt verzonden.
5.  Selecteer de naamruimte van de metrische gegevens voor uw aangepaste metrische gegevens.
6.  Selecteer de aangepaste metrische gegevens.

## <a name="supported-regions"></a>Ondersteunde regio’s
Tijdens de preview-versie is de mogelijkheid voor het publiceren van aangepaste metrische gegevens alleen beschikbaar in een subset van de Azure-regio's. Deze beperking betekent dat de metrische gegevens alleen voor resources in een van de ondersteunde regio's kunnen worden gepubliceerd. De volgende tabel wordt de lijst met ondersteunde Azure-regio's voor aangepaste metrische gegevens. Het is ook een lijst met de bijbehorende eindpunten die metrische gegevens voor resources in die regio's moet worden gepubliceerd op:

|Azure-regio|Regionale eindpunt voorvoegsel|
|---|---|
|US - oost|https://eastus.monitoring.azure.com/|
|US - zuid-centraal|https://southcentralus.monitoring.azure.com/|
|US - west-centraal|https://westcentralus.monitoring.azure.com/|
|US - west 2|https://westus2.monitoring.azure.com/|
|Azië - zuidoost|https://southeastasia.monitoring.azure.com/|
|Europa - noord|https://northeurope.monitoring.azure.com/|
|Europa -west|https://westeurope.monitoring.azure.com/|

## <a name="quotas-and-limits"></a>Quota en limieten
Azure Monitor legt de volgende limieten op aangepaste metrische gegevens:

|Categorie|Limiet|
|---|---|
|Actieve tijd reeks/abonnementen/regio|50,000|
|Dimensiesleutels per metrisch gegeven|10|
|De lengte van tekenreeks voor metrische naamruimten, metrische namen, dimensiesleutels en dimensiewaarden|256 tekens|
Een reeks actieve tijd wordt gedefinieerd als een unieke combinatie van metrische gegevens, dimensie-sleutel of de waarde van de dimensie die metrische waarden die zijn gepubliceerd in de afgelopen 12 uur heeft gehad.

## <a name="next-steps"></a>Volgende stappen
Aangepaste metrische gegevens van andere services gebruiken: 
 - [Virtuele machines](collect-custom-metrics-guestos-resource-manager-vm.md)
 - [Virtuele-machineschaalset](collect-custom-metrics-guestos-resource-manager-vmss.md)
 - [Virtuele Azure-Machines (klassiek)](collect-custom-metrics-guestos-vm-classic.md)
 - [Met behulp van de agent Telegraf virtuele Linux-Machine](collect-custom-metrics-linux-telegraf.md)
 - [REST API](../../monitoring-and-diagnostics/metrics-store-custom-rest-api.md)
 - [Klassieke Cloudservices](collect-custom-metrics-guestos-vm-cloud-service-classic.md)
 