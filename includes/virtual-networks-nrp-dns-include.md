## <a name="azure-dns"></a>Azure DNS
Azure DNS is een hosting service voor DNS-domeinen omzetten van namen met behulp van Microsoft Azure-infrastructuur.

| Eigenschap | Beschrijving | Voorbeeldwaarde |
| --- | --- | --- |
| **DNSzones** |Zone domeingegevens host DNS-records van een bepaald domein |/ subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com ' |

### <a name="dns-record-sets"></a>DNS-recordsets
DNS-zones hebben een onderliggend object met de naam Recordset. Recordsets zijn een verzameling van hostrecords per voor een DNS-zone. Recordtypen worden A, AAAA, CNAME, MX, NS, SOA, SRV en TXT.

| Eigenschap | Beschrijving | Voorbeeldwaarde |
| --- | --- | --- |
| A |IPv4-recordtype |/Subscriptions/{GUID}/.../providers/Microsoft.Network/dnszones/contoso.com/A/www |
| AAAA |IPv6-recordtype |/Subscriptions/{GUID}/.../providers/Microsoft.Network/dnszones/contoso.com/AAAA/hostrecord |
| CNAME |canonieke naam recordtype <sup>1</sup> |/Subscriptions/{GUID}/.../providers/Microsoft.Network/dnszones/contoso.com/CNAME/www |
| MX |e-recordtype |/Subscriptions/{GUID}/.../providers/Microsoft.Network/dnszones/contoso.com/MX/mail |
| NS |naam server recordtype |/Subscriptions/{GUID}/.../providers/Microsoft.Network/dnszones/contoso.com/NS/ |
| SOA |Begin van het recordtype autoriteit <sup>2</sup> |/Subscriptions/{GUID}/.../providers/Microsoft.Network/dnszones/contoso.com/SOA |
| SRV |recordtype Service |/Subscriptions/{GUID}/.../providers/Microsoft.Network/dnszones/contoso.com/SRV |

<sup>1</sup> staat slechts één waarde per Recordset.

<sup>2</sup> staat slechts één recordtype SOA per DNS-zone. 

Voorbeeld van DNS-zone in Json-indeling:

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "newZoneName": {
          "type": "String",
          "metadata": {
              "description": "The name of the DNS zone to be created."
          }
        },
        "newRecordName": {
          "type": "String",
          "defaultValue": "www",
          "metadata": {
              "description": "The name of the DNS record to be created.  The name is relative to the zone, not the FQDN."
          }
        }
      },
      "resources": 
      [
        {
          "type": "microsoft.network/dnszones",
          "name": "[parameters('newZoneName')]",
          "apiVersion": "2015-05-04-preview",
          "location": "global",
          "properties": {
          }
        },
        {
          "type": "microsoft.network/dnszones/a",
          "name": "[concat(parameters('newZoneName'), concat('/', parameters('newRecordName')))]",
          "apiVersion": "2015-05-04-preview",
          "location": "global",
          "properties": 
          {
            "TTL": 3600,
            "ARecords": 
            [
                {
                    "ipv4Address": "1.2.3.4"
                },
                {
                    "ipv4Address": "1.2.3.5"
                }
            ]
          },
          "dependsOn": [
            "[concat('Microsoft.Network/dnszones/', parameters('newZoneName'))]"
          ]
        }
          ]
    }

## <a name="additional-resources"></a>Aanvullende bronnen
Lees de [REST API-documentatie voor DNS-zones ](https://msdn.microsoft.com/library/azure/mt130626.aspx) voor meer informatie.

Lees de [REST-API-documentatie voor DNS-recordsets](https://msdn.microsoft.com/library/azure/mt130627.aspx) voor meer informatie.

