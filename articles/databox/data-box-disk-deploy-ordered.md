---
title: Zelfstudie om Microsoft Azure Data Box Disk te bestellen | Microsoft Docs
description: Gebruik deze zelfstudie om te leren hoe u zich registreert en een Azure Data Box Disk bestelt waarmee u gegeven in Azure importeert.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.date: 09/04/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: bd90d3c4c9207374d6a6085df6a3962ef42b68a9
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49091421"
---
# <a name="tutorial-order-an-azure-data-box-disk-preview"></a>Zelfstudie: een Azure Data Box Disk (preview) bestellen

Azure Data Box Disk is een hybride cloudoplossing waarmee u uw on-premises gegevens snel, eenvoudig en betrouwbaar in Azure importeert. U zet uw gegevens op SSD-schijven (solid-state drives) die Microsoft naar u heeft verstuurd en u stuurt de schijven terug. Deze gegevens worden vervolgens geüpload in Azure. 

In deze zelfstudie wordt beschreven hoe u een Azure Data Box Disk bestelt. In deze zelfstudie komen deze onderwerpen aan bod:

> [!div class="checklist"]
> * Registreren voor Data Box Disk
> * Een Data Box Disk bestellen
> * De bestelling volgen
> * De bestelling annuleren

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

> [!IMPORTANT]
> - Data Box Disk is in de preview-fase. Lees de [Gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing bestelt en implementeert. 
> - Tijdens de preview-fase wordt Data Box Disk verzonden naar klanten in de Verenigde Staten, West- en Noord-Europa, en Australië. Ga naar [Beschikbaarheid in de regio](data-box-disk-overview.md#region-availability) voor meer informatie.

## <a name="sign-up"></a>Aanmelden 

Data Box Disk is in preview en u moet zich registreren voor de service. Voer de volgende stappen uit om u te registreren voor de Data Box-service:

1. Meld u aan bij Azure Portal via: [https://aka.ms/azuredataboxfromdiskdocs](https://aka.ms/azuredataboxfromdiskdocs).
2. Selecteer het abonnement dat u wilt gebruiken voor de preview. Beantwoord de vragen met betrekking tot de gegevensgrootte, in welk land de gegevens zich bevinden, het tijdskader en de frequentie van gegevensoverdracht. Klik op **Inschrijven**.
3. Nadat u zich hebt ingeschreven en aan de preview mag meedoen, kunt u een Data Box Disk bestellen.

## <a name="order-data-box-disk"></a>Data Box Disk bestellen

Voer de volgende stappen uit in [Azure Portal](https://aka.ms/azuredataboxfromdiskdocs) om een Data Box Disk te bestellen.

1. Klink linksboven in de hoek van de portal op **+ Een resource maken** en zoek naar *Azure Data Box*. Klik op **Azure Data Box**.
    
   ![Zoek naar Azure Data Box 1](media/data-box-disk-deploy-ordered/search-data-box11.png)

2. Klik op **Create**.

3. Controleer of de Data Box-service beschikbaar is in uw regio. Voer de volgende gegevens in of selecteer deze en klik op **Toepassen**.

    ![De optie Data Box Disk selecteren](media/data-box-disk-deploy-ordered/select-data-box-sku-1.png)

    |Instelling|Waarde|
    |---|---|
    |Abonnement|Selecteer het abonnement waarvoor Data Box is ingeschakeld.<br> Het abonnement is gekoppeld aan uw factureringsrekening. |
    |Type overdracht| Importeren in Azure|
    |Bronland | Selecteer het land waar uw gegevens momenteel worden bewaard.|
    |Doel-Azure-regio|Selecteer de Azure-regio waarnaar u uw gegevens wilt overdragen.|

  
5.  Selecteer **Data Box Disk**. De maximumcapaciteit van de oplossing voor één bestelling van 5 schijven is 35 TB. U kunt meerdere bestellingen doen voor grotere gegevensgrootten. 

     ![De optie Data Box Disk selecteren](media/data-box-disk-deploy-ordered/select-data-box-sku-zoom.png)

6.  Voer in **Bestellen** de **Orderdetails** in. Voer de volgende informatie in of selecteer deze.

    |Instelling|Waarde|
    |---|---|
    |Naam|Geef een beschrijvende naam op om de bestelling te volgen.<br> De naam kan tussen 3 en 24 tekens bevatten (letters, cijfers en afbreekstreepjes). <br> De naam moet beginnen en eindigen met een letter of cijfer. |
    |Resourcegroep| Gebruik een bestaande of maak een nieuwe. <br> Een resourcegroep is een logische container voor resources die samen kunnen worden beheerd of geïmplementeerd. |
    |Doel-Azure-regio| Selecteer een regio voor uw opslagaccount.<br> Momenteel worden opslagaccounts in alle regio's in de VS, West- en Noord-Europa, Canada en Australië ondersteund. |
    |Opslagaccount(s)|Selecteer deze uit de gefilterde lijst van een bestaand opslagaccount, gebaseerd op de opgegeven Azure-regio. <br>U kunt ook een nieuw account voor Algemeen gebruik v1 of Algemeen gebruik v2 maken. |
    |Geschatte gegevensgrootte in TB| Voer een schatting in TB in. <br>Op basis van de gegevensgrootte stuurt Microsoft u het juiste aantal SSD-schijven van 8 TB (7 TB aan bruikbare capaciteit). <br>De maximale bruikbare capaciteit van 5 schijven is 35 TB. |

13. Klik op **Volgende**. 

    ![Ordergegevens invoeren](media/data-box-disk-deploy-ordered/data-box-order-details.png)

14. In het tabblad **Verzendadres** geeft u uw voor- en achternaam, de naam en het postadres van het bedrijf en een geldig telefoonnummer op. Klik op **Adres valideren**. De service controleert of de service beschikbaar is voor de regio van het verzendadres. Als de service beschikbaar is voor het opgegeven verzendadres, ontvangt u daarover een melding. 

    ![Verzendadres opgeven](media/data-box-disk-deploy-ordered/data-box-shipping-address.png)
15. In de **Meldingsdetails** geeft u een e-mailadres op. De service stuurt e-mailmeldingen naar het opgegeven e-mailadres over updates van de bestelstatus. 

    We raden u aan een e-mailadres van een groep te gebruiken, zodat u meldingen blijft ontvangen als een beheerder de groep verlaat.

16. Bekijk de gegevens met betrekking tot de bestelling, het contact, de meldingen en de privacyvoorwaarden in **Samenvatting**. Vink het selectievakje aan waarmee u akkoord gaat met de privacyvoorwaarden.

17. Klik op **Bestellen**. Het duurt een paar minuten voordat de bestelling is gemaakt.

 
## <a name="track-the-order"></a>De bestelling volgen

Nadat u uw bestelling hebt geplaatst, kunt u de status van de bestelling volgen via Azure Portal. Ga naar uw bestelling en vervolgens naar **Overzicht** om de status te bekijken. In de portal ziet u dat de taak de status **Besteld** heeft. 

![Status bestel van Data Box Disk](media/data-box-disk-deploy-ordered/data-box-portal-ordered.png) 

Als de schijven niet beschikbaar zijn, ontvangt u een melding. Als de schijven beschikbaar zijn, identificeert Microsoft de schijven die moeten worden verzonden en bereidt Microsoft een schijfpakket voor. Tijdens de voorbereiding van de schijf vinden de volgende acties plaats:

- De schijven worden versleuteld met AES-128 BitLocker-versleuteling.  
- De schijven worden vergrendeld om onbevoegde toegang tot de schijven te voorkomen.
- De sleutel die de schijven ontgrendelt, wordt tijdens dit proces gegenereerd.

Wanneer de schijfvoorbereiding is voltooid, wordt de bestelling in de portal weergegeven met de status **Verwerkt**.

Microsoft bereidt uw schijven vervolgens voor en verzendt deze met een regionale vervoerder. U ontvangt uw volgnummer wanneer de schijven zijn verzonden. In de portal wordt bestelling weergegeven met de status **Verzonden**.



## <a name="cancel-the-order"></a>De bestelling annuleren

Als u deze bestelling wilt annuleren, gaat u in de Azure-previewportal naar **Overzicht** en klikt u in de opdrachtbalk op **Annuleren**. 

U kunt alleen annuleren wanneer de schijven zijn besteld en de bestelling nog wordt verwerkt voor verzending. Nadat de bestelling is verwerkt, kunt u de bestelling niet meer annuleren. 

![Bestelling annuleren](media/data-box-disk-deploy-ordered/cancel-order1.png)

Als u een geannuleerde bestelling wilt verwijderen, gaat u naar **Overzicht** en klikt u in de opdrachtbalk op **Verwijderen**. 


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie bent u meer te weten gekomen over verschillende onderwerpen met betrekking tot Azure Data Box, zoals:

> [!div class="checklist"]
> * Registreren voor Data Box Disk
> * Data Box Disk bestellen
> * De bestelling volgen
> * De bestelling annuleren

Ga naar de volgende zelfstudie om te lezen hoe u uw Data Box Disk instelt.

> [!div class="nextstepaction"]
> [Uw Azure Data Box Disk instellen](./data-box-disk-deploy-set-up.md)


