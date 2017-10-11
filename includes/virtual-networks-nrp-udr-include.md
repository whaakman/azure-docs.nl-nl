## <a name="route-tables"></a>Routetabellen
Route tabel resources bevat routes die worden gebruikt om te definiëren hoe verkeer stroomt binnen uw Azure-infrastructuur. De gebruiker gedefinieerde routes (UDR) kunt u alle verkeer verzenden vanaf een bepaald subnet naar een virtueel apparaat zoals een firewall of inbraakdetectie detectie-systeem (id's). U kunt een routetabel aan subnetten koppelen. 

Routetabellen bevatten de volgende eigenschappen.

| Eigenschap | Beschrijving | Voorbeeldwaarden |
| --- | --- | --- |
| **routes** |Verzameling van de gebruiker gedefinieerde routes in de routetabel |Zie [door de gebruiker gedefinieerde routes](#User-defined-routes) |
| **subnetten** |Verzameling van de routetabel wordt toegepast op subnetten |Zie [subnetten](#Subnets) |

### <a name="user-defined-routes"></a>De gebruiker gedefinieerde routes
U kunt udr's om op te geven waar verkeer moet worden gezonden, maken op basis van het doeladres. U kunt een route beschouwen als de standaard gateway-definitie op basis van het doeladres van een netwerkpakket.

Udr's bevatten de volgende eigenschappen. 

| Eigenschap | Beschrijving | Voorbeeldwaarden |
| --- | --- | --- |
| **addressPrefix** |Adresvoorvoegsel of volledige IP-adres voor de bestemming |192.168.1.0/24, 192.168.1.101 |
| **nextHopType** |Type apparaat dat het verkeer wordt verzonden naar |Internet VirtualAppliance, VPN-Gateway |
| **nextHopIpAddress** |IP-adres voor de volgende hop |192.168.1.4 |

Voorbeeld routetabel in JSON-indeling:

    {
        "name": "UDR-BackEnd",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/routeTables",
        "location": "westus",
        "properties": {
            "provisioningState": "Succeeded",
            "routes": [
                {
                    "name": "RouteToFrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd/routes/RouteToFrontEnd",
                    "etag": "W/\"v\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "nextHopType": "VirtualAppliance",
                        "nextHopIpAddress": "192.168.0.4"
                    }
                }
            ],
            "subnets": [
                {
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd"
                }
            ]
        }
    }

### <a name="additional-resources"></a>Aanvullende bronnen
* Vindt u meer informatie over [udr's](../articles/virtual-network/virtual-networks-udr-overview.md).
* Lees de [REST-API-naslagdocumentatie](https://msdn.microsoft.com/library/azure/mt502549.aspx) voor routetabellen.
* Lees de [REST-API-naslagdocumentatie](https://msdn.microsoft.com/library/azure/mt502539.aspx) voor de gebruiker gedefinieerde routes (udr's).

