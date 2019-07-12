---
title: Jacht mogelijkheden met behulp van notebooks in Azure Sentinel Preview | Microsoft Docs
description: In dit artikel wordt beschreven hoe u notebooks gebruiken met de mogelijkheden van Azure Sentinel jacht.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 1721d0da-c91e-4c96-82de-5c7458df566b
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: cc84db4bd3dc06d4e2dbecb3be8ceee37d554ae7
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67619834"
---
# <a name="use-jupyter-notebooks-to-hunt-for-security-threats"></a>Jupyter-Notebooks gebruiken om te zoeken op beveiligingsrisico 's

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

De basis van Azure Sentinel is het gegevensarchief; het combineert uitvoeren van query's, dynamisch schema, en worden geschaald om grote gegevensvolumes te krachtige. De portal voor Azure Sentinel en alle hulpprogramma's voor Azure Sentinel gebruiken een gemeenschappelijke API voor toegang tot dit gegevensarchief. Dezelfde API is ook beschikbaar voor externe hulpprogramma's zoals [Jupyter](https://jupyter.org/) laptops en Python. Hoewel veel algemene taken kunnen worden uitgevoerd in de portal, Jupyter een uitbreiding voor het bereik van wat u met deze gegevens doen kunt. Het combineert volledige programmeerbaarheid met een grote verzameling van bibliotheken voor machine learning, visualisatie en gegevensverzameling analyse. Deze kenmerken maken Jupyter zo'n aantrekkelijke hulpprogramma voor beveiligingsonderzoek en jacht.

![Voorbeeld van de notebook](./media/notebooks/sentinel-nb-mapandtimeline.png)

