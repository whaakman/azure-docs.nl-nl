---
title: 'Problemen oplossen: Maken en verbinding maken met een Machine Learning-werkruimte | Microsoft Docs'
description: Oplossingen voor algemene problemen bij het maken en verbinding maken met een Azure Machine Learning-werkruimte
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 1a8aec4b-35f9-44e8-9570-2575b8979ab1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: cdccd4ce7b87f47d21578076653bde901748188b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a>Gids voor probleemoplossing: een Machine Learning-werkruimte maken en hiermee verbinding maken
Deze handleiding bevat oplossingen voor enkele uitdagingen vaak aangetroffen bij het instellen van Azure Machine Learning-werkruimten.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>De eigenaar van de werkruimte
Om te openen van een werkruimte in Machine Learning Studio, moet u zijn aangemeld bij de Microsoft-Account dat u gebruikt voor het maken van de werkruimte of moet u een uitnodiging ontvangen van de eigenaar van het koppelen van de werkruimte. U kunt de werkruimte, inclusief de mogelijkheid om toegang te configureren beheren vanuit de Azure-portal.

Zie voor meer informatie over het beheren van een werkruimte [beheren van een Azure Machine Learning-werkruimte].

[beheren van een Azure Machine Learning-werkruimte]: manage-workspace.md

## <a name="allowed-regions"></a>Toegestane regio 's
Machine Learning is momenteel beschikbaar in een beperkt aantal regio's. Als uw abonnement niet onder een van deze gebieden, kan het foutbericht wordt weergegeven, "U hebt geen abonnementen in de toegestane regio's."

Als u wilt dat een regio worden toegevoegd aan uw abonnement, een nieuw Microsoft-ondersteuningsverzoek maken vanuit de Azure-portal, kiest u **facturering** als het probleemtype en volg de aanwijzingen om uw aanvraag te verzenden.

## <a name="storage-account"></a>Storage-account
Er moet een opslagaccount voor het opslaan van gegevens voor de Machine Learning-service. U kunt een bestaand opslagaccount gebruiken of u kunt een nieuw opslagaccount maken wanneer u de nieuwe Machine Learning-werkruimte maken (als u het quotum voor het maken van een nieuw opslagaccount hebt).

Nadat de nieuwe Machine Learning-werkruimte is gemaakt, kunt u zich aanmeldt bij Machine Learning Studio via het Microsoft-account dat u gebruikt voor het maken van de werkruimte. Als u het foutbericht 'Werkruimte niet gevonden' (vergelijkbaar met de volgende schermopname) optreden, gebruik de volgende stappen uit om uw browsercookies te verwijderen.

![Werkruimte niet gevonden][screen3]

**Browsercookies verwijderen**

1. Als u Internet Explorer gebruikt, klikt u op de **extra** in de rechterbovenhoek en selecteer **Internetopties**.  

![Internet-opties][screen4]

2. Onder de **algemene** tabblad **verwijderen...**

![Tabblad Algemeen][screen5]

3. In de **Browsegeschiedenis verwijderen** dialoogvenster zorg **Cookies en websitegegevens** is geselecteerd en klik op **verwijderen**.

![Verwijderen van cookies][screen6]

Nadat de cookies die zijn verwijderd, start de browser opnieuw en gaat u naar de [Microsoft Azure Machine Learning](https://studio.azureml.net) pagina. Wanneer u wordt gevraagd om een gebruikersnaam en wachtwoord, typt u hetzelfde Microsoft-account dat u gebruikt voor het maken van de werkruimte.

## <a name="comments"></a>Opmerkingen

Ons doel is om de ervaring van de Machine Learning zo weinig mogelijk. Neem na de eventuele opmerkingen en problemen op de [Azure Machine Learning-forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) om ons u beter van dienst te helpen.

[screen1]:media/troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/troubleshooting-creating-ml-workspace/screen6.png
