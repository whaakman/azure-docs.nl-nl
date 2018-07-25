---
title: Databricks CLI van Azure Cloudshell gebruiken | Microsoft Docs
description: Informatie over het gebruik van de Databricks CLI van Azure Cloud Shell.
services: azure-databricks
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: 3ea4ebbd95237b50054fb0e344f260120d597ab5
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225231"
---
# <a name="use-databricks-cli-from-azure-cloud-shell"></a>Databricks CLI van Azure Cloud Shell gebruiken

Informatie over het gebruik van de Databricks CLI van Azure Cloud Shell bewerkingen op Databricks uit te voeren.

## <a name="prerequisites"></a>Vereisten

* Een Azure Databricks-werkruimte en het cluster. Zie voor instructies [aan de slag met Azure Databricks](quickstart-create-databricks-workspace-portal.md). 

* Instellen van een persoonlijk toegangstoken in Databricks. Zie voor instructies [management Token](https://docs.azuredatabricks.net/api/latest/authentication.html#token-management).

## <a name="use-the-azure-cloud-shell"></a>De Azure Cloudshell gebruiken

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
 
2. Klik in de rechterbovenhoek op het **Cloud Shell** pictogram.

   ![Open Cloudshell](./media/databricks-cli-from-azure-cloud-shell/launch-azure-cloud-shell.png "Azure Cloudshell starten")

3. Zorg ervoor dat u selecteert **Bash** voor de Cloud Shell-omgeving. U kunt kiezen uit de vervolgkeuzemenu-optie, zoals wordt weergegeven in de volgende schermafbeelding.

   ![Selecteer Bash voor de Cloud Shell-omgeving](./media/databricks-cli-from-azure-cloud-shell/select-bash-for-shell.png "Selecteer Bash") 

4. Maak een virtuele omgeving waarin u de Databricks CLI kunt installeren. In het onderstaande codefragment maakt u een virtuele omgeving met de naam `databrickscli`.

       virtualenv -p /usr/bin/python2.7 databrickscli

5. Schakel over naar de virtuele omgeving die u hebt gemaakt.

       source databrickscli/bin/activate

6. De Databricks-CLI installeren.

       pip install databricks-cli

7. Instellen van verificatie met Databricks met behulp van het toegangstoken dat u hebt gemaakt, als onderdeel van de vereisten vermeld. Gebruik de volgende opdracht:

       databricks configure --token

    U ontvangt de volgende stappen:

    * U wordt eerst gevraagd om in te voeren van de Databricks-host. Voer de waarde in de indeling `https://eastus2.azuredatabricks.net`. Hier **VS-Oost 2** is de Azure-regio waar u uw Azure Databricks-werkruimte hebt gemaakt.

    * U wordt vervolgens gevraagd om in te voeren van een token. Voer het token dat u eerder hebt gemaakt.

Nadat u deze stappen hebt voltooid, kunt u beginnen met Databricks CLI van Azure Cloud Shell.

## <a name="use-databricks-cli"></a>Databricks CLI gebruiken

U kunt nu starten met behulp van de Databricks-CLI. Bijvoorbeeld, voer de volgende opdracht om alle Databricks-clusters die u in uw werkruimte hebt weer te geven.

    databricks clusters list

U kunt ook de volgende opdracht gebruiken voor toegang tot het Databricks filesystem (DBFS).

    databricks fs ls


Zie voor een volledig overzicht van opdrachten, [Databricks CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html).


## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over Azure CLI, [overzicht van Azure CLI](../cloud-shell/overview.md)
* Zie voor een lijst van opdrachten voor Azure CLI [Azure CLI-verwijzing](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest)
* Zie voor een lijst van opdrachten voor Databricks CLI [Databricks CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html)


