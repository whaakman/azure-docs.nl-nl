---
title: Databricks CLI van Azure-Cloud-Shell gebruiken | Microsoft Docs
description: Informatie over het gebruik van de CLI Databricks vanuit Azure Cloud-Shell.
services: azure-databricks
documentationcenter: 
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2018
ms.author: nitinme
ms.openlocfilehash: 212789e8189abf362bb8eaf12e4c551b113b7adb
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2018
---
# <a name="use-databricks-cli-from-azure-cloud-shell"></a>Databricks CLI van Azure-Cloud-Shell gebruiken

Informatie over het gebruik van de CLI Databricks vanuit Azure Cloud-Shell op bewerkingen uitvoeren op Databricks.

## <a name="prerequisites"></a>Vereisten

* Een Azure-Databricks werkruimte en de cluster. Zie voor instructies [aan de slag met Azure Databricks](quickstart-create-databricks-workspace-portal.md). 

* Een persoonlijk toegangstoken in Databricks instellen. Zie voor instructies [management Token](https://docs.azuredatabricks.net/api/latest/authentication.html#token-management).

## <a name="use-the-azure-cloud-shell"></a>De Azure-Cloud-Shell gebruiken

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
 
2. Klik in de rechterbovenhoek op het **Cloud Shell** pictogram.

   ![Cloud-Shell starten](./media/databricks-cli-from-azure-cloud-shell/launch-azure-cloud-shell.png "ODBC vanuit Excel starten")

3. Zorg ervoor dat u selecteert **Bash** voor de Cloud Shell-omgeving. U kunt selecteren uit de vervolgkeuzelijst optie, zoals wordt weergegeven in de volgende schermafbeelding.

   ![Cloud-Shell starten](./media/databricks-cli-from-azure-cloud-shell/select-bash-for-shell.png "ODBC vanuit Excel starten") 

4. Maak een virtuele omgeving waarin u de CLI Databtricks kunt installeren. In het onderstaande codefragment maakt u een virtuele omgeving aangeroepen `databrickscli`.

       virtualenv -p /usr/bin/python2.7 databrickscli

5. Overschakelen naar de virtuele omgeving die u hebt gemaakt.

       source databrickscli/bin/activate

6. Installeer de CLI Databricks.

       pip install databricks-cli

7. Instellen van Databricks-verificatie met behulp van het toegangstoken dat u hebt gemaakt, vermeld als deel van de vereisten. Gebruik de volgende opdracht:

       databricks configure --token

    U ontvangt de volgende vragen:

    * U wordt gevraagd de Databricks host invoeren. De waarde in de indeling Ente `https://eastus2.azuredatabricks.net`. Hier **VS-Oost 2** is de Azure-regio waar u uw Azure-Databricks-werkruimte gemaakt.

    * U wordt gevraagd een gebruikersnaam invoeren. Voer **token**.

    * Ten slotte wordt u gevraagd het wachtwoord invoeren. Voer in het token dat u eerder hebt gemaakt.

Wanneer u deze stappen hebt voltooid, kunt u beginnen met Databricks CLI van Azure Cloud-Shell.

## <a name="use-databricks-cli"></a>Databricks CLI gebruiken

U kunt nu starten met de Databricks CLI. Voer bijvoorbeeld de volgende opdracht voor een lijst met alle Databricks-clusters die u in uw werkruimte hebt.

    databricks clusters list

U kunt ook de volgende opdracht gebruiken voor toegang tot het bestandssysteem Databricks (DBFS).

    databricks fs ls


Zie voor een volledig overzicht van opdrachten, [Databricks CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html).


## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over Azure CLI, [overzicht van Azure CLI](../cloud-shell/overview.md)
* Zie voor een lijst met opdrachten voor Azure CLI [Azure CLI-verwijzing](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)
* Zie voor een lijst met opdrachten voor Databricks CLI [Databricks CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html)


