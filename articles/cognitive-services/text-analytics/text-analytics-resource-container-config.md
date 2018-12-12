---
title: Containers configureren
titlesuffix: Text Analytics - Azure Cognitive Services
description: Text Analytics biedt elke container met een gemeenschappelijk framework van de configuratie, zodat u eenvoudig kunt configureren en beheren van instellingen voor opslag, logboekregistratie en Telemetrie en beveiliging voor uw containers.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: diberry
ms.openlocfilehash: 7e993b9ccc57359ac64186765b7b704535eb5a57
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53086671"
---
# <a name="configure-containers"></a>Containers configureren

Text Analytics biedt elke container met een gemeenschappelijk framework van de configuratie, zodat u eenvoudig kunt configureren en beheren van instellingen voor opslag, logboekregistratie en Telemetrie en beveiliging voor uw containers.

## <a name="configuration-settings"></a>Configuratie-instellingen

Configuratie-instellingen in Text Analytics-containers zijn hiërarchisch en alle containers gebruiken een gedeelde hiërarchie, op basis van de volgende structuur op het hoogste niveau:

* [ApiKey](#apikey-configuration-setting)
* [ApplicationInsights](#applicationinsights-configuration-settings)
* [Verificatie](#authentication-configuration-settings)
* [Facturering](#billing-configuration-setting)
* [Gebruiksrechtovereenkomst](#eula-configuration-setting)
* [Fluentd](#fluentd-configuration-settings)
* [Logboekregistratie](#logging-configuration-settings)
* [Hiermee wordt gekoppeld](#mounts-configuration-settings)

U kunt een gebruiken [omgevingsvariabelen](#configuration-settings-as-environment-variables) of [opdrachtregelargumenten](#configuration-settings-as-command-line-arguments) om op te geven van configuratie-instellingen bij het instantiëren van een container van Text Analytics-containers.

Waarden van omgevingsvariabelen overschrijven opdrachtregelargument waarden, die op zijn beurt de standaardwaarden voor de containerinstallatiekopie te overschrijven. Met andere woorden, als u verschillende waarden in een omgevingsvariabele en een opdrachtregelargument voor de dezelfde configuratie-instelling, zoals `Logging:Disk:LogLevel`exemplaar maken van een container, de waarde in de omgevingsvariabele wordt gebruikt door de geïnstantieerde de container.

### <a name="configuration-settings-as-environment-variables"></a>Configuratie-instellingen als omgevingsvariabelen

U kunt de [ASP.NET Core-omgeving variabele syntaxis](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1&tabs=basicconfiguration#configuration-by-environment) om op te geven van configuratie-instellingen.

De container leest omgevingsvariabelen van de gebruiker wanneer de container wordt geïnstantieerd. Als een omgevingsvariabele bestaat, overschrijft de waarde van de omgevingsvariabele de standaardwaarde voor de opgegeven configuratie-instelling. Het voordeel van het gebruik van omgevingsvariabelen is dat meerdere configuratie-instellingen kunnen worden ingesteld voordat het instantiëren van containers en meerdere containers kunnen u dezelfde set configuratie-instellingen automatisch gebruikmaken.

Bijvoorbeeld de volgende opdrachten een omgevingsvariabele configureren van de console logboekregistratieniveau te gebruiken [LogLevel.Information](https://msdn.microsoft.com), waarmee vervolgens wordt een container van de containerinstallatiekopie Sentimentanalyse. De waarde van de omgevingsvariabele overschrijft de standaard configuratie-instelling.

  ```Docker
  SET Logging:Console:LogLevel=Information
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 mcr.microsoft.com/azure-cognitive-services/sentiment Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0 ApiKey=0123456789
  ```

### <a name="configuration-settings-as-command-line-arguments"></a>Configuratie-instellingen als opdrachtregelargumenten

U kunt de [ASP.NET Core opdrachtregelargument syntaxis](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1&tabs=basicconfiguration#arguments) om op te geven van configuratie-instellingen.

U kunt configuratie-instellingen opgeven in de optionele `ARGS` parameter van de [docker uitvoeren](https://docs.docker.com/engine/reference/commandline/run/) opdracht die wordt gebruikt voor het starten van een container van een gedownloade containerinstallatiekopie. Het voordeel van het gebruik van opdrachtregelargumenten is dat elke container kan worden gebruikt voor het gebruik van een andere, aangepaste set met configuratie-instellingen.

Bijvoorbeeld, de volgende opdracht wordt een container van de containerinstallatiekopie Sentimentanalyse en configureert u de console het niveau van logboekregistratie in LogLevel.Information, voorrang op de standaard-configuratie-instelling.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 mcr.microsoft.com/azure-cognitive-services/sentiment Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0 ApiKey=0123456789 Logging:Console:LogLevel=Information
  ```

## <a name="apikey-configuration-setting"></a>ApiKey configuratie-instelling

De `ApiKey` configuratie-instelling geeft de configuratiesleutel van de Text Analytics-resource in Azure gebruikt voor het bijhouden van informatie over facturering voor de container. U moet een waarde voor deze configuratie-instelling opgeven en de waarde moet een geldige configuratie-sleutel voor de Text Analytics-resource die is opgegeven voor de [ `Billing` ](#billing-configuration-setting) configuratie-instelling.

> [!IMPORTANT]
> De [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), en [ `Eula` ](#eula-configuration-setting) configuratie-instellingen samen worden gebruikt en u moet geldige waarden opgeven voor alle drie Deze; anders start uw container niet. Zie voor meer informatie over het gebruik van deze configuratie-instellingen voor het starten van een container [facturering](how-tos/text-analytics-how-to-install-containers.md#billing).

## <a name="applicationinsights-configuration-settings"></a>Application Insights-configuratie-instellingen

De configuratie-instellingen in de `ApplicationInsights` sectie kunt u om toe te voegen [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) ondersteuning voor telemetrie naar de container. Application Insights biedt uitgebreide bewaking van de container op het codeniveau. U kunt de container voor beschikbaarheid, prestaties en gebruik eenvoudig bewaken. U kunt ook snel identificeren en opsporen van fouten in de container zonder te wachten op een gebruiker ze heeft gerapporteerd.

De volgende tabel beschrijft de configuratieinstellingen die worden ondersteund onder de `ApplicationInsights` sectie.

| Name | Gegevenstype | Description |
|------|-----------|-------------|
| `InstrumentationKey` | Reeks | De instrumentatiesleutel van de Application Insights-exemplaar aan welke telemetrie gegevens voor de container is verzonden. Zie voor meer informatie, [Application Insights voor ASP.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). |

## <a name="authentication-configuration-settings"></a>Verificatie-configuratie-instellingen

De `Authentication` configuratie-instellingen bieden opties voor Azure-beveiliging voor de container. Hoewel de configuratie-instellingen in deze sectie beschikbaar voor alle containers in Text Analytics-containers zijn, de manier waarop de configuratiewaarden voor de instelling worden gebruikt, is specifiek voor elke container en containers mogelijk helemaal niet gebruikt in deze sectie.

De volgende tabel beschrijft de configuratieinstellingen die worden ondersteund onder de `Authentication` sectie.

| Name | Gegevenstype | Description |
|------|-----------|-------------|
| `ApiKey` | tekenreeks of matrix | De Azure-abonnementssleutels die door de container worden gebruikt voor toegang tot andere Azure-resources, indien nodig op de container.<br/> Als meer dan één abonnementssleutel wordt gebruikt door de container, is klikt u vervolgens deze waarde opgegeven als een matrix met tekenreeksen; anders wordt wordt een string-waarde gebruikt om op te geven van een enkel abonnement-sleutel die wordt gebruikt door de container. |

## <a name="billing-configuration-setting"></a>Facturering van configuratie-instelling

De `Billing` configuratie-instelling geeft aan welk eindpunt-URI van de Text Analytics-resource in Azure wordt gebruikt om te meten factureringsgegevens voor de container. U moet een waarde voor deze configuratie-instelling opgeven en de waarde moet een geldige URI van het eindpunt voor een Text Analytics-resource in Azure.

> [!IMPORTANT]
> De [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), en [ `Eula` ](#eula-configuration-setting) configuratie-instellingen samen worden gebruikt en u moet geldige waarden opgeven voor alle drie Deze; anders start uw container niet. Zie voor meer informatie over het gebruik van deze configuratie-instellingen voor het starten van een container [facturering](how-tos/text-analytics-how-to-install-containers.md#billing).

## <a name="eula-configuration-setting"></a>Voor de gebruiksrechtovereenkomst voor configuratie-instelling

De `Eula` configuratie-instelling geeft aan dat u de licentie voor de container hebt geaccepteerd. U moet een waarde voor deze configuratie-instelling opgeven en de waarde moet worden ingesteld op `accept`.

> [!IMPORTANT]
> De [ `ApiKey` ](#apikey-configuration-setting), [ `Billing` ](#billing-configuration-setting), en [ `Eula` ](#eula-configuration-setting) configuratie-instellingen samen worden gebruikt en u moet geldige waarden opgeven voor alle drie Deze; anders start uw container niet. Zie voor meer informatie over het gebruik van deze configuratie-instellingen voor het starten van een container [facturering](how-tos/text-analytics-how-to-install-containers.md#billing).

Cognitive Services-containers zijn in licentie gegeven onder [uw overeenkomst](https://go.microsoft.com/fwlink/?linkid=2018657) voor uw gebruik van Azure. Als u een bestaande overeenkomst voor uw gebruik van Azure hebt, gaat u ermee akkoord dat uw overeenkomst voor gebruik van Azure is de [Microsoft Online Subscription overeenkomst](https://go.microsoft.com/fwlink/?linkid=2018755) (waarin de [voorwaarden voor onlineservices ](https://go.microsoft.com/fwlink/?linkid=2018760)). Voor Preview-versies, gaat u ook akkoord met de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://go.microsoft.com/fwlink/?linkid=2018815). Met behulp van de container gaat u akkoord met deze voorwaarden.

## <a name="fluentd-configuration-settings"></a>Fluentd configuratie-instellingen

De `Fluentd` sectie beheert configuratie-instellingen voor [Fluentd](https://www.fluentd.org), een open-source-gegevensverzamelaar voor centrale logboekregistratie. Text Analytics containers bevat een Fluentd-provider voor logboekregistratie waarmee uw container naar het logboek schrijven en, optioneel, metrische gegevens naar een Fluentd-server.

De volgende tabel beschrijft de configuratieinstellingen die worden ondersteund onder de `Fluentd` sectie.

| Name | Gegevenstype | Description |
|------|-----------|-------------|
| `Host` | Reeks | De IP-adres of de DNS-hostnaam van de Fluentd-server. |
| `Port` | Geheel getal | De poort van de Fluentd-server.<br/> De standaardwaarde is 24224. |
| `HeartbeatMs` | Geheel getal | De heartbeat-interval in milliseconden. Als er geen verkeer van de gebeurtenis is verzonden voordat dit interval is verstreken, wordt er een heartbeat naar de Fluentd-server verzonden. De standaardwaarde is 60000 milliseconden (1 minuut). |
| `SendBufferSize` | Geheel getal | De bufferruimte netwerk verzendt in bytes, dat wordt toegewezen voor bewerkingen. De standaardwaarde is 32768 bytes (32 kB). |
| `TlsConnectionEstablishmentTimeoutMs` | Geheel getal | De time-out in milliseconden, een SSL/TLS-verbinding met de Fluentd-server tot stand brengen. De standaardwaarde is 10000 milliseconden (10 seconden).<br/> Als `UseTLS` is ingesteld op false, deze waarde wordt genegeerd. |
| `UseTLS` | Booleaans | Geeft aan of SSL/TLS in de container moet worden gebruikt voor communicatie met de Fluentd-server. De standaardwaarde is false. |

## <a name="logging-configuration-settings"></a>Configuratie-instellingen voor logboekregistratie

De `Logging` configuratie-instellingen beheren van ASP.NET Core logboekregistratie ondersteuning voor de container. U kunt de dezelfde configuratie-instellingen en waarden voor de container die u voor een ASP.NET Core-toepassing kunt gebruiken. De volgende logboekregistratie-providers worden ondersteund door de Text Analytics-containers:

* [Console](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#console-provider)  
  De ASP.NET Core `Console` provider voor logboekregistratie. Alle configuratie-instellingen voor ASP.NET Core en standaardwaarden voor deze provider voor logboekregistratie worden ondersteund.
* [Foutopsporing](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#debug-provider)  
  De ASP.NET Core `Debug` provider voor logboekregistratie. Alle configuratie-instellingen voor ASP.NET Core en standaardwaarden voor deze provider voor logboekregistratie worden ondersteund.
* Schijf  
  De provider van de JSON-logboekregistratie. Deze provider voor logboekregistratie schrijft logboekgegevens naar het koppelpunt van de uitvoer.  
  De `Disk` logboekregistratie-provider ondersteunt de volgende configuratie-instellingen:  

  | Name | Gegevenstype | Description |
  |------|-----------|-------------|
  | `Format` | Reeks | De indeling van de uitvoer voor de logboekbestanden.<br/> **Opmerking:** deze waarde moet worden ingesteld op `json` om in te schakelen van de provider voor logboekregistratie. Als deze waarde is opgegeven zonder ook een koppelpunt uitvoer op te geven bij het instantiëren van een container, wordt er een fout optreedt. |
  | `MaxFileSize` | Geheel getal | De maximale grootte in megabytes (MB) van een logboekbestand. Wanneer de grootte van het huidige logboekbestand voldoet aan of deze waarde overschrijdt, wordt een nieuw logboekbestand wordt gestart door de provider voor logboekregistratie. Als -1 is opgegeven, wordt de grootte van het logboekbestand alleen beperkt door de maximale bestandsgrootte, indien aanwezig, voor het koppelen van de uitvoer. De standaardwaarde is 1. |

Zie voor meer informatie over het configureren van ondersteuning voor ASP.NET Core-logboekregistratie [instellingen bestandsconfiguratie](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#settings-file-configuration).

## <a name="mounts-configuration-settings"></a>Hiermee koppelt u configuratie-instellingen

De Docker-containers die is geleverd door de Text Analytics-containers zijn ontworpen om te worden van staatloze en onveranderbaar. Met andere woorden, worden bestanden die zijn gemaakt binnen een container opgeslagen in een beschrijfbare container-laag, die zich blijft voordoen alleen vast als de container wordt uitgevoerd en gemakkelijk kan niet worden geopend. Als deze container wordt gestopt of verwijderd, worden de bestanden die zijn gemaakt in die container ermee vernietigd.

Echter, omdat ze Docker-containers, u kunt gebruiken opslagopties voor Docker, zoals volumes en binden koppelingen te lezen en schrijven van persistente gegevens buiten de container als de container wordt ondersteund. Zie voor meer informatie over hoe u kunt opgeven en beheren van Docker-opslagopties [beheren van gegevens in Docker](https://docs.docker.com/storage/).

> [!NOTE]
> Normaal gesproken hoeft u de waarden voor deze configuratie-instellingen wijzigen. In plaats daarvan gebruikt u de waarden die zijn opgegeven in deze configuratie-instellingen als doel bij het opgeven van de invoer- en koppelingen voor de container. Zie voor meer informatie over het opgeven van de invoer- en koppelingen [invoer en uitvoer van de koppeling](#input-and-output-mounts).

De volgende tabel beschrijft de configuratieinstellingen die worden ondersteund onder de `Mounts` sectie.

| Name | Gegevenstype | Description |
|------|-----------|-------------|
| `Input` | Reeks | Het doel van de invoer koppelen. De standaardwaarde is `/input`. |
| `Output` | Reeks | Het doel van de uitvoer-koppelpunt. De standaardwaarde is `/output`. |

### <a name="input-and-output-mounts"></a>Invoer- en koppelingen

Standaard elke container kan ondersteunen een *invoer koppelpunt*, die de container kan worden gelezen gegevens, en een *uitvoer koppelpunt*, die de container kan om gegevens te schrijven. Containers zijn niet vereist voor de ondersteuning van invoer of uitvoer van de koppeling, echter en elke container zowel invoer- en koppelingen voor container-specifieke toepassing naast de opties voor logboekregistratie wordt ondersteund door de Text Analytics-containers kunt gebruiken. De volgende tabel een lijst met invoer en uitvoer koppelen van ondersteuning voor elke container in Text Analytics-containers.

| Container | Invoer koppelen | Uitvoer koppelen |
|-----------|-------------|--------------|
|[Sleuteltermextractie](#working-with-key-phrase-extraction) | Niet ondersteund | Optioneel |
|[Taaldetectie](#working-with-language-detection) | Niet ondersteund | Optioneel |
|[Sentimentanalyse](#working-with-sentiment-analysis) | Niet ondersteund | Optioneel |

U kunt opgeven van een koppelpunt invoer of uitvoer koppelen door op te geven de `--mount` optie in de [docker uitvoeren](https://docs.docker.com/engine/reference/commandline/run/) opdracht die wordt gebruikt voor het starten van een container van een gedownloade containerinstallatiekopie. Standaard de gebruikt voor het koppelen van de invoer `/input` als de bestemming en de uitvoer gebruikt koppelt `/output` als bestemming. Een Docker-opslagoptie die beschikbaar zijn voor de Docker-containerhost kan worden opgegeven in de `--mount` optie.

De volgende opdracht wordt bijvoorbeeld gedefinieerd voor een koppelpunt van de Docker-binding aan de `D:\Output` map op de hostcomputer als het koppelpunt uitvoer wordt vervolgens een container van de containerinstallatiekopie Sentimentanalyse, het opslaan van logboekbestanden in JSON-indeling voor het koppelen van de uitvoer.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 --mount type=bind,source=D:\Output,destination=/output mcr.microsoft.com/azure-cognitive-services/sentiment Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/text/analytics/v2.0 ApiKey=0123456789 Logging:Disk:Format=json
  ```
