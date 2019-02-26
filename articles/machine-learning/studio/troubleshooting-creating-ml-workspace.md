---
title: 'Problemen oplossen: Maken, verbinding maken met een Machine Learning Studio-werkruimte'
titleSuffix: Azure Machine Learning Studio
description: Deze handleiding bevat oplossingen voor enkele uitdagingen vaak aangetroffen bij het instellen van Azure Machine Learning Studio-werkruimten.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/20/2017
ms.openlocfilehash: f7b5bdbbf322a0d7ea290f2543af07d5b11f439a
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56816127"
---
# <a name="troubleshooting-guide-create-and-connect-to-an-azure-machine-learning-studio-workspace"></a>Probleemoplossingsgids: Maken en verbinding maken met een Azure Machine Learning Studio-werkruimte
Deze handleiding bevat oplossingen voor enkele uitdagingen vaak aangetroffen bij het instellen van Azure Machine Learning Studio-werkruimten.



## <a name="workspace-owner"></a>Eigenaar van de werkruimte
Als u wilt een werkruimte opent in Machine Learning Studio, u moet zijn aangemeld bij de Microsoft-Account dat u gebruikt voor het maken van de werkruimte, of moet u een uitnodiging ontvangen van de eigenaar voor deelname aan de werkruimte. U kunt de werkruimte, inclusief de mogelijkheid om toegang te configureren beheren vanuit Azure portal.

Zie voor meer informatie over het beheren van een werkruimte [een Azure Machine Learning Studio-werkruimte beheren].

[Een Azure Machine Learning Studio-werkruimte beheren]: manage-workspace.md

## <a name="allowed-regions"></a>Toegestane regio 's
Machine Learning is momenteel beschikbaar in een beperkt aantal regio's. Als uw abonnement niet onder een van deze regio's, ziet u mogelijk het foutbericht, "U hebt geen abonnementen in de toegestane regio's."

Als u wilt dat een regio worden toegevoegd aan uw abonnement, een nieuwe Microsoft-ondersteuningsaanvraag maken vanuit Azure portal, kiest u **facturering** als het probleemtype, en volg de aanwijzingen om uw aanvraag te verzenden.

## <a name="storage-account"></a>Storage-account
De Machine Learning-service moet een storage-account voor het opslaan van gegevens. U kunt een bestaand opslagaccount gebruiken of u kunt een nieuw opslagaccount maken bij het maken van de nieuwe Machine Learning Studio-werkruimte (als u een quotum voor het maken van een nieuw opslagaccount hebt).

Nadat de nieuwe Machine Learning Studio-werkruimte is gemaakt, kunt u zich aanmelden voor Machine Learning Studio met behulp van het Microsoft-account dat u gebruikt voor het maken van de werkruimte. Als u het foutbericht 'Werkruimte niet gevonden' (vergelijkbaar met de volgende schermafbeelding), gebruik de volgende stappen uit om uw browsercookies te verwijderen.

![Kan de werkruimte niet vinden](media/troubleshooting-creating-ml-workspace/screen3.png)

**Browsercookies te verwijderen**

1. Als u Internet Explorer gebruikt, klikt u op de **extra** in de rechterbovenhoek en selecteer **Internetopties**.  

   ![Internet-opties](media/troubleshooting-creating-ml-workspace/screen4.png)

2. Onder de **algemene** tabblad **verwijderen...**

   ![Tabblad Algemeen](media/troubleshooting-creating-ml-workspace/screen5.png)

3. In de **Browsegeschiedenis verwijderen** dialoogvenster vak, zorg ervoor dat **Cookies en websitegegevens** is geselecteerd en klik op **verwijderen**.

   ![Verwijderen van cookies](media/troubleshooting-creating-ml-workspace/screen6.png)

Nadat de cookies die zijn verwijderd, opnieuw opstarten van de browser en ga vervolgens naar de [Microsoft Azure Machine Learning Studio](https://studio.azureml.net) pagina. Wanneer u wordt gevraagd een gebruikersnaam en wachtwoord, typt u hetzelfde Microsoft-account dat u gebruikt voor het maken van de werkruimte.

## <a name="comments"></a>Opmerkingen

Ons doel is om de Machine Learning-ervaring zo weinig mogelijk. Boek eventuele opmerkingen en problemen op de [Azure Machine Learning-forum](https://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) zodat u beter van dienst.
