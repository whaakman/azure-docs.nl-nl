---
title: Artefacten mislukte diagnoses in een virtuele machine in Azure DevTest Labs | Microsoft Docs
description: Informatie over het oplossen van artefacten fouten in Azure DevTest Labs.
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 115e0086-3293-4adf-8738-9f639f31f918
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: v-craic
ms.openlocfilehash: 6c03ce8f91ec688e32e379f1284767db9a45920c
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="diagnose-artifact-failures-in-the-lab"></a>Artefacten mislukte diagnoses in het lab 
Nadat u een artefact hebt gemaakt, kunt u controleren om te zien of deze is geslaagd of mislukt. Artefacten Logboeken in Azure DevTest Labs bevatten informatie die u gebruiken kunt voor het vaststellen van een artefact-fout. U hebt een aantal opties voor het weergeven van de artefacten logboekgegevens voor een virtuele machine van Windows:

* In de Azure portal
* In de virtuele machine

> [!NOTE]
> Om ervoor te zorgen dat fouten correct worden geïdentificeerd en uitgelegd, is het belangrijk dat het artefact de juiste structuur heeft. Zie voor meer informatie over het correct maken een artefact [aangepaste artefacten maken](devtest-lab-artifact-author.md). Bekijk een voorbeeld van een goed gestructureerd artefact vindt de [testen parametertypen](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes) artefacten.

## <a name="troubleshoot-artifact-failures-by-using-the-azure-portal"></a>Problemen met artefacten met behulp van de Azure-portal

1. Selecteer in de Azure-portal in de lijst met bronnen, uw testomgeving.
2. Kies de virtuele machine van Windows met de artefacten die u wilt onderzoeken.
3. In het linkerdeelvenster onder **algemene**, selecteer **artefacten**. Een lijst van artefacten die zijn gekoppeld aan die VM wordt weergegeven. De naam van het artefact en de status van de artefacten zijn vermeld.

   ![Status van artefacten](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifacts-failure.png)

4. Selecteer een artefact waarin een **mislukt** status. Hiermee opent u het artefact. Een extensie-bericht met meer informatie over de fout van het artefact wordt weergegeven.

   ![Foutbericht artefact](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error.png)


## <a name="troubleshoot-artifact-failures-from-within-the-virtual-machine"></a>Problemen met artefacten uit vanuit de virtuele machine

1. Meld u aan de virtuele machine met het artefact die u wilt onderzoeken.
2. Ga naar C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\\*1,9*\Status, waarbij *1,9* is het versienummer van de aangepaste Scriptextensie Azure.

   ![Het statusbestand](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error-vm-status.png)

3. Open de **status** bestand.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Verwante blogberichten
* [Een virtuele machine toevoegen aan een bestaand Active Directory-domein met behulp van een Resource Manager-sjabloon in DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [een Git-opslagplaats toevoegen aan een lab](devtest-lab-add-artifact-repo.md).

