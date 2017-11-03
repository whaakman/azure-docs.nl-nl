---
title: Het beheren van uw installatiekopie van de virtuele machine in Azure Marketplace | Microsoft Docs
description: "Gedetailleerde richtlijnen voor het beheren van uw installatiekopie van de virtuele machine in Azure Marketplace na de initiële publicatie"
services: Azure Marketplace
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: cc8648d4-59c2-4678-b47d-992300677537
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/03/2016
ms.author: hascipio;
ms.openlocfilehash: e1f90650e71345957c2d353774cb8bef62c1868b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="post-production-guide-for-virtual-machine-offers-in-the-azure-marketplace"></a>Na productie-handleiding voor aanbiedingen van de virtuele machine in Azure Marketplace
Dit artikel wordt uitgelegd hoe u een aanbieding live virtuele machine in Azure Marketplace kunt bijwerken. Dit leidt u door het proces van een of meer nieuwe SKU's toevoegen aan een bestaande aanbieding. Ook leidt u door het proces van het verwijderen van een aanbieding van live virtuele machine of de SKU van de Marketplace.

Nadat een aanbieding/SKU tijdelijk worden opgeslagen de [Azure-portal](http://portal.azure.com), kunt u de volgende tekstvakken niet wijzigen:

* **ID bieden**: In de portal voor publiceren, gaat u naar **virtuele machines** en selecteer uw aanbieding. Klik vervolgens op **VM-INSTALLATIEKOPIEËN** > **bieden id**.
* **SKU-id**: In de portal voor publiceren, gaat u naar **virtuele machines** en selecteer uw aanbieding. Klik vervolgens op **SKU's** > **toevoegen van een SKU**.
* **Publisher Namespace**: In de portal voor publiceren, gaat u naar **virtuele machines** > **scenario** > **Vertel ons over uw bedrijf** (te vinden onder 'Stap 2 registreren van uw bedrijf') > **Publisher Namespace** > **Namespace**.

Nadat de aanbieding/SKU wordt vermeld in de [Marketplace](http://azure.microsoft.com/marketplace), kunt u de volgende tekstvakken niet wijzigen:

* **ID bieden**: In de portal voor publiceren, gaat u naar **virtuele machines** en selecteer uw aanbieding. Klik vervolgens op **VM-INSTALLATIEKOPIEËN** > **bieden id**.
* **SKU-id**: In de portal voor publiceren, gaat u naar **virtuele machines** en selecteer uw aanbieding. Klik vervolgens op **SKU's** > **toevoegen van een SKU**.
* **Publisher Namespace**: In de portal voor publiceren, gaat u naar **virtuele machines** > **scenario** > **Vertel ons over uw bedrijf** (te vinden onder "Stap 2 registreren") **Publisher Namespace** > **Namespace**.
* **Poorten**: In de portal voor publiceren, gaat u naar **virtuele machines** en selecteer uw aanbieding. Klik vervolgens op **VM-INSTALLATIEKOPIEËN** > **poorten openen**.
* **Wijziging van de vermelde SKU(s) prijzen**
* **Facturering model wijziging van de vermelde SKU(s)**
* **Verwijderen van de gebieden van de vermelde SKU(s) facturering**
* **Het aantal gegevensschijven van de vermelde SKU(s) wijzigen**

## <a name="update-the-technical-details-of-a-sku"></a>De technische details van een SKU bijwerken
Een nieuwe versie op de vermelde SKU toevoegen en uw aanbieding publiceren, als volgt te werk:

1. Aanmelden bij de [portal publiceren](https://publish.windowsazure.com).
2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu links op de **VM-INSTALLATIEKOPIEËN** tabblad.
4. In de **SKU's** sectie, zoek de SKU die u wilt bijwerken.
5. Een nieuwe versienummer voor de SKU toevoegen en klik op de  **+**  knop. De nieuwe versie moet in een indeling X.Y.Z, waarbij X, Y en Z gehele getallen zijn. Wijzigingen in versie moet alleen incrementele.
6. In de **OS VHD URL** vak, voer de shared access signature URI voor de besturingssysteem-VHD gemaakt en de wijzigingen opslaan.

   > [!IMPORTANT]
   > U kan geen incrementele/verlagen het aantal gegevensschijven van een vermelde SKU. U moet een nieuwe SKU in dit geval maken. Raadpleeg het gedeelte voor gedetailleerde richtlijnen [toevoegen van een nieuwe SKU onder een vermelde aanbieding](#add-a-new-sku-under-a-listed-offer).
   >
   >
7. Ga naar de **publiceren** tabblad en klik op **PUSH FASERING**. Zie voor gedetailleerde instructies over het testen van uw aanbieding in de testomgeving [testen van uw VM-aanbieding voor de Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Nadat u uw aanbieding in fasering getest hebt, gaat u naar de **publiceren** tabblad in de publicatie van de portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** uw aanbieding in de Marketplace opnieuw te publiceren.

    ![VM-installatiekopieën](media/marketplace-publishing-vm-image-post-publishing/img01_07.png)

## <a name="update-the-nontechnical-details-of-an-offer-or-a-sku"></a>Bijwerken van de niet-technische details van een aanbieding of een SKU
U kunt de niet-technische bijwerken (marketing, juridische, ondersteunen, categorieën) details van uw live aanbieding of SKU in de Marketplace.

### <a name="update-the-offer-description-and-logos"></a>De beschrijving van de aanbieding en logo's bijwerken
De details van de aanbieding bijwerken en uw aanbieding publiceren, als volgt te werk:

1. Aanmelden bij de [portal publiceren](https://publish.windowsazure.com).
2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu links op de **MARKETING** tabblad.
4. Klik op **Engels (V.S.)**.
5. Klik op de **DETAILS** tabblad. In de **beschrijving** sectie, het bijwerken van de aanbieding **titel**, **samenvatting**, en **LONG SUMMARY** en de wijzigingen op te slaan.

   > [!NOTE]
   > Wanneer u de details van de SKU bijwerkt, worden op de hoogte van deze beperkingen: 
   * Geef geen dubbele tekst voor de beschrijving van de aanbieding en de SKU-beschrijving.
   * Geef geen dubbele tekst voor de SKU-titel en de lange samenvatting aanbieding. 
   * Geef geen dubbele tekst voor de SKU-titel en de samenvatting van de aanbieding.
   >
   >

    ![Details](media/marketplace-publishing-vm-image-post-publishing/img02.1_05.png)
6. In de **logo's** sectie van de **DETAILS** tabblad, het bijwerken van de logo's. Zorg ervoor dat de logo's volgt de [richtlijnen voor Azure Marketplace](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).

   > [!NOTE]
   > Een pictogram hero is optioneel. U kunt geen upload een pictogram hero. Nadat een pictogram hero is geüpload, is er echter geen richten voor verwijderen uit de publicatie portal. Ga als volgt de [hero pictogram richtlijnen](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
   >
   >
7. Ga naar de **publiceren** tabblad en klik op **PUSH FASERING**. Zie voor gedetailleerde instructies over het testen van uw aanbieding in de testomgeving [testen van uw VM-aanbieding voor de Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Nadat u uw aanbieding in fasering getest hebt, gaat u naar de **publiceren** tabblad in de publicatie van de portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** uw aanbieding in de Marketplace opnieuw te publiceren.

    ![Logo 's](media/marketplace-publishing-vm-image-post-publishing/img02.1_08.png)

### <a name="update-the-sku-description"></a>De SKU-beschrijving van update
De details van de SKU bijwerken en uw aanbieding publiceren, als volgt te werk:

1. Aanmelden bij de [portal publiceren](https://publish.windowsazure.com).
2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu links op de **MARKETING** tabblad.
4. Klik op **Engels (V.S.)**.
5. Klik op de **plannen** tabblad. In de **SKU's** sectie, het bijwerken van de SKU **titel**, **samenvatting**, en **beschrijving** en de wijzigingen op te slaan.

   > [!NOTE]
   > Wanneer u de details van de SKU bijwerkt, worden op de hoogte van deze beperkingen: 
   * Geef geen dubbele tekst voor de beschrijving van de aanbieding en de SKU-beschrijving. 
   * Geef geen dubbele tekst voor de SKU-titel en de lange samenvatting aanbieding. 
   * Geef geen dubbele tekst voor de SKU-titel en de samenvatting van de aanbieding.
   >
   >
6. Ga naar de **publiceren** tabblad en klik op **PUSH FASERING**. Zie voor gedetailleerde instructies over het testen van uw aanbieding in de testomgeving [testen van uw VM-aanbieding voor de Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
7. Nadat u uw aanbieding in fasering getest hebt, gaat u naar de **publiceren** tabblad in de publicatie van de portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** uw aanbieding in de Marketplace opnieuw te publiceren.

    ![Abonnementen](media/marketplace-publishing-vm-image-post-publishing/img02.2_07.png)

### <a name="change-existing-links-or-add-new-links"></a>Bestaande koppelingen wijzigen of toevoegen van nieuwe koppelingen
Bestaande koppelingen wijzigen of nieuwe koppelingen toevoegen en vervolgens opnieuw uw aanbieding publiceren, als volgt te werk:

1. Aanmelden bij de [portal publiceren](https://publish.windowsazure.com).
2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu links op de **MARKETING** tabblad.
4. Klik op **Engels (V.S.)**.
5. Klik op de **koppelingen** tabblad.
6. Een nieuwe koppeling toevoegen in de **koppelingen** sectie, klikt u op **+ koppeling toevoegen**. In de **koppeling toevoegen** dialoogvenster vak, voert u de koppeling **titel** en **URL** en de wijzigingen op te slaan. U kunt een koppeling met informatie waarmee klanten invoeren.
7. Selecteer de koppeling als u wilt bijwerken of verwijderen van een bestaande koppeling, en klik op de **bewerken** knop of de **verwijderen** knop.

   > [!NOTE]
   > Zorg ervoor dat de koppelingen die u hebt ingevoerd in deze sectie naar behoren werken omdat deze koppelingen ophalen gevalideerd tijdens het aanvraagproces van uw productieomgeving.
   >
   >
8. Ga naar de **publiceren** tabblad en klik op **PUSH FASERING**. Zie voor gedetailleerde instructies over het testen van uw aanbieding in de testomgeving [testen van uw VM-aanbieding voor de Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
9. Nadat u uw aanbieding in fasering getest hebt, gaat u naar de **publiceren** tabblad in de publicatie van de portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** uw aanbieding in de Marketplace opnieuw te publiceren.

    ![Koppelingen](media/marketplace-publishing-vm-image-post-publishing/img02.3_09-01.png)

    ![Koppeling toevoegen](media/marketplace-publishing-vm-image-post-publishing/img02.3-2.png)

### <a name="change-an-existing-sample-image-or-add-a-new-sample-image"></a>Een bestaande installatiekopie van het voorbeeld wijzigen of toevoegen van een nieuwe installatiekopie van het voorbeeld
Een bestaande installatiekopie van het voorbeeld wijzigen of nieuwe voorbeeld installatiekopieën toevoegen en vervolgens opnieuw uw aanbieding publiceren, als volgt te werk:

> [!NOTE]
> Slechts één voorbeeld van afbeelding wordt weergegeven in de [Azure-portal](https://portal.azure.com).
>
>

1. Aanmelden bij de [portal publiceren](https://publish.windowsazure.com).
2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu links op de **MARKETING** tabblad.
4. Klik op **Engels (V.S.)**.
5. Klik op de **voorbeeld INSTALLATIEKOPIEËN** tabblad.
6. Toevoegen van een nieuwe installatiekopie voor het voorbeeld in de **voorbeeld installatiekopieën** sectie, klikt u op **nieuwe INSTALLATIEKOPIE uploaden** en de wijzigingen op te slaan.

   > [!NOTE]
   > Inclusief de installatiekopie van een voorbeeld is optioneel.
   >
7. Ga naar de **publiceren** tabblad en klik op **PUSH FASERING**. Zie voor gedetailleerde instructies over het testen van uw aanbieding in de testomgeving [testen van uw VM-aanbieding voor de Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Nadat u uw aanbieding in fasering getest hebt, gaat u naar de **publiceren** tabblad in de publicatie van de portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** uw aanbieding in de Marketplace opnieuw te publiceren.

    ![Installatiekopieën van het voorbeeld](media/marketplace-publishing-vm-image-post-publishing/img02.4_09.png)

### <a name="update-the-legal-content"></a>De juridische inhoud bijwerken
Bijwerken van de juridische inhoud en uw aanbieding publiceren, als volgt te werk:

1. Aanmelden bij de [portal publiceren](https://publish.windowsazure.com).
2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu links op de **MARKETING** tabblad.
4. Klik op **Engels (V.S.)**.
5. Klik op de **juridische** tabblad. In de **juridische** sectie, het bijwerken van uw beleid/gebruiksvoorwaarden. Typt of plakt u de beleidsregels/voorwaarden in de **gebruiksvoorwaarden** en sla de wijzigingen.
6. Het aantal tekens voor de juridische gebruiksvoorwaarden is 1 miljoen tekens.
7. Ga naar de **publiceren** tabblad en klik op **PUSH FASERING**. Zie voor gedetailleerde instructies over het testen van uw aanbieding in de testomgeving [testen van uw VM-aanbieding voor de Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Nadat u uw aanbieding in fasering getest hebt, gaat u naar de **publiceren** tabblad in de publicatie van de portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** uw aanbieding in de Marketplace opnieuw te publiceren.

    ![Juridische informatie](media/marketplace-publishing-vm-image-post-publishing/img02.5_08.png)

### <a name="update-the-support-information"></a>Bijwerken van de informatie over ondersteuning
Bijwerken van de informatie over ondersteuning en uw aanbieding publiceren, als volgt te werk:

1. Aanmelden bij de [portal publiceren](https://publish.windowsazure.com).
2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu links op de **ondersteuning** tabblad.
4. In de **Engineering, neem contact op met** sectie, het bijwerken van de gegevens van de technische contactpersoon. Deze gegevens worden alleen voor interne communicatie tussen de partner en Microsoft gebruikt.
5. In de **Customer Support** sectie, het bijwerken van de contactgegevens voor ondersteuning en de **ONDERSTEUNEN URL**. Deze gegevens worden alleen voor interne communicatie tussen de partner en Microsoft gebruikt.

   > [!NOTE]
   > Als u bieden alleen ondersteuning voor e-mail wilt, voert u een dummy-telefoonnummer in de **Customer Support** sectie. In dit geval wordt de e-mail die u hebt opgegeven in plaats daarvan gebruikt.
   >
   >
6. Ga naar de **publiceren** tabblad en klik op **PUSH FASERING**. Zie voor gedetailleerde instructies over het testen van uw aanbieding in de testomgeving [testen van uw VM-aanbieding voor de Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
7. Nadat u uw aanbieding in fasering getest hebt, gaat u naar de **publiceren** tabblad in de publicatie van de portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** uw aanbieding in de Marketplace opnieuw te publiceren.

    ![Ondersteuning](media/marketplace-publishing-vm-image-post-publishing/img02.6_07.png)

### <a name="update-the-categories"></a>Bijwerken van de categorieën
Update de categoriesectie voor uw aanbieding en uw aanbieding publiceren, als volgt te werk:

1. Aanmelden bij de [portal publiceren](https://publish.windowsazure.com).
2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu links op de **categorieën** tabblad.
4. In de **categorieën** sectie, het bijwerken van de categorieën voor uw aanbieding en sla de wijzigingen. U kunt maximaal vijf categorieën voor de galerie van Azure Marketplace.
5. Ga naar de **publiceren** tabblad en klik op **PUSH FASERING**. Zie voor gedetailleerde instructies over het testen van uw aanbieding in de testomgeving [testen van uw VM-aanbieding voor de Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
6. Nadat u uw aanbieding in fasering getest hebt, gaat u naar de **publiceren** tabblad in de publicatie van de portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** uw aanbieding in de Marketplace opnieuw te publiceren.

    ![Categorieën](media/marketplace-publishing-vm-image-post-publishing/img02.7_06.png)

## <a name="add-a-new-sku-under-a-listed-offer"></a>Voeg een nieuwe SKU onder een vermelde aanbieding
U een nieuwe SKU toevoegen aan uw live aanbieding; de volgende stappen uit:

1. Aanmelden bij de [portal publiceren](https://publish.windowsazure.com).
2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu links op de **SKU's** tabblad. Klik vervolgens op **toevoegen van een SKU**. 
4. Voer in het dialoogvenster een **SKU-id** in kleine letters. Selecteer de **brengt uw eigen factureringsmodel license (BYOL)** selectievakje in als u wilt publiceren van de nieuwe SKU met een facturering BYOL-model. Schakel het selectievakje anders uit. Klik op het maatstreepje voor het maken van een nieuwe SKU. Als u niet het facturering model BYOL kiest, wordt het facturering model automatisch ingesteld op elk uur. Als u wilt dat de gratis proefversie van 30 dagen voor de per uur facturering model, selecteert u **één maand** voor **Is een gratis proefversie beschikbaar?** Selecteer anders **Nee proefversie**. (**Is een gratis proefversie beschikbaar?**  wordt alleen weergegeven als u geen BYOL hebt geselecteerd tijdens het maken van de nieuwe SKU.)

   > [!IMPORTANT]
   > **De SKU van de Marketplace verbergen omdat moet altijd worden aangekocht via een oplossingssjabloon** moet **Ja** *alleen* als u bent goedgekeurd voor het publiceren van een oplossingssjabloon. Anders wordt deze optie altijd moet **Nee**.
   >
   >
4. Klik in het menu links op de **VM-INSTALLATIEKOPIEËN** tabblad en ontdek de nieuwe SKU die u hebt gemaakt.
5. Als u de nieuwe SKU instelt, Zie [behalen van certificaten voor uw VM-installatiekopie](marketplace-publishing-vm-image-creation.md#5-obtain-certification-for-your-vm-image) voor hulp.
6. Zie het toevoegen van marketingmateriaal voor de nieuwe SKU [bieden Marketplace marketing inhoud](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
7. Zie het toevoegen van informatie over de prijzen voor de nieuwe SKU [instellen van uw prijzen](marketplace-publishing-push-to-staging.md#step-2-set-your-prices).
8. Ga naar de **publiceren** tabblad en klik op **PUSH FASERING**. Zie voor gedetailleerde instructies over het testen van uw aanbieding in de testomgeving [testen van uw VM-aanbieding voor de Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
9. Nadat u uw aanbieding in fasering getest hebt, gaat u naar de **publiceren** tabblad in de publicatie van de portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** uw aanbieding in de Marketplace opnieuw te publiceren.

    ![SKU 's](media/marketplace-publishing-vm-image-post-publishing/img03_09-01.png)

    ![Toevoegen van een SKU](media/marketplace-publishing-vm-image-post-publishing/img03_09-02.png)

## <a name="change-the-data-disk-count-for-a-listed-sku"></a>Wijzig het aantal gegevensschijven voor een lijst SKU
U kan geen incrementele/verlagen het aantal gegevensschijven van een vermelde SKU. U moet een nieuwe SKU in dit geval maken. Raadpleeg het gedeelte voor gedetailleerde richtlijnen [toevoegen van een nieuwe SKU onder een vermelde aanbieding](#add-a-new-sku-under-a-listed-offer).

## <a name="delete-a-listed-offer-from-the-marketplace"></a>Een vermelde aanbieding verwijderen uit de Marketplace
Verschillende aspecten moeten worden afgehandeld in het geval van een verzoek om te verwijderen van een live aanbieding. Als u de richtlijnen van het ondersteuningsteam een vermelde aanbieding verwijderen uit de Marketplace, de volgende stappen uit:

1. Een ondersteuningsticket genereren op de [een incident maken](https://support.microsoft.com/en-us/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&locale=en-us&supportregion=en-us&pesid=15635&ccsid=635993707583706681) pagina.

2. Selecteer **probleemtype** als **aanbiedingen beheren**, en selecteer **categorie** als **al een aanbieding en/of SKU wijzigen in productie**.
3. De aanvraag indienen.

Het ondersteuningsteam begeleidt u bij de verwijdering van de aanbieding/SKU.

> [!NOTE]
> U kunt altijd de aanbieding verwijderen terwijl het de status concept (maar niet fasering of productie). Op de **geschiedenis** tabblad **concept verwijderen**.
>
>

## <a name="delete-a-listed-sku-from-the-marketplace"></a>Een vermelde SKU verwijderen uit de Marketplace
Als u wilt verwijderen een vermelde SKU van Marketplace, de volgende stappen uit:

1. Aanmelden bij de [portal publiceren](https://publish.windowsazure.com).

2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het deelvenster aan de linkerkant op de **SKU's** tabblad.
4. Selecteer de SKU die u wilt verwijderen en klik op de **verwijderen** knop.
5. Ga naar de **publiceren** tabblad in de publicatie van de portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** te publiceren van de aanbieding in de Marketplace.
6. Nadat de aanbieding wordt gepubliceerd in de Marketplace, wordt de SKU van de Marketplace en de Azure portal verwijderd.

## <a name="delete-the-current-version-of-a-listed-sku-from-the-marketplace"></a>De huidige versie van een vermelde SKU verwijderen uit de Marketplace
Voor het verwijderen van de huidige versie van een vermelde SKU van Marketplace, de volgende stappen uit: 

1. Aanmelden bij de [portal publiceren](https://publish.windowsazure.com).

2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu links op de **VM-INSTALLATIEKOPIEËN** tabblad.
4. Selecteer de SKU waarvan u wilt verwijderen en klik op de huidige versie de **verwijderen** knop.
5. Ga naar de **publiceren** tabblad in de publicatie van de portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** te publiceren van de aanbieding in de Marketplace.
6. Nadat de aanbieding wordt opnieuw gepubliceerd in de Marketplace, wordt de huidige versie van de vermelde SKU verwijderd uit de Marketplace en de Azure portal. De SKU vervolgens teruggedraaid naar de vorige versie.

## <a name="revert-the-listing-price-to-production-values"></a>De aanbieding prijs naar productiewaarden herstellen
Als u de aanbieding prijs naar productiewaarden, de volgende stappen uit:

1. Aanmelden bij de [portal publiceren](https://publish.windowsazure.com).
2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu links op de **prijzen** tabblad.
4. Selecteer een regio waarvan prijzen die u wilt instellen.

    ![Prijzen van regio 's](media/marketplace-publishing-vm-image-post-publishing/img08-04.png)
5. Voor SKU's met een uur facturering model reset de prijzen voor alle kernen ze zich in productie voor de geselecteerde regio. Voor SKU's met een factureringsmodel BYOL de SKU beschikbaar maken in de regio door het selectievakje in voor de SKU in de **EXTERNALLY-LICENSED (BYOL) SKU beschikbaarheid** sectie.

    ![Prijsmodellen](media/marketplace-publishing-vm-image-post-publishing/img08-05.png)
6. Klik op **AUTOPRICE andere markten op basis van de prijzen IN de Verenigde Staten**.

   > [!NOTE]
   > De knop label kan afwijken afhankelijk van de regio die u selecteert. Omdat we Verenigde Staten hebt geselecteerd, wordt de knop label **AUTOPRICE andere markten gebaseerd op prijzen IN de Verenigde Staten**.
   >
   >

    ![Autoprice](media/marketplace-publishing-vm-image-post-publishing/img08-06.png)
7. Op pagina 1 van de wizard Autoprice, kies de base markt en op de **pijl** knop.

    ![Base markt](media/marketplace-publishing-vm-image-post-publishing/img08-07.png)
8. Op pagina 2, service-abonnementen en meters (kernen) kiezen en op de **pijl** knop.

    ![Service-abonnementen en meters](media/marketplace-publishing-vm-image-post-publishing/img08-08.png)
9. Klik op de pagina 3 **wisselknop alle** alle regio's selecteren. Ook kunt u de selectievakjes voor specifieke regio's handmatig selecteren. Klik op de **pijl** knop.

    ![Kies markten](media/marketplace-publishing-vm-image-post-publishing/img08-09.png)
10. Op pagina 4, Controleer de wisselkoersen en klik op **voltooien**. De wizard Hiermee stelt u de prijzen volgens uw selecties.

11. Op de **prijzen** tabblad **weergave Samenvatting en wijzigingen**.
    Voor **versie weergeven**, selecteer **concept**, en voor **vergelijken met**, selecteer **productie**. Als u geen prijzen verschil ziet, teruggezet de prijzen naar de productiewaarden.

    ![Samenvatting weergeven en wijzigingen](media/marketplace-publishing-vm-image-post-publishing/img08-11.png)
12. Ga naar de **publiceren** tabblad en klik op **PUSH FASERING**. Zie voor gedetailleerde instructies over het testen van uw aanbieding in de testomgeving [testen van uw VM-aanbieding voor de Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
13. Nadat u uw aanbieding in fasering getest hebt, gaat u naar de **publiceren** tabblad in de publicatie van de portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** uw aanbieding in de Marketplace opnieuw te publiceren.

## <a name="revert-the-billing-model-to-production-values"></a>Het facturering model naar productiewaarden herstellen
Als u wilt herstellen het facturering model voor productiewaarden, de volgende stappen uit:

1. Aanmelden bij de [portal publiceren](https://publish.windowsazure.com).

2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu links op de **SKU's** tabblad.
4. Klik op de **bewerken** knop om het facturering model terug te zetten. Selecteer in het venster dat wordt geopend, of schakel de **facturering en licenties extern van Azure (aka Bring Your Own License) wordt uitgevoerd** selectievakje.

    ![Facturering bewerken](media/marketplace-publishing-vm-image-post-publishing/img09-04.png)
5. Volg de stappen in 'De aanbieding prijs naar productiewaarden Revert' in dit artikel.
6. Ga naar de **publiceren** tabblad en klik op **PUSH FASERING**. Zie voor gedetailleerde instructies over het testen van uw aanbieding in de testomgeving [testen van uw VM-aanbieding voor de Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
7. Nadat u uw aanbieding in fasering getest hebt, gaat u naar de **publiceren** tabblad in de publicatie van de portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** uw aanbieding in de Marketplace opnieuw te publiceren.

## <a name="revert-the-visibility-setting-of-a-listed-sku-to-the-production-value"></a>De zichtbaarheidsinstelling van een vermelde SKU om de waarde van de productie te herstellen
Als u de zichtbaarheidsinstelling van een vermelde SKU voor de waarde van de productie, de volgende stappen uit:

1. Aanmelden bij de [portal publiceren](https://publish.windowsazure.com).

2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu links op de **SKU's** tabblad.
4. Selecteer uw SKU en de zichtbaarheidsinstelling van de SKU op de productiewaarde te herstellen.

    ![Zichtbaarheid](media/marketplace-publishing-vm-image-post-publishing/img10-04.png)
5. Nadat u met de wijzigingen bent klaar, klikt u op **aanvragen goedkeuring te PUSHEN naar productie** uw aanbieding in de Marketplace opnieuw te publiceren.

## <a name="see-also"></a>Zie ook
* [Aan de slag: Een aanbieding publiceren in Azure Marketplace](marketplace-publishing-getting-started.md)
* [Toekenning reporting begrijpen](marketplace-publishing-report-payout.md)
* [Uw wederverkoper Cloud Solution Provider stimulans wijzigen](marketplace-publishing-csp-incentive.md)
* [Algemene problemen publiceren in de Marketplace](marketplace-publishing-support-common-issues.md)
* [Ondersteuning krijgen als een publisher](marketplace-publishing-get-publisher-support.md)
* [Maak een VM-installatiekopie lokale](marketplace-publishing-vm-image-creation-on-premise.md)
* [Maak een virtuele machine waarop Windows wordt uitgevoerd in de Azure preview-portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
