---
title: Een bestaande virtuele machine in Azure Marketplace bieden update | Microsoft Docs
description: Wordt uitgelegd hoe u een bestaande VM-aanbieding op Azure Marketplace bijwerkt.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 08/27/2018
ms.author: Ankit.Sud
ms.openlocfilehash: 254215389d129874f7ac3fc855b26819d41a095c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57852471"
---
# <a name="update-an-existing-vm-offer-on-azure-marketplace"></a>Bijwerken van een bestaande VM-aanbieding op Azure Marketplace

Dit artikel begeleidt u bij de verschillende aspecten van het bijwerken van uw aanbieding van de virtuele machine (VM) in de [Cloud Partner-Portal](https://cloudpartner.azure.com/) en vervolgens de aanbieding opnieuw te publiceren. 

Er zijn een aantal gebruikelijke redenen waarom u bij het bijwerken van uw aanbieding, met inbegrip van:

-  Een nieuwe versie van de VM-installatiekopie toevoegen aan bestaande SKU 's
-  Wijziging regio's een SKU is beschikbaar
-  Toevoegen van nieuwe SKU 's
-  De marketplace-metagegevens voor de aanbieding of de afzonderlijke SKU's bijwerken
-  Prijzen voor betalen per gebruik-aanbiedingen bijwerken

Om te helpen u bij deze wijzigingen, de portal biedt de **vergelijken** en **geschiedenis** functies.  

>[!Note]
>Cloud Solution Providers (CSP) partner kanaal aanmelden is nu beschikbaar.  Raadpleeg [Cloud Solution Providers](../../cloud-solution-providers.md) voor meer informatie over marketingcontactpersonen van uw aanbieding via de Microsoft CSP partner kanalen.

## <a name="unpermitted-changes-to-vm-offer-or-sku"></a>Ongeoorloofde wijzigingen voor de VM-aanbieding of SKU

Er zijn enkele kenmerken van een VM-aanbieding of de SKU die niet worden gewijzigd zodra de aanbieding live in Azure Marketplace, voornamelijk is:

-  **Aanbiedings-ID** en **uitgevers-ID** van de aanbieding
-  **SKU-ID** van bestaande SKU's
-  Telling van bestaande SKU's van de gegevensschijf
-  Facturering/licenties model wijzigingen in bestaande SKU 's
-  Prijs verhoogt naar een gepubliceerde SKU


## <a name="common-update-operations"></a>Algemene updatebewerkingen

Hoewel er een breed scala aan kenmerken die u wijzigen op een VM-aanbieding zijn kunt, worden de volgende bewerkingen gelden.

### <a name="update-the-vm-image-version-for-a-sku"></a>Update de versie van de VM-installatiekopie voor een SKU

Het is gebruikelijk voor een VM-installatiekopie moet worden regelmatig bijgewerkt met beveiligingspatches en extra functies.  Onder dergelijke scenario's die u wilt bijwerken van de VM-installatiekopie die uw SKU verwijst naar de volgende stappen uit:

1.  Meld u aan bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).

2.  Onder **alle aanbiedingen**, vinden de aanbieding om bij te werken.

3.  In de **SKU's** tabblad, klikt u op de SKU die zijn gekoppeld aan de VM-installatiekopie om bij te werken.

4.  Onder **schijf versie**, klikt u op **+ nieuwe versie van de schijf** om toe te voegen een nieuwe VM-installatiekopie.

