---
title: Containers configureren
titlesuffix: Text Analytics - Azure Cognitive Services
description: Text Analytics biedt elke container met een gemeenschappelijk framework van de configuratie, zodat u eenvoudig kunt configureren en beheren van instellingen voor opslag, logboekregistratie en Telemetrie en beveiliging voor uw containers.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/20/2019
ms.author: dapine
ms.openlocfilehash: 2a9b9ce5109315d940d6dcadf395489612faddec
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68356931"
---
# <a name="configure-text-analytics-docker-containers"></a>Text Analytics docker-containers configureren

Text Analytics biedt elke container met een gemeenschappelijk framework van de configuratie, zodat u eenvoudig kunt configureren en beheren van instellingen voor opslag, logboekregistratie en Telemetrie en beveiliging voor uw containers.

## <a name="configuration-settings"></a>Configuratie-instellingen

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> De [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), en [ `Eula` ](#eula-setting) instellingen samen worden gebruikt en u moet geldige waarden opgeven voor alle drie deze, anders uw container start niet. Zie voor meer informatie over het gebruik van deze configuratie-instellingen voor het starten van een container [facturering](how-tos/text-analytics-how-to-install-containers.md#billing).

## <a name="apikey-configuration-setting"></a>ApiKey configuratie-instelling

De `ApiKey` instelling geeft u aan de Azure-resource-sleutel die wordt gebruikt voor het bijhouden van informatie over facturering voor de container. U moet een waarde opgeven voor de ApiKey en de waarde moet een geldige sleutel zijn voor de _Text Analytics_ bron die is opgegeven [`Billing`](#billing-configuration-setting) voor de configuratie-instelling.

Deze instelling bevindt zich op de volgende locatie:

* Azure Portal: **Text Analytics** resource beheer onder **sleutels**

## <a name="applicationinsights-setting"></a>Application Insights-instelling

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Facturering van configuratie-instelling

Met `Billing` deze instelling geeft u de eindpunt-URI op van de _Text Analytics_ resource op Azure die wordt gebruikt om de facturerings gegevens voor de container te meten. U moet een waarde opgeven voor deze configuratie-instelling en de waarde moet een geldige eindpunt-URI zijn voor een __Text Analytics_ resource in Azure. De container rapporteert het gebruik ongeveer elke 10 tot 15 minuten.

Deze instelling bevindt zich op de volgende locatie:

* Azure Portal: **Text Analytics** Overzicht, label`Endpoint`

|Verplicht| Name | Gegevenstype | Description |
|--|------|-----------|-------------|
|Ja| `Billing` | Tekenreeks | De vereiste URL voor het facturerings eindpunt |

## <a name="eula-setting"></a>Overeenkomst instelling

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Fluentd-instellingen

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>Instellingen voor http-proxy referenties

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Instellingen voor logboekregistratie
 
[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]

## <a name="mount-settings"></a>Instellingen voor koppelen

Gebruik bind koppelt om te lezen en schrijven van gegevens naar en van de container. U kunt opgeven van een koppelpunt invoer of uitvoer koppelen door op te geven de `--mount` optie in de [docker uitvoeren](https://docs.docker.com/engine/reference/commandline/run/) opdracht.

De Text Analytics-containers gebruiken geen invoer-of uitvoer koppelingen om training of service gegevens op te slaan. 

De exacte syntaxis van de locatie van de host koppelen, is afhankelijk van het hostbesturingssysteem. Bovendien de [hostcomputer](how-tos/text-analytics-how-to-install-containers.md#the-host-computer)van koppelpunten locatie is mogelijk niet toegankelijk is vanwege een conflict tussen de machtigingen die wordt gebruikt door de docker-service-account en de host koppelen locatie machtigingen. 

|Optioneel| Name | Gegevenstype | Description |
|-------|------|-----------|-------------|
|Niet toegestaan| `Input` | Tekenreeks | Text Analytics containers gebruiken deze niet.|
|Optioneel| `Output` | Tekenreeks | Het doel van de uitvoer-koppelpunt. De standaardwaarde is `/output`. Dit is de locatie van de logboeken. Dit omvat container Logboeken. <br><br>Voorbeeld:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Voorbeeld van de docker-opdrachten uitvoeren 

De volgende voorbeelden gebruiken de configuratie-instellingen om te laten zien hoe u om te schrijven en gebruik `docker run` opdrachten.  Zodra actief is, de container blijft actief totdat u [stoppen](how-tos/text-analytics-how-to-install-containers.md#stop-the-container) deze.

* **Regel voortzettings teken**: De docker-opdrachten in de volgende secties gebruiken de back slash `\`,, als een regel voortzetting teken. Vervang of verwijder deze op basis van het hostbesturingssysteem vereisten. 
* **Argument volgorde**: Wijzig de volgorde van de argumenten niet, tenzij u bekend bent met docker-containers.

Vervang {_argument_name_} door uw eigen waarden:

| Tijdelijke aanduiding | Waarde | Indeling of voorbeeld |
|-------------|-------|---|
|{API_KEY} | De eindpunt sleutel van de `Text Analytics` resource die beschikbaar is op `Text Analytics` de pagina Azure Keys. |`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`|
|{ENDPOINT_URI} | De waarde van het facturerings eindpunt is beschikbaar `Text Analytics` op de pagina overzicht van Azure.|`https://westus.api.cognitive.microsoft.com/text/analytics/v2.1`|

> [!IMPORTANT]
> De `Eula`, `Billing`, en `ApiKey` opties moeten worden opgegeven voor het uitvoeren van de container; anders wordt de container niet start.  Zie voor meer informatie, [facturering](how-tos/text-analytics-how-to-install-containers.md#billing).
> De ApiKey-waarde is de **sleutel** van de `Text Analytics` pagina Azure-resource sleutels. 

## <a name="key-phrase-extraction-container-docker-examples"></a>Voor beelden van extractie van sleutel woordgroepen container

De volgende docker-voor beelden zijn voor de sleutel woord extractie container. 

### <a name="basic-example"></a>Eenvoudige voorbeeld 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 mcr.microsoft.com/azure-cognitive-services/keyphrase Eula=accept Billing={ENDPOINT_URI} ApiKey={API_KEY} 
  ```

### <a name="logging-example"></a>Voor beeld van logboek registratie 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 mcr.microsoft.com/azure-cognitive-services/keyphrase Eula=accept Billing={ENDPOINT_URI} ApiKey={API_KEY} Logging:Console:LogLevel:Default=Information
  ```

## <a name="language-detection-container-docker-examples"></a>Voor beelden van de docker voor taal detectie container

De volgende docker-voor beelden zijn voor de taal detectie container. 

### <a name="basic-example"></a>Eenvoudige voorbeeld

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 mcr.microsoft.com/azure-cognitive-services/language Eula=accept Billing={ENDPOINT_URI} ApiKey={API_KEY} 
  ```

### <a name="logging-example"></a>Voor beeld van logboek registratie

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 mcr.microsoft.com/azure-cognitive-services/language Eula=accept Billing={ENDPOINT_URI} ApiKey={API_KEY} Logging:Console:LogLevel:Default=Information
  ```
 
## <a name="sentiment-analysis-container-docker-examples"></a>Voor beelden van sentiment Analysis container docker

De volgende docker-voor beelden zijn voor de sentiment Analysis-container. 

### <a name="basic-example"></a>Eenvoudige voorbeeld

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 mcr.microsoft.com/azure-cognitive-services/sentiment Eula=accept Billing={ENDPOINT_URI} ApiKey={API_KEY} 
  ```

### <a name="logging-example"></a>Voor beeld van logboek registratie

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 mcr.microsoft.com/azure-cognitive-services/sentiment Eula=accept Billing={ENDPOINT_URI} ApiKey={API_KEY} Logging:Console:LogLevel:Default=Information
  ```

## <a name="next-steps"></a>Volgende stappen

* Beoordeling [over het installeren en uitvoeren van containers](how-tos/text-analytics-how-to-install-containers.md)
* Meer [Cognitive Services containers](../cognitive-services-container-support.md) gebruiken
