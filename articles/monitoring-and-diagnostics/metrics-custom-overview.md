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
ms.openlocfilehash: 1bdf1e1f5e58ecb0939d5876e0cef349e32de517
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344743"
---
# <a name="custom-metrics-in-azure-monitor"></a>Aangepaste metrische gegevens in Azure Monitor

Als u bronnen en toepassingen in Azure implementeert, moet u beginnen met het verzamelen van telemetriegegevens om inzicht te verkrijgen over de prestaties en status. Azure maakt sommige metrische gegevens voor u beschikbaar out-of-the-box terwijl u resources implementeert. Deze worden metrische gegevens voor standard- of -platform genoemd. Deze metrische gegevens zijn echter beperkt in aard. U kunt desgewenst voor het verzamelen van sommige aangepaste prestatie-indicatoren of -business-specifieke metrische gegevens voor meer inzicht te krijgen.
Deze 'aangepaste' metrische gegevens worden verzameld via de telemetriegegevens van uw toepassing, een agent die wordt uitgevoerd op uw Azure-resources of zelfs buitenaf in bewakingssysteem en rechtstreeks naar Azure Monitor verzonden. Zodra gepubliceerd naar Azure Monitor, kunt u bladeren, query's uitvoeren en waarschuwing op basis van aangepaste metrische gegevens voor uw Azure-resources en toepassingen naast de standaard metrische gegevens die door Azure worden verzonden.

## <a name="send-custom-metrics"></a>Aangepaste metrische gegevens verzenden
Aangepaste metrische gegevens kunnen worden verzonden naar Azure Monitor via verschillende methoden.
- Instrumenteer uw toepassing met behulp van de Application Insights-SDK en aangepaste telemetrie verzendt naar Azure Monitor 
- Installeer de Windows-extensie voor diagnostische gegevens op uw [virtuele Azure-machine](metrics-store-custom-guestos-resource-manager-vm.md), [virtuele-machineschaalset](metrics-store-custom-guestos-resource-manager-vmss.md), [klassieke VM](metrics-store-custom-guestos-classic-vm.md), of [klassieke Cloudservice](metrics-store-custom-guestos-classic-cloud-service.md)en prestatiemeteritems verzenden naar Azure Monitor 
- Installeer de [InfluxDB Telegraf agent](metrics-store-custom-linux-telegraf.md) op uw virtuele Azure Linux-machine en de metrische gegevens over verzenden met behulp van de invoegtoepassing Azure Monitor-uitvoer
- Aangepaste metrische gegevens verzenden [rechtstreeks naar Azure Monitor REST API](metrics-store-custom-rest-api.md) https://<azureregion>.monitoring.azure.com/<AzureResourceID>/metrics

Wanneer u aangepaste metrische gegevens naar Azure Monitor verzenden, moet elke gegevens punt (of waarde) gerapporteerd de volgende informatie bevatten:

### <a name="authentication"></a>Verificatie
De entiteit voor het indienen van de metrische gegevens moet een geldige Azure Active Directory-token hebben in de header 'Bearer' van de aanvraag om in te dienen aangepaste metrische gegevens naar Azure Monitor. Er zijn enkele ondersteunde manieren om een geldige bearer-token verkrijgen:
1. [Identiteiten voor een Azure-resources beheerd](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) -biedt een identiteit en een Azure-Resource zelf (zoals een virtuele machine). MSI is ontworpen om resources machtigingen voor het uitvoeren van bepaalde activiteiten, bijvoorbeeld, zodat een resource om te verzenden van metrische gegevens over zichzelf. Een resource (of de MSI-bestand) kan machtigingen worden verleend 'Bewaking van metrische gegevens Publisher' voor een andere bron, waardoor u het MSI-bestand om te verzenden van metrische gegevens voor andere resources ook.
2. [AAD-Service-Principal](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals) -hier het scenario is een AAD-toepassing (service) kan worden toegewezen machtigingen voor het verzenden van metrische gegevens over een Azure-resource.
Azure Monitor worden gevalideerd voor de verificatie van de aanvraag heeft de token van de toepassing met behulp van openbare sleutels van AAD. De bestaande 'Bewaking van metrische gegevens Publisher'-rol heeft al deze machtiging beschikt, die beschikbaar in de Azure-portal is. De service-principal, afhankelijk van welke resources er aangepaste metrische gegevens voor, wordt die kan worden opgegeven als 'Bewaking van metrische gegevens Publisher'-rol op het bereik vereist (abonnement, resourcegroep of specifieke resource).

