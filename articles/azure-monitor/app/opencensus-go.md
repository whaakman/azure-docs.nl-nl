---
title: OpenCensus gaat u traceren met Azure Application Insights | Microsoft Docs
description: Biedt instructies voor het integreren van OpenCensus gaat u traceren met de lokale doorstuurserver en Application Insights
services: application-insights
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/15/2018
ms.service: application-insights
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 8f0dc0db3f50d191b558f69252a4557410c30adc
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "54003384"
---
# <a name="collect-distributed-traces-from-go-preview"></a>Gedistribueerde traceringen verzameld vanuit Go (Preview)

Application Insights nu ondersteunt tracering van Go-toepassingen door de integratie met gedistribueerde [OpenCensus](https://opencensus.io) en onze nieuwe [lokale doorstuurserver](./opencensus-local-forwarder.md). In dit artikel begeleidt u stapsgewijs door het proces van het instellen van OpenCensus voor Go en het ophalen van uw traceringsgegevens naar Application Insights.

## <a name="prerequisites"></a>Vereisten

- U hebt een Azure-abonnement nodig.
- Ga naar moet worden geïnstalleerd, in dit artikel wordt de versie 1.11 [gaat downloaden](https://golang.org/dl/).
- Volg de instructies voor het installeren van de [lokale doorstuurserver als een Windows-service](./opencensus-local-forwarder.md#windows-service).

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-application-insights-resource"></a>Application Insights-resource maken

U moet eerst maken van een Application Insights-resource die een instrumentatiesleutel (ikey) wordt gegenereerd. De ikey wordt vervolgens gebruikt voor het configureren van de lokale doorstuurserver voor het verzenden van gedistribueerde traceringen van uw toepassing OpenCensus geïnstrumenteerd met Application Insights.   

1. Selecteer **een resource maken** > **hulpprogramma's voor ontwikkelaars** > **Application Insights**.

   ![Application Insights-resource toevoegen](./media/opencensus-Go/0001-create-resource.png)

   Er wordt een configuratiescherm weergegeven. Gebruik de volgende tabel om de invoervelden in te vullen.

    | Instellingen        | Waarde           | Beschrijving  |
   | ------------- |:-------------|:-----|
   | **Naam**      | Globaal unieke waarde | Naam die de app beschrijft die u wilt controleren |
   | **Toepassingstype** | Algemeen | Type app dat u wilt controleren |
   | **Resourcegroep**     | myResourceGroup      | Naam voor de nieuwe resourcegroep waarin App Insights-gegevens worden gehost |
   | **Locatie** | US - oost | Kies een locatie in uw buurt of in de buurt van waar de app wordt gehost |

2. Klik op **Create**.

## <a name="configure-local-forwarder"></a>Lokale forwarder configureren

1. Selecteer **Overzicht** > **Essentials** > kopieer de **Instrumentatiesleutel** van uw toepassing.

   ![Schermafbeelding van de instrumentatiesleutel](./media/opencensus-Go/0003-instrumentation-key.png)

2. Bewerk uw `LocalForwarder.config` bestand en voeg de instrumentatiesleutel toe. Als u de instructies in de gevolgd de [vereiste](./opencensus-local-forwarder.md#windows-service) het bestand bevindt zich in `C:\LF-WindowsServiceHost`

    ```xml
      <OpenCensusToApplicationInsights>
        <!--
          Instrumentation key to track telemetry to.
          -->
        <InstrumentationKey>{enter-instrumentation-key}</InstrumentationKey>
      </OpenCensusToApplicationInsights>
    
      <!-- Describes aspects of processing Application Insights telemetry-->
      <ApplicationInsights>
        <LiveMetricsStreamInstrumentationKey>{enter-instrumentation-key}</LiveMetricsStreamInstrumentationKey>
      </ApplicationInsights>
    </LocalForwarderConfiguration>
    ```

3. De toepassing opnieuw hebt gestart **lokale doorstuurserver** service.

## <a name="opencensus-go-packages"></a>Ga naar OpenCensus pakketten

1. Installeer de pakketten Open telling voor Go vanaf de opdrachtregel:

    ```go
    go get -u go.opencensus.io
    go get -u contrib.go.opencensus.io/exporter/ocagent
    ```

2. Voeg de volgende code toe aan een .go-bestand en vervolgens bouwen en uitvoeren. (In dit voorbeeld is afgeleid van de officiële OpenCensus richtlijnen met toegevoegde code die het mogelijk de integratie met de lokale doorstuurserver maakt)

     ```go
        // Copyright 2018, OpenCensus Authors
        //
        // Licensed under the Apache License, Version 2.0 (the "License");
        // you may not use this file except in compliance with the License.
        // You may obtain a copy of the License at
        //
        //     https://www.apache.org/licenses/LICENSE-2.0
        //
        // Unless required by applicable law or agreed to in writing, software
        // distributed under the License is distributed on an "AS IS" BASIS,
        // WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
        // See the License for the specific language governing permissions and
        // limitations under the License.
        package main
        
        import (
        
            "bytes"
            "fmt"
            "log"
            "net/http"
            os "os"
            
            ocagent "contrib.go.opencensus.io/exporter/ocagent"
            "go.opencensus.io/plugin/ochttp"
            "go.opencensus.io/plugin/ochttp/propagation/tracecontext"
            "go.opencensus.io/trace"
        
        )
        
        func main() {
            // Register stats and trace exporters to export the collected data.
            serviceName := os.Getenv("SERVICE_NAME")
            if len(serviceName) == 0 {
                serviceName = "go-app"
            }
            fmt.Printf(serviceName)
            agentEndpoint := os.Getenv("OCAGENT_TRACE_EXPORTER_ENDPOINT")

            if len(agentEndpoint) == 0 {
                agentEndpoint = fmt.Sprintf("%s:%d", ocagent.DefaultAgentHost, ocagent.DefaultAgentPort)
            }
        
            fmt.Printf(agentEndpoint)
            exporter, err := ocagent.NewExporter(ocagent.WithInsecure(), ocagent.WithServiceName(serviceName), ocagent.WithAddress(agentEndpoint))
        
            if err != nil {
                log.Printf("Failed to create the agent exporter: %v", err)
            }
        
            trace.RegisterExporter(exporter)
        
            trace.ApplyConfig(trace.Config{DefaultSampler: trace.AlwaysSample()})
        
            client := &http.Client{Transport: &ochttp.Transport{Propagation: &tracecontext.HTTPFormat{}}}
        
            http.HandleFunc("/", func(w http.ResponseWriter, req *http.Request) {
                fmt.Fprintf(w, "hello world")
        
                var jsonStr = []byte(`[ { "url": "http://blank.org", "arguments": [] } ]`)
                r, _ := http.NewRequest("POST", "http://blank.org", bytes.NewBuffer(jsonStr))
                r.Header.Set("Content-Type", "application/json")
        
                // Propagate the trace header info in the outgoing requests.
                r = r.WithContext(req.Context())
                resp, err := client.Do(r)
                if err != nil {
                    log.Println(err)
                } else {
                    // TODO: handle response
                    resp.Body.Close()
                }
            })
        
            http.HandleFunc("/call_blank", func(w http.ResponseWriter, req *http.Request) {
                fmt.Fprintf(w, "hello world")
        
                r, _ := http.NewRequest("GET", "http://blank.org", nil)

                // Propagate the trace header info in the outgoing requests.
                r = r.WithContext(req.Context())
                resp, err := client.Do(r)
        
                if err != nil {
                    log.Println(err)
                } else {
                    // TODO: handle response
                    resp.Body.Close()
                }        
            })
        
            log.Fatal(http.ListenAndServe(":50030", &ochttp.Handler{Propagation: &tracecontext.HTTPFormat{}}))
        
        }
     ```

3. Wanneer de eenvoudige go-app wordt uitgevoerd navigeert u naar `http://localhost:50030`. Elke vernieuwing van de browser wordt de tekst "Hallo wereld" vergezeld gaan van overeenkomende reeks gegevens die wordt voortgezet op de lokale doorstuurserver gegenereerd.

4. Om te bevestigen dat de **lokale doorstuurserver** ophaalt van de controle traceringen het `LocalForwarder.config` bestand. Als u de stappen in de [vereiste](https://docs.microsoft.com/azure/application-insights/local-forwarder#windows-service), deze bevindt zich `C:\LF-WindowsServiceHost`.

    In onderstaande afbeelding van het logboekbestand, kunt u zien dat vóór het uitvoeren van het tweede script waar we een uitvoerder toegevoegd `OpenCensus input BatchesReceived` is 0. Zodra we begonnen met het bijgewerkte script is uitgevoerd `BatchesReceived` verhoogd gelijk is aan het aantal waarden dat we ingevoerd:
    
    ![Nieuw App Insights-resourceformulier](./media/opencensus-go/0004-batches-received.png)

## <a name="start-monitoring-in-the-azure-portal"></a>Beginnen met controleren in Azure Portal

1. U kunt nu opnieuw openen met de Application Insights **overzicht** pagina in de Azure-portal om weer te geven details over de toepassing die momenteel wordt uitgevoerd. Selecteer **metrische Stream Live**.

   ![Schermopname van het overzichtsvenster met live, metrische gegevensstroom geselecteerd in een rood kader](./media/opencensus-go/0005-overview-live-metrics-stream.png)

2. Als u de tweede Go-app opnieuw uitvoeren en start de browser vernieuwen `http://localhost:50030`, ziet u live traceringsgegevens wanneer ze in Application Insights worden ontvangen van de lokale-doorstuurserver.

   ![Schermafbeelding van live, metrische gegevensstroom met prestatiegegevens weergegeven](./media/opencensus-go/0006-stream.png)

3. Ga terug naar de **overzicht** pagina en selecteer **Toepassingsoverzicht** voor een visueel overzicht van de afhankelijkheidsrelaties tussen en de timing van de oproep tussen onderdelen van uw toepassing.

    ![Schermafbeelding van de basistoepassing toewijzing](./media/opencensus-go/0007-application-map.png)

    Omdat we alleen een methodeaanroep traceren zijn, is onze overzicht van de toepassing niet zo interessant. Maar overzicht van de toepassing kunt schalen om te visualiseren veel gedistribueerde toepassingen:

   ![Toepassingskaart](media/opencensus-go/application-map.png)

4. Selecteer **prestaties onderzoeken** gedetailleerde prestatieanalyse uitvoeren en de hoofdoorzaak van trage prestaties vast.

    ![Schermafbeelding van deelvenster prestaties](./media/opencensus-go/0008-performance.png)

5. Selecteren **voorbeelden** en vervolgens te klikken op een van de voorbeelden die worden weergegeven in het rechter deelvenster op de ervaring van de details van end-to-end-transactie wordt gestart. Hoewel onze voorbeeld-app ons slechts één gebeurtenis leert, zou een complexere toepassing kunt u de end-to-end-transactie op niveau van een afzonderlijke gebeurtenis aanroepstack verkennen.

     ![Schermafbeelding van end-to-end-transactie-interface](./media/opencensus-go/0009-end-to-end-transaction.png)

## <a name="opencensus-trace-for-go"></a>OpenCensus tracering voor Go

We alleen behandeld de basisbeginselen van het integreren van OpenCensus voor Go met de lokale doorstuurserver en Application Insights. De [officiële OpenCensus Go gebruiksrichtlijnen](https://godoc.org/go.opencensus.io) omvat meer geavanceerde onderwerpen.

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van de toepassing](./../../azure-monitor/app/app-map.md)
* [End-to-end-prestaties controleren](./../../application-insights/app-insights-tutorial-performance.md)
