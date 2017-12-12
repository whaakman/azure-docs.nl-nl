Er gelden enkele beperkingen voor het aantal meetgegevens en gebeurtenissen per toepassing (per instrumentatiesleutel). De limieten zijn afhankelijk van de [prijscategorie](https://azure.microsoft.com/pricing/details/application-insights/) die u kiest.

| **Resource** | **Standaardlimiet** | **Opmerking**
| --- | --- | --- |
| Totale hoeveelheid gegevens per dag | 100 GB | U kunt gegevens beperken door een maximum in te stellen. Als u meer nodig hebt, kunt u de limiet verhogen tot 1000 GB vanuit de portal. Voor meer dan 1000 GB capaciteit verzend een e-mail naar AIDataCap@microsoft.com.
| Gratis gegevens per maand<br/> (prijscategorie Basic) | 1 GB | Aanvullende gegevens worden per gigabyte in rekening gebracht.
| Beperking | 32.000 gebeurtenissen per seconde | De limiet wordt gemeten in een minuut.
| Bewaartijd van gegevens | 90 dagen | Deze resource is voor [Search](../articles/application-insights/app-insights-diagnostic-search.md), [Analytics](../articles/application-insights/app-insights-analytics.md) en [Metrics Explorer](../articles/application-insights/app-insights-metrics-explorer.md).
| Bewaartijd van gedetailleerde resultaten van [beschikbaarheidstests met meerdere stappen](../articles/application-insights/app-insights-monitor-web-app-availability.md#multi-step-web-tests) | 90 dagen | Deze resource biedt gedetailleerde resultaten van elke stap.
| Maximale gebeurtenisgrootte | 64 KB | 
| Naamlengte voor de eigenschappen en meetgegevens | 150 | Zie [typt u schema's](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/)
| Lengte van de tekenreeks eigenschapswaarde | 8.192 | Zie [typt u schema's](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/)
| Lengte van berichten voor tracering en uitzonderingen | 10.000 | Zie [typt u schema's](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/)
| Aantal [beschikbaarheidstests](../articles/application-insights/app-insights-monitor-web-app-availability.md) per app  | 100 |
| [Profiler](../articles/application-insights/app-insights-profiler.md) bewaren van gegevens | Er zijn vijf dagen |
| [Profiler](../articles/application-insights/app-insights-profiler.md) gegevens verzonden per dag | 10GB |

Zie [Over prijzen en quota voor Application Insights](../articles/application-insights/app-insights-pricing.md) voor meer informatie.

