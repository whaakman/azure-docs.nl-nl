---
title: Store geheimen in een key vault in Azure DevTest Labs | Microsoft Docs
description: Leer hoe u aan de geheimen in een Azure Key Vault opslaan en gebruiken tijdens het maken van een virtuele machine, een formule, of een omgeving.
services: devtest-lab
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2018
ms.author: spelluru
ms.openlocfilehash: d87c8a46459a9b4bf80bef895ec97e436d38e699
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39186829"
---
# <a name="store-secrets-in-a-key-vault-in-azure-devtest-labs"></a>Store-geheimen in een key vault in Azure DevTest Labs
Mogelijk moet u een complexe geheim invoeren bij het gebruik van Azure DevTest Labs: wachtwoord voor uw Windows-VM, openbare SSH-sleutel voor uw Linux-VM of persoonlijk toegangstoken voor het klonen van de Git-repo via een artefact. Geheimen zijn gewoonlijk lang en willekeurige tekens bevatten. Ze voeren kan dus lastig en onhandig, met name als u meerdere keren dezelfde geheim gebruiken.

Als u wilt dit probleem is opgelost en houd er ook uw geheimen in een veilige plaats, DevTest Labs biedt ondersteuning voor opslag geheimen in een [Azure sleutelkluis](../key-vault/key-vault-overview.md). Wanneer een gebruiker wordt een geheim voor de eerste keer opgeslagen, maakt de DevTest Labs-service automatisch een key vault in dezelfde resourcegroep die het lab bevat en het geheim opslaat in de key vault. DevTest Labs Hiermee maakt u een afzonderlijke key vault voor elke gebruiker. 

## <a name="save-a-secret-in-azure-key-vault"></a>Een geheim opslaan in Azure Key Vault
Als u wilt uw geheim opslaan in Azure Key Vault, moet u de volgende stappen uitvoeren:

1. Selecteer **Moje tajné kódy** in het menu links.
2. Voer een **naam** voor de geheime sleutel. U ziet deze naam in de vervolgkeuzelijst bij het maken van een VM, formule, of in een omgeving. 
3. Voer de geheime sleutel als de **waarde**.

    ![Store-geheim](media/devtest-lab-store-secrets-in-key-vault/store-secret.png)

## <a name="use-a-secret-from-azure-key-vault"></a>Een geheim uit Azure Key Vault gebruiken
Wanneer u een geheim te maken van een VM, formule, of een omgeving in te voeren, kunt u een geheim handmatig invoeren of Selecteer een geheim opgeslagen in de key vault. Voor het gebruik van een geheim opgeslagen in uw key vault, moet u de volgende acties uitvoeren:

1. Selecteer **een geheim opgeslagen**. 
2. Selecteer uw geheim in de vervolgkeuzelijst voor **een geheim kiezen**. 

    ![Geheim in VM gebruiken](media/devtest-lab-store-secrets-in-key-vault/secret-store-pick-a-secret.png)

## <a name="use-a-secret-in-an-azure-resource-manager-template"></a>Een geheim in een Azure Resource Manager-sjabloon gebruiken
U kunt de naam van uw geheim opgeven in een Azure Resource Manager-sjabloon die wordt gebruikt voor het maken van een virtuele machine, zoals wordt weergegeven in het volgende voorbeeld:

![Geheim in formule- of -omgeving gebruiken](media/devtest-lab-store-secrets-in-key-vault/secret-store-arm-template.png)

## <a name="next-steps"></a>Volgende stappen

- [Een virtuele machine met behulp van het geheim maken](devtest-lab-add-vm.md) 
- [Een formule met behulp van het geheim maken](devtest-lab-manage-formulas.md)
- [Een omgeving met behulp van het geheim maken](devtest-lab-create-environment-from-arm.md)
