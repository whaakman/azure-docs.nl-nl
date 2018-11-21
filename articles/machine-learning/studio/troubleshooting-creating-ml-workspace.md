---
title: 'Problemen oplossen: Maken en verbinding maken met een Machine Learning-werkruimte | Microsoft Docs'
description: Oplossingen voor veelvoorkomende problemen bij het maken en verbinding te maken met een Azure Machine Learning-werkruimte
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=hshapiro, author=heatherbshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 1a8aec4b-35f9-44e8-9570-2575b8979ab1
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.openlocfilehash: f86018c78de842717bf387c007fdd4c7cd048ef1
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52265199"
---
# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a>Gids voor probleemoplossing: een Machine Learning-werkruimte maken en hiermee verbinding maken
Deze handleiding bevat oplossingen voor enkele uitdagingen vaak aangetroffen bij het instellen van Azure Machine Learning-werkruimten.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>Eigenaar van de werkruimte
Als u wilt een werkruimte opent in Machine Learning Studio, u moet zijn aangemeld bij de Microsoft-Account dat u gebruikt voor het maken van de werkruimte, of moet u een uitnodiging ontvangen van de eigenaar voor deelname aan de werkruimte. U kunt de werkruimte, inclusief de mogelijkheid om toegang te configureren beheren vanuit Azure portal.

Zie voor meer informatie over het beheren van een werkruimte [een Azure Machine Learning-werkruimte beheren].

[Een Azure Machine Learning-werkruimte beheren]: manage-workspace.md

## <a name="allowed-regions"></a>Toegestane regio 's
Machine Learning is momenteel beschikbaar in een beperkt aantal regio's. Als uw abonnement niet onder een van deze regio's, ziet u mogelijk het foutbericht, "U hebt geen abonnementen in de toegestane regio's."

Als u wilt dat een regio worden toegevoegd aan uw abonnement, een nieuwe Microsoft-ondersteuningsaanvraag maken vanuit Azure portal, kiest u **facturering** als het probleemtype, en volg de aanwijzingen om uw aanvraag te verzenden.

## <a name="storage-account"></a>Storage-account
De Machine Learning-service moet een storage-account voor het opslaan van gegevens. U kunt een bestaand opslagaccount gebruiken of u kunt een nieuw opslagaccount maken bij het maken van de nieuwe Machine Learning-werkruimte (als u een quotum voor het maken van een nieuw opslagaccount hebt).

Nadat de nieuwe Machine Learning-werkruimte is gemaakt, kunt u zich aanmelden voor Machine Learning Studio met behulp van het Microsoft-account dat u gebruikt voor het maken van de werkruimte. Als u het foutbericht 'Werkruimte niet gevonden' (vergelijkbaar met de volgende schermafbeelding), gebruik de volgende stappen uit om uw browsercookies te verwijderen.

![De werkruimte niet vinden][screen3]

**Browsercookies te verwijderen**

1. Als u Internet Explorer gebruikt, klikt u op de **extra** in de rechterbovenhoek en selecteer **Internetopties**.  

![Internet-opties][screen4]

2. Onder de **algemene** tabblad **verwijderen...**

![Tabblad Algemeen][screen5]

3. In de **Browsegeschiedenis verwijderen** dialoogvenster vak, zorg ervoor dat **Cookies en websitegegevens** is geselecteerd en klik op **verwijderen**.

![Verwijderen van cookies][screen6]

Nadat de cookies die zijn verwijderd, opnieuw opstarten van de browser en ga vervolgens naar de [Microsoft Azure Machine Learning](https://studio.azureml.net) pagina. Wanneer u wordt gevraagd een gebruikersnaam en wachtwoord, typt u hetzelfde Microsoft-account dat u gebruikt voor het maken van de werkruimte.

## <a name="comments"></a>Opmerkingen

Ons doel is om de Machine Learning-ervaring zo weinig mogelijk. Boek eventuele opmerkingen en problemen op de [Azure Machine Learning-forum](https://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) zodat u beter van dienst.

[screen1]:media/troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/troubleshooting-creating-ml-workspace/screen6.png
