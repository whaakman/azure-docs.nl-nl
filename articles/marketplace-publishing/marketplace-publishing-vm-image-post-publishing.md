---
title: Beheren van uw virtuele machine-installatiekopie in de Azure Marketplace | Microsoft Docs
description: Gedetailleerde richtlijnen voor het beheren van uw virtuele machine-installatiekopie in de Azure Marketplace na de initiële publicatie
services: Azure Marketplace
documentationcenter: ''
author: v-miclar
manager: hascipio
editor: ''
ms.assetid: cc8648d4-59c2-4678-b47d-992300677537
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/03/2016
ms.author: hascipio;
ROBOTS: NOINDEX
ms.openlocfilehash: 7aea357c22a928d8af96791bf9cb4bd3c94f37b4
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54077059"
---
# <a name="post-production-guide-for-virtual-machine-offers-in-the-azure-marketplace"></a>Na productie-handleiding voor de virtuele machine-aanbiedingen in de Azure Marketplace
In dit artikel wordt uitgelegd hoe u een live virtuele machine-aanbieding in Azure Marketplace kunt bijwerken. Dit leidt u door het proces van het toevoegen van een of meer nieuwe SKU's op een bestaande aanbieding. Ook leidt u door het proces van het verwijderen van een live virtuele machine-aanbieding of de SKU van de Marketplace.