> [!NOTE]
> Wanneer aanvragen van een AAD-token voor het verzenden van aangepaste metrische gegevens Zorg ervoor dat de doelgroep/resource het token wordt aangevraagd voor https://monitoring.azure.com/ (Zorg ervoor dat u de afsluitende slash (/))

### <a name="subject"></a>Onderwerp
Deze eigenschap wordt vastgelegd welke Azure-resource-ID voor de aangepaste metrische gegevens wordt gerapporteerd. Deze gegevens worden gecodeerd in de URL van de API-aanroep wordt gemaakt. Elke API kan alleen indienen voor metrische waarden voor een enkel Azure-resource.

> [!NOTE]
> U kunt aangepaste metrische gegevens op basis van de resource-ID van een resourcegroep of abonnement kan niet verzenden.
>
>

### <a name="region"></a>Regio
Deze eigenschap bevat wat de resource die u bij het verzenden van metrische gegevens voor is geïmplementeerd in Azure-regio. Metrische gegevens moeten worden verzonden naar de dezelfde Azure-Monitor regionale eindpunt als de regio van de resource is geïmplementeerd in. Bijvoorbeeld, moeten aangepaste metrische gegevens voor een virtuele machine zijn geïmplementeerd in VS-West worden verzonden naar het eindpunt van WestUS regionale Azure Monitor. De regio-informatie is ook gecodeerd in de URL van de API-aanroep.

> [!NOTE]
> Aangepaste metrische gegevens is alleen beschikbaar in een subset van de Azure-regio's tijdens de openbare preview. Een lijst met ondersteunde regio's wordt in een volgende sectie van dit artikel beschreven.
>
>

### <a name="timestamp"></a>Tijdstempel
Elk gegevenspunt verzonden naar Azure Monitor moet worden gemarkeerd met een tijdstempel. Deze timestamp bevat de datum en tijd waarop de metrische waarde gemeten/verzameld is. Azure Monitor worden metrische gegevens met tijdstempels die tot 20 minuten in het verleden en van 5 minuten in de toekomst worden geaccepteerd.

### <a name="namespace"></a>Naamruimte
Naamruimten zijn een manier voor het categoriseren of vergelijkbare metrische gegevens te groeperen. Naamruimten bieden isolatie tussen groepen van metrische gegevens die kunnen worden verzamelen van verschillende insights of prestatie-indicatoren. Bijvoorbeeld, kan er een naamruimte met de naam *ContosoMemoryMetrics* dat wil zeggen gebruikte bijhouden geheugen gebruik metrische gegevens die het profiel en een andere naamruimte met de naam van uw app *ContosoAppTransaction* die al worden bijgehouden metrische gegevens over gebruikerstransacties in uw toepassing.

### <a name="name"></a>Naam
De naam van de metrische gegevens die wordt gerapporteerd. Meestal is de naam van de beschrijving voor het identificeren van wat er wordt gemeten. Een metrische waarde die is meten van het aantal bytes van het geheugen wordt gebruikt op een bepaalde virtuele machine kan bijvoorbeeld een metrische naam, zoals 'Geheugen Bytes In gebruik' hebben.

