---
title: Container - formulier herkenning configureren
titleSuffix: Azure Cognitive Services
description: Informatie over het configureren van de container herkenning van formulier voor het parseren van formulier-en tabelgegevens.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: overview
ms.date: 05/31/2019
ms.author: pafarley
ms.openlocfilehash: 28acc2d1eafacb9e53fac3e3cce092738401f838
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66475383"
---
# <a name="configure-form-recognizer-containers"></a>Formulier herkenning containers configureren

Formulier herkenning containers bieden klanten kunnen maken van de toepassingsarchitectuur van een die is geoptimaliseerd om te profiteren van zowel mogelijkheden van de robuuste cloud en edge plaats.

De **formulier herkenning** container runtime-omgeving is geconfigureerd met behulp van de `docker run` opdracht argumenten. Deze container heeft meerdere vereiste instellingen, samen met een aantal optionele instellingen. Verschillende [voorbeelden](#example-docker-run-commands) van de opdracht beschikbaar zijn. De container-specifieke instellingen zijn de instellingen voor facturering.

## <a name="configuration-settings"></a>Configuratie-instellingen

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> De [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), en [ `Eula` ](#eula-setting) instellingen samen worden gebruikt en u moet geldige waarden opgeven voor alle drie deze, anders uw container start niet. Zie voor meer informatie over het gebruik van deze configuratie-instellingen voor het starten van een container [facturering](form-recognizer-container-howto.md#billing).

## <a name="apikey-configuration-setting"></a>ApiKey configuratie-instelling

De `ApiKey` instelling geeft u aan de Azure-resource-sleutel die wordt gebruikt voor het bijhouden van informatie over facturering voor de container. U moet een waarde opgeven voor de ApiKey en de waarde moet een geldige sleutel voor de _formulier herkenning_ resource die is opgegeven voor de [ `Billing` ](#billing-configuration-setting) configuratie-instelling.

Deze instelling kan worden gevonden in de volgende plaats:

* Azure Portal: **Herkenning van formulier** resourcebeheer onder **sleutels**

## <a name="applicationinsights-setting"></a>Application Insights-instelling

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Facturering van configuratie-instelling

De `Billing` instelling geeft u aan de URI van het eindpunt van de _formulier herkenning_ resource in Azure gebruikt voor het meten van factureringsgegevens voor de container. U moet een waarde voor deze configuratie-instelling opgeven en de waarde moet een geldige URI van het eindpunt voor een _formulier herkenning_ resource in Azure. Gebruik de container rapporteert over elke 10 tot 15 minuten.

Deze instelling kan worden gevonden in de volgende plaats:

* Azure Portal: **Herkenning van formulier** overzicht, met het label `Endpoint`

|Vereist| Name | Gegevenstype | Description |
|--|------|-----------|-------------|
|Ja| `Billing` | Reeks | URI van de facturering-eindpunt<br><br>Voorbeeld:<br>`Billing=https://westus2.api.cognitive.microsoft.com/` |

## <a name="eula-setting"></a>Overeenkomst instelling

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Fluentd-instellingen

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>HTTP-proxy-instellingen voor referenties

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Instellingen voor logboekregistratie

[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]


## <a name="mount-settings"></a>Instellingen voor koppelen

Gebruik bind koppelt om te lezen en schrijven van gegevens naar en van de container. U kunt opgeven van een koppelpunt invoer of uitvoer koppelen door op te geven de `--mount` optie in de [docker uitvoeren](https://docs.docker.com/engine/reference/commandline/run/) opdracht.

De container formulier herkenning vereist een koppelpunt invoer en uitvoer. De invoer koppeling kan alleen-lezen en is vereist voor toegang tot de gegevens die worden gebruikt voor training en scoren. De uitvoer-koppeling moet beschrijfbaar en wordt gebruikt voor het opslaan van de modellen en tijdelijke gegevens.

De exacte syntaxis van de locatie van de host koppelen, is afhankelijk van het hostbesturingssysteem. Bovendien de [hostcomputer](form-recognizer-container-howto.md#the-host-computer)van koppelpunten locatie is mogelijk niet toegankelijk is vanwege een conflict tussen de machtigingen die wordt gebruikt door de Docker-service-account en de host koppelen locatie machtigingen.

|Optioneel| Name | Gegevenstype | Description |
|-------|------|-----------|-------------|
|Vereist| `Input` | String | Het doel van de invoer koppelen. De standaardwaarde is `/input`.    <br><br>Voorbeeld:<br>`--mount type=bind,src=c:\input,target=/input`|
|Vereist| `Output` | String | Het doel van de uitvoer-koppelpunt. De standaardwaarde is `/output`.  <br><br>Voorbeeld:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Voorbeeld van de docker-opdrachten uitvoeren

De volgende voorbeelden gebruiken de configuratie-instellingen om te laten zien hoe u om te schrijven en gebruik `docker run` opdrachten.  Zodra actief is, de container blijft actief totdat u [stoppen](form-recognizer-container-howto.md#stop-the-container) deze.

* **Voortzetting van regel tekens**: De Docker-opdrachten in de volgende secties gebruiken de backslash `\`, als een voortzetting van regel tekens. Vervang of verwijder deze op basis van het hostbesturingssysteem vereisten.
* **Argument order**: Wijzig de volgorde van de argumenten niet, tenzij u bekend bent met Docker-containers.

Vervang {_argument_name_} door uw eigen waarden:

| Tijdelijke aanduiding | Value |
|-------------|-------|
|{BILLING_KEY} | Deze sleutel wordt gebruikt voor het starten van de container en is beschikbaar op de pagina van de Azure portal-formulier herkenning sleutels.  |
|{BILLING_ENDPOINT_URI} | Het eindpunt van de facturering URI-waarde is beschikbaar op de pagina van de vorm herkenning overzicht van de Azure portal.|
|{COMPUTER_VISION_API_KEY}| De sleutel is beschikbaar op de Computer Vision-API-sleutels-pagina van de Azure portal.|
|{COMPUTER_VISION_ENDPOINT_URI}|De facturering-eindpunt. Als u van een cloud-gebaseerde Computer Vision-resource gebruikmaakt, is de URI-waarde is beschikbaar op de Computer Vision-API-overzicht-pagina van de Azure portal. Als u een `cognitive-services-recognize-text` container, gebruiken de facturering eindpunt-URL die is doorgegeven aan de container in de `docker run` opdracht.|

> [!IMPORTANT]
> De `Eula`, `Billing`, en `ApiKey` opties moeten worden opgegeven voor het uitvoeren van de container; anders wordt de container niet start.  Zie voor meer informatie, [facturering](#billing-configuration-setting).
> De waarde ApiKey is de **sleutel** op de pagina Azure formulier herkenning Resource sleutels.

## <a name="form-recognizer-container-docker-examples"></a>Formulier herkenning container Docker-voorbeelden

De volgende Docker-voorbeelden zijn voor de herkenning van formulier-container.

### <a name="basic-example-for-form-recognizer"></a>Eenvoudige voorbeeld voor herkenning van formulier

```Docker
docker run --rm -it -p 5000:5000 --memory 8g --cpus 2 \
--mount type=bind,source=c:\input,target=/input  \
--mount type=bind,source=c:\output,target=/output \
containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY} \
FormRecognizer:ComputerVisionApiKey={COMPUTER_VISION_API_KEY} \
FormRecognizer:ComputerVisionEndpointUri={COMPUTER_VISION_ENDPOINT_URI}
```

### <a name="logging-example-for-form-recognizer"></a>Voorbeeld van de logboekregistratie voor herkenning van formulier

```Docker
docker run --rm -it -p 5000:5000 --memory 8g --cpus 2 \
--mount type=bind,source=c:\input,target=/input  \
--mount type=bind,source=c:\output,target=/output \
containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY} \
FormRecognizer:ComputerVisionApiKey={COMPUTER_VISION_API_KEY} \
FormRecognizer:ComputerVisionEndpointUri={COMPUTER_VISION_ENDPOINT_URI}
Logging:Console:LogLevel:Default=Information
```


## <a name="next-steps"></a>Volgende stappen

* Beoordeling [over het installeren en uitvoeren van containers](form-recognizer-container-howto.md)
