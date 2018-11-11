---
title: Een Git-opslagplaats toevoegen aan een lab in Azure DevTest Labs | Microsoft Docs
description: Informatie over het toevoegen van een GitHub- of Azure DevOps Services Git-opslagplaats voor de bron van uw aangepaste artefacten in Azure DevTest Labs.
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: 01b459f7-eaf2-45a8-b4b5-2c0a821b33c8
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: c7b9921d7eacb9b40e39f8e68d13357ce6bcfd78
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51241545"
---
# <a name="add-a-git-repository-to-store-custom-artifacts-and-resource-manager-templates"></a>Een Git-opslagplaats voor het opslaan van aangepaste artefacten en Resource Manager-sjablonen toevoegen

U kunt [maken van aangepaste artefacten](devtest-lab-artifact-author.md) voor de virtuele machines in uw testomgeving of [Azure Resource Manager-sjablonen gebruiken om te maken van een aangepaste testomgeving](devtest-lab-create-environment-from-arm.md). U moet een persoonlijke Git-opslagplaats voor de artefacten of Resource Manager-sjablonen die uw team maakt toevoegen. De opslagplaats kan worden gehost op [GitHub](https://github.com) of op [Azure DevOps Services](https://visualstudio.com).

We bieden een [GitHub-opslagplaats voor artefacten](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) die u kunt implementeren als-is, of u kunt deze aanpassen voor uw labs. Wanneer u aanpassen of een artefact maken, kunt u het artefact niet opslaan in de openbare opslagplaats. U kunt uw eigen privé-opslagplaats voor aangepaste artefacten en artefacten die u maakt moet maken. 

Wanneer u een virtuele machine maakt, kunt u de Resource Manager-sjabloon hebt opgeslagen, aanpassen als u wilt, en vervolgens later gebruiken om meer virtuele machines. U moet uw eigen privé-opslagplaats voor het opslaan van uw aangepaste Resource Manager-sjablonen maken.  

* Zie voor meer informatie over het maken van een GitHub-opslagplaats, [GitHub Bootcamp](https://help.github.com/categories/bootcamp/).
* Zie voor meer informatie over het maken van een Services van Azure DevOps-project met een Git-opslagplaats, [verbinding maken met Azure DevOps Services](https://www.visualstudio.com/get-started/setup/connect-to-visual-studio-online).

De volgende afbeelding is een voorbeeld van hoe een opslagplaats met artefacten in GitHub kunt zien:  

![Voorbeeld van een GitHub-opslagplaats voor artefacten](./media/devtest-lab-add-repo/devtestlab-github-artifact-repo-home.png)

## <a name="get-the-repository-information-and-credentials"></a>Verkrijg de informatie van de opslagplaats en de referenties
Een opslagplaats toevoegen aan uw testomgeving, moet u eerst, belangrijke informatie ophalen uit uw opslagplaats. In de volgende secties wordt beschreven hoe u aan de vereiste gegevens voor opslagplaatsen die worden gehost op GitHub of Azure DevOps-Services.

### <a name="get-the-github-repository-clone-url-and-personal-access-token"></a>De GitHub-opslagplaats kloon-URL en Pat-token ophalen

1. Ga naar de startpagina van de GitHub-opslagplaats die het artefact of Sjabloondefinities van de Resource Manager-bevat.
2. Selecteer **Klonen of downloaden**.
3. Als de URL naar het Klembord kopiëren, selecteert de **HTTPS kloon-url** knop. De URL voor later gebruik opslaan.
4. Selecteer de installatiekopie van het profiel in de rechterbovenhoek van GitHub, en selecteer vervolgens **instellingen**.
5. In de **persoonlijke instellingen** menu aan de linkerkant, selecteer **persoonlijke toegangstokens**.
6. Selecteer **nieuw token genereren**.
7. Op de **nieuwe persoonlijke toegangstoken** pagina onder **beschrijving Token**, voer een beschrijving in. Accepteer de standaarditems onder **bereiken selecteren**, en selecteer vervolgens **Token genereren**.
8. Sla het gegenereerde token. U gebruikt het token later.
9. Sluit GitHub.   
10. Blijven de [verbinding maken met uw lab in de opslagplaats](#connect-your-lab-to-the-repository) sectie.

### <a name="get-the-azure-repos-clone-url-and-personal-access-token"></a>De Azure-opslagplaatsen-kloon-URL en persoonlijk toegangstoken ophalen

1. Ga naar de startpagina van uw team-verzameling (bijvoorbeeld https://contoso-web-team.visualstudio.com), en selecteer vervolgens uw project.
2. Selecteer op de startpagina van project **Code**.
3. Om weer te geven van de kloon-URL op het project **Code** weergeeft, schakelt **kloon**.
4. Sla de URL op. Gebruikt u later de URL.
5. Voor het maken van een persoonlijk toegangstoken in de vervolgkeuzelijst in het gebruikersaccountmenu selecteert **Mijn profiel**.
6. Selecteer op de pagina van de informatie profiel **Security**.
7. Op de **Security** tabblad **toevoegen**.
8. Op de **maken van een persoonlijk toegangstoken** pagina:
   1. Voer een **beschrijving** voor het token.
   2. In de **verloopt In** in de lijst met **180 dagen**.
   3. In de **Accounts** in de lijst met **alle toegankelijke accounts**.
   4. Selecteer de **alle scopes** optie.
   5. Selecteer **maakt u Token**.
9. Het nieuwe token wordt weergegeven in de **persoonlijke toegangstokens** lijst. Selecteer **kopie Token**, en sla vervolgens de waarde voor de token voor later gebruik.
10. Blijven de [verbinding maken met uw lab in de opslagplaats](#connect-your-lab-to-the-repository) sectie.

## <a name="connect-your-lab-to-the-repository"></a>Verbinding maken met uw lab in de opslagplaats
1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecteer **meer Services**, en selecteer vervolgens **DevTest Labs** uit de lijst met services.
3. Selecteer in de lijst met labs, uw lab. 
4. Selecteer **configuratie en het beleid** > **opslagplaatsen** > **+ toevoegen**.

    ![De knop van de opslagplaats toevoegen](./media/devtest-lab-add-repo/devtestlab-add-repo.png)
5. In het tweede **opslagplaatsen** pagina, geeft u de volgende informatie:
  1. **Naam**. Voer een naam voor de opslagplaats.
  2. **GIT kloon-Url**. Voer de Git-HTTPS-kloon-URL die u eerder hebt gekopieerd vanuit GitHub of Azure DevOps-Services.
  3. **Vertakking**. Als u uw definities, voer de vertakking.
  4. **Persoonlijk toegangstoken**. Voer in het persoonlijke toegangstoken dat u eerder hebt ontvangen van GitHub of Azure DevOps-Services.
  5. **Mappaden**. Geef ten minste één pad ten opzichte van de kloon-URL die uw artefacten of Sjabloondefinities van de Resource Manager-bevat. Wanneer u een submap opgeeft, zorg ervoor dat u de schuine streep in het mappad.

     ![Opslagplaatsen gebied](./media/devtest-lab-add-repo/devtestlab-repo-blade.png)
6. Selecteer **Opslaan**.

### <a name="related-blog-posts"></a>Gerelateerde blogberichten
* [Problemen oplossen mislukt artefacten in DevTest Labs](devtest-lab-troubleshoot-artifact-failure.md)
* [Een VM toevoegen aan een bestaand Active Directory-domein met behulp van Resource Manager-sjabloon in DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
Nadat u uw persoonlijke Git-opslagplaats hebt gemaakt, kunt u een of beide van de volgende kunt doen, afhankelijk van uw behoeften:
* Store uw [aangepaste artefacten](devtest-lab-artifact-author.md). U kunt ze later aan nieuwe virtuele machines maken.
* [Multi-VM-omgevingen en PaaS-resources maken met Resource Manager-sjablonen](devtest-lab-create-environment-from-arm.md). Vervolgens kunt u de sjablonen opslaan in uw privé-opslagplaats.

Wanneer u een virtuele machine maakt, kunt u controleren dat de artefacten of sjablonen worden toegevoegd aan de Git-opslagplaats. Ze zijn onmiddellijk beschikbaar in de lijst van artefacten of sjablonen. De naam van uw persoonlijke opslagplaats wordt weergegeven in de kolom die u de bron geeft. 
