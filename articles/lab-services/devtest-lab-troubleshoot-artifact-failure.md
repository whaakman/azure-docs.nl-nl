---
title: Met artefacten vaststellen in een virtuele machine van Azure DevTest Labs | Microsoft Docs
description: Informatie over het oplossen van fouten in Azure DevTest Labs artefact.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 115e0086-3293-4adf-8738-9f639f31f918
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: d5e03b7645fcacaeba4ccf147d48886cefd33ffb
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57531438"
---
# <a name="diagnose-artifact-failures-in-the-lab"></a>Met artefacten vaststellen in de testomgeving 
Nadat u een artefact hebt gemaakt, kunt u controleren om te zien of deze is geslaagd of mislukt. Artefact Logboeken in Azure DevTest Labs bevatten informatie die u gebruiken kunt om een artefact fout vast te stellen. U hebt een aantal opties voor het weergeven van het artefact-logboekgegevens voor een Windows-VM:

* In de Azure Portal
* In de virtuele machine

> [!NOTE]
> Om ervoor te zorgen dat de fouten correct worden geïdentificeerd en uitgelegd, is het belangrijk dat het artefact de juiste structuur heeft. Zie voor meer informatie over het maken van een artefact correct [maken van aangepaste artefacten](devtest-lab-artifact-author.md). Bekijk een voorbeeld van een goed gestructureerd artefact vindt de [testen parametertypen](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes) artefact.

## <a name="troubleshoot-artifact-failures-by-using-the-azure-portal"></a>Problemen met artefacten met behulp van Azure portal

1. Selecteer uw lab in Azure portal, in de lijst met resources.
2. Kies de Windows-VM met de artefacten die u wilt onderzoeken.
3. In het linkerdeelvenster onder **algemene**, selecteer **artefacten**. Er wordt een lijst weergegeven van de artefacten die zijn gekoppeld aan die virtuele machine. De naam van het artefact en de status van de artefacten worden aangegeven.

   ![Status van het artefact](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifacts-failure.png)

4. Selecteer een artefact waarin een **mislukt** status. Hiermee opent u het artefact. Een extensie-bericht met meer informatie over de fout van het artefact weergegeven.

   ![Foutbericht artefact](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error.png)


## <a name="troubleshoot-artifact-failures-from-within-the-virtual-machine"></a>Problemen oplossen met mislukte artefact uit vanuit de virtuele machine

1. Meld u aan de virtuele machine met het artefact dat u wilt onderzoeken.
2. Ga naar C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\\*1.9*\Status, waar *1.9* is het versienummer van de aangepaste Scriptextensie van Azure.

   ![De Status van bestand](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error-vm-status.png)

3. Open de **status** bestand.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Gerelateerde blogberichten
* [Een VM toevoegen aan een bestaand Active Directory-domein met behulp van Resource Manager-sjabloon in DevTest Labs](https://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [een Git-opslagplaats toevoegen aan een lab](devtest-lab-add-artifact-repo.md).

