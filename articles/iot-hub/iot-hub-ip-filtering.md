---
title: Azure IoT Hub-IP-verbindingsfilters | Microsoft Docs
description: Het gebruik van IP-filtering om verbindingen te blokkeren van bepaalde IP-adressen voor uw Azure-IoT-hub. U kunt verbindingen van afzonderlijke of bereiken van IP-adressen blokkeren.
author: rezasherafat
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: rezas
ms.openlocfilehash: cd382c0daff79b487f4ecae01ad852f6e57f3a25
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55734246"
---
# <a name="use-ip-filters"></a>IP-filters gebruiken

Beveiliging is een belangrijk aspect van een IoT-oplossing op basis van Azure IoT Hub. Soms moet u expliciet opgeven van de IP-adressen van waaruit de apparaten kunnen verbinding maken als onderdeel van uw beveiligingsconfiguratie. De *IP-filter* functie kunt u regels configureren voor het weigeren van of verkeer van specifieke IPv4-adressen accepteert.

## <a name="when-to-use"></a>Wanneer gebruikt u dit?

Er zijn twee specifieke use cases als het is handig om te blokkeren van de IoT Hub-eindpunten voor bepaalde IP-adressen:

* Uw IoT-hub moet verkeer ontvangen van een opgegeven bereik van IP-adressen alleen en alle andere afwijzen. Bijvoorbeeld, gebruikt u uw IoT-hub met [Azure Express Route](https://azure.microsoft.com/documentation/articles/expressroute-faqs/#supported-services) particuliere verbindingen maken tussen een IoT-hub en uw on-premises infrastructuur.

* U moet verkeer van IP-adressen die zijn geïdentificeerd als verdacht door de beheerder van de IoT-hub afwijzen.

## <a name="how-filter-rules-are-applied"></a>Hoe regels worden toegepast

De IP-filterregels worden toegepast op het niveau van de IoT Hub-service. Daarom de IP-filterregels van toepassing op alle verbindingen vanaf apparaten en back-end-apps met behulp van een ondersteund protocol.

Elke verbindingspoging vanaf een IP-adres dat overeenkomt met een rejecting IP-regel in uw IoT-hub ontvangt een niet-geautoriseerde 401-statuscode en de beschrijving. Het antwoordbericht wordt niet vermeld voor de IP-regel.

## <a name="default-setting"></a>Standaardinstelling

Standaard de **IP-Filter** raster in de portal voor een IoT-hub is leeg. Deze instelling betekent dat uw hub verbindingen van elk IP-adres aanvaardt. Deze instelling is gelijk aan een regel waarmee het 0.0.0.0/0 IP-adresbereik accepteert.

![IoT Hub-standaard IP-filter-instellingen](./media/iot-hub-ip-filtering/ip-filter-default.png)

## <a name="add-or-edit-an-ip-filter-rule"></a>Een IP-filter-regel toevoegen of bewerken

Wanneer u een IP-filter rule toevoegt, wordt u gevraagd de volgende waarden op te geven:

* Een **IP-filter regelnaam** die moet een unieke, niet-hoofdlettergevoelig, alfanumerieke tekenreeks maximaal 128 tekens lang zijn. Alleen de ASCII-7-bits alfanumerieke tekens plus `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` worden geaccepteerd.

* Selecteer een **afwijzen** of **accepteren** als de **actie** voor de IP-filter rule.

* Geef één IPv4-adres of een blok IP-adressen in CIDR-notatie. Bijvoorbeeld, in CIDR vertegenwoordigt notatie 192.168.100.0/22 de 1024 IPv4-adressen van 192.168.100.0 tot 192.168.103.255 aangeeft.

![Een regel voor IP-filter toevoegen aan een IoT-hub](./media/iot-hub-ip-filtering/ip-filter-add-rule.png)

Nadat u de regel niet opslaan, ziet u een waarschuwing dat de update uitgevoerd wordt.

![Melding over het opslaan van een IP-filter rule](./media/iot-hub-ip-filtering/ip-filter-save-new-rule.png)

De **toevoegen** optie wordt uitgeschakeld wanneer u het maximum van 10 filterregels voor de IP-bereiken.

U kunt een bestaande regel bewerken door te dubbelklikken op de rij die de regel bevat.

> [!NOTE]
> Weigeren IP adressen kunnen voorkomen dat andere Azure-Services (zoals Azure Stream Analytics, Azure Virtual Machines of het Device Explorer in de portal) interactie met de IoT-hub.

> [!WARNING]
> Als u Azure Stream Analytics (ASA) gebruikt om berichten te lezen van een IoT-hub met IP-filtering is ingeschakeld, gebruikt u de Event Hub-compatibele naam en het eindpunt van uw IoT-Hub in de ASA-verbindingsreeks.

## <a name="delete-an-ip-filter-rule"></a>Een regel voor IP-filter verwijderen

Een IP-filter als regel wilt verwijderen, selecteert u een of meer regels in het raster en klikt u op **verwijderen**.

![Een regel voor IoT Hub-IP-filter verwijderen](./media/iot-hub-ip-filtering/ip-filter-delete-rule.png)

## <a name="retrieve-and-update-ip-filters-using-azure-cli"></a>Ophalen en bijwerken van IP-filters met behulp van Azure CLI

IP-filters voor uw IoT-Hub kunnen worden opgehaald en bijgewerkt via [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest). 

Als u wilt ophalen van huidige IP-filters van uw IoT-Hub, voert u de volgende uit:

```azurecli-interactive
az resource show -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs
```

Dit resulteert in een JSON-object dat is waar uw bestaande IP-filters worden vermeld in de `properties.ipFilterRules` sleutel:

```json
{
...
    "properties": {
        "ipFilterRules": [
        {
            "action": "Reject",
            "filterName": "MaliciousIP",
            "ipMask": "6.6.6.6/6"
        },
        {
            "action": "Allow",
            "filterName": "GoodIP",
            "ipMask": "131.107.160.200"
        },
        ...
        ],
    },
...
}
```

Als u wilt een nieuw IP-filter toevoegen voor uw IoT-Hub, voert u de volgende uit:

```azurecli-interactive
az resource update -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs --add properties.ipFilterRules "{\"action\":\"Reject\",\"filterName\":\"MaliciousIP\",\"ipMask\":\"6.6.6.6/6\"}"
```

Voer het volgende voor meer informatie over het verwijderen van een bestaande IP-filter in uw IoT-Hub:

```azurecli-interactive
az resource update -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs --add properties.ipFilterRules <ipFilterIndexToRemove>
```

Houd er rekening mee dat `<ipFilterIndexToRemove>` moet overeenkomen met de volgorde van de IP-filters in uw IoT-Hub `properties.ipFilterRules`.


## <a name="retrieve-and-update-ip-filters-using-azure-powershell"></a>Ophalen en bijwerken van IP-filters met behulp van Azure PowerShell

IP-filters voor uw IoT-Hub kunnen worden opgehaald en ingesteld via [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview?view=azps-1.2.0). 

```powershell
# Get your IoT Hub resource using its name and its resource group name
$iothubResource = Get-AzureRmResource -ResourceGroupName <resourceGroupNmae> -ResourceName <iotHubName> -ExpandProperties

# Access existing IP filter rules
$iothubResource.Properties.ipFilterRules |% { Write-host $_ }

# Construct a new IP filter
$filter = @{'filterName'='MaliciousIP'; 'action'='Reject'; 'ipMask'='6.6.6.6/6'}

# Add your new IP filter rule
$iothubResource.Properties.ipFilterRules += $filter

# Remove an existing IP filter rule using its name, e.g., 'GoodIP'
$iothubResource.Properties.ipFilterRules = @($iothubResource.Properties.ipFilterRules | Where 'filterName' -ne 'GoodIP')

# Update your IoT Hub resource with your updated IP filters
$iothubResource | Set-AzureRmResource -Force
```

## <a name="update-ip-filter-rules-using-rest"></a>Regels voor IP-filter met behulp van REST bijwerken

U kunt ook ophalen en wijzigen van IP-adresfilter van uw IoT-Hub met behulp van Azure Resourceprovider REST-eindpunt. Zie `properties.ipFilterRules` in [createorupdate-methode](https://docs.microsoft.com/en-us/rest/api/iothub/iothubresource/createorupdate).


## <a name="ip-filter-rule-evaluation"></a>Evaluatie van IP-filter

IP-filterregels worden toegepast in volgorde en de eerste regel die overeenkomt met het IP-adres bepaalt de actie accepteren of weigeren.

Als u adressen in het bereik 192.168.100.0/22 accepteren en weigeren alle andere wilt, moet de eerste regel in het raster bijvoorbeeld, het adresbereik 192.168.100.0/22 accepteren. De volgende regel moet alle adressen weigeren met behulp van het adresbereik 0.0.0.0/0.

U kunt de volgorde van de regels van uw IP-filter in het raster wijzigen door de drie verticale puntjes aan het begin van een rij te klikken en slepen en neerzetten.

Om uw nieuwe regelvolgorde van de IP-filter hebt opgeslagen, klikt u op **opslaan**.

![De volgorde van uw IoT Hub-IP-filterregels wijzigen](./media/iot-hub-ip-filtering/ip-filter-rule-order.png)

## <a name="next-steps"></a>Volgende stappen

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Controle van bewerkingen](iot-hub-operations-monitoring.md)
* [Metrische gegevens van IoT Hub](iot-hub-metrics.md)