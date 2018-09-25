---
title: U kunt ontwikkelomgevingen voor Azure Machine Learning | Microsoft Docs
description: Een overzicht van ontwikkelomgevingen die kunt u met de Azure Machine Learning-service. Python 3 is de enige vereiste voor uw ontwikkelomgeving, maar wordt u aangeraden ook Conda-omgevingen. Voor de ontwikkeling van hulpprogramma's, wordt aangeraden Jupyter Notebooks, Azure-notitieblokken en IDE-votingapplication-code-editors.
services: machine-learning
author: rastala
ms.author: roastala
manager: cgronlun
ms.service: machine-learning
ms.reviewer: larryfr
ms.topic: conceptual
ms.date: 9/24/2018
ms.openlocfilehash: 260e209bcf00396ee545851684038578c4fd148a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46971039"
---
# <a name="development-environment-for-azure-machine-learning"></a>Ontwikkelomgeving voor Azure Machine Learning 

Meer informatie over de ontwikkelomgevingen die u met de Azure Machine Learning-service gebruiken kunt. 

De service Azure Machine Learning platform-agnostische is en er zijn vereist om een specifieke omgeving. Hiervoor __Python 3__, en wordt u aangeraden __Conda__ geïsoleerde omgevingen maken. __Als u al een omgeving die voldoet aan deze vereisten__, kunt u dit document overslaan en gaat u naar de [uw ontwikkelomgeving configureren](how-to-configure-environment.md) document.

De rest van dit document worden de ontwikkelomgevingen die we aanbevelen:

* __Jupyter Notebooks__
* __Azure-laptops__
* __Geïntegreerde ontwikkelomgevingen (IDE) en code-editors__
* __Virtuele Machine voor Datatechnologie__

Een vergelijking tussen deze omgevingen is het lastig om zowel notitieblokken en -IDE's kunnen worden verlengd. Sommige IDE's kan bijvoorbeeld worden gebruikt als clients voor Jupyter-Notebooks. In het algemeen __notitieblokken__ zijn ontworpen voor __interactieve experimenten__ en __visualisatie__. __IDE's en code-editors__ hulpmiddelen __code kwaliteit verbeteren__ en __integratie met externe systemen__ zoals versiebeheer.

> [!TIP]
> Met behulp van een omgeving niet worden vergrendeld met behulp van de andere. Notitieblokken en -IDE's zijn alleen hulpprogramma's en indien nodig kan worden gecombineerd. U kunt bijvoorbeeld begin met experimenteren in een notitieblok en vervolgens exporteren naar een python-script voor het bewerken en fouten opsporen in een IDE.
>
> Dit is de reden waarom de Data Science Virtual Machine zowel Jupyter Notebooks en verschillende populaire Python IDE's biedt.

## <a name="jupyter-notebooks"></a>Jupyter Notebooks

Jupyter-Notebooks maakt deel uit van de [Jupyter Project](https://jupyter.org/). Ze zijn gericht op het bieden van een interactieve ervaring met codering waar het maken van documenten die live code met verhalende tekst en afbeeldingen combineren. U kunt Jupyter-Notebooks installeren op een aantal verschillende platformen.

Met de installatie van uw eigen Jupyter-Notebook kunt u installeren en configureren van de omgeving naar behoefte. Maar u bent verantwoordelijk voor het beheren van het systeem.

## <a name="azure-notebooks"></a>Azure Notebooks

[Azure-notitieblokken](https://notebooks.azure.com) (preview) is een omgeving voor notebooks in de Azure-cloud. Deze is gebaseerd op Jupyter en biedt een omgeving die vooraf is geladen met populaire bibliotheken. De SDK van Azure Machine Learning is al geïnstalleerd in notitieblokken van Azure, zodat u kunt beginnen te experimenteren met bijna geen instellingen.

Azure-notitieblokken vereenvoudigt ook het proces van het delen van uw notitieblokken. U kunt een URL naar uw notitieblokken delen, openbaar maken van de bibliotheek of delen op sociale media van de Azure-notitieblokken-interface.

Het nadeel van Azure-notitieblokken is dat u nog geen volledige controle over de omgeving en misschien niet mogelijk om aangepaste software die u nodig hebt te installeren. Het is ook een gedeelde omgeving, zodat uw laptops kunnen uitvoeren langzamer dan bij de installatie van een toegewezen Jupyter-Notebook.

## <a name="ides-and-code-editors"></a>IDE's en code-editors

Er zijn veel IDE en code-editors die met Azure Machine Learning werkt. De vereiste enige software die is de Azure Machine Learning SDK, die kan worden geïnstalleerd met behulp van de `pip` hulpprogramma.

Het is raadzaam [Visual Studio Code](https://code.visualstudio.com/), zoals het hulpprogramma's voor het werken met de taal Python en de Azure Machine Learning biedt. Het is beschikbaar voor Linux, macOS en Windows-platforms.

## <a name="data-science-virtual-machine"></a>Data Science Virtual Machine

De Data Science Virtual Machine (DSVM) is een combinatie van de vorige omgevingen. Het is een virtuele machine op de Azure-Platform met Jupyter-Notebooks, Visual Studio Code en de Azure Machine Learning SDK vooraf zijn geïnstalleerd. Het maken van de virtuele machine is complexer dan Azure Notebooks, maar minder complex is dan het instellen van een virtuele machine helemaal. Omdat de vereiste software is vooraf geïnstalleerd in de VM-installatiekopie, kunt u beginnen met het experimenteren met Azure Machine Learning snel nadat de virtuele machine is gemaakt.

De DSVM kunt u selecteren die rekenresources die u nodig, zoals CPU, GPU en geheugen hebt. Het is ook vooraf geïnstalleerd met andere editors, zoals PyCharm en populaire machine learning-software, zoals TensorFlow, Keras en PyTorch. Als de software die u nodig hebt, is niet geïnstalleerd, kunt u deze zelf installeren.

Zie voor meer informatie over wat vooraf geïnstalleerd is, de [wat is de virtuele Machine voor Datatechnologie](../data-science-virtual-machine/overview.md) document.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd over de ontwikkelomgevingen, meer [het configureren van een ontwikkelomgeving](how-to-configure-environment.md).

