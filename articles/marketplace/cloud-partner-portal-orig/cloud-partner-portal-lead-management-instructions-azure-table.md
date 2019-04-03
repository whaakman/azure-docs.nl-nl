---
title: Azure Table | Microsoft Docs
description: Leadbeheer configureren voor Azure Table.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: pbutlerm
ms.openlocfilehash: 2a8ae3ab71b258d92d9761cc813b168717e44d82
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58878006"
---
# <a name="lead-management-instructions-for-azure-table"></a>Potentiële klanten Management instructies voor het Azure-tabel

In dit artikel wordt beschreven hoe u Azure Table configureren voor het opslaan van de potentiële verkopen. Azure-tabel kunt u opslaan en aanpassen van klantgegevens.

## <a name="to-configure-azure-table"></a>Het configureren van Azure-tabel

1.  Als u een Azure-account niet hebt, kunt u [maken van een gratis proefaccount](https://azure.microsoft.com/pricing/free-trial/).

2.  Nadat u uw Azure-account actief is, aanmelden bij de [Azure-portal](https://portal.azure.com).
3.  Maak een opslagaccount in de Azure-portal. De volgende schermopname ziet u hoe u een opslagaccount maken. Zie voor meer informatie over prijzen voor storage [prijzen voor storage](https://azure.microsoft.com/pricing/details/storage/).

    ![Stappen voor het maken van een Azure storage-account](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragecreate.png)

4.  Kopieer de verbindingsreeks van de storage-account voor de sleutel en plak deze in de **Storage Account Connection String** veld in de Cloud Partner-Portal. Een voorbeeld van een connection-string is `DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net`
    
    ![Azure-opslagsleutel](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragekeys.png)

U kunt [Azure storage explorer](https://azurestorageexplorer.codeplex.com/) of een ander hulpprogramma om de gegevens in uw storage-tabel te bekijken. U kunt ook de gegevens in Azure Table exporteren.
de gegevens.

## <a name="optional-use-microsoft-flow-with-an-azure-table"></a>**(Optioneel)**  Gebruik Microsoft Flow met een Azure-tabel

U kunt [Microsoft Flow](https://docs.microsoft.com/flow/) voor het automatiseren van meldingen telkens als een potentiële klant wordt toegevoegd aan Azure-tabel. Als u dit niet een account hebt, kunt u [zich registreren voor een gratis account](https://flow.microsoft.com/).

### <a name="lead-notification-example"></a>Voorbeeld van potentiële klanten

In dit voorbeeld als richtlijn gebruiken om een eenvoudige stroom waarmee automatisch een e-mailmelding wordt verzonden wanneer een nieuwe lead is toegevoegd aan een Azure-tabel te maken. In dit voorbeeld stelt u een terugkeerpatroon voor het verzenden van gegevens over leads elk uur als tabelopslag wordt bijgewerkt.

1. Aanmelden bij uw Microsoft Flow-account.
2. Selecteer op de linker navigatiebalk **mijn stromen**.
3. Selecteer op de bovenste navigatiebalk **+ nieuw**.  
4. Selecteer op de vervolgkeuzelijst **+ leeg item maken**
5. Selecteer onder maken een stroom met een lege App, **leeg item maken**.

   ![Maak een nieuwe stroom volledig nieuwe](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-create-from-blank.png)

6. Selecteer op de connectors en triggers zoekpagina **Triggers**.
7. Onder **Triggers**, selecteer **terugkeerpatroon**.
8. In de **terugkeerpatroon** venster, laat de standaardinstelling van 1 voor **Interval**. Uit de **frequentie** vervolgkeuzelijst **uur**.

   >[!NOTE] 
   >Hoewel dit voorbeeld een interval van 1 uur wordt, kunt u het interval en de frequentie waarmee het meest geschikt is voor uw bedrijfsbehoeften.

   ![Frequentie van 1 uur voor het terugkeerpatroon ingesteld](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-recurrence-dropdown.png)

9. Selecteer **+ nieuwe stap**.
10. Zoek naar 'Get afgelopen tijd' en selecteer vervolgens **ophalen tijd in het verleden** onder acties. 

    ![Zoek en selecteer ophalen in het verleden Tijdactie](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-search-getpasttime.png)

11. In de **ophalen tijd in het verleden** venster de **Interval** op 1.  Uit de **tijdseenheid** vervolgkeuzelijst **uur**.
    >[!IMPORTANT] 
    >Zorg ervoor dat dit Interval en -tijdseenheid overeenkomt met het Interval en frequentie die u hebt geconfigureerd voor een terugkeerpatroon.

    ![Set-get te tijdsinterval](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-getpast-time.png)

    >[!TIP] 
    >U kunt uw stroom controleren op elk gewenst moment om te controleren of dat elke stap correct is geconfigureerd. Selecteer om te controleren of de stroom, **stroom checker** in de menubalk stroom.

In de volgende reeks stappen, maakt u verbinding met uw Azure-tabel en stelt u de van Verwerkingslogica voor het afhandelen van nieuwe leads.

1. Selecteer na het Get afgelopen tijdstap **+ nieuwe stap**, en zoek naar 'Get-entiteiten'.
2. Onder **acties**, selecteer **entiteiten ophalen**, en selecteer vervolgens **geavanceerde opties weergeven**.
3. In de **entiteiten ophalen** venster informatie te verstrekken voor de volgende velden:

   - **Tabel** – Geef de naam van uw Azure-tabelopslag. De volgende schermopname ziet u de prompt als 'MarketPlaceLeads' voor dit voorbeeld wordt ingevoerd. 

     ![Kies een aangepaste waarde voor de naam van de Azure-tabel](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-getentities-table-name.png)

   - **Filterquery** : in dit veld klikt en de Get afgelopen tijd pictogram wordt weergegeven in een pop-upvenster. Selecteer **tijd in het verleden** dit te gebruiken als tijdstempel voor het filteren van de query. Ook kunt u deze functie in het veld plakken: `gt datetime'@{body('Get_past_time')}'`

     ![Filter-query-functie instellen](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-getentities-filterquery.png)

4. Selecteer **nieuwe stap** om toe te voegen een voorwaarde om te scannen op de Azure-tabel voor nieuwe leads.

   ![Gebruik nieuwe stap een voorwaarde om te scannen op Azure-tabel toevoegen](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-add-filterquery-new-step.png)

5. In de **een actie kiezen** venster **acties**, en selecteer vervolgens de **voorwaarde** besturingselement.

     ![Voorwaarde toevoegen](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-action-condition-control.png)

6. In de **voorwaarde** venster de **een waarde kiezen** veld en selecteer vervolgens **expressie** in het pop-upvenster.
7. Plakken `length(body('Get_entities')?['value'])` in de ***fx*** veld. Selecteer **OK** om toe te voegen deze functie. Instellen van de voorwaarde voltooien:

   - Selecteer 'is groter dan' in de vervolgkeuzelijst.
   - 0 invoeren als waarde 

     ![Een functie toevoegen aan de voorwaarde](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-condition-fx0.png)

8. Instellen van de actie te ondernemen op basis van het resultaat van de voorwaarde.

     ![Actie op basis van resultaten van de voorwaarde instellen](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-condition-pick-action.png)

9. Als de voorwaarde **als er geen**, iets doen. 
10. Als de voorwaarde **als Ja**, een actie die verbinding uw Office 365-account maakt voor het verzenden van een e-mailbericht activeren. Selecteer **een actie toevoegen**.
11. Selecteer **een e-mailbericht verzenden**. 
12. In de **een e-mailbericht verzenden** venster informatie te verstrekken voor de volgende velden:

    - **Naar** -Voer een e-mailadres voor iedereen die deze melding wordt ontvangen.
    - **Onderwerp** – Geef een onderwerp voor het e-mailbericht. Bijvoorbeeld: Nieuwe leads!
    - **Hoofdtekst**:   Toevoegen van de tekst die u wilt opnemen in elke e-mail (optioneel) en plak deze in de hoofdtekst `('Get_entities')?['value']` als een functie voor het invoegen van gegevens over leads.

      >[!NOTE] 
      >U kunt extra statisch of dynamisch gegevenspunten aan de hoofdtekst van dit e-mailbericht invoegen.

       ![E-mailadres voor lead melding instellen](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-emailbody-fx.png)

13. Selecteer **opslaan** om op te slaan van de stroom. Microsoft Flow wordt automatisch de stroom voor fouten testen. Als er geen fouten, de stroom wordt gestart nadat deze opgeslagen.

De volgende schermopname ziet u een voorbeeld van hoe de uiteindelijke stroom moet zien.

 ![Laatste stroom-reeks](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-end-to-end.png)

### <a name="managing-your-flow"></a>Uw stroom beheren

Uw stroom beheren nadat deze wordt uitgevoerd, is eenvoudig.  Hebt u volledige controle over uw stroom. U kunt bijvoorbeeld voorkomen dat deze, bewerken, Zie een uitvoeringsgeschiedenis en analyses. De volgende schermopname ziet u de opties die beschikbaar zijn voor het beheren van een stroom. 

 ![Een stroom beheren](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-manage-completed.png)

De stroom blijft actief totdat u voorkomen dat deze met behulp van de **stroom uitschakelen** optie.

Als u een e-mailmeldingen voor lead ontvangt, betekent dit dat er nieuwe leads bibliotheekscripts zijn toegevoegd aan de Azure-tabel. Als er fouten bij stromen, krijgt u een e-mailbericht als in het voorbeeld in de volgende schermopname.

 ![Stroom fout e-mailmelding](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-failure-note.png)

## <a name="next-steps"></a>Volgende stappen

[Klantenleads configureren](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads)