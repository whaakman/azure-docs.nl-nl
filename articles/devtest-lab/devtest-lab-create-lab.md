<properties
    pageTitle="Een lab maken in Azure DevTest Labs | Microsoft Azure"
    description="Een lab voor virtuele machines maken in Azure DevTest Labs"
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/12/2016"
    ms.author="tarcher"/>

# Een lab maken in Azure DevTest Labs

## Vereisten

Als u een lab wilt maken, hebt u het volgende nodig:

- Een Azure-abonnement. Zie [Azure aanschaffen](https://azure.microsoft.com/pricing/purchase-options/) of [Gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie over Azure-aankoopopties. U moet de eigenaar van het abonnement zijn om het lab te maken.

## Stappen voor het maken van een lab in Azure DevTest Labs

In de volgende stappen ziet u hoe u de Azure-portal kunt gebruiken om een lab te maken in Azure DevTest Labs. 

1. Meld u aan bij de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selecteer **Meer services** en selecteer in de lijst vervolgens **DevTest Labs**.

1. Selecteer op de blade **DevTest Labs** de optie **Toevoegen**.

    ![Een lab toevoegen](./media/devtest-lab-create-lab/add-lab-button.png)

1. Op de blade **Een DevTest Lab maken**:

    1. Voer een **labnaam** in voor het nieuwe lab.
    
    1. Selecteer het **abonnement** dat u wilt koppelen aan het lab.
    
    1. Selecteer op welke **locatie** u het lab wilt opslaan.
    
    1. Selecteer **Auto-shutdown** om op te geven of u het automatisch afsluiten van alle virtuele machines van het lab wilt inschakelen en de parameters voor deze machines wilt definiëren.
    
    1. Selecteer het **opslagtype** om het opslagtype voor de schijf aan te geven voor de virtuele machines van het lab. 
    
    1. Selecteer **Maken**.

    ![Een labblade maken](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## Volgende stappen

Wanneer u uw lab hebt gemaakt, kunt u onder andere de volgende stappen uitvoeren:

- [Toegang tot een lab beveiligen](devtest-lab-add-devtest-user.md).

- [Labbeleidsregels instellen](devtest-lab-set-lab-policy.md).

- [Een labsjabloon maken](devtest-lab-create-template.md).

- [Aangepaste artefacten maken voor uw virtuele machines](devtest-lab-artifact-author.md).

- [Een VM met artefacten toevoegen aan een lab](devtest-lab-add-vm-with-artifacts.md).


<!--HONumber=sep16_HO2-->