### <a name="dimension-keys"></a>Dimensiesleutels
Een dimensie is een sleutel/waarde-paar waarmee u kunt aanvullende kenmerken Geef een beschrijving van de metrische gegevens worden verzameld. De aanvullende kenmerken inschakelen voor het verzamelen van informatie over de metrische gegevens voor meer inzicht te krijgen. De metriek 'Geheugen Bytes In gebruik' kan bijvoorbeeld een dimensie-sleutel met de naam 'Proces', waarmee het aantal bytes van het geheugen elk proces op een virtuele machine is verbruikt wordt vastgelegd. Hiermee kunt u voor het filteren van de metrische gegevens om te zien hoeveel geheugen-specifieke processen worden gebruikt of voor het identificeren van de Top 5-processen door geheugengebruik.
Elke aangepaste metrische gegevens kan maximaal 10 dimensies hebben.

### <a name="dimension-values"></a>Dimensiewaarden
Tijdens het rapporteren van een metrische datapoint, voor elke dimensie-sleutel op de metrische gegevens worden gerapporteerd, is er een overeenkomende dimensiewaarde. Bijvoorbeeld, als u wilt rapporteren van het geheugen dat wordt gebruikt door de ContosoApp op de virtuele machine:

* De naam van de meetwaarde zou worden *geheugen Bytes in gebruik*
* De dimensie-sleutel zou worden *proces*
* De waarde van de dimensie *ContosoApp.exe*

Bij het publiceren van een metrische waarde, kunt u alleen een waarde van één dimensie per dimensie sleutel opgeven. Als u de dezelfde geheugengebruik voor meerdere processen op de virtuele machine hebt verzameld, kunt u meerdere metrische waarden voor deze timestamp kan rapporteren. Elke metrische waarde geeft de waarde van een andere dimensie voor de sleutel van de dimensie proces.

### <a name="metric-values"></a>Metrische waarden
Azure Monitor slaat alle metrische gegevens met granulariteit van één minuut intervallen. We begrijpen dat een metrische waarde mogelijk moeten worden verzameld meerdere keren (ex. CPU-gebruik) of gemeten voor veel afzonderlijke gebeurtenissen (ex.) Meld u aan bij transactie latenties) tijdens een opgegeven minuut. Als u wilt beperken het nummer van ruwe waarden die u moet verzenden en te betalen in Azure Monitor, kunt u lokaal vooraf aggregeren en hoe de waarden:

* Min.: De minimale waarde uit alle voorbeelden/metingen waargenomen in de minuut
* Maximale: De maximale waarde van alle voorbeelden/metingen waargenomen in de minuut
* Som: De som is van de geobserveerde waarden van alle voorbeeld/metingen in de minuut
* Aantal: Het aantal samples/metingen in de minuut

Bijvoorbeeld, als er zijn 4 aanmelden transacties aan uw app tijdens een basis van een minuut en de resulterende gemeten latentie voor elk zijn:

|Transactie 1|Transactie 2|Transactie 3|Transactie 4|
|---|---|---|---|
|7 ms|4 ms|13 ms|16 ms|
|

De resulterende metrische publicatie naar Azure Monitor zou zijn:
* Min.: 4
* Maximale: 16
* Som: 40
* Aantal: 4

Als uw toepassing kan niet worden vooraf aggregeren lokaal en moet elke afzonderlijke voorbeeld of gebeurtenis onmiddellijk na verzameling verzenden, kunt u de onbewerkte meetwaarden kan verzenden.
Bijvoorbeeld, telkens wanneer een transactie aanmelden is opgetreden op uw app dat wilt u een metrische waarde publiceren naar Azure Monitor met alleen een meting. Dus voor een transactie aanmelding dat heeft geduurd 12 ms en metrische gegevens moet publicatie zou zijn:
* Min.: 12
* Maximum aantal: 12
* Som: 12
* Aantal: 1

Dit proces kunt u meerdere waarden voor de dezelfde metric + combinatie van de dimensie tijdens een opgegeven minuut verzenden. Azure Monitor worden vervolgens de onbewerkte waarden verzonden voor een bepaalde minuut en ze samen te voegen.