We hebben de Jupyter-ervaring geïntegreerd in de portal Azure Sentinel zodat u eenvoudig voor u maken en uitvoeren van notitieblokken voor het analyseren van uw gegevens. De *Kqlmagic* -bibliotheek biedt de lijm waarmee u query's van Azure Sentinel nemen en ze rechtstreeks worden uitgevoerd in een notitieblok uit te voeren. Query's gebruiken de [Kusto-querytaal](https://kusto.azurewebsites.net/docs/query/index.html). Verschillende notebooks, die zijn ontwikkeld door enkele van de beveiligingsanalisten van Microsoft, zijn verpakt met de Azure-Sentinel. Sommige van deze laptops worden gebouwd voor een specifiek scenario en kan worden gebruikt als-is. Anderen zijn bedoeld als voorbeelden ter illustratie van technieken en functies die u kunt kopiëren of aan te passen voor gebruik in uw eigen notebooks. Andere laptops kunnen ook worden geïmporteerd uit de community Azure Sentinel GitHub.

Maakt gebruik van de geïntegreerde ervaring die Jupyter [Azure notitieblokken](https://notebooks.azure.com/) wilt opslaan, delen en uitvoeren van notitieblokken. U kunt deze notebooks ook uitvoeren lokaal (als u een Python-omgeving en Jupyter op uw computer) of in andere omgevingen JupterHub zoals Azure Databricks.

Notitieblokken bestaan uit twee onderdelen:

- de browser-gebaseerde interface waar u invoeren en uitvoeren van query's en code, en waarbij de resultaten van de uitvoering worden weergegeven.
- een *kernel* die verantwoordelijk is voor het parseren en de code zelf uitvoert. 

In Azure-notitieblokken, deze kernel wordt uitgevoerd op Azure *gratis Cloud Compute en Storage* standaard. Als uw laptops complexe machine learning-modellen opnemen of visualisaties moet u overwegen krachtigere, toegewezen resources, zoals compute [virtuele Machines voor Datatechnologie](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/) (DSVM). Notitieblokken in uw account blijven, tenzij u ervoor kiest om deze te delen.

De Azure-Sentinel notebooks gebruiken veel populaire Python-bibliotheken zoals pandas, matplotlib, bokeh en anderen. Er zijn een zeer groot aantal andere pakketten voor Python voor u om de verkeersbelasting, onderwerpen, zoals:

- visualisaties en afbeeldingen
- Data-verwerking en analyse
- statistieken en numerieke computing
- machine learning- en deep learning

We hebben ook enkele open-source Jupyter beveiligingsprogramma's in een pakket met de naam uitgebracht [msticpy](https://github.com/Microsoft/msticpy/). Dit pakket wordt gebruikt in veel van de opgenomen notebooks. Hulpprogramma's voor Msticpy zijn speciaal ontworpen om te helpen bij het maken van notitieblokken voor jacht en onderzoek en we werken er hard aan nieuwe functies en verbeteringen.

De eerste notebooks zijn onder andere:

- **Onderzoek - proces waarschuwingen begeleide**: Hiermee kunt u snel kunt classificeren waarschuwingen door het analyseren van de activiteit op de geïnfecteerde host (s).
- **Begeleide jacht - Windows-host explorer**: Hiermee kunt u accountactiviteit, proces uitvoeringen netwerkactiviteit en andere gebeurtenissen verkennen op een host.  
- **Begeleide jacht - Office 365 verkennen**: Hunt voor verdachte activiteiten voor Office 365 in meerdere O365-gegevenssets.

De [Azure Sentinel Community GitHub-opslagplaats](https://github.com/Azure/Azure-Sentinel) is de locatie voor alle toekomstige Azure Sentinel laptops die zijn gemaakt door Microsoft of van de community bijgedragen.

## <a name="run-a-notebook"></a>Uitvoeren op een notebook

In het volgende voorbeeld maken we een Azure-notitieblokken-project vanuit de portal voor Azure-Sentinel invullen van het project met notitieblokken. Voordat u deze laptops, is het een goed idee om een kopie van de notebook maken en werken met de kopie. Werken met kopieën kunt u veilig bijwerken voor toekomstige versies van notitieblokken zonder dat een van uw gegevens worden overschreven.

1. Klik in de portal voor Azure Sentinel **notitieblokken** in het navigatiemenu. Als u wilt een nieuw project voor de Azure-notitieblokken maken, klikt u op **kloon Azure Sentinel notitieblokken** of projecten klikt u op uw bestaande notitieblokken geopend **gaat u naar uw laptops**.
  
   ![notitieblokken selecteren](./media/notebooks/sentinel-az-notebooks-home.png)

2. Als u ervoor hebt gekozen **kloon Azure Sentinel notitieblokken** in de vorige stap is het volgende dialoogvenster wordt weergegeven. Klik op **importeren** naar kloon de GitHub-opslagplaats naar uw Azure-notitieblokken-project. Als u geen bestaande Azure-notitieblokken account hebt, wordt u gevraagd een te maken en meld u aan.

   ![Import-notebook](./media/notebooks/sentinel-nb-signin-and-clone.png)

3. Wanneer u een nieuw project maakt, moet u het project een naam - gebruik van de standaard-naam of het type in een nieuwe. Niet controleren de **kloon recursief** optie - deze optie verwijst naar de gekoppelde GitHub-opslagplaatsen. Te klikken op **importeren** begint met het klonen van de GitHub-inhoud, wat kan een paar minuten duren.

   ![Import-notebook](./media/notebooks/sentinel-create-nb-project.png)

4. Open de **notitieblokken** map om de notebooks. Elk notitieblok leidt u door de stappen voor het uitvoeren van een hunt of onderzoek. Bibliotheken en andere afhankelijkheden die nodig zijn voor de notebook kunnen worden geïnstalleerd vanuit het notitieblok zelf of via een eenvoudige configuratieprocedure. Configuratie die uw laptop project terug naar uw abonnement Azure Sentinel bindt wordt automatisch ingericht in de voorgaande stappen. Uw laptops zijn gereed om uit te voeren op basis van uw Azure Sentinel Log Analytics-werkruimte.

   ![Importeren van opslagplaats](./media/notebooks/sentinel-open-notebook1.png)

5. Een notitieblok opent. Gratis computermogelijkheden is standaard geselecteerd om uit te voeren van de notebooks (gemarkeerd). Als u een DSVM als u wilt gebruiken (Zie hierboven) hebt geconfigureerd, selecteert u de DSVM en verifiëren voordat u het eerste notitieblok opent. Klik op een laptop om dit te openen.

   ![notitieblok selecteren](./media/notebooks/sentinel-open-notebook2.png)

6. Selecteren van de Python-versie. Wanneer u een notitieblok voor het eerst opent, u wordt gevraagd om een kernelversie te selecteren. Als dat niet het geval is, selecteert u de kernel te gebruiken als volgt. Python 3.6 of hoger moet de geselecteerde kernel (in de rechterbovenhoek van het venster notebook).

   ![notitieblok selecteren](./media/notebooks/sentinel-select-kernel.png)

Een korte inleiding tot het opvragen van gegevens in Azure Sentinel kijken naar de [GetStarted](https://github.com/Azure/Azure-Sentinel/blob/master/Notebooks/Get%20Started.ipynb) notitieblok in de hoofdmap van notitieblokken. Aanvullende voorbeeldnotitieblokken kunnen u vinden in de **voorbeeldnotitieblokken** submap. De voorbeeld-notebooks zijn opgeslagen met gegevens, zodat het eenvoudiger om de gewenste uitvoer te zien is (het is raadzaam weergeven in [nbviewer](https://nbviewer.jupyter.org/)). De **HowTos** map bevat notitieblokken beschrijving, bijvoorbeeld: instelling die u standaard Python-versie, van een laptop en andere onderwerpen u configureert een DSVM, het maken van Azure Sentinel bladwijzers.

Deze laptops zijn bedoeld als beide nuttige hulpprogramma's en afbeeldingen en codevoorbeelden die u bij de ontwikkeling van uw eigen notebooks gebruiken kunt.

We stellen feedback of suggesties voor aanvragen voor functies,-laptops, foutenrapporten of verbeteringen en toevoegingen aan bestaande laptops bijgedragen. Ga naar de [Azure Sentinel Community GitHub](https://github.com/Azure/Azure-Sentinel) maken van een probleem of een fork en een bijdrage uploaden.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u aan de slag met Jupyter-notebooks in Azure Sentinel. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:

- [Proactief bedreigingen opsporen](hunting.md)
- [Bladwijzers gebruiken om op te slaan interessante gegevens terwijl jacht](bookmarks.md)
