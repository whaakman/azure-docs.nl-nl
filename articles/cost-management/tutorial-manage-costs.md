---
title: Kosten beheren met behulp van Azure kosten Management | Microsoft Docs
description: Kosten beheren met behulp van de toewijzing en showback en terugstorting Kostenrapporten.
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 11/06/2017
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 83ddc0cb4227235069b0027a24a52f4d8e818126
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="manage-costs-by-using-azure-cost-management"></a>Kosten beheren met behulp van Azure kosten Management

U beheert kosten en produceren van rapporten in Azure kosten Management Cloudyn showback door toe te wijzen kosten op basis van labels. Kosten van het proces van kostentoewijzing toegewezen aan uw verbruikte cloudresources. Wanneer al uw resources worden gecategoriseerd met labels kosten volledig toegewezen. Nadat de kosten worden toegewezen, kunt u showback of terugstorting bieden aan uw gebruikers met dashboards en rapporten. Echter veel resources mogelijk zonder tags of untaggable wanneer u begint met het gebruik van beheer van kosten.

U wilt ophalen voor technische kosten terugbetaald. U wilt dat uw engineeringteam die u een bepaald bedrag moet, op basis van de kosten voor resources weergeven. U ze kunt weergeven, kunt u een rapport voor de verbruikte resources die zijn gelabeld *engineering*.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Gebruik aangepaste labels kosten wilt toewijzen.
> * Showback en doorberekeningsrapporten maken.

## <a name="use-custom-tags-to-allocate-costs"></a>Gebruik aangepaste labels kosten wilt toewijzen

Wanneer u kostentoewijzing start, is het eerste wat dat u doet het bereik definiëren met behulp van een kostprijsmodel. Het kostprijsmodel kosten niet verandert, wordt dit verdeeld. Wanneer u een kostprijsmodel maakt, kunt u uw gegevens door kosten entiteit, account of -abonnement en op meerdere labels segmenteren. Veelgebruikte voorbeeld tags kunnen bevatten een facturering code, kostenplaats of groepsnaam op. Labels kunnen u ook uitvoeren showback of terugstorting aan andere onderdelen van uw organisatie.

Selecteer voor het maken van een model van de toewijzing van aangepaste kosten **kosten** &gt; **kostenbeheer** &gt; **kostentoewijzing 360°** in het rapport menu.

![Kosten toewijzing 360-selectie](./media/tutorial-manage-costs/cost-allocation-360.png)

Op de **kosten toewijzing 360** pagina **toevoegen** en voer een naam en beschrijving voor het kostprijsmodel van uw. Selecteer alle accounts of individuele accounts. Als u gebruiken van individuele accounts wilt, kunt u meerdere accounts van meerdere cloudserviceproviders. Klik vervolgens op **categorisatie** de gedetecteerde labels die uw kostengegevens categoriseren kiezen. Kies labels (categorieën) die u wilt opnemen in uw model. In het volgende voorbeeld wordt de **eenheid** is geselecteerd.

![Voorbeeld van de kosten model categorisatie](./media/tutorial-manage-costs/cost-model01.png)



Het voorbeeld ziet $14,444 is niet-gecategoriseerde (zonder tags).

Selecteer vervolgens **niet-gecategoriseerde Resources** en selecteer de services die niet-kosten toegewezen. Vervolgens definieert u regels voor het toewijzen van kosten.

