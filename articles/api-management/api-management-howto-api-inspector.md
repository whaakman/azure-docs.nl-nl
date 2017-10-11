---
title: Trace-aanroepen met API-Inspector - Azure API Management | Microsoft Docs
description: Informatie over het traceren van aanroepen met behulp van de API-Inspector in Azure API Management.
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 4b222327-c8a4-4f33-9a06-adff2a9834d9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: a9d4d3be7f046af975f6dc25670070204848588c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-api-inspector-to-trace-calls-in-azure-api-management"></a>Het gebruik van de API-Inspector te traceren aanroepen in Azure API Management
API Management biedt een API-Inspector-hulpprogramma om u te helpen bij het opsporen en oplossen van uw API's. De API-Inspector via een programma kan worden gebruikt en kan ook rechtstreeks vanuit de portal voor ontwikkelaars worden gebruikt. 

Naast de tracering bewerkingen, API-Inspector ook traceert [beleidsexpressie](https://msdn.microsoft.com/library/azure/dn910913.aspx) evaluaties. Zie voor een demonstratie [Cloud hebben betrekking op aflevering 177: meer API Management-functies](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) en vooruit tot 21:00.

Deze handleiding biedt een overzicht van het gebruik van API-Inspector.

> [!NOTE]
> API-Inspector traceringen worden alleen gegenereerd en beschikbaar gesteld voor aanvragen met abonnement sleutels die deel uitmaken van de [beheerder](api-management-howto-create-groups.md) account.
> 
> 

## <a name="trace-call"></a> API-Inspector gebruikt om te traceren, een aanroep
Voor het gebruik van API-Inspector toevoegen een **ocp-apim-trace: true** aanvraagheader voor uw bewerking gesprek, en vervolgens downloaden en controleren van de tracering via de URL die is aangegeven door de **ocp-apim-trace-locatie** antwoordheader. Dit via programmacode kan worden uitgevoerd en is ook mogelijk rechtstreeks vanuit de portal voor ontwikkelaars.

Deze zelfstudie laat zien hoe u de API-Inspector trace-bewerkingen met behulp van de basisrekenmachine-API die is geconfigureerd in de [uw eerste API beheren](api-management-get-started.md) zelfstudie aan de slag. Als u dit nog niet hebt voltooid die zelfstudie duurt slechts enkele ogenblikken voor het importeren van de basisrekenmachine-API of kunt u een andere API keuze op, zoals de Echo-API. Elk service-exemplaar van API Management wordt al geconfigureerd geleverd met een Echo-API die kan worden gebruikt om te experimenteren met API Management en hier meer over te leren. De Echo-API retourneert terug ongeacht invoer is verzonden. Als u wilt gebruiken, kunt u een HTTP-term aanroept en de retourwaarde worden gewoon wat u hebt verzonden. 

Om te beginnen, klikt u op **ontwikkelaarsportal** in de Azure-Portal voor uw API Management-service. Bewerkingen kunnen rechtstreeks vanuit de portal voor ontwikkelaars die biedt een handige manier om te bekijken en testen van de bewerkingen van een API worden aangeroepen.

> Als u nog een exemplaar van API Management-service hebt gemaakt, Zie [API Management service-exemplaar maken] [ Create an API Management service instance] in de [aan de slag met Azure API Management] [ Get started with Azure API Management] zelfstudie.
> 
> 

![API Management-portal voor ontwikkelaars][api-management-developer-portal-menu]

Klik op **API's** van het bovenste menu en klik op **basisrekenmachine**.

![Echo-API][api-management-api]

Klik op **Try it** om te proberen de **twee gehele getallen toevoegen** bewerking.

![Probeer het nu][api-management-open-console]

Gebruik de standaardwaarde parameterwaarden en selecteert u de sleutel van het abonnement voor het product dat u gebruiken wilt vanaf de **abonnementssleutel** vervolgkeuzelijst.

In de portal voor ontwikkelaars standaard de **Ocp-Apim-Trace** header is al ingesteld op **true**. Deze header configureert u of er een tracering wordt gegenereerd.

![Verzenden][api-management-http-get]

Klik op **verzenden** aanroepen van de bewerking.

![Verzenden][api-management-send-results]

In het antwoord headers is een **ocp-apim-trace-locatie** met een waarde gelijkaardig aan het volgende voorbeeld.

```
ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742
```

De tracering kan worden gedownload vanaf de opgegeven locatie en gecontroleerd, zoals wordt beschreven in de volgende stap. Houd er rekening mee dat alleen de laatste 100 logboekvermeldingen worden opgeslagen en logboeklocaties opnieuw worden gebruikt in de draaihoek. In dat geval als u meer dan 100 aanroept met tracering ingeschakeld begint u met het uiteindelijk wordt overschreven de eerste traceringen aanwezig.

## <a name="inspect-trace"></a>Inspecteer de tracering
Om de waarden in de tracering controleren, downloaden de traceringsbestand van de **ocp-apim-trace-locatie** URL. Er is een tekstbestand in JSON-indeling en vermeldingen die lijken op het volgende voorbeeld bevat.

```json
{
    "traceId": "abcd8ea63d134c1fabe6371566c7cbea",
    "traceEntries": {
        "inbound": [
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.2998610Z",
                "elapsed": "00:00:00.0725926",
                "data": {
                    "request": {
                        "method": "GET",
                        "url": "https://contoso5.azure-api.net/calc/add?a=51&b=49",
                        "headers": [
                            {
                                "name": "Ocp-Apim-Subscription-Key",
                                "value": "5d7c41af64a44a68a2ea46580d271a59"
                            },
                            {
                                "name": "Connection",
                                "value": "Keep-Alive"
                            },
                            {
                                "name": "Host",
                                "value": "contoso5.azure-api.net"
                            }
                        ]
                    }
                }
            },
            {
                "source": "mapper",
                "timestamp": "2015-06-23T19:51:35.2998610Z",
                "elapsed": "00:00:00.0726213",
                "data": {
                    "configuration": {
                        "api": {
                            "from": "/calc",
                            "to": {
                                "scheme": "http",
                                "host": "calcapi.cloudapp.net",
                                "port": 80,
                                "path": "/api",
                                "queryString": "",
                                "query": {},
                                "isDefaultPort": true
                            }
                        },
                        "operation": {
                            "method": "GET",
                            "uriTemplate": "/add?a={a}&b={b}"
                        },
                        "user": {
                            "id": 1,
                            "groups": [
                                "Administrators",
                                "Developers"
                            ]
                        },
                        "product": {
                            "id": 1
                        }
                    }
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.2998610Z",
                "elapsed": "00:00:00.0727522",
                "data": {
                    "message": "Request is being forwarded to the backend service.",
                    "request": {
                        "method": "GET",
                        "url": "http://calcapi.cloudapp.net/api/add?a=51&b=49",
                        "headers": [
                            {
                                "name": "Ocp-Apim-Subscription-Key",
                                "value": "5d7c41af64a44a68a2ea46580d271a59"
                            },
                            {
                                "name": "X-Forwarded-For",
                                "value": "33.52.215.35"
                            }
                        ]
                    }
                }
            }
        ],
        "outbound": [
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1960601",
                "data": {
                    "response": {
                        "status": {
                            "code": 200,
                            "reason": "OK"
                        },
                        "headers": [
                            {
                                "name": "Pragma",
                                "value": "no-cache"
                            },
                            {
                                "name": "Content-Length",
                                "value": "124"
                            },
                            {
                                "name": "Cache-Control",
                                "value": "no-cache"
                            },
                            {
                                "name": "Content-Type",
                                "value": "application/xml; charset=utf-8"
                            },
                            {
                                "name": "Date",
                                "value": "Tue, 23 Jun 2015 19:51:35 GMT"
                            },
                            {
                                "name": "Expires",
                                "value": "-1"
                            },
                            {
                                "name": "Server",
                                "value": "Microsoft-IIS/8.5"
                            },
                            {
                                "name": "X-AspNet-Version",
                                "value": "4.0.30319"
                            },
                            {
                                "name": "X-Powered-By",
                                "value": "ASP.NET"
                            }
                        ]
                    }
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1961112",
                "data": {
                    "message": "Response headers have been sent to the caller. Starting to stream the response body."
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1963155",
                "data": {
                    "message": "Response body streaming to the caller is complete."
                }
            }
        ]
    }
}
```

## <a name="next-steps"> </a>Volgende stappen
* Bekijk een demo voor tracering van beleidsexpressies in [Cloud hebben betrekking op aflevering 177: meer API Management-functies](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). Vooruit tot 21:00 voor een overzicht van de demo.

> [!VIDEO https://channel9.msdn.com/Shows/Cloud+Cover/Episode-177-More-API-Management-Features-with-Vlad-Vinogradsky/player]
> 
> 

[Use API Inspector to trace a call]: #trace-call
[Inspect the trace]: #inspect-trace
[Next steps]: #next-steps

[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Azure Classic Portal]: https://manage.windowsazure.com/


[api-management-developer-portal-menu]: ./media/api-management-howto-api-inspector/api-management-developer-portal-menu.png
[api-management-api]: ./media/api-management-howto-api-inspector/api-management-api.png
[api-management-echo-api-get]: ./media/api-management-howto-api-inspector/api-management-echo-api-get.png
[api-management-developer-key]: ./media/api-management-howto-api-inspector/api-management-developer-key.png
[api-management-open-console]: ./media/api-management-howto-api-inspector/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-api-inspector/api-management-http-get.png
[api-management-send-results]: ./media/api-management-howto-api-inspector/api-management-send-results.png




