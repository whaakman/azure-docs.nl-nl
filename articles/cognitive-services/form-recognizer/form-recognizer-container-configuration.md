---
title: Container - formulier herkenning configureren
titleSuffix: Azure Cognitive Services
description: Informatie over het configureren van de container herkenning van formulier voor het parseren van formulier-en tabelgegevens.
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: overview
ms.date: 06/10/2019
ms.author: dapine
ms.openlocfilehash: bdff74be8578bb862974479b3151b0d922f00dd9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67063991"
---
# <a name="configure-form-recognizer-containers"></a>Formulier herkenning containers configureren

U kunt de toepassingsarchitectuur van een die geoptimaliseerd om te profiteren van zowel mogelijkheden van de robuuste cloud en edge plaats bouwen met behulp van Azure formulier herkenning containers.

U het formulier herkenning container runtime-omgeving configureren met behulp van de `docker run` opdracht argumenten. Deze container heeft diverse vereist en een aantal optionele instellingen. Zie voor een paar voorbeelden van de ["Voorbeeld van de docker-opdrachten uitvoeren"](#example-docker-run-commands) sectie. De container-specifieke instellingen zijn de instellingen voor facturering.

## <a name="configuration-settings"></a>Configuratie-instellingen

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> De [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), en [ `Eula` ](#eula-setting) instellingen samen worden gebruikt. U moet geldige waarden opgeven voor alle drie instellingen. anders start uw container niet. Zie voor meer informatie over het gebruik van deze configuratie-instellingen voor het starten van een container [facturering](form-recognizer-container-howto.md#billing).

## <a name="apikey-configuration-setting"></a>ApiKey configuratie-instelling

De `ApiKey` instelling geeft u aan de Azure-resource-sleutel die wordt gebruikt voor het bijhouden van informatie over facturering voor de container. De waarde voor de ApiKey moet een geldige sleutel voor de _formulier herkenning_ resource die opgegeven voor `Billing` in de sectie 'Configuratie-instelling facturering'.

U vindt deze instelling in de Azure-portal in **formulier herkenning Resource Management**onder **sleutels**.

## <a name="applicationinsights-setting"></a>Application Insights-instelling

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Facturering van configuratie-instelling

De `Billing` instelling geeft u aan de URI van het eindpunt van de _formulier herkenning_ resource in Azure die wordt gebruikt voor het meten van factureringsgegevens voor de container. De waarde voor deze configuratie-instelling moet een geldige URI van het eindpunt voor een _formulier herkenning_ resource in Azure. Gebruik de container rapporteert over elke 10 tot 15 minuten.

U vindt deze instelling in de Azure-portal in **formulier herkenning overzicht**onder **eindpunt**.

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

Gebruik bind koppelt om te lezen en schrijven van gegevens naar en van de container. U kunt een invoer koppelpunt of een koppelpunt uitvoer opgeven door op te geven de `--mount` optie in de [ `docker run` opdracht](https://docs.docker.com/engine/reference/commandline/run/).

De container formulier herkenning vereist een invoer koppelen en een uitvoer-koppelpunt. De invoer koppeling kan alleen-lezen en het is vereist voor toegang tot de gegevens die worden gebruikt voor training en scoren. Het koppelen van de uitvoer moet beschrijfbaar, en u deze gebruiken voor het opslaan van de modellen en tijdelijke gegevens.

De exacte syntaxis van de locatie van de host koppelen, is afhankelijk van het hostbesturingssysteem. Bovendien de mount-locatie van de [hostcomputer](form-recognizer-container-howto.md#the-host-computer) mogelijk niet toegankelijk is vanwege een conflict tussen de machtigingen van de account van de Docker-service en de host koppelen locatie machtigingen.

|Optioneel| Name | Gegevenstype | Description |
|-------|------|-----------|-------------|
|Vereist| `Input` | String | Het doel van de invoer koppelen. De standaardwaarde is `/input`.    <br><br>Voorbeeld:<br>`--mount type=bind,src=c:\input,target=/input`|
|Vereist| `Output` | String | Het doel van de uitvoer-koppelpunt. De standaardwaarde is `/output`.  <br><br>Voorbeeld:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Voorbeeld van de docker-opdrachten uitvoeren

De volgende voorbeelden gebruiken de configuratie-instellingen om te laten zien hoe u om te schrijven en gebruik `docker run` opdrachten. Wanneer deze wordt uitgevoerd, de container blijft actief totdat u [voorkomen dat deze](form-recognizer-container-howto.md#stop-the-container).

* **Voortzetting van regel tekens**: De Docker-opdrachten in de volgende secties gebruiken een backslash (\\) als een voortzetting van regel tekens. Vervang of verwijder dit teken, afhankelijk van de vereisten voor de host-besturingssysteem.
* **Argument order**: De volgorde van de argumenten niet te wijzigen, tenzij u bekend met Docker-containers bent.

Vervang {_argument_name_} in de volgende tabel met uw eigen waarden:

| Tijdelijke aanduiding | Value |
|-------------|-------|
|{BILLING_KEY} | De sleutel die wordt gebruikt voor het starten van de container. Het is beschikbaar in Azure portal op de pagina sleutels van formulier-herkenning.  |
|{BILLING_ENDPOINT_URI} | Het eindpunt van de facturering URI-waarde is beschikbaar op de Azure portal de overzichtspagina van formulier herkenning.|
|{COMPUTER_VISION_API_KEY}| De sleutel is beschikbaar op de Azure portal-pagina van de Computer Vision-API-sleutels.|
|{COMPUTER_VISION_ENDPOINT_URI}|De facturering-eindpunt. Als u een cloud-gebaseerde Computer Vision-resource, is de URI-waarde is beschikbaar in de Azure portal-pagina van de Computer Vision-API-overzicht. Als u een *cognitive services-herkennen-tekst* container, gebruikt u de facturering eindpunt-URL die wordt doorgegeven aan de container in de `docker run` opdracht.|

> [!IMPORTANT]
> Als u wilt uitvoeren van de container, geef de `Eula`, `Billing`, en `ApiKey` opties; anders wordt de container niet start. Zie voor meer informatie, [facturering](#billing-configuration-setting).

> [!NOTE] 
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

* Beoordeling [installeren en uitvoeren van containers](form-recognizer-container-howto.md).