U wilt bijvoorbeeld uw Azure opslagkosten nemen en distribueren van de kosten evenveel naar virtuele Azure-machines (VM's). Om dit te doen, selecteert u de **Azure-opslag** service, selecteer **die evenredig is met gecategoriseerd**, en selecteer vervolgens **/virtuele machine van Azure**. Selecteer **maken**.

![Voorbeeld van de kosten model toewijzingsregel voor evenredige verdeling](./media/tutorial-manage-costs/cost-model02.png)



In een ander voorbeeld is het raadzaam alle kosten van uw Azure-netwerk toewijzen aan een specifieke zakelijke eenheid in uw organisatie. Om dit te doen, selecteert u de **Azure/Network** service en selecteer vervolgens **expliciete distributie**. Vervolgens stelt u het distributiepercentage en 100 en selecteer de business unit:**G&amp;A** in de volgende afbeelding:

![Voorbeeld van de kosten model toewijzingsregel voor een specifieke zakelijke eenheid](./media/tutorial-manage-costs/cost-model03.png)



Voor alle resterende niet-gecategoriseerde resources van extra toewijzingsregels te maken.

Als u geen niet-toegewezen Amazon Web Services (AWS) gereserveerd instanties hebt, kunt u deze toewijzen aan gelabelde categorieën met **gereserveerde exemplaren**.

U kunt informatie over de opties die u hebt aangebracht kosten wilt toewijzen, selecteer **samenvatting**. Uw gegevens op te slaan en om te blijven werken op extra regels later, selecteert u **als concept opslaan**. Of als u wilt uw gegevens opslaan en Cloudyn begonnen met de verwerking van uw kosten toewijzing model, selecteer **opslaan en activeren**.

De lijst met kosten modellen wordt het nieuwe kostprijsmodel met **verwerkingsstatus**. Het kan even duren voordat de Cloudyn-database wordt bijgewerkt met het kostprijsmodel van uw. Bij het verwerken is voltooid, wordt de status bijgewerkt naar **voltooid**. Vervolgens kunt u gegevens uit uw kostprijsmodel in het rapport analyse van de kosten onder bekijken **uitgebreid Filters** &gt; **kostprijsmodel**.

### <a name="category-manager"></a>Categorie Manager

Categorie Manager is een hulpprogramma voor opschonen van de gegevens waarmee u de waarden van verschillende categorieën (tags) voor het maken van nieuwe samenvoegen. Dit is een eenvoudige regel gebaseerd hulpprogramma waar u een categorie te selecteren en regels voor het samenvoegen van bestaande waarden maken. Bijvoorbeeld, u wellicht bestaande categorieën voor **R&amp;D** en **dev** waarbij zowel de ontwikkelingsgroep vertegenwoordigen.

Klik op het symbool tandwielpictogram in het bovenste rechts en selecteer in de portal Cloudyn **categorie Manager**. Als u een nieuwe categorie, selecteert het plusteken (**+**). Voer een naam voor de categorie en klik vervolgens onder **sleutels**, voer de categorie sleutels die u wilt opnemen in de nieuwe categorie.

Wanneer u een regel definieert, kunt u meerdere waarden met een alternatieve voorwaarde kunt toevoegen. U kunt ook bepaalde tekenreeksbewerkingen basic doen. Voor beide gevallen klikt u op het weglatingsteken (**...** ) rechts van **regel**.

Voor het definiëren van een nieuwe regel in de **regels** gebied, maakt u een nieuwe regel. Voer bijvoorbeeld **dev** onder **regels** en voer vervolgens **R&amp;D** onder **acties**. Wanneer u bent klaar, slaat u uw nieuwe categorie.

De volgende afbeelding toont een voorbeeld van regels die zijn gemaakt voor een nieuwe categorie met de naam **werklast**:

![Voorbeeld van de categorie](./media/tutorial-manage-costs/category01.png)



## <a name="create-showback-and-chargeback-reports"></a>Showback en doorberekeningsrapporten maken

De methode die organisaties gebruiken om uit te voeren showback en terugstorting varieert aanzienlijk. Echter, kunt u een van de dashboards en rapporten in de portal Cloudyn als basis voor een doel. U kunt gebruikerstoegang voor iedereen in uw organisatie opgeven zodat ze dashboards en rapporten op aanvraag kunnen bekijken. Alle kosten Analysis rapporten showback ondersteunen, omdat ze de resources die ze verbruikt gebruikers weergeven. En Hiermee kunnen gebruikers inzoomen kosten of gebruik gegevens die specifiek is voor de groep binnen uw organisatie.

De resultaten van de kostentoewijzing van de weergeven, opent u het rapport analyse van de kosten en selecteer het kostprijsmodel die u hebt gemaakt. Vervolgens voegt u een groepering door een of meer van de labels die is geselecteerd in het kostprijsmodel.

![Rapport analyse kosten](./media/tutorial-manage-costs/cost-analysis.png)

U kunt gemakkelijk maken en opslaan van rapporten die gericht zijn op specifieke services die worden gebruikt door specifieke groepen. U wellicht bijvoorbeeld een afdeling in die gebruikmaakt van Azure Virtual machines uitgebreid. U kunt een rapport wordt gefilterd op Azure Virtual machines om weer te geven en de kosten maken.

Als u momentopnamegegevens leveren aan andere teams wilt, kunt u een rapport in PDF- of CSV-indeling te exporteren.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Gebruik aangepaste labels kosten wilt toewijzen.
> * Showback en doorberekeningsrapporten maken.



Voor meer informatie over aan de slag met Cloudyn en ga verder met behulp van de functies de Cloudyn-documentatie.

> [!div class="nextstepaction"]
> [Cloudyn documentatie](https://support.cloudyn.com/hc/)
