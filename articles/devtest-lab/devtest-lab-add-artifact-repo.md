---
title: Voeg een Git-opslagplaats toe aan een lab in Azure DevTest Labs | Microsoft Docs
description: Informatie over het toevoegen van een GitHub of Visual Studio Team Services Git-opslagplaats voor uw aangepaste artefacten-bron in Azure DevTest Labs.
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 01b459f7-eaf2-45a8-b4b5-2c0a821b33c8
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: v-craic
ms.openlocfilehash: 345d095762b75b7a177bf4c6e7a07360a9c14068
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="add-a-git-repository-to-store-custom-artifacts-and-resource-manager-templates"></a>Voeg een Git-opslagplaats voor het opslaan van aangepaste artefacten en Resource Manager-sjablonen

U kunt [aangepaste artefacten maken](devtest-lab-artifact-author.md) voor de virtuele machines in uw lab of [Azure Resource Manager-sjablonen gebruiken voor het maken van een aangepaste testomgeving](devtest-lab-create-environment-from-arm.md). U moet een persoonlijke Git-opslagplaats voor de artefacten of Resource Manager-sjablonen die worden gemaakt met uw team toevoegen. De opslagplaats kan worden gehost op [GitHub](https://github.com) of op [Visual Studio Team Services](https://visualstudio.com).

Wij bieden een [GitHub-opslagplaats van artefacten](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) die u kunt implementeren als-is, of u kunt deze aanpassen voor uw labs. Wanneer u een artefact maken of aanpassen, kunt u het artefact in de openbare opslagplaats niet opslaan. U kunt uw eigen persoonlijke opslagplaats voor aangepaste artefacten en artefacten die u maakt moet maken. 

Wanneer u een virtuele machine maakt, kunt u de Resource Manager-sjabloon opslaan, aanpassen als u wilt en later gebruiken meer virtuele machines maken. U moet uw eigen persoonlijke opslagplaats voor het opslaan van uw aangepaste Resource Manager-sjablonen maken.  

* Zie voor meer informatie over het maken van een GitHub-opslagplaats, [GitHub Bootcamp](https://help.github.com/categories/bootcamp/).
* Zie voor meer informatie over het maken van een Team Services-project met een Git-opslagplaats, [verbinding maken met Visual Studio Team Services](https://www.visualstudio.com/get-started/setup/connect-to-visual-studio-online).

De volgende afbeelding is een voorbeeld van hoe een opslagplaats met artefacten in GitHub eruit:  

![Voorbeeld GitHub-repo-artefacten](./media/devtest-lab-add-repo/devtestlab-github-artifact-repo-home.png)

## <a name="get-the-repository-information-and-credentials"></a>De informatie van de opslagplaats en referenties ophalen
Als een opslagplaats toevoegen aan uw testomgeving, moet u eerst, belangrijke informatie ophalen van uw opslagplaats. De volgende secties wordt beschreven hoe gegevens worden opgehaald vereist voor opslagplaatsen die worden gehost op GitHub of Visual Studio Team Services.

### <a name="get-the-github-repository-clone-url-and-personal-access-token"></a>De GitHub-opslagplaats kloon-URL en personal access token ophalen

1. Ga naar de introductiepagina van de GitHub-opslagplaats met het artefact of Sjabloondefinities van Resource Manager.
2. Selecteer **klonen of downloaden**.
3. Als de URL naar het Klembord kopiëren, selecteert de **url voor HTTPS-kloon** knop. De URL voor later gebruik opslaan.
4. Selecteer de installatiekopie van het profiel in de rechterbovenhoek van GitHub, en selecteer vervolgens **instellingen**.
5. In de **persoonlijke instellingen** menu aan de linkerkant, selecteer **persoonlijke toegangstokens**.
6. Selecteer **nieuw token genereren**.
7. Op de **nieuwe persoonlijke toegangstoken** pagina onder **Token beschrijving**, voer een beschrijving. Accepteer de standaardwaarde items onder **Selecteer scopes**, en selecteer vervolgens **Token genereren**.
8. Sla het gegenereerde token. Het token gaat u later gebruiken.
9. GitHub te sluiten.   
10. Blijven de [verbinding maken met uw lab naar de opslagplaats](#connect-your-lab-to-the-repository) sectie.

### <a name="get-the-visual-studio-team-services-repository-clone-url-and-personal-access-token"></a>De Visual Studio Team Services opslagplaats kloon-URL en personal access token ophalen

1. Ga naar de startpagina van de verzameling van uw team (bijvoorbeeld https://contoso-web-team.visualstudio.com) en selecteer vervolgens uw project.
2. Selecteer op de startpagina project **Code**.
3. De kloon-URL op het project weergeven **Code** pagina **kloon**.
4. De URL niet opslaan. De URL gaat u later gebruiken.
5. Voor het maken van een persoonlijk toegangstoken in de vervolgkeuzelijst in het gebruikersaccountmenu selecteert **Mijn profiel**.
6. Selecteer op de pagina van de informatie profiel **beveiliging**.
7. Op de **beveiliging** tabblad **toevoegen**.
8. Op de **maken van een persoonlijk toegangstoken** pagina:
   1. Voer een **beschrijving** voor het token.
   2. In de **verloopt In** selecteert **180 dagen**.
   3. In de **Accounts** selecteert **alle toegankelijke accounts**.
   4. Selecteer de **alle scopes** optie.
   5. Selecteer **Token aanmaken**.
9. Het nieuwe token wordt weergegeven in de **persoonlijke toegangstokens** lijst. Selecteer **kopie Token**, en sla de token waarde voor later gebruik.
10. Blijven de [verbinding maken met uw lab naar de opslagplaats](#connect-your-lab-to-the-repository) sectie.

## <a name="connect-your-lab-to-the-repository"></a>Verbinding maken met uw lab naar de opslagplaats
1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecteer **meer Services**, en selecteer vervolgens **DevTest Labs** uit de lijst met services.
3. Selecteer in de lijst van labs uw testomgeving. 
4. Selecteer **configuratie en het beleid** > **opslagplaatsen** > **+ toevoegen**.

    ![De knop van de opslagplaats toevoegen](./media/devtest-lab-add-repo/devtestlab-add-repo.png)
5. Op de tweede **opslagplaatsen** pagina, geeft u de volgende informatie:
  1. **Naam**. Voer een naam voor de opslagplaats.
  2. **GIT kloon-Url**. Voer de Git-HTTPS-kloon-URL die u eerder hebt gekopieerd in GitHub of Visual Studio Team Services.
  3. **Vertakking**. Als u uw definities, voer de vertakking.
  4. **Persoonlijke toegangstoken**. Voer in het persoonlijke toegangstoken die u eerder hebt verkregen in GitHub of Visual Studio Team Services.
  5. **Paden voor mappen**. Geef ten minste één mappad ten opzichte van de kloon-URL die uw artefacten of Sjabloondefinities van Resource Manager-bevat. Wanneer u een submap opgeeft, moet dat u de schuine streep opnemen in het mappad.

     ![Opslagplaatsen gebied](./media/devtest-lab-add-repo/devtestlab-repo-blade.png)
6. Selecteer **Opslaan**.

### <a name="related-blog-posts"></a>Verwante blogberichten
* [Problemen oplossen mislukt artefacten in DevTest Labs](devtest-lab-troubleshoot-artifact-failure.md)
* [Een virtuele machine toevoegen aan een bestaand Active Directory-domein met behulp van een Resource Manager-sjabloon in DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
Nadat u uw persoonlijke Git-opslagplaats hebt gemaakt, kunt u een of beide van de volgende kunt doen, afhankelijk van uw behoeften:
* Store uw [aangepaste artefacten](devtest-lab-artifact-author.md). U kunt ze later gebruiken voor het maken van nieuwe virtuele machines.
* [Maken van meerdere VM-omgevingen en PaaS-resources met behulp van Resource Manager-sjablonen](devtest-lab-create-environment-from-arm.md). Vervolgens kunt u de sjablonen in de opslagplaats van uw persoonlijke opslaan.

Wanneer u een virtuele machine maakt, kunt u controleren dat de artefacten of sjablonen worden toegevoegd aan de Git-opslagplaats. Ze zijn onmiddellijk beschikbaar in de lijst met artefacten of sjablonen. De naam van uw persoonlijke opslagplaats wordt weergegeven in de kolom waarmee de gegevensbron. 