5.  Geef de nieuwe VM-installatiekopieën **schijf versie**. De versie van de schijf moet volgen de [semantische versie](https://semver.org/) indeling. Versies moeten zijn van het formulier X.Y.Z, waarbij X, Y en Z gehele getallen zijn. Controleer of de nieuwe versie die u opgeeft groter is dan alle vorige versies. anders na het opnieuw uitgeven van wordt de nieuwe versie niet weergegeven in de portal of Azure Marketplace.

6.  Voor **OS VHD URL**, voer de [handtekening voor gedeelde toegang (SAS) URI](./cpp-get-sas-uri.md) gemaakt voor het besturingssysteem VHD. 

    > [!WARNING] 
    > Het aantal gegevensschijven wisselen niet tussen verschillende versies van de SKU. Als vorige versies had gegevensschijven die zijn geconfigureerd, moet hetzelfde aantal gegevensschijven ook in deze nieuwe versie hebben.

7.  Klik op **publiceren** om de werkstroom voor het publiceren van de nieuwe versie van de virtuele machine in de Azure Marketplace te starten.


### <a name="change-region-availability-of-a-sku"></a>Beschikbaarheid in regio's van een SKU wijzigen

Na verloop van tijd kunt u uw aanbieding/SKU beschikbaar maken in meer regio's.  U kunt ook stoppen met het ondersteunen van de aanbieding/SKU in een bepaalde regio.
Voor het wijzigen van beschikbaarheid, gebruikt u de volgende stappen uit:

1.  Meld u aan bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).

2.  Onder **alle aanbiedingen** vinden van de aanbieding die u wilt bijwerken.

3.  In de **SKU's** tabblad, klikt u op de SKU die u wilt wijzigen van de beschikbaarheid ervan.

4.  Klik op de **landen selecteren** knop onder de **land/regio beschikbaarheid** veld.

5.  In de regio beschikbaarheid pop, toevoegen of verwijderen van de regio's voor deze SKU.

6.  Klik op **publiceren** om de werkstroom publiceren voor het bijwerken van de beschikbaarheid van de regio van uw SKU's te starten.

Als u een SKU wordt beschikbaar gesteld in een nieuwe regio, hebt u de mogelijkheid om op te geven van de prijzen voor die bepaalde regio via het **prijzen gegevens exporteren** functionaliteit. Als u een regio weer die is zodra dit beschikbaar is voor toevoegt, wordt het niet mogelijk om bij te werken met de prijzen omdat prijswijzigingen zijn niet toegestaan.


### <a name="add-a-new-sku"></a>Voeg een nieuwe SKU toe

Gebruik de volgende stappen uit om een nieuwe Voorraadeenheid beschikbaar voor uw bestaande aanbieding: 

1.  Meld u aan bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).

2.  Onder **alle aanbiedingen** vinden van de aanbieding die u wilt bijwerken.

3.  Onder de **SKU's** tabblad, klikt u op **toevoegen van nieuwe Voorraadeenheid** en bieden een **SKU-ID** in het pop-upvenster.

4.  Publiceren van de virtuele machine zoals beschreven in het artikel [publiceren van een virtuele machine op Azure Marketplace](./cpp-publish-offer.md).

5.  Klik op **publiceren** om de werkstroom voor het publiceren van uw nieuwe SKU te starten.


### <a name="update-offer-marketplace-metadata"></a>Metagegevens van marketplace-aanbieding update

Gebruik de volgende stappen om bij te werken van de metagegevens van de marketplace, bedrijf naam, logo's, enzovoort, die zijn gekoppeld aan uw aanbieding: 

1.  Meld u aan bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).

2.  Onder **alle aanbiedingen** vinden van de aanbieding die u wilt bijwerken.

3.  Ga naar de **Marketplace** tabblad en volg de instructies in het artikel [publiceren van een virtuele machine op Azure Marketplace](./cpp-publish-offer.md) metagegevens wijzigingen aanbrengen.

4.  Klik op **publiceren** om de werkstroom voor het publiceren van uw wijzigingen te starten.


### <a name="update-pricing-on-published-offers"></a>De prijzen van gepubliceerde aanbiedingen bijwerken

Zodra uw betalen per gebruik-aanbieding is gepubliceerd, kan niet rechtstreeks verhoogt u de SKU-prijzen.  (, U kunt een nieuwe Voorraadeenheid onder de dezelfde aanbieding maken, verwijderen van de oude SKU en vervolgens opnieuw publiceren van uw aanbieding voor nieuwe klanten.)  U kunt daarentegen verlagen de prijs van een gepubliceerde aanbieding met behulp van de volgende stappen uit:

1.  Meld u aan bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).

2.  Onder **alle aanbiedingen**, vinden de aanbieding om bij te werken.

3.  Klik op de SKU die u wilt verkleinen prijzen.

4.  Als u hebt ingesteld de prijzen in de 1 x 1 GUI, kunt u de prijs rechtstreeks in de gebruikersinterface. Als u via importeren/exporteren werkblad prijzen hebt ingesteld, kunt u alleen prijzen via de functie voor importeren/exporteren verminderen.

3.  Klik op **Opslaan**.

4.  Klik op **publiceren** om de werkstroom voor het publiceren van uw wijzigingen te starten.

De nieuwe lagere prijzen zijn zichtbaar voor nieuwe klanten zodra ze actief zijn op de website is.  Deze nieuwe prijs invloed op uw klanten in de volgende manieren:

- Nieuwe klanten wordt deze nieuwe tarief gefactureerd. 
- Voor bestaande klanten worden, de afname van de prijs met terugwerkende kracht weergegeven aan het begin van de factureringscyclus gedurende welke de afname van de prijs werd van kracht.
Als ze al zijn gefactureerd voor de cyclus waarbij er een lagere prijs is opgetreden, ontvangt deze een restitutie tijdens de volgende factureringscyclus voor de lagere prijs.


<!-- TD: This has been implemented, need to change the SKU Tab topic to reflect and move this section there. -->
### <a name="simplified-currency-pricing"></a>Vereenvoudigde valuta prijzen

Vanaf 1 September-2018, een nieuwe sectie met de naam **vereenvoudigd valuta prijzen** wordt toegevoegd aan de portal. Microsoft is de Azure Marketplace-bedrijven stroomlijnen door in te schakelen van meer voorspelbare prijzen en verzamelingen van uw klanten over de hele wereld. Deze stroomlijnen bevat het aantal valuta waarin wij factureren van uw klanten te verminderen.

De nieuwe sectie duurt prijzen in deze nieuwe valuta's. Wanneer alle klanten zijn gemigreerd naar deze nieuwe verwerken valuta's, de oorspronkelijke prijzensectie wordt beëindigd en alleen de sectie vereenvoudigd valuta-prijzen blijven.

Hebt u tot 1 November 2018 om in te stellen van een nieuwe prijs voor de regio's waarin de valuta verwerken wordt gewijzigd. Niet mogelijk de prijs voor regio's waarin de valuta verwerken niet is gewijzigd, te verhogen.

> [!NOTE] 
> Als u API's gebruiken voor het publiceren van uw aanbieding, ziet u mogelijk een nieuwe sectie in de JSON bieden. Dit zou worden voorzien van een `virtualMachinePricingV2` of `monthlyPricingV2`, afhankelijk van het type aanbieding. 

Als u vragen hebt over deze wijziging hebt, neem dan contact op met [ondersteuning voor Azure Marketplace](../../support-azure-marketplace.md).


## <a name="compare-feature"></a>Functie vergelijken

Wanneer u wijzigingen in een al gepubliceerde aanbieding aanbrengt, kunt u gebruikmaken van de **vergelijken** functie om te controleren van de wijzigingen die zijn gemaakt. Deze functie wilt gebruiken:

1.  Op elk gewenst moment tijdens het bewerken, klikt u op de **vergelijken** knop van uw aanbieding.

    ![Knop vergelijken](./media/publishvm_037.png)


2.  Side-by-side-versies van marketing activa en metagegevens weergeven.


## <a name="history-of-publishing-actions"></a>Geschiedenis van de publicatie van acties

Als u wilt een historische publicatie-activiteit weergeven, klikt u op de **geschiedenis** item in de menubalk van de navigatiebalk aan de linkerkant van de Cloud Partner-Portal. Hier zich kunt u kunt voorzien van een tijdstempel acties die zijn ondernomen tijdens de levensduur van uw Azure Marketplace-aanbiedingen weergeven.  
<!-- TD: Add after section authored: For more information, see [History page](../portal-tour/cpp-history-page.md). -->

