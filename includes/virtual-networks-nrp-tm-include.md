## <a name="traffic-manager-profile"></a>Traffic Manager-profiel
Traffic manager en de onderliggende endpoint-bron inschakelen DNS-routering naar de eindpunten in Azure en buiten Azure. Dergelijke distributie van verkeer wordt geregeld door de methoden voor het doorsturen beleid. Traffic manager kunt ook eindpunt health moeten worden bewaakt en verkeer op de juiste wijze wordt gewijzigd op basis van de status van een eindpunt. 

| Eigenschap | Beschrijving |
| --- | --- |
| **trafficRoutingMethod** |mogelijke waarden zijn *prestaties*, *gewogen*, en *prioriteit* |
| **dnsConfig** |FQDN-naam voor het profiel |
| **Protocol** |mogelijke waarden zijn protocol bewaking, *HTTP* en *HTTPS* |
| **Poort** |bewaking van poort |
| **Pad** |controlepad |
| **Eindpunten** |container voor eindpunt bronnen |

### <a name="endpoint"></a>Eindpunt
Een eindpunt is een onderliggende resource van een Traffic Manager-profiel. Vertegenwoordigt een service of web-eindpunt waaraan gebruiker verkeer wordt verdeeld op basis van het geconfigureerde beleid in de resource Traffic Manager-profiel. 

| Eigenschap | Beschrijving |
| --- | --- |
| **Type** |het type van het eindpunt, mogelijke waarden zijn *Azure eindpunt*, *Extern eindpunt*, en *genest eindpunt* |
| **targetResourceId** |openbare IP-adres van een service of web-eindpunt. Dit is een Azure of het externe eindpunt. |
| **Gewicht** |eindpunt gewicht in verkeer management gebruikt. |
| **Prioriteit** |prioriteit van het eindpunt, gebruikt voor het definiëren van een actie van failover |

Voorbeeld van Traffic Manager in Json-indeling: 

        {
            "apiVersion": "[variables('tmApiVersion')]",
            "type": "Microsoft.Network/trafficManagerProfiles",
            "name": "VMEndpointExample",
            "location": "global",
            "dependsOn": [
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '0')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '1')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '2')]",
            ],
            "properties": {
                "profileStatus": "Enabled",
                "trafficRoutingMethod": "Weighted",
                "dnsConfig": {
                    "relativeName": "[parameters('dnsname')]",
                    "ttl": 30
                },
                "monitorConfig": {
                    "protocol": "http",
                    "port": 80,
                    "path": "/"
                },
                "endpoints": [
                    {
                        "name": "endpoint0",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 0))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint1",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 1))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint2",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 2))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    }
                ]
            }
        }


## <a name="additional-resources"></a>Aanvullende bronnen
Lees [REST API-documentatie voor Traffic Manager](https://msdn.microsoft.com/library/azure/mt163664.aspx) voor meer informatie.

