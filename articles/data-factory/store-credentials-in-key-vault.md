---
title: Referenties Store in Azure Key Vault | Microsoft Docs
description: Informatie over het opslaan van referenties voor gegevens die worden gebruikt in een Azure-sleutelkluis die Azure Data Factory automatisch in runtime kunnen ophalen.
services: data-factory
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/22/2017
ms.author: jingwang
ms.openlocfilehash: 3428fb5034435d9f3444347329171d803136177c
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49944665"
---
# <a name="store-credential-in-azure-key-vault"></a>Store-referentie in Azure Key Vault

Slaat u referenties op voor gegevensarchieven en COMPUTE in een [Azure Key Vault](../key-vault/key-vault-whatis.md). Azure Data Factory worden opgehaald van de referenties bij het uitvoeren van een activiteit die gebruikmaakt van het gegevensarchief/rekenservices er gegevens.

Deze functie wordt op dit moment ondersteund door alle activiteitstypen, met uitzondering van aangepaste activiteit. Voor de connectorconfiguratie van de Controleer met name de sectie 'gekoppelde service-eigenschappen' [elk onderwerp connector](copy-activity-overview.md#supported-data-stores-and-formats) voor meer informatie.

## <a name="prerequisites"></a>Vereisten

Deze functie is afhankelijk van de data factory-service-identiteit. Ontdek hoe het werkt vanuit [Data factory-service-identiteit](data-factory-service-identity.md) en zorg ervoor dat uw data factory hebben een gekoppeld.

>[!TIP]
>In Azure Key Vault, bij het maken van een geheim **put de gehele waarde van een geheime eigenschap dat ADF service gekoppelde vraagt om (bijvoorbeeld connection string/wachtwoord/service principal sleutel/enz.)**. Bijvoorbeeld, voor Azure Storage gekoppelde service plaatsen `DefaultEndpointsProtocol=http;AccountName=myAccount;AccountKey=myKey;` plaatsen als AKV geheim, en vervolgens een verwijzing naar in "connectionString"-veld van ADF; voor de gekoppelde service van Dynamics, `myPassword` als AKV geheim, vervolgens verwijzen naar 'wachtwoord'-veld in ADF. Verwijzen naar elk artikel connector/rekenservices er bij de eigenschapdetails van de ondersteunde.

## <a name="steps"></a>Stappen

Als u wilt verwijzen naar een referenties die zijn opgeslagen in Azure Key Vault, moet u naar:

1. **Ophalen van data factory-service-identiteit** door de waarde van "SERVICE-identiteit TOEPASSINGS-ID gegenereerd samen met uw factory kopiëren. Als u gebruikmaakt van ADF ontwerpen van de service-identiteit ID wordt weergegeven in het venster van Azure Key Vault gekoppelde service maken;-gebruikersinterface u kunt ook het ophalen van Azure-portal verwijzen naar [ophalen van data factory-service-identiteit](data-factory-service-identity.md#retrieve-service-identity).
2. **De service-identiteit toegang verlenen aan uw Azure Key Vault.** In uw key vault -> beleid -> toegang toevoegen -> nieuwe zoekopdracht deze servicetoepassing voor identiteit-ID te verlenen **ophalen** machtiging in de vervolgkeuzelijst geheime machtigingen. Hiermee kunt deze aangewezen factory voor toegang tot geheim in de sleutelkluis.
3. **Een gekoppelde service die verwijst naar uw Azure Key Vault maken.** Raadpleeg [gekoppelde service van Azure Key Vault](#azure-key-vault-linked-service).
4. **Data store gekoppelde service, in welke verwijzing naar de bijbehorende geheime sleutel opgeslagen in kluis maken.** Raadpleeg [verwijzing geheim is opgeslagen in key vault](#reference-secret-stored-in-key-vault).

## <a name="azure-key-vault-linked-service"></a>Azure Key Vault gekoppelde service

De volgende eigenschappen worden ondersteund voor Azure Key Vault gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **AzureKeyVault**. | Ja |
| baseUrl | De Azure Key Vault-URL opgeven. | Ja |

**Met behulp van gebruikersinterface ontwerpen:**

Klik op **verbindingen** -> **gekoppelde Services** -> **+ nieuw** -zoek naar "Azure Key Vault" >:

![Zoeken in Azure Sleutelkluis](media/store-credentials-in-key-vault/search-akv.png)

Selecteer de ingerichte Azure Key Vault waarin uw referenties worden opgeslagen. U kunt doen **testverbinding** om te controleren of uw AKV verbinding is geldig. 

![Configureren van Azure Sleutelkluis](media/store-credentials-in-key-vault/configure-akv.png)

**JSON-voorbeeld:**

```json
{
    "name": "AzureKeyVaultLinkedService",
    "properties": {
        "type": "AzureKeyVault",
        "typeProperties": {
            "baseUrl": "https://<azureKeyVaultName>.vault.azure.net"
        }
    }
}
```

## <a name="reference-secret-stored-in-key-vault"></a>Naslaginformatie over geheim is opgeslagen in key vault

De volgende eigenschappen worden ondersteund wanneer u een veld in de gekoppelde service verwijst naar een key vault-geheim configureren:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van het veld moet worden ingesteld op: **AzureKeyVaultSecret**. | Ja |
| secretName | De naam van het geheim in azure key vault. | Ja |
| secretVersion | De versie van het geheim in azure key vault.<br/>Indien niet opgegeven, gebruikt deze altijd de nieuwste versie van de geheime sleutel.<br/>Als u opgeeft, klikt u vervolgens vastprikken het naar de opgegeven versie.| Nee |
| Store | Verwijst naar een Azure Key Vault gekoppelde service die u gebruikt voor het opslaan van de referentie. | Ja |

**Met behulp van gebruikersinterface ontwerpen:**

Selecteer **Azure Key Vault** voor geheime velden tijdens het maken van de verbinding met uw gegevens gegevensarchief/rekenservices. Selecteer de ingerichte Azure Key Vault gekoppelde Service en geef de **geheime naam**. U kunt eventueel ook een geheime versie opgeven. 

![AKV geheim configureren](media/store-credentials-in-key-vault/configure-akv-secret.png)

**JSON-voorbeeld: (Zie de sectie 'wachtwoord')**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "typeProperties": {
            "deploymentType": "<>",
            "organizationName": "<>",
            "authenticationType": "<>",
            "username": "<>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
