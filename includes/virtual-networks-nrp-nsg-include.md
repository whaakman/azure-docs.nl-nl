## <a name="network-security-group"></a>Netwerkbeveiligingsgroep
Een NSG-resource wordt het maken van een beveiligingsgrens voor werkbelastingen, door het implementeren van toestaan en weigeren regels. Deze regels kunnen worden toegepast op een virtuele machine, een NIC of een subnet.

| Eigenschap | Beschrijving | Voorbeeldwaarden |
| --- | --- | --- |
| **subnetten** |Lijst met subnet-id's die het NSG wordt toegepast op. |/Subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd |
| **securityRules** |Lijst met beveiligingsregels voor verbindingen die gezamenlijk de NSG |Zie [beveiligingsregel](#Security-rule) hieronder |
| **defaultSecurityRules** |Lijst met beveiligingsregels standaard aanwezig zijn in elke NSG |Zie [standaard beveiligingsregels](#Default-security-rules) hieronder |

* **De beveiligingsregel** -meerdere regels gedefinieerd door een NSG kan hebben. Elke regel kunt toestaan of weigeren van verschillende typen verkeer.

### <a name="security-rule"></a>De beveiligingsregel
Een beveiligingsregel is een onderliggende resource van een NSG met de volgende eigenschappen.

| Eigenschap | Beschrijving | Voorbeeldwaarden |
| --- | --- | --- |
| **Beschrijving** |Beschrijving voor de regel |Binnenkomend verkeer toestaan voor alle VM's in het subnet X |
| **Protocol** |Te matchen protocol voor de regel |TCP, UDP of * |
| **sourcePortRange** |Te matchen bronpoortbereik voor de regel |80, 100-200, * |
| **destinationPortRange** |Te matchen doelpoortbereik voor de regel |80, 100-200, * |
| **sourceAddressPrefix** |Voorvoegsel voor bronadres als overeenkomst voor de regel |10.10.10.1, 10.10.10.0/24, VirtualNetwork |
| **destinationAddressPrefix** |Voorvoegsel voor doeladres aan voor de regel |10.10.10.1, 10.10.10.0/24, VirtualNetwork |
| **richting** |Te matchen richting van verkeer voor de regel |binnenkomend of uitgaand |
| **prioriteit** |Prioriteit voor de regel. Regels worden gecontroleerd op volgorde van prioriteit, wanneer een regel van toepassing is, geen regels meer getest voor matching. |10, 100, 65000 |
| **toegang** |Toe te passen type toegang als de regel matcht |toestaan of weigeren |

Voorbeeld NSG in JSON-indeling:

    {
        "name": "NSG-BackEnd",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/networkSecurityGroups",
        "location": "westus",
        "tags": {
            "displayName": "NSG - Front End"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "securityRules": [
                {
                    "name": "rdp-rule",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/rdp-rule",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "description": "Allow RDP",
                        "protocol": "Tcp",
                        "sourcePortRange": "*",
                        "destinationPortRange": "3389",
                        "sourceAddressPrefix": "Internet",
                        "destinationAddressPrefix": "*",
                        "access": "Allow",
                        "priority": 100,
                        "direction": "Inbound"
                    }
                }
            ],
            "defaultSecurityRules": [
                { [...],
            "subnets": [
                {
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
                }
            ]
        }
    }

### <a name="default-security-rules"></a>Standaardbeveiligingsregels

Standaard-Beveiligingsregels hebben dezelfde eigenschappen beschikbaar zijn in de beveiligingsregels voor verbindingen. Ze bestaan om basic verbinding mogelijk tussen resources met nsg's die worden toegepast een. Zorg ervoor dat u weet welk [standaard beveiligingsregels](../articles/virtual-network/virtual-networks-nsg.md#default-rules) bestaan.

### <a name="additional-resources"></a>Aanvullende bronnen
* Vindt u meer informatie over [nsg's](../articles/virtual-network/virtual-networks-nsg.md).
* Lees de [REST-API-naslagdocumentatie](https://msdn.microsoft.com/library/azure/mt163615.aspx) voor nsg's.
* Lees de [REST-API-naslagdocumentatie](https://msdn.microsoft.com/library/azure/mt163580.aspx) voor beveiligingsregels voor verbindingen.
