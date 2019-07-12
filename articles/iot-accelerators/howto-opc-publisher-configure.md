---
title: OPC Publisher - Azure configureren | Microsoft Docs
description: OPC Publisher configureren
author: dominicbetts
ms.author: dobett
ms.date: 06/10/2019
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: bccab4dde5e17ec30a0b8c5e36dd78bdd1bdff93
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67605727"
---
# <a name="configure-opc-publisher"></a>OPC Publisher configureren

OPC Publisher om op te geven, kunt u configureren:

- De OPC UA-knooppunt gegevens-wijzigingen te publiceren.
- De OPC UA-gebeurtenissen te publiceren.
- De telemetrie-indeling.

U kunt OPC Publisher met behulp van configuratiebestanden of methodeaanroepen configureren.

## <a name="use-configuration-files"></a>Configuratiebestanden gebruiken

Deze sectie beschrijft opties voor het configureren van OPC UA-knooppunt publiceren met-configuratiebestanden.

### <a name="use-a-configuration-file-to-configure-publishing-data-changes"></a>Gebruik een configuratiebestand om te publiceren gegevenswijzigingen configureren

Er is de eenvoudigste manier om het configureren van de OPC UA-knooppunten te publiceren met een configuratiebestand. De indeling van het configuratiebestand wordt gedocumenteerd in [publishednodes.json](https://github.com/Azure/iot-edge-opc-publisher/blob/master/opcpublisher/publishednodes.json) in de opslagplaats.

De syntaxis van de configuratie-bestand is gewijzigd na verloop van tijd. OPC Publisher leest oude indelingen, maar die ze converteert naar de nieuwste indeling wanneer deze actief de configuratie blijft.

Het volgende voorbeeld ziet u de indeling van het configuratiebestand:

```json
[
  {
    "EndpointUrl": "opc.tcp://testserver:62541/Quickstarts/ReferenceServer",
    "UseSecurity": true,
    "OpcNodes": [
      {
        "Id": "i=2258",
        "OpcSamplingInterval": 2000,
        "OpcPublishingInterval": 5000,
        "DisplayName": "Current time"
      }
    ]
  }
]
```

### <a name="use-a-configuration-file-to-configure-publishing-events"></a>Gebruik een configuratiebestand om te publiceren gebeurtenissen configureren

Voor het publiceren van OPC UA-gebeurtenissen, moet u het bestand met dezelfde configuratie als gegevens worden gewijzigd voor gebruiken.

Het volgende voorbeeld laat zien hoe het configureren van publicatie van gebeurtenissen die worden gegenereerd door de [SimpleEvents server](https://github.com/OPCFoundation/UA-.NETStandard/tree/master/SampleApplications/Workshop/SimpleEvents/Server). De server SimpleEvents vindt u de [OPC Foundation opslagplaats](https://github.com/OPCFoundation/UA-.NETStandard) is:

```json
[
  {
    "EndpointUrl": "opc.tcp://testserver:62563/Quickstarts/SimpleEventsServer",
    "OpcEvents": [
      {
        "Id": "i=2253",
        "DisplayName": "SimpleEventServerEvents",
        "SelectClauses": [
          {
            "TypeId": "i=2041",
            "BrowsePaths": [
              "EventId"
            ]
          },
          {
            "TypeId": "i=2041",
            "BrowsePaths": [
              "Message"
            ]
          },
          {
            "TypeId": "nsu=http://opcfoundation.org/Quickstarts/SimpleEvents;i=235",
            "BrowsePaths": [
              "/2:CycleId"
            ]
          },
          {
            "TypeId": "nsu=http://opcfoundation.org/Quickstarts/SimpleEvents;i=235",
            "BrowsePaths": [
              "/2:CurrentStep"
            ]
          }
        ],
        "WhereClause": [
          {
            "Operator": "OfType",
            "Operands": [
              {
                "Literal": "ns=2;i=235"
              }
            ]
          }
        ]
      }
    ]
  }
]
```

## <a name="use-method-calls"></a>Methodeaanroepen gebruiken

Deze sectie beschrijft de methodeaanroepen die kunt u het configureren van OPC Publisher.

### <a name="configure-using-opc-ua-method-calls"></a>Configureren met behulp van OPC UA-methodeaanroepen

OPC Publisher bevat een OPC UA-Server, die kunnen worden geopend op poort 62222. Als de hostnaam **publisher**, en vervolgens de eindpunt-URI is: `opc.tcp://publisher:62222/UA/Publisher`.

Dit eindpunt bevat de volgende vier methoden:

- PublishNode
- UnpublishNode
- GetPublishedNodes
- IoT HubDirectMethod

### <a name="configure-using-iot-hub-direct-method-calls"></a>Configureren met behulp van IoT-Hub rechtstreekse methodeaanroepen

OPC Publisher worden geïmplementeerd voor de volgende rechtstreekse methodeaanroepen voor IoT-Hub:

- PublishNodes
- UnpublishNodes
- UnpublishAllNodes
- GetConfiguredEndpoints
- GetConfiguredNodesOnEndpoint
- GetDiagnosticInfo
- GetDiagnosticLog
- GetDiagnosticStartupLog
- ExitApplication
- GetInfo

De indeling van de JSON-nettolading van de methode aanvragen en antwoorden zijn gedefinieerd in [opcpublisher/HubMethodModel.cs](https://github.com/Azure/iot-edge-opc-publisher/blob/master/opcpublisher/HubMethodModel.cs).

Als u een onbekende methode op de module aanroepen, reageert met een tekenreeks met de tekst van dat de methode is niet geïmplementeerd. U kunt een onbekende methode aanroepen als een manier om te pingen van de module.

### <a name="configure-username-and-password-for-authentication"></a>Gebruikersnaam en wachtwoord voor verificatie configureren

De verificatiemodus kan worden ingesteld via een IoT-Hub rechtstreekse methodeaanroepen. De nettolading mag de eigenschap **OpcAuthenticationMode** en de gebruikersnaam en het wachtwoord:

```csharp
{
    "EndpointUrl": "<Url of the endpoint to set authentication settings>",
    "OpcAuthenticationMode": "UsernamePassword",
    "Username": "<Username>",
    "Password": "<Password>"
    ...
}
```

Het wachtwoord is versleuteld door de IoT Hub Workload-Client en die zijn opgeslagen in de configuratie van de uitgever. Als u wilt wijzigen verificatie op anoniem, gebruikt u de methode met de nettolading van de volgende:

```csharp
{
    "EndpointUrl": "<Url of the endpoint to set authentication settings>",
    "OpcAuthenticationMode": "Anonymous"
    ...
}
```

Als de **OpcAuthenticationMode** eigenschap is niet ingesteld in de nettolading van, de verificatie-instellingen in de configuratie ongewijzigd blijven.

## <a name="configure-telemetry-publishing"></a>Telemetrie-publicatie configureren

Wanneer OPC Publisher een melding van een wijziging in de waarde in een gepubliceerde knooppunt ontvangt, genereert deze een opgemaakte JSON-bericht dat wordt verzonden naar IoT Hub.

U kunt de inhoud van dit JSON opgemaakte bericht met behulp van een configuratiebestand configureren. Als er is geen configuratiebestand wordt opgegeven met de `--tc` optie, een standaardconfiguratie wordt gebruikt die compatibel is met de [oplossingsverbetering voor verbonden factory](https://github.com/Azure/azure-iot-connected-factory).

Als OPC Publisher is geconfigureerd voor berichten batchgewijs, bent worden ze verzonden als een geldige JSON-matrix.

De telemetrie is afgeleid van de volgende bronnen:

- De configuratie van de OPC Publisher knooppunten voor het knooppunt
- De **MonitoredItem** object van de OPC UA-stack OPC Publisher hebt waarvoor u een melding.
- Het argument doorgegeven aan dit bericht vindt u informatie over het wijzigen van de waarde data.

De telemetrie die wordt geplaatst in het opgemaakte JSON-bericht is een aantal belangrijke eigenschappen van deze objecten. Als u meer eigenschappen, die u wilt wijzigen van de codebasis OPC Publisher.

De syntaxis van het configuratiebestand is als volgt:

```json
// The configuration settings file consists of two objects:
// 1) The 'Defaults' object, which defines defaults for the telemetry configuration
// 2) An array 'EndpointSpecific' of endpoint specific configuration
// Both objects are optional and if they are not specified, then publisher uses
// its internal default configuration, which generates telemetry messages compatible
// with the Microsoft Connected factory Preconfigured Solution (https://github.com/Azure/azure-iot-connected-factory).

// A JSON telemetry message for Connected factory looks like:
//  {
//      "NodeId": "i=2058",
//      "ApplicationUri": "urn:myopcserver",
//      "DisplayName": "CurrentTime",
//      "Value": {
//          "Value": "10.11.2017 14:03:17",
//          "SourceTimestamp": "2017-11-10T14:03:17Z"
//      }
//  }

// The 'Defaults' object in the sample below, are similar to what publisher is
// using as its internal default telemetry configuration.
{
    "Defaults": {
        // The first two properties ('EndpointUrl' and 'NodeId' are configuring data
        // taken from the OpcPublisher node configuration.
        "EndpointUrl": {

            // The following three properties can be used to configure the 'EndpointUrl'
            // property in the JSON message send by publisher to IoT Hub.

            // Publish controls if the property should be part of the JSON message at all.
            "Publish": false,

            // Pattern is a regular expression, which is applied to the actual value of the
            // property (here 'EndpointUrl').
            // If this key is ommited (which is the default), then no regex matching is done
            // at all, which improves performance.
            // If the key is used you need to define groups in the regular expression.
            // Publisher applies the regular expression and then concatenates all groups
            // found and use the resulting string as the value in the JSON message to
            //sent to IoT Hub.
            // This example mimics the default behaviour and defines a group,
            // which matches the conplete value:
            "Pattern": "(.*)",
            // Here some more exaples for 'Pattern' values and the generated result:
            // "Pattern": "i=(.*)"
            // defined for Defaults.NodeId.Pattern, will generate for the above sample
            // a 'NodeId' value of '2058'to be sent by publisher
            // "Pattern": "(i)=(.*)"
            // defined for Defaults.NodeId.Pattern, will generate for the above sample
            // a 'NodeId' value of 'i2058' to be sent by publisher

            // Name allows you to use a shorter string as property name in the JSON message
            // sent by publisher. By default the property name is unchanged and will be
            // here 'EndpointUrl'.
            // The 'Name' property can only be set in the 'Defaults' object to ensure
            // all messages from publisher sent to IoT Hub have a similar layout.
            "Name": "EndpointUrl"

        },
        "NodeId": {
            "Publish": true,

            // If you set Defaults.NodeId.Name to "ni", then the "NodeId" key/value pair
            // (from the above example) will change to:
            //      "ni": "i=2058",
            "Name": "NodeId"
        },

        // The MonitoredItem object is configuring the data taken from the MonitoredItem
        // OPC UA object for published nodes.
        "MonitoredItem": {

            // If you set the Defaults.MonitoredItem.Flat to 'false', then a
            // 'MonitoredItem' object will appear, which contains 'ApplicationUri'
            // and 'DisplayNode' proerties:
            //      "NodeId": "i=2058",
            //      "MonitoredItem": {
            //          "ApplicationUri": "urn:myopcserver",
            //          "DisplayName": "CurrentTime",
            //      }
            // The 'Flat' property can only be used in the 'MonitoredItem' and
            // 'Value' objects of the 'Defaults' object and will be used
            // for all JSON messages sent by publisher.
            "Flat": true,

            "ApplicationUri": {
                "Publish": true,
                "Name": "ApplicationUri"
            },
            "DisplayName": {
                "Publish": true,
                "Name": "DisplayName"
            }
        },
        // The Value object is configuring the properties taken from the event object
        // the OPC UA stack provided in the value change notification event.
        "Value": {
            // If you set the Defaults.Value.Flat to 'true', then the 'Value'
            // object will disappear completely and the 'Value' and 'SourceTimestamp'
            // members won't be nested:
            //      "DisplayName": "CurrentTime",
            //      "Value": "10.11.2017 14:03:17",
            //      "SourceTimestamp": "2017-11-10T14:03:17Z"
            // The 'Flat' property can only be used for the 'MonitoredItem' and 'Value'
            // objects of the 'Defaults' object and will be used for all
            // messages sent by publisher.
            "Flat": false,

            "Value": {
                "Publish": true,
                "Name": "Value"
            },
            "SourceTimestamp": {
                "Publish": true,
                "Name": "SourceTimestamp"
            },
            // 'StatusCode' is the 32 bit OPC UA status code
            "StatusCode": {
                "Publish": false,
                "Name": "StatusCode"
                // 'Pattern' is ignored for the 'StatusCode' value
            },
            // 'Status' is the symbolic name of 'StatusCode'
            "Status": {
                "Publish": false,
                "Name": "Status"
            }
        }
    },

    // The next object allows to configure 'Publish' and 'Pattern' for specific
    // endpoint URLs. Those will overwrite the ones specified in the 'Defaults' object
    // or the defaults used by publisher.
    // It is not allowed to specify 'Name' and 'Flat' properties in this object.
    "EndpointSpecific": [
        // The following shows how a endpoint specific configuration can look like:
        {
            // 'ForEndpointUrl' allows to configure for which OPC UA server this
            // object applies and is a required property for all objects in the
            // 'EndpointSpecific' array.
            // The value of 'ForEndpointUrl' must be an 'EndpointUrl' configured in
            // the publishednodes.json confguration file.
            "ForEndpointUrl": "opc.tcp://<your_opcua_server>:<your_opcua_server_port>/<your_opcua_server_path>",
            "EndpointUrl": {
                // We overwrite the default behaviour and publish the
                // endpoint URL in this case.
                "Publish": true,
                // We are only interested in the URL part following the 'opc.tcp://' prefix
                // and define a group matching this.
                "Pattern": "opc.tcp://(.*)"
            },
            "NodeId": {
                // We are not interested in the configured 'NodeId' value, 
                // so we do not publish it.
                "Publish": false
                // No 'Pattern' key is specified here, so the 'NodeId' value will be
                // taken as specified in the publishednodes configuration file.
            },
            "MonitoredItem": {
                "ApplicationUri": {
                    // We already publish the endpoint URL, so we do not want
                    // the ApplicationUri of the MonitoredItem to be published.
                    "Publish": false
                },
                "DisplayName": {
                    "Publish": true
                }
            },
            "Value": {
                "Value": {
                    // The value of the node is important for us, everything else we
                    // are not interested in to keep the data ingest as small as possible.
                    "Publish": true
                },
                "SourceTimestamp": {
                    "Publish": false
                },
                "StatusCode": {
                    "Publish": false
                },
                "Status": {
                    "Publish": false
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Volgende stappen

Nu hebt u geleerd hoe het configureren van OPC Publisher, de voorgestelde volgende stap is te leren hoe u [uitvoeren OPC Publisher](howto-opc-publisher-run.md).
