---
title: Een geografisch kunstmatige Intelligence virtuele Machine in Azure - inrichten Azure | Microsoft Docs
description: Het inrichten van een geografisch AI virtuele Machine in Azure.
keywords: deep leren, AI en hulpmiddelen voor wetenschappelijke gegevens, gegevens wetenschappelijke virtuele machine, georuimtelijke analytics
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/05/2018
ms.author: gokuma
ms.openlocfilehash: 93dfe6594aeaf45a6905fe8cb55c98dd37cc9599
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
ms.locfileid: "31408605"
---
# <a name="provision-a-geo-artificial-intelligence-virtual-machine-on-azure"></a>Inrichten van een geografisch kunstmatige Intelligence virtuele Machine in Azure 

De Geo AI gegevens wetenschappelijke virtuele Machine (Geo-DSVM) is een uitbreiding van de populaire [virtuele Machine van Azure Data wetenschappelijke](http://aka.ms/dsvm) dat speciaal is geconfigureerd voor het combineren van AI en georuimtelijke analyses. De georuimtelijke analyses in de virtuele machine zijn gemaakt, aangeboden via [ArcGIS Pro](https://www.arcgis.com/features/index.html). De VM van de wetenschappelijke gegevens kunt de snelle training van machine learning-modellen en zelfs van diep learning-modellen voor gegevens die is uitgebreid met geografische informatie. Dit wordt alleen ondersteund op Windows 2016 DSVM. 

De Geo-DSVM bevat verschillende hulpprogramma's voor het opnemen van AI:

- GPU-edities van populaire grondige learning frameworks, zoals Microsoft cognitieve Toolkit, TensorFlow, Keras, Caffe2, Chainer; 
- hulpprogramma's te verkrijgen en vooraf verwerken installatiekopie, tekstgegevens, 
- hulpprogramma's voor ontwikkelingsactiviteiten zoals Microsoft R Server Developer Edition, Python, Anaconda, Jupyter-notebooks voor Python en R, IDE voor Python en R, SQL databases
- De ESRI ArcGIS Pro desktopsoftware samen met Python en R-interfaces dat met de georuimtelijke gegevens uit uw AI-toepassingen werkt. 


## <a name="create-your-geo-ai-data-science-vm"></a>Uw geografisch AI-Gegevenswetenschap VM maken

Hier volgt de procedure voor het maken van een exemplaar van de virtuele machine Geo AI gegevens wetenschappelijke: 


1. Navigeer naar de virtuele machine op aanbieding [Azure-portal](https://ms.portal.azure.com/#create/microsoft-ads.geodsvmwindows).
2. Selecteer de **maken** knop onderaan in een wizard moeten worden genomen.
![create-geo-ai-dsvm](./media/provision-geo-ai-dsvm/Create-Geo-AI.png)
3. De wizard gebruikt voor het maken van de Geo-DSVM vereist **invoer** voor elk van de **bestaat uit vier stappen** opgesomd aan de rechterkant van de volgende afbeelding. Hier volgen de invoer voor het configureren van deze stappen:



   - **Basisinstellingen**

      1. **Naam**: naam van de gegevens wetenschap-server die u maakt.

      2. **Gebruikersnaam**: Admin-account aanmeldings-id.

      3. **Wachtwoord**: wachtwoord van de beheerder.

      4. **Abonnement**: als u meer dan één abonnement hebt, selecteert u een op waarop de machine is gemaakt en kosten in rekening gebracht.

      5. **Resourcegroep**: U kunt een nieuwe maken of gebruik een **leeg** bestaande Azure-resourcegroep in uw abonnement.

      6. **Locatie**: Selecteer het datacenter die het meest geschikt is. Het is doorgaans het datacenter die de meeste van uw gegevens of heeft de dichtstbijzijnde fysieke voor snelste toegang tot het netwerk. Als u grondige kennis op GPU wilt, moet u een van de locaties in Azure met NC-serie GPU VM-exemplaren. De locaties die GPU VM's zijn momenteel: **VS-Oost, Noord-centraal VS, Zuid-centraal VS, VS-West 2, Noord-Europa, West-Europa**. Raadpleeg voor de meest recente lijst de [Azure producten op de pagina regio](https://azure.microsoft.com/regions/services/) en zoekt u **NC-serie** onder **Compute**. 


   - **Instellingen**: Selecteer een van de grootte van de virtuele machine NC-serie GPU als u van plan bent uit te voeren grondige learning op GPU op uw geografisch DSVM. Anders kunt u een van de CPU op basis van exemplaar.  Een opslagaccount maken voor uw virtuele machine. 
   
   - **Samenvatting**: Controleer of alle informatie die u hebt ingevoerd juist is.

   - **Kopen**: klik op **kopen** starten van de inrichting. Een koppeling wordt geleverd aan de voorwaarden van de service. De virtuele machine heeft geen eventuele extra kosten afgezien van de berekening die voor de servergrootte van de die u hebt gekozen in de **grootte** stap. 

>[!NOTE]
> De inrichting duren ongeveer 20-30 minuten. De status van de inrichting wordt weergegeven op de Azure-portal.


## <a name="how-to-access-the-geo-ai-data-science-virtual-machine"></a>Toegang tot de Geo AI gegevens wetenschappelijke virtuele Machine

Zodra uw virtuele machine is gemaakt, bent u klaar om te beginnen met behulp van de hulpprogramma's die zijn geïnstalleerd en vooraf geconfigureerde erop. Er zijn start menu tegels en bureaubladpictogrammen voor veel van de hulpprogramma's. U kunt Extern bureaublad in met behulp van de referenties van het Administrator-account die u hebt geconfigureerd in de voorgaande **basisbeginselen** sectie. 


## <a name="using-arcgis-pro-installed-in-the-vm"></a>Met behulp van ArcGIS Pro geïnstalleerd in de virtuele machine

De Geo-DSVM al ArcGIS Pro bureaublad vooraf is geïnstalleerd en de omgeving die vooraf is geconfigureerd om te werken met de hulpprogramma's in de DSVM. Wanneer u ArcGIS Start wordt u gevraagd een aanmelding bij je account ArcGIS. Als u al een account ArcGIS en licenties voor de software hebt, kunt u uw bestaande referenties gebruiken.  

![Boog GIS aanmelding](./media/provision-geo-ai-dsvm/ArcGISLogon.png)

Anders aanmelden voor een nieuwe account ArcGIS en licentie of ophalen van een [gratis proefversie](https://www.arcgis.com/features/free-trial.html). 

![ArcGIS-Free-Trial](./media/provision-geo-ai-dsvm/ArcGIS-Free-Trial.png)

Zodra u een betaald of een gratis proefaccount ArcGIS aanmelden voor een hebt, kunt u ArcGIS Pro autoriseren voor uw account door de instructies in de [aan de slag met ArcGIS Pro-documentatie](http://www.esri.com/library/brochures/getting-started-with-arcgis-pro.pdf). 

Nadat u zich aanmeldt bij ArcGIS Pro bureaublad met uw account ArcGIS, bent u klaar om te beginnen met behulp van de hulpprogramma's voor wetenschap van gegevens die zijn geïnstalleerd en geconfigureerd op de virtuele machine voor uw georuimtelijke analytics en machine learning-projecten.

## <a name="next-steps"></a>Volgende stappen

Aan de slag met behulp van de virtuele machine Geo AI gegevens wetenschappelijke met richtlijnen van de volgende onderwerpen:

* [Gebruik Geo AI Gegevenswetenschap VM](use-geo-ai-dsvm.md)