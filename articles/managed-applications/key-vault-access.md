---
title: Azure Key Vault gebruiken met beheerde toepassingen | Microsoft Docs
description: Laat zien hoe u toegang tot geheimen in Azure Key Vault gebruiken bij het implementeren van beheerde toepassingen
services: managed-applications
author: tfitzmac
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 01/30/2019
ms.author: tomfitz
ms.openlocfilehash: 55410250ccd4dfceac8ac9ae5b81d4736de0d91a
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55492671"
---
# <a name="access-key-vault-secret-when-deploying-azure-managed-applications"></a>Toegang tot Key Vault-geheim bij het implementeren van Azure Managed Applications

Als u een veilige waarde (zoals een wachtwoord) als een parameter doorgeven tijdens de implementatie wilt, haalt u de waarde van een [Azure Key Vault](../key-vault/key-vault-whatis.md). Voor toegang tot de Key Vault bij het implementeren van beheerde toepassingen, u moet toegang verlenen tot de **toestel Resource Provider** service-principal. De beheerde toepassingen-service gebruikt deze identiteit voor bewerkingen uitvoeren. Als u wilt ophalen is een waarde van een Key Vault tijdens de implementatie, moet de service-principal toegang krijgen tot de Key Vault.

In dit artikel wordt beschreven hoe het configureren van de Key Vault om te werken met beheerde toepassingen.

## <a name="enable-template-deployment"></a>Voor sjabloonimplementatie inschakelen

1. Selecteer in de portal voor uw Key Vault.

1. Selecteer **Toegangsbeleid**.   

   ![Toegangsbeleid selecteren](./media/key-vault-access/select-access-policies.png)

1. Selecteer **Klik hierop om geavanceerde beleidsregels weer te geven**.

   ![Geavanceerde beleidsregels weergeven](./media/key-vault-access/advanced.png)

1. Selecteer **inschakelen van toegang tot Azure Resource Manager voor sjabloonimplementatie**. Selecteer vervolgens **Opslaan**.

   ![Voor sjabloonimplementatie inschakelen](./media/key-vault-access/enable-template.png)

## <a name="add-service-as-contributor"></a>Service toevoegen als Inzender

1. Selecteer **toegangsbeheer (IAM)**.

   ![Selecteer toegangsbeheer](./media/key-vault-access/access-control.png)

1. Selecteer **roltoewijzing toevoegen**.

   ![Selecteer toevoegen](./media/key-vault-access/add-access-control.png)

1. Selecteer **Inzender** voor de rol. Zoeken naar **toestel Resource Provider** en selecteert u deze uit de beschikbare opties.

   ![Zoeken naar provider](./media/key-vault-access/search-provider.png)

1. Selecteer **Opslaan**.

## <a name="reference-key-vault-secret"></a>Naslaginformatie over Key Vault-geheim

Een geheim vanuit een Key Vault doorgeven aan een sjabloon in uw toepassing worden beheerd, moet u een [gekoppelde sjabloon](../azure-resource-manager/resource-group-linked-templates.md) en verwijzen naar de Key Vault in de parameters voor de gekoppelde sjabloon. Geef de resource-ID van de Key Vault en de naam van het geheim.

```json
"resources": [{
  "apiVersion": "2015-01-01",
  "name": "linkedTemplate",
  "type": "Microsoft.Resources/deployments",
  "properties": {
    "mode": "incremental",
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver.json",
      "contentVersion": "1.0.0.0"
    },
    "parameters": {
      "adminPassword": {
        "reference": {
          "keyVault": {
            "id": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.KeyVault/vaults/<key-vault-name>"
          },
          "secretName": "<secret-name>"
        }
      },
      "adminLogin": { "value": "[parameters('adminLogin')]" },
      "sqlServerName": {"value": "[parameters('sqlServerName')]"}
    }
  }
}],
```

## <a name="next-steps"></a>Volgende stappen

U kunt uw Key Vault, zodat ze toegankelijk zijn tijdens de implementatie van een beheerde toepassing hebt geconfigureerd.

* Zie voor meer informatie over het doorgeven van een waarde uit een Key Vault als een sjabloonparameter [Azure Key Vault gebruikt om door te geven beveiligde parameterwaarde tijdens de implementatie van](../azure-resource-manager/resource-manager-keyvault-parameter.md).
* Zie voor voorbeelden van beheerde toepassing [voorbeeldprojecten voor Azure beheerde toepassingen](sample-projects.md).
* Zie [Aan de slag met CreateUiDefinition](create-uidefinition-overview.md) voor meer informatie over het maken van een UI-definitiebestand voor een beheerde toepassing.