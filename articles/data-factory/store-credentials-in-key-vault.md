---
title: Referenties opslaan in Azure Sleutelkluis | Microsoft Docs
description: Informatie over het opslaan van referenties voor gegevensopslag gebruikt in een Azure sleutelkluis die Azure Data Factory automatisch tijdens runtime ophalen kunnen.
services: data-factory
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/25/2017
ms.author: jingwang
ms.openlocfilehash: e1be16ec6a7536cedf3a27ffacb9c4dffe42bbef
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37052412"
---
# <a name="store-credential-in-azure-key-vault"></a>Opslaan van referenties in Azure Sleutelkluis

Slaat u referenties voor gegevensopslag en computers in een [Azure Key Vault](../key-vault/key-vault-whatis.md). Azure Data Factory haalt de referenties bij het uitvoeren van een activiteit die gebruikmaakt van het gegevensarchief/rekenservices er gegevens.

Deze functie wordt op dit moment wordt ondersteund door alle activiteitstypen, met uitzondering van aangepaste activiteit. Voor de connectorconfiguratie van de Controleer met name de sectie 'gekoppelde service-eigenschappen' [elk onderwerp connector](copy-activity-overview.md#supported-data-stores-and-formats) voor meer informatie.

## <a name="prerequisites"></a>Vereisten

Deze functie is afhankelijk van de data factory-service-identiteit. Meer informatie over hoe het werkt van [Data factory-service-identiteit](data-factory-service-identity.md) en controleert u of uw gegevensfactory een gekoppeld.

>[!TIP]
>In Azure Sleutelkluis, bij het maken van een geheim **put (bijvoorbeeld verbinding wachtwoord-tekenreeks-service principal sleutel/enz.) om de volledige waarde van een geheime eigenschap dat ADF service gekoppelde vraagt**. Bijvoorbeeld: voor Azure Storage gekoppelde service plaatst `DefaultEndpointsProtocol=http;AccountName=myAccount;AccountKey=myKey;` plaatsen als Azure Sleutelkluis geheim en vervolgens een verwijzing in het veld 'connectionString' van de ADF; voor de gekoppelde service Dynamics, `myPassword` als Azure Sleutelkluis-geheim vervolgens verwijzen naar in het veld van ADF 'Clientservice voor'. Raadpleeg elk artikel connector/rekenservices er bij de eigenschapdetails van de ondersteunde.

## <a name="steps"></a>Stappen

Om te verwijzen naar een referentie die zijn opgeslagen in Azure Sleutelkluis, moet u:

1. **Ophalen van de data factory-service-identiteit** door de waarde van 'SERVICE identiteit TOEPASSINGS-ID gegenereerd samen met uw gegevensfactory. Als u ADF UI ontwerpen, wordt de identity-service-ID weergegeven in het venster Azure Key Vault gekoppelde service maken; u kunt het ook ophalen vanuit Azure-portal verwijzen naar [ophalen van de data factory-service-identiteit](data-factory-service-identity.md#retrieve-service-identity).
2. **De service-identiteit toegang verlenen aan uw Azure Sleutelkluis.** In de sleutelkluis -> toegang beleid -> toevoegen nieuw -> Zoek deze servicetoepassing identity-ID toe te kennen **ophalen** machtiging in de vervolgkeuzelijst geheime machtigingen. Hierdoor kan de aangewezen fabriek voor toegang tot geheim in de sleutelkluis.
3. **Maak een gekoppelde service verwijst naar uw Azure Sleutelkluis.** Raadpleeg [gekoppelde service van Azure Sleutelkluis](#azure-key-vault-linked-service).
4. **Gegevens store gekoppelde service, in welke verwijzing naar de bijbehorende opgeslagen in geheime sleutel-kluis maken.** Raadpleeg [verwijzing geheim sleutelkluis opgeslagen](#reference-secret-stored-in-key-vault).

## <a name="azure-key-vault-linked-service"></a>Azure Sleutelkluis gekoppelde service

De volgende eigenschappen worden ondersteund voor Azure Sleutelkluis gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **AzureKeyVault**. | Ja |
| baseUrl | Geef de URL van Azure Sleutelkluis. | Ja |

**Met behulp van gebruikersinterface ontwerpen:**

Klik op **verbindingen** -> **gekoppelde Services** -> **+ nieuw** -> Zoek naar 'Azure Key Vault':

![Azure Sleutelkluis zoeken](media/store-credentials-in-key-vault/search-akv.png)

Selecteer de ingerichte Azure Sleutelkluis waar uw referenties worden opgeslagen. U kunt doen **testverbinding** om te controleren of uw Azure Sleutelkluis verbinding is geldig. 

![Azure Sleutelkluis configureren](media/store-credentials-in-key-vault/configure-akv.png)

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

## <a name="reference-secret-stored-in-key-vault"></a>Verwijzing geheim sleutelkluis opgeslagen

De volgende eigenschappen worden ondersteund wanneer u een veld in de gekoppelde service verwijst naar een geheim sleutelkluis configureren:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van het veld moet worden ingesteld op: **AzureKeyVaultSecret**. | Ja |
| secretName | De naam van het geheim in azure sleutelkluis. | Ja |
| secretVersion | De versie van het geheim in azure sleutelkluis.<br/>Als niet wordt opgegeven, gebruikt deze altijd de nieuwste versie van het geheim.<br/>Indien opgegeven, klikt u vervolgens blijven deze hangen naar de opgegeven versie.| Nee |
| opslaan | Verwijst naar een Azure Key Vault gekoppelde service die u gebruikt voor het opslaan van de referentie. | Ja |

**Met behulp van gebruikersinterface ontwerpen:**

Selecteer **Azure Key Vault** voor geheime velden tijdens het maken van de verbinding met uw gegevensarchief/rekenservices er gegevens. Selecteer de ingerichte Azure Key Vault gekoppelde Service en geef de **geheime naam**. U kunt eventueel ook een geheime versie opgeven. 

![Azure Sleutelkluis geheim configureren](media/store-credentials-in-key-vault/configure-akv-secret.png)

**JSON-voorbeeld: (Zie de sectie 'password')**

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
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
