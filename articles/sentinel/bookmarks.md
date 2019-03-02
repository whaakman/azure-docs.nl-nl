---
title: Bijhouden van gegevens tijdens het zoeken in Azure Sentinel Preview jacht bladwijzers gebruiken | Microsoft Docs
description: In dit artikel wordt beschreven hoe u de jacht Azure Sentinel bladwijzers gebruiken om gegevens bij te houden.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 320ccdad-8767-41f3-b083-0bc48f1eeb37
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 9a7ceeab6d2ad761752f778038692256bd87624b
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57242833"
---
# <a name="keep-track-of-data-during-hunting"></a>Bijhouden van gegevens tijdens jacht

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.
 
Doorgaans vereist dat Threat jacht bergen van logboekgegevens op zoek naar bewijs van schadelijk gedrag controleren. Tijdens dit proces vinden onderzoekers gebeurtenissen die ze willen onthouden, bezoeken en analyseren van als onderdeel van de mogelijke hypothesen valideren en inzicht krijgen in het volledige verhaal van een inbreuk op.
Jacht bladwijzers kunnen u dit doet, met behoud van de query's die u hebt uitgevoerd in Log Analytics, samen met de resultaten van de query die u relevant acht. U kunt ook uw contextuele opmerkingen vastleggen en verwijzen naar uw bevindingen door het toevoegen van opmerkingen en tags. Bladwijzer gegevens zijn zichtbaar voor u en uw teamleden om eenvoudig samen te werken.   

U kunt uw bladwijzers gegevens bezoeken op elk gewenst moment op de **bladwijzer** tabblad van de **jacht** pagina. U kunt gebruik filters en opties om snel specifieke gegevens voor uw huidige onderzoek zoeken. U kunt ook vindt u uw bladwijzers gegevens rechtstreeks in de **HuntingBookmark** tabel in Log Analytics. Hiermee kunt u wilt filteren, samenvatten en bladwijzer gegevens met andere gegevensbronnen, zodat u eenvoudig kunt zoeken naar aanwijzingen oliefabriek samenvoegen.

U kunt ook uw bladwijzers gegevens visualiseren door te klikken op **onderzoeken**. Hiermee wordt de ervaring voor het onderzoeken waarin u kunt weergeven, onderzoeken en visueel communiceren uw conclusies met behulp van een diagram van interactieve entiteit-grafiek en een tijdlijn gestart.


## <a name="run-a-log-analytics-query-from-azure-sentinel"></a>Een Log Analytics-query uitvoeren vanuit Azure Sentinel

1. Klik in de portal voor Azure Sentinel **jacht** uitvoeren van query's voor verdachte en afwijkend gedrag.

1. Als u wilt een campagne jacht uitvoeren, selecteert u een van de jacht query's en op de linkerkant, Controleer de resultaten. 

1. Klik op **queryresultaten weer** in de query jacht **Details** pagina om de query weer te geven resultaten in Log Analytics. Hier volgt een voorbeeld van wat u ziet als u een aangepaste SSH bruteforce aanval query uitgevoerd.
  
   ![resultaten weergeven](./media/bookmarks/ssh-bruteforce-example.png)

## <a name="add-a-bookmark"></a>Een bladwijzer toevoegen

1. Vouw in de lijst met Log Analytics-query resultaten uit de rij met de informatie die u interessant vindt.

4. Selecteer het weglatingsteken (...) aan het einde van de rij en selecteer **jacht bladwijzers toevoegen**.
5. Aan de rechterkant in de **Details** pagina, de naam van de update en tags en opmerkingen om te herkennen wat was interessant over het item toevoegen.
6. Klik op **opslaan** uw wijzigingen doorvoeren. Alle gegevens van de bladwijzer wordt gedeeld met andere onderzoekers en is een eerste stap naar een gezamenlijke onderzoeksmethode.

   ![resultaten weergeven](./media/bookmarks/add-bookmark-la.png)

 