### <a name="sample-custom-metric-publication"></a>Voorbeeld-publicatie voor aangepaste metrische gegevens
In het volgende voorbeeld maakt u een aangepaste metrische gegevens met de naam 'Geheugen Bytes in gebruik', onder de metrische naamruimte "geheugen profiel' voor een virtuele Machine. De metrische gegevens heeft één dimensie met de naam 'Proces'. Voor de opgegeven timestamp, zijn we dat de metrische waarden voor twee verschillende processen verzendt:

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
> Application Insights, de Windows Azure Diagnostics-extensie en de agent InfluxData Telegraf zijn al geconfigureerd voor metrische waarden voor het juiste regionale eindpunt verzenden en uitvoeren van de bovenstaande eigenschappen in elke uitstoot.
>
>

## <a name="custom-metric-definitions"></a>Aangepaste metrische definities
Er is niet nodig voor het definiëren van een aangepaste metrische gegevens in Azure Monitor vooraf voordat deze wordt verzonden. Omdat elk metrische gegevenspunt gepubliceerd naamruimte, naam en informatie van de dimensie bevat, wordt de eerste keer dat een aangepaste meetwaarde is verzonden naar Azure Monitor een metrische definitie automatisch worden gemaakt. Deze metrische definitie is vervolgens kunnen worden gedetecteerd voor de resources die de metrische gegevens tegen werd verzonden via de metrische definities.

> [!NOTE]
> Azure Monitor biedt geen nog ondersteuning "Units" voor een aangepaste meetwaarde definiëren.

## <a name="using-custom-metrics"></a>Met aangepaste metrische gegevens
Als aangepaste metrische gegevens worden verzonden naar Azure Monitor dat kunt u ze via de Azure-portal bladeren en query's voor uitvoert via Azure Monitor REST API's of waarschuwingen maken op deze, zodat u kunt op de hoogte gesteld wanneer aan bepaalde voorwaarden wordt voldaan.
### <a name="browse-your-custom-metrics-via-the-azure-portal"></a>Uw aangepaste metrische gegevens via de Azure-portal bladeren
1.  Ga naar [Azure Portal](https://portal.azure.com)
2.  Selecteer de Monitor-blade
3.  Klik op de metrische gegevens
4.  Selecteer een resource die u kunt aangepaste metrische gegevens tegen hebben verzonden
5.  Selecteer de naamruimte van de metrische gegevens voor uw aangepaste metrische gegevens
6.  Selecteer de aangepaste metrische gegevens

## <a name="supported-regions"></a>Ondersteunde regio 's
Tijdens de preview-versie is de mogelijkheid voor het publiceren van aangepaste metrische gegevens alleen beschikbaar in een subset van de Azure-regio's. Dit betekent dat metrische gegevens kunnen alleen worden gepubliceerd voor resources in een van de ondersteunde regio's. De volgende tabel bevat de set ondersteunde Azure-regio's voor aangepaste metrische gegevens en het eindpunt van de bijbehorende metrische gegevens voor resources in die regio's moet worden gepubliceerd op.

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
Azure Monitor legt de volgende limieten op aangepaste metrische gegevens.

|Categorie|Limiet|
|---|---|
|Actieve Time Series/abonnementen/regio|50,000|
|Dimensiesleutels per metrisch gegeven|10|
|De lengte van tekenreeks voor metrische naamruimten, metrische namen, dimensiesleutels en dimensiewaarden|256 tekens|
Een reeks actieve tijd wordt gedefinieerd als een unieke combinatie van metrische gegevens, dimensie sleutel, de waarde van de dimensie die metrische waarden die zijn gepubliceerd in de afgelopen 12 uur heeft gehad.

## <a name="next-steps"></a>Volgende stappen
Aangepaste metrische gegevens van andere services gebruiken 
 - [Virtuele machine](metrics-store-custom-guestos-resource-manager-vm.md)
 - [VM-schaalset](metrics-store-custom-guestos-resource-manager-vmss.md)
 - [Virtuele Machine (klassiek)](metrics-store-custom-guestos-classic-vm.md)
 - [Linux-Machine met Telegraf agent](metrics-store-custom-linux-telegraf.md)
 - [REST API](metrics-store-custom-rest-api.md)
 - [Cloudservice (klassiek)](metrics-store-custom-guestos-classic-cloud-service.md)
 