---
title: Jacht mogelijkheden met behulp van notebooks in Azure Sentinel Preview | Microsoft Docs
description: In dit artikel wordt beschreven hoe u notebooks gebruiken met de mogelijkheden van Azure Sentinel jacht.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 1721d0da-c91e-4c96-82de-5c7458df566b
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 83bbb7f6a683265e4f2ed20801d0c7541a54e9f9
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57244958"
---
# <a name="use-notebooks-to-hunt-for-anomalies"></a>Notitieblokken gebruiken voor het opsporen van afwijkingen

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Azure Sentinel maakt gebruik van de kracht van Jupyter-notebooks voor interactieve inzichten en acties om te onderzoeken of zoeken naar een baan voor beveiligingsafwijkingen binnen uw omgeving te geven. Azure Sentinel wordt meegeleverd met laptops die zijn ontwikkeld door beveiligingsanalisten van Microsoft. Elke laptop is speciaal daarvoor ontwikkelde met een ingesloten workflow voor een specifieke use case. Visualisaties zijn opgenomen in elke laptop voor snellere gegevens verkennen en jacht bedreigingen. Aanpassen van de ingebouwde laptops om te voldoen aan uw behoeften, nieuwe notitieblokken maken vanaf het begin of notitieblokken importeren vanuit de Azure-Sentinel' GitHub-community. Jupyter-notebooks worden geïmporteerd als een project in de Azure-notitieblokken-pagina – dit kan de gebruiker toegang tot al hun laptops Azure Sentinel op één plek. Notitieblokken kunnen worden uitgevoerd met een op een knop te klikken of door de gebruiker zodat deze overeenkomen met hun omgeving kunnen worden geconfigureerd.

De volledig geïntegreerde ervaring kunnen laptops om uit te voeren op Cloud Compute en opslag zonder onderliggende overhead voor onderhoud. Gebruikmaken van de bestaande Jupyter-Notebooks-ecosysteem kunt u modellen massa-bron, zonder het delen van gegevens van de klant. 


Elke notebook wordt gehost op de Azure-laptops (momenteel in preview), een interactive development environment; in de Azure-cloud. Notitieblokken zijn altijd toegankelijk is, altijd beschikbaar is via elke browser, overal ter wereld.  De Azure-Sentinel' ingebouwde laptops voor onderzoek en jacht worden gekloond naar een project dat u behoort en die u kunt wijzigen en aanpassen aan uw omgeving. Enkele van de meest populaire notebooks ingebouwde zijn:

- **Onderzoek en jacht gewaarschuwd**: Dit notitieblok interactieve maakt snelle schifting van verschillende soorten waarschuwingen door bij het ophalen van gerelateerde activiteit en verrijken van de waarschuwing worden gebruikt met de bijbehorende activiteit en gegevens op basis waarvan de waarschuwing is gegenereerd.

- **Host van het eindpunt begeleide jacht**: Hiermee kunt u zoeken naar een baan voor het tekenen van een schending van de beveiliging door het inzoomen op de beveiliging relevante activiteiten die zijn gekoppeld aan een host van het eindpunt.  

- **Office melden bij begeleide jacht**: Hiermee kunt u zoeken naar een baan voor verdachte aanmeldingen met geografische locaties van verdachte logboeken te visualiseren, evenals ongewone aanmeldpatronen afgeleid van Office 365-gegevens weergeven.

## <a name="run-a-notebook"></a>Uitvoeren op een notebook
In het volgende voorbeeld uitvoeren we het ingebouwde notitieblok om te zoeken naar afwijkingen van de locatie om te zien als iemand anders in een verkeerde locatie iets in uw netwerk gaat diep u.

1. Klik in de portal voor Azure Sentinel **notitieblokken** en selecteer vervolgens een van de ingebouwde notebooks.
  
   ![notitieblok selecteren](./media/notebooks/select-notebook.png)

3.  Klik op **importeren** naar kloon de GitHub-opslagplaats naar uw Azure-notitieblokken-project.
  ![Import-notebook](./media/notebooks/import1.png)
4.  Elk notitieblok leidt u door de stappen voor het uitvoeren van een hunt of onderzoek. Modellen, bibliotheken en andere afhankelijkheden en de configuratie voor de verbinding met Azure Sentinel wordt automatisch geïmporteerd om in te schakelen van één klik kan worden uitgevoerd. Alle code en bibliotheken die nodig zijn voor het uitvoeren van een laptop worden vooraf geladen. U kunt direct beginnen met het uitvoeren van een notebook in uw Log Analytics-werkruimte zonder configuratie.

   ![Importeren van opslagplaats](./media/notebooks/import2.png)

5.  Verken, wijzigen en uitvoeren van alle voorbeeldnotitieblokken die zijn opgegeven. Deze kunnen worden gebruikt als bouwstenen voor veel verschillende scenario's.

  ![notitieblok selecteren](./media/notebooks/import3.png)



## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u een jacht onderzoek met behulp van notebooks in Azure Sentinel uitvoert. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:

- [Proactief bedreigingen opsporen](hunting.md)
- [Bladwijzers gebruiken om op te slaan interessante gegevens terwijl jacht](bookmarks.md)