> [!NOTE]
> U kunt bladwijzers ook gebruiken met willekeurige Log Analytics-query's van de pagina Azure Sentinel Log Analytics-Logboeken, of query's die zijn gemaakt op het invoegen van de Log Analytics-pagina en geopend op de pagina jacht gestart. Niet mogelijk een bladwijzer worden toegevoegd als u Log Analytics van buiten Azure Sentinel starten. 

## <a name="view-and-update-bookmarks"></a>Bladwijzers weergeven en bijwerken 

1. Klik in de portal voor Azure Sentinel **jacht**. 
2. Klik op de **bladwijzers** tabblad in het midden van de pagina om de lijst met bladwijzers weer te geven.
3. Gebruik de zoekopties voor box of filter voor een specifieke bladwijzer vinden.
4. Selecteer afzonderlijke bladwijzers in het raster hieronder om de details van de bladwijzer in het detailvenster rechts weer te geven.
5. Voor het bijwerken van tags en opmerkingen, klik op de bewerkbare tekstvakken en op **opslaan** uw wijzigingen te behouden.

   ![resultaten weergeven](./media/bookmarks/view-update-bookmarks.png)

## <a name="view-bookmarked-data-in-log-analytics"></a>Bladwijzer gegevens weergeven in Log Analytics 

Er zijn meerdere opties om uw bladwijzers gegevens weergeven in Log Analytics. 

De eenvoudigste manier om bladwijzers query's, resultaten en geschiedenis weer te geven, is door het selecteren van de gewenste bladwijzer in de **bladwijzers** tabel en gebruik de koppelingen in het detailvenster. Een aantal opties: 
- Klik op **weergavequery** om de bronquery in Log Analytics weer te geven.  
- Klik op **bladwijzer geschiedenis weergeven** om te zien alle bladwijzer met inbegrip van metagegevens: wie de update, de bijgewerkte waarden en het tijdstip waarop de update heeft plaatsgevonden gemaakt. 

- U kunt ook de bladwijzer onbewerkte gegevens voor alle bladwijzers weergeven door te klikken op **bladwijzer logboeken** boven het raster bladwijzer. In deze weergave ziet u de alle uw bladwijzers in de tabel jacht bladwijzer met de bijbehorende metagegevens. KQL query's kunt u omlaag naar de nieuwste versie van de specifieke bladwijzer voor filteren.  


> [!NOTE]
> Er mag aanzienlijke vertraging (gemeten in minuten) tussen het maken van een bladwijzer en wanneer deze wordt weergegeven de **HuntingBookmark** tabel. Het verdient aanbeveling uw bladwijzers eerst, en ze analyseren nadat de gegevens worden opgenomen. 

## <a name="delete-a-bookmark"></a>Verwijder een bladwijzer
Als u verwijderen van een bladwijzer het volgende te doen wilt: 
1.  Open th **jacht bladwijzer** tabblad. 
2.  Selecteer de doel-bladwijzer.
3.  Selecteer het weglatingsteken (...) aan het einde van de rij en selecteer **verwijderen bladwijzer**.
    
De bladwijzer verwijdert, wordt de bladwijzer in de lijst in de **bladwijzer** tabblad.  De met Log Analytics "HuntingBookmark" tabel blijft vorige bladwijzer vermeldingen bevatten, maar de meest recente vermelding verandert de **SoftDelete** waarde True is, zodat u eenvoudig kunt uitfilteren oude bladwijzers.  Verwijderen van een bladwijzer, wordt de entiteiten niet verwijderd uit de ervaring voor het onderzoeken die zijn gekoppeld aan andere bladwijzers of waarschuwingen. 


## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u een jacht onderzoek bladwijzers gebruiken in Azure Sentinel uitvoert. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:


- [Proactief bedreigingen opsporen](hunting.md)
- [Notitieblokken gebruiken voor het uitvoeren van geautomatiseerde jacht campagnes](notebooks.md)