Nadat een aanbieding/SKU tijdelijk worden opgeslagen de [Azure-portal](http://portal.azure.com), kunt u de volgende tekstvakken niet wijzigen:

* **ID bieden**: In de publicatie-portal, gaat u naar **virtuele machines** en selecteert u uw aanbieding. Klik vervolgens op **VM-INSTALLATIEKOPIEËN** > **bieden id**.
* **SKU-id**: In de publicatie-portal, gaat u naar **virtuele machines** en selecteert u uw aanbieding. Klik vervolgens op **SKU's** > **Voeg een SKU toe**.
* **Uitgever Namespace**: In de publicatie-portal, gaat u naar **virtuele machines** > **scenario** > **Vertel ons over uw bedrijf** (te vinden onder ' stap 2 registreren uw Bedrijf") > **Publisher Namespace** > **Namespace**.

Nadat de aanbieding/SKU wordt vermeld in de [Marketplace](https://azure.microsoft.com/marketplace), kunt u de volgende tekstvakken niet wijzigen:

* **ID bieden**: In de publicatie-portal, gaat u naar **virtuele machines** en selecteert u uw aanbieding. Klik vervolgens op **VM-INSTALLATIEKOPIEËN** > **bieden id**.
* **SKU-id**: In de publicatie-portal, gaat u naar **virtuele machines** en selecteert u uw aanbieding. Klik vervolgens op **SKU's** > **Voeg een SKU toe**.
* **Uitgever Namespace**: In de publicatie-portal, gaat u naar **virtuele machines** > **scenario** > **Vertel ons over uw bedrijf** (te vinden onder "Stap 2 registreren") **Publisher Namespace** > **Namespace**.
* **Poorten**: In de publicatie-portal, gaat u naar **virtuele machines** en selecteert u uw aanbieding. Klik vervolgens op **VM-INSTALLATIEKOPIEËN** > **Open poorten**.
* **Wijziging van de vermelde SKU('s) prijzen**
* **Facturering modelwijziging van de vermelde SKU('s)**
* **Het verwijderen van regio's van de vermelde SKU('s) facturering**
* **Het aantal gegevensschijven van vermelde SKU('s) wijzigen**

## <a name="update-the-technical-details-of-a-sku"></a>Bijwerken van de technische details van een SKU
Een nieuwe versie toevoegen aan de vermelde SKU en uw aanbod opnieuw publiceren, als volgt te werk:

1. Aanmelden bij de [portal voor publiceren](https://publish.windowsazure.com).
2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu aan de linkerkant op de **VM-INSTALLATIEKOPIEËN** tabblad.
4. In de **SKU's** sectie, Ga naar de SKU die u wilt bijwerken.
5. Voeg een nieuwe versienummer voor de SKU toe en klik op de **+** knop. De nieuwe versie moet zich in een indeling X.Y.Z, waarbij X, Y en Z gehele getallen zijn. Wijzigingen in versie moeten alleen worden incrementele.
6. In de **OS VHD URL** vak, voer de shared access signature URI voor het besturingssysteem VHD gemaakt en de wijzigingen opslaan.

   > [!IMPORTANT]
   > U kan niet verhogen/verlagen het aantal gegevensschijven van een vermelde SKU. U moet een nieuwe SKU in dit geval maken. Voor uitgebreide begeleiding, raadpleegt u de sectie [toevoegen van een nieuwe Voorraadeenheid bij een aanbieding van vermelde](#add-a-new-sku-under-a-listed-offer).
   >
   >
7. Ga naar de **publiceren** tabblad en klik op **FASERING naar**. Zie voor gedetailleerde richtlijnen voor het testen van uw aanbieding in de faseringsomgeving [testen van uw VM-aanbieding voor de Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Nadat u uw aanbieding in fasering hebt getest, gaat u naar de **publiceren** tabblad in de publicatie portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** om opnieuw te publiceren van uw aanbieding op Marketplace.

    ![VM-installatiekopieën](media/marketplace-publishing-vm-image-post-publishing/img01_07.png)

## <a name="update-the-nontechnical-details-of-an-offer-or-a-sku"></a>Bijwerken van de niet-technische details van een aanbieding of een SKU
U kunt de niet-technische bijwerken (marketing, juridische, ondersteuning, categorieën) details van uw live-aanbieding of de SKU in de Marketplace.

### <a name="update-the-offer-description-and-logos"></a>De beschrijving van aanbieding en logo's bijwerken
Details van de aanbieding bijwerken en uw aanbod opnieuw publiceren, als volgt te werk:

1. Aanmelden bij de [portal voor publiceren](https://publish.windowsazure.com).
2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu aan de linkerkant op de **MARKETING** tabblad.
4. Klik op **Engels (V.S.)**.
5. Klik op de **DETAILS** tabblad. In de **beschrijving** sectie, bij te werken van de aanbieding **titel**, **samenvatting**, en **LONG SUMMARY** en sla de wijzigingen op.

   > [!NOTE]
   > Wanneer u de details van de SKU bijwerkt, worden op de hoogte van de volgende beperkingen: 
   * Geef geen dubbele tekst voor de beschrijving van de aanbieding en de beschrijving van de SKU.
   * Geef geen dubbele tekst voor de SKU-titel en de lange samenvatting aanbieding. 
   * Geef geen dubbele tekst voor de SKU-titel en samenvatting van de aanbieding.
   >
   >

    ![Details](media/marketplace-publishing-vm-image-post-publishing/img02.1_05.png)
6. In de **logo's** sectie van de **DETAILS** tabblad, het bijwerken van de logo's. Zorg ervoor dat de logo's gaat u als volgt de [richtlijnen voor Azure Marketplace](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).

   > [!NOTE]
   > Een hero-pictogram is optioneel. U kunt niet een hero-pictogram uploaden. Nadat een hero-pictogram is geüpload, er is echter geen inrichten om te verwijderen uit de publicatie portal. Ga als volgt de [hero pictogram richtlijnen](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
   >
   >
7. Ga naar de **publiceren** tabblad en klik op **FASERING naar**. Zie voor gedetailleerde richtlijnen voor het testen van uw aanbieding in de faseringsomgeving [testen van uw VM-aanbieding voor de Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Nadat u uw aanbieding in fasering hebt getest, gaat u naar de **publiceren** tabblad in de publicatie portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** om opnieuw te publiceren van uw aanbieding op Marketplace.

    ![Logo 's](media/marketplace-publishing-vm-image-post-publishing/img02.1_08.png)

### <a name="update-the-sku-description"></a>De SKU-beschrijving van update
De SKU-details bijwerken en uw aanbod opnieuw publiceren, als volgt te werk:

1. Aanmelden bij de [portal voor publiceren](https://publish.windowsazure.com).
2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu aan de linkerkant op de **MARKETING** tabblad.
4. Klik op **Engels (V.S.)**.
5. Klik op de **plannen** tabblad. In de **SKU's** sectie, bij te werken van de SKU **titel**, **samenvatting**, en **beschrijving** en sla de wijzigingen op.

   > [!NOTE]
   > Wanneer u de details van de SKU bijwerkt, worden op de hoogte van de volgende beperkingen: 
   * Geef geen dubbele tekst voor de beschrijving van de aanbieding en de beschrijving van de SKU. 
   * Geef geen dubbele tekst voor de SKU-titel en de lange samenvatting aanbieding. 
   * Geef geen dubbele tekst voor de SKU-titel en samenvatting van de aanbieding.
   >
   >
6. Ga naar de **publiceren** tabblad en klik op **FASERING naar**. Zie voor gedetailleerde richtlijnen voor het testen van uw aanbieding in de faseringsomgeving [testen van uw VM-aanbieding voor de Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
7. Nadat u uw aanbieding in fasering hebt getest, gaat u naar de **publiceren** tabblad in de publicatie portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** om opnieuw te publiceren van uw aanbieding op Marketplace.

    ![Abonnementen](media/marketplace-publishing-vm-image-post-publishing/img02.2_07.png)

### <a name="change-existing-links-or-add-new-links"></a>Bestaande koppelingen wijzigen of nieuwe koppelingen toevoegen
Bestaande koppelingen wijzigen of nieuwe koppelingen toevoegen en vervolgens uw aanbod opnieuw publiceren, als volgt te werk:

1. Aanmelden bij de [portal voor publiceren](https://publish.windowsazure.com).
2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu aan de linkerkant op de **MARKETING** tabblad.
4. Klik op **Engels (V.S.)**.
5. Klik op de **koppelingen** tabblad.
6. Om een nieuwe koppeling de **koppelingen** sectie, klikt u op **+ koppeling toevoegen**. In de **koppeling toevoegen** dialoogvenster vak, voert u de koppeling **titel** en **URL** en sla de wijzigingen op. U kunt een koppeling met informatie die mogelijk nuttig voor klanten.
7. Als u wilt bijwerken of verwijderen van een bestaande koppeling, selecteer de koppeling en klikt u op de **bewerken** knop of de **verwijderen** knop.

   > [!NOTE]
   > Zorg ervoor dat de koppelingen die u hebt ingevoerd in deze sectie correct werken, omdat deze koppelingen gevalideerd tijdens de aanvraag voor de productie.
   >
   >
8. Ga naar de **publiceren** tabblad en klik op **FASERING naar**. Zie voor gedetailleerde richtlijnen voor het testen van uw aanbieding in de faseringsomgeving [testen van uw VM-aanbieding voor de Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
9. Nadat u uw aanbieding in fasering hebt getest, gaat u naar de **publiceren** tabblad in de publicatie portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** om opnieuw te publiceren van uw aanbieding op Marketplace.

    ![Koppelingen](media/marketplace-publishing-vm-image-post-publishing/img02.3_09-01.png)

    ![Koppeling toevoegen](media/marketplace-publishing-vm-image-post-publishing/img02.3-2.png)

### <a name="change-an-existing-sample-image-or-add-a-new-sample-image"></a>Een bestaande installatiekopie van het voorbeeld wijzigen of toevoegen van een nieuwe installatiekopie van het voorbeeld
Een bestaande installatiekopie van het voorbeeld wijzigen of toevoegen van nieuwe voorbeeldafbeeldingen en vervolgens uw aanbod opnieuw publiceren, als volgt te werk:

> [!NOTE]
> Van slechts één voorbeeldafbeelding wordt weergegeven in de [Azure-portal](https://portal.azure.com).
>
>

1. Aanmelden bij de [portal voor publiceren](https://publish.windowsazure.com).
2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu aan de linkerkant op de **MARKETING** tabblad.
4. Klik op **Engels (V.S.)**.
5. Klik op de **VOORBEELDAFBEELDINGEN** tabblad.
6. Toevoegen van een nieuwe installatiekopie van het voorbeeld in de **voorbeeldafbeeldingen** sectie, klikt u op **nieuwe INSTALLATIEKOPIE uploaden** en sla de wijzigingen op.

   > [!NOTE]
   > Met inbegrip van een voorbeeldafbeelding is optioneel.
   >
7. Ga naar de **publiceren** tabblad en klik op **FASERING naar**. Zie voor gedetailleerde richtlijnen voor het testen van uw aanbieding in de faseringsomgeving [testen van uw VM-aanbieding voor de Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Nadat u uw aanbieding in fasering hebt getest, gaat u naar de **publiceren** tabblad in de publicatie portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** om opnieuw te publiceren van uw aanbieding op Marketplace.

    ![Voorbeeldafbeeldingen](media/marketplace-publishing-vm-image-post-publishing/img02.4_09.png)

### <a name="update-the-legal-content"></a>De juridische inhoud bijwerken
De juridische inhoud bijwerken en uw aanbod opnieuw publiceren, als volgt te werk:

1. Aanmelden bij de [portal voor publiceren](https://publish.windowsazure.com).
2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu aan de linkerkant op de **MARKETING** tabblad.
4. Klik op **Engels (V.S.)**.
5. Klik op de **juridische** tabblad. In de **juridische** sectie, bij te werken uw beleidsrichtlijnen/gebruiksvoorwaarden. Typt of plakt u de beleidsregels/voorwaarden in de **gebruiksvoorwaarden** en sla de wijzigingen.
6. Het aantal tekens voor de juridische voorwaarden van het gebruik is 1 miljoen tekens.
7. Ga naar de **publiceren** tabblad en klik op **FASERING naar**. Zie voor gedetailleerde richtlijnen voor het testen van uw aanbieding in de faseringsomgeving [testen van uw VM-aanbieding voor de Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Nadat u uw aanbieding in fasering hebt getest, gaat u naar de **publiceren** tabblad in de publicatie portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** om opnieuw te publiceren van uw aanbieding op Marketplace.

    ![Juridische informatie](media/marketplace-publishing-vm-image-post-publishing/img02.5_08.png)

### <a name="update-the-support-information"></a>Bijwerken van de informatie over ondersteuning
Bijwerken van de informatie over ondersteuning en uw aanbod opnieuw publiceren, als volgt te werk:

1. Aanmelden bij de [portal voor publiceren](https://publish.windowsazure.com).
2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu aan de linkerkant op de **ondersteuning** tabblad.
4. In de **Engineering, neem contact op met** sectie, de details van de technische contactpersoon bijwerken. Deze gegevens worden gebruikt voor interne communicatie tussen de partner en Microsoft alleen.
5. In de **Customer Support** sectie, bij te werken van de contactgegevens voor ondersteuning en de **ondersteuning voor URL**. Deze gegevens worden gebruikt voor interne communicatie tussen de partner en Microsoft alleen.

   > [!NOTE]
   > Als u bieden alleen ondersteuning voor e-mailbericht wilt, voert u een dummy-telefoonnummer in de **Customer Support** sectie. In dit geval wordt de e-mail die u hebt opgegeven in plaats daarvan gebruikt.
   >
   >
6. Ga naar de **publiceren** tabblad en klik op **FASERING naar**. Zie voor gedetailleerde richtlijnen voor het testen van uw aanbieding in de faseringsomgeving [testen van uw VM-aanbieding voor de Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
7. Nadat u uw aanbieding in fasering hebt getest, gaat u naar de **publiceren** tabblad in de publicatie portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** om opnieuw te publiceren van uw aanbieding op Marketplace.

    ![Ondersteuning](media/marketplace-publishing-vm-image-post-publishing/img02.6_07.png)

### <a name="update-the-categories"></a>De categorieën bijwerken
Bijwerken van de categoriesectie voor uw aanbod en uw aanbod opnieuw publiceren, als volgt te werk:

1. Aanmelden bij de [portal voor publiceren](https://publish.windowsazure.com).
2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu aan de linkerkant op de **categorieën** tabblad.
4. In de **categorieën** sectie, bijwerken van de categorieën voor uw aanbod en de wijzigingen opslaan. U kunt maximaal vijf categorieën voor de Azure Marketplace-galerie.
5. Ga naar de **publiceren** tabblad en klik op **FASERING naar**. Zie voor gedetailleerde richtlijnen voor het testen van uw aanbieding in de faseringsomgeving [testen van uw VM-aanbieding voor de Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
6. Nadat u uw aanbieding in fasering hebt getest, gaat u naar de **publiceren** tabblad in de publicatie portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** om opnieuw te publiceren van uw aanbieding op Marketplace.

    ![Categorieën](media/marketplace-publishing-vm-image-post-publishing/img02.7_06.png)

## <a name="add-a-new-sku-under-a-listed-offer"></a>Een nieuwe SKU voor een vermelde aanbieding toevoegen
Om toe te voegen een nieuwe SKU in uw live aanbieding, de volgende stappen uit:

1. Aanmelden bij de [portal voor publiceren](https://publish.windowsazure.com).
2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu aan de linkerkant op de **SKU's** tabblad. Klik vervolgens op **Voeg een SKU toe**. 
4. Voer in het dialoogvenster een **SKU-id** in kleine letters. Selecteer de **doen om uw eigen factureringsmodel license (BYOL)** selectievakje in als u wilt publiceren van de nieuwe SKU met een BYOL-model voor facturering. Schakel anders het selectievakje in. Klik op het maatstreepje voor het maken van een nieuwe SKU. Als u de BYOL-model voor facturering niet kiest, wordt het factureringsmodel automatisch ingesteld op elk uur. Als u wilt dat de gratis proefversie van 30 dagen voor het factureringsmodel per uur, selecteert u **één maand** voor **Is een gratis proefversie beschikbaar?** Selecteer anders **geen proefversie**. (**Is een gratis proefversie beschikbaar?**  wordt alleen weergegeven als u geen BYOL tijdens het maken van de nieuwe SKU hebt geselecteerd.)

   > [!IMPORTANT]
   > **De SKU van de Marketplace verbergen omdat moet altijd worden gekocht via een oplossingssjabloon** moet **Ja** *alleen* als u voor het publiceren van een oplossingssjabloon worden goedgekeurd. Anders wordt deze optie moet altijd worden **Nee**.
   >
   >
4. Klik in het menu aan de linkerkant op de **VM-INSTALLATIEKOPIEËN** tabblad en ontdek de nieuwe SKU die u hebt gemaakt.
5. Als u de nieuwe SKU instelt, Zie [certificering verkrijgen voor uw VM-installatiekopie](marketplace-publishing-vm-image-creation.md#5-obtain-certification-for-your-vm-image) voor hulp.
6. Zie het marketingmateriaal voor de nieuwe SKU toevoegen [bieden Marketplace marketing inhoud](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
7. Zie het toevoegen van informatie over de prijzen voor de nieuwe SKU [uw prijzen instellen](marketplace-publishing-push-to-staging.md#step-2-set-your-prices).
8. Ga naar de **publiceren** tabblad en klik op **FASERING naar**. Zie voor gedetailleerde richtlijnen voor het testen van uw aanbieding in de faseringsomgeving [testen van uw VM-aanbieding voor de Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
9. Nadat u uw aanbieding in fasering hebt getest, gaat u naar de **publiceren** tabblad in de publicatie portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** om opnieuw te publiceren van uw aanbieding op Marketplace.

    ![Voorraadeenheden](media/marketplace-publishing-vm-image-post-publishing/img03_09-01.png)

    ![Voeg een SKU toe](media/marketplace-publishing-vm-image-post-publishing/img03_09-02.png)

## <a name="change-the-data-disk-count-for-a-listed-sku"></a>Het aantal gegevensschijven voor een vermelde SKU wijzigen
U kan niet verhogen/verlagen het aantal gegevensschijven van een vermelde SKU. U moet een nieuwe SKU in dit geval maken. Voor uitgebreide begeleiding, raadpleegt u de sectie [toevoegen van een nieuwe Voorraadeenheid bij een aanbieding van vermelde](#add-a-new-sku-under-a-listed-offer).

## <a name="delete-a-listed-offer-from-the-marketplace"></a>Een vermelde aanbieding uit Marketplace verwijderen
Diverse aspecten moeten worden afgehandeld in het geval van een aanvraag voor het verwijderen van een live-aanbieding. Voor richtlijnen van het ondersteuningsteam om te verwijderen van de aanbieding voor een lijst van de Marketplace, als volgt te werk:

1. Een ondersteuningsticket verhogen op het [maken van een incident](https://support.microsoft.com/en-us/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&locale=en-us&supportregion=en-us&pesid=15635&ccsid=635993707583706681) pagina.

2. Selecteer **probleemtype** als **beheren van aanbiedingen**, en selecteer **categorie** als **wijzigen van een aanbieding en/of SKU al in de productieomgeving**.
3. De aanvraag indienen.

Het ondersteuningsteam begeleidt u bij de verwijdering van aanbieding/SKU.

> [!NOTE]
> U kunt altijd de aanbieding verwijderen terwijl het in de status concept (maar niet fasering of productie). Op de **geschiedenis** tabblad **concept verwijderen**.
>
>

## <a name="delete-a-listed-sku-from-the-marketplace"></a>Een lijst SKU verwijderen uit de Marketplace
Als een vermelde SKU verwijderen uit de Marketplace, de volgende stappen uit:

1. Aanmelden bij de [portal voor publiceren](https://publish.windowsazure.com).

2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het deelvenster aan de linkerkant op de **SKU's** tabblad.
4. Selecteer de SKU die u wilt verwijderen, en klik op de **verwijderen** knop.
5. Ga naar de **publiceren** tabblad in de publicatie portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** om opnieuw te publiceren van de aanbieding op Marketplace.
6. Nadat de aanbieding wordt gepubliceerd in de Marketplace, wordt de SKU wordt verwijderd uit de Marketplace en de Azure-portal.

## <a name="delete-the-current-version-of-a-listed-sku-from-the-marketplace"></a>De huidige versie van een vermelde SKU verwijderen uit de Marketplace
Als de huidige versie van een vermelde SKU verwijderen uit de Marketplace, de volgende stappen uit: 

1. Aanmelden bij de [portal voor publiceren](https://publish.windowsazure.com).

2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu aan de linkerkant op de **VM-INSTALLATIEKOPIEËN** tabblad.
4. Selecteer de SKU waarvan u wilt verwijderen, en klik op de huidige versie de **verwijderen** knop.
5. Ga naar de **publiceren** tabblad in de publicatie portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** om opnieuw te publiceren van de aanbieding op Marketplace.
6. Nadat de aanbieding wordt gepubliceerd in de Marketplace, wordt de huidige versie van de vermelde SKU verwijderd uit de Marketplace en de Azure-portal. De SKU wordt vervolgens teruggedraaid naar de vorige versie.

## <a name="revert-the-listing-price-to-production-values"></a>Herstellen van de prijs van de lijst met productiewaarden
Als u wilt herstellen van de prijs van de lijst met productiewaarden, de volgende stappen uit:

1. Aanmelden bij de [portal voor publiceren](https://publish.windowsazure.com).
2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu aan de linkerkant op de **prijzen** tabblad.
4. Selecteer een regio waarvan de prijzen u opnieuw wilt instellen.

    ![Prijzen van regio 's](media/marketplace-publishing-vm-image-post-publishing/img08-04.png)
5. Opnieuw instellen van de prijzen voor alle kernen voor SKU's met een uurtarief factureringsmodel, zoals in productie voor de geselecteerde regio. Voor SKU's met een BYOL-factureringsmodel, de SKU beschikbaar maken in de regio door het selectievakje in voor de SKU in de **EXTERNALLY-LICENSED (BYOL) SKU beschikbaarheid** sectie.

    ![Prijzen modellen](media/marketplace-publishing-vm-image-post-publishing/img08-05.png)
6. Klik op **AUTOPRICE andere markten op basis van de prijzen IN de Verenigde Staten**.

   > [!NOTE]
   > Label van de knop kan afwijken afhankelijk van de regio die u selecteert. Omdat we Verenigde Staten hebt geselecteerd, de knop heeft het label **AUTOPRICE andere markten op basis van op prijzen IN de Verenigde Staten**.
   >
   >

    ![Autoprice](media/marketplace-publishing-vm-image-post-publishing/img08-06.png)
7. Op pagina 1 van de wizard Autoprice, kiest u de basis markt en op de **pijl** knop.

    ![Basis markt](media/marketplace-publishing-vm-image-post-publishing/img08-07.png)
8. Op pagina 2, kiest u service-plannen en meters (kernen) en klikt u op de **pijl** knop.

    ![Service-abonnementen en meters](media/marketplace-publishing-vm-image-post-publishing/img08-08.png)
9. Klik op de pagina 3 **in-/ uitschakelen alle** naar alle regio's selecteren. Of u kunt handmatig de selectievakjes uit voor specifieke regio's selecteren. Klik op de **pijl** knop.

    ![Kies markten](media/marketplace-publishing-vm-image-post-publishing/img08-09.png)
10. Op 4 pagina, Controleer de wisselkoersverschillen en op **voltooien**. De wizard stelt de prijzen op basis van uw selecties.

11. Op de **prijzen** tabblad **weergave Samenvatting van en wijzigingen**.
    Voor **Weergaveversie**, selecteer **Draft**, en voor **vergelijken met**, selecteer **productie**. Als u geen prijzen verschil zien, teruggezet de prijzen naar de productiewaarden.

    ![Geef een samenvatting weer en wijzigingen](media/marketplace-publishing-vm-image-post-publishing/img08-11.png)
12. Ga naar de **publiceren** tabblad en klik op **FASERING naar**. Zie voor gedetailleerde richtlijnen voor het testen van uw aanbieding in de faseringsomgeving [testen van uw VM-aanbieding voor de Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
13. Nadat u uw aanbieding in fasering hebt getest, gaat u naar de **publiceren** tabblad in de publicatie portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** om opnieuw te publiceren van uw aanbieding op Marketplace.

## <a name="revert-the-billing-model-to-production-values"></a>Het factureringsmodel naar productiewaarden herstellen
Als u wilt herstellen het factureringsmodel voor productiewaarden, de volgende stappen uit:

1. Aanmelden bij de [portal voor publiceren](https://publish.windowsazure.com).

2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu aan de linkerkant op de **SKU's** tabblad.
4. Klik op de **bewerken** knop om terug te zetten van de facturering van maandabonnementen. In het venster dat wordt geopend, selecteert u of schakelt u de **van facturering en licenties extern van Azure (ook wel Bring Your Own License) wordt uitgevoerd** selectievakje.

    ![Facturering bewerken](media/marketplace-publishing-vm-image-post-publishing/img09-04.png)
5. Volg de stappen in 'Revert de prijs van de lijst met productiewaarden ' in dit artikel.
6. Ga naar de **publiceren** tabblad en klik op **FASERING naar**. Zie voor gedetailleerde richtlijnen voor het testen van uw aanbieding in de faseringsomgeving [testen van uw VM-aanbieding voor de Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
7. Nadat u uw aanbieding in fasering hebt getest, gaat u naar de **publiceren** tabblad in de publicatie portal. Klik op **aanvragen goedkeuring te PUSHEN naar productie** om opnieuw te publiceren van uw aanbieding op Marketplace.

## <a name="revert-the-visibility-setting-of-a-listed-sku-to-the-production-value"></a>De zichtbaarheidsinstelling van een vermelde SKU op de productiewaarde ongedaan maken
Als u de zichtbaarheidsinstelling van een vermelde SKU op de productiewaarde, de volgende stappen uit:

1. Aanmelden bij de [portal voor publiceren](https://publish.windowsazure.com).

2. Ga naar de **virtuele machines** tabblad, en selecteer uw aanbieding.
3. Klik in het menu aan de linkerkant op de **SKU's** tabblad.
4. Selecteer uw SKU en herstellen van de zichtbaarheidsinstelling van de van de SKU op de productiewaarde.

    ![Zichtbaarheid](media/marketplace-publishing-vm-image-post-publishing/img10-04.png)
5. Nadat u klaar bent met de wijzigingen, klikt u op **aanvragen goedkeuring te PUSHEN naar productie** om opnieuw te publiceren van uw aanbieding op Marketplace.

## <a name="see-also"></a>Zie ook
* [Aan de slag: Een aanbieding publiceren in Azure Marketplace](marketplace-publishing-getting-started.md)
* [Inzicht in payout melden](marketplace-publishing-report-payout.md)
* [Uw Cloud Solution Provider-wederverkoper stimulans wijzigen](marketplace-publishing-csp-incentive.md)
* [Algemene problemen publiceren in de Marketplace](marketplace-publishing-support-common-issues.md)
* [Ondersteuning krijgen als uitgever](marketplace-publishing-get-publisher-support.md)
* [On-premises voor een VM-installatiekopie maken](marketplace-publishing-vm-image-creation-on-premise.md)
* [Een virtuele machine waarop Windows wordt uitgevoerd in Azure portal maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
