---
title: Een gefaseerde data analytics-oplossing maken met Azure en Azure Stack | Microsoft Docs
description: Informatie over het maken van een gefaseerde data analytics-oplossing met Azure en Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/01/2018
ms.author: mabrigg
ms.reviewer: anajod
ms.openlocfilehash: 88c72994ec0c6cfc6222ccb31cabae7e354412eb
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54245528"
---
# <a name="tutorial-create-a-staged-data-analytics-solution-with-azure-and-azure-stack"></a>Zelfstudie: Een gefaseerde data analytics-oplossing maken met Azure en Azure Stack 

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Informatie over het gebruik van zowel on-premises en openbare cloudomgevingen om te voldoen aan de behoeften van ondernemingen die meerdere faciliteit. Azure Stack biedt een snelle, veilige en flexibele oplossing voor het verzamelen, verwerken, opslaan en distribueren van lokale en externe gegevens, met name wanneer beveiliging, vertrouwelijkheid, bedrijfsbeleid en regelgeving tussen locaties verschillen kunnen en gebruikers.

In dit patroon, zijn uw klanten verzamelen van gegevens die analysis moment van de verzameling, vereist zodat snel beslissingen kunnen worden gemaakt. Vaak gebeurt dit verzamelen van gegevens met geen toegang tot Internet. Wanneer de verbinding tot stand is gebracht, moet u mogelijk een resource-intensieve analyse van de gegevens om meer inzicht te krijgen. U kunt nog steeds gegevens analyseren wanneer een openbare cloud te laat of niet beschikbaar is.

In deze zelfstudie bouwt u een Voorbeeldomgeving:

> [!div class="checklist"]
> - De onbewerkte gegevens opslag-blob maken.
> - Maak een nieuwe Azure Stack-functie schone om gegevens te verplaatsen van Azure Stack in Azure.
> - Een Blob storage geactiveerde functie maken.
> - Maak een Azure Stack-storage-account met een blob en een wachtrij.
> - Een wachtrij geactiveerde functie maken.
> - Test de wachtrij geactiveerde functie.

> [!Tip]  
> ![hybride-pillars.png](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack is een uitbreiding van Azure. Azure Stack brengt de flexibiliteit en innovatie van cloud computing naar uw on-premises omgeving en de enige hybride cloud waarmee u waar dan ook hybride apps bouwen en implementeren in te schakelen.  
> 
> Het technische document [ontwerpoverwegingen voor hybride toepassingen](https://aka.ms/hybrid-cloud-applications-pillars) beoordelingen pijlers van softwarekwaliteit (plaatsing, schaalbaarheid, beschikbaarheid, tolerantie, beheerbaarheid en beveiliging) voor het ontwerpen, implementeren en besturingssysteem hybride toepassingen. De overwegingen bij het ontwerpen helpen bij het optimaliseren van hybride toepassingsontwerp, uitdagingen in een productieomgeving te minimaliseren.

## <a name="prerequisites"></a>Vereisten

Voorbereiding is vereist om deze oplossing te bouwen:

-   Een Azure-Stack is geïnstalleerd en correct werken (meer informatie vindt u hier: [Azure Stack-overzicht](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-storage-overview))

-   Een Azure-abonnement. (Maken een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F))

-   De [Microsoft Azure Storage Explorer](http://storageexplorer.com/) downloaden en installeren.

-   U moet uw eigen gegevens die moeten worden verwerkt door de functies bieden. Gegevens moet worden gegenereerd en beschikbaar om te uploaden naar de Azure Stack-blob-opslagcontainer.

## <a name="issues-and-considerations"></a>Problemen en overwegingen

### <a name="scalability-considerations"></a>Schaalbaarheidsoverwegingen

Azure functions en oplossingen voor opslag schalen om te voldoen aan gegevensvolume en verwerken van aanvragen. Zie voor informatie van de Azure-schaalbaarheid en doelen, [documentatie voor Azure-schaalbaarheid](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets).

### <a name="availability-considerations"></a>Beschikbaarheidsoverwegingen

Opslag is de primaire beschikbaarheidsreplica overweging voor dit patroon. Verbinding via snelle koppelingen is vereist voor de verwerking van grote hoeveelheden gegevens volume en distributie.

### <a name="manageability-considerations"></a>Beheerbaarheidsoverwegingen

Houd rekening met hoe uw hulpprogramma's voor ontwikkeling en bronbeheer u kunt, voor het beheren van uw oplossing.

## <a name="create-the-raw-data-storage-blob"></a>De onbewerkte gegevens opslag-blob maken

De storage-account en blob-container wordt alle oorspronkelijke gegevens die worden gegenereerd door de on-premises activiteiten, met inbegrip van de machine en werknemer activiteit, een gebouw van gegevens, productie metrische gegevens en andere reporting bevatten.

1.  Aanmelden bij de [ *Azure Stack-portal*](https://portal.local.azurestack.external/).

2.  Vouw in de Azure Stack-portal het menu aan de linkerkant op het menu met services te openen en kies **alle Services**. Schuif omlaag naar **opslag** en kies **opslagaccounts**. Kies in het venster Storage-Accounts **toevoegen**.

3.  Gebruik de volgende informatie voor het account:

    a.  Naam: **Uw keuze**

    b.  Implementatiemodel: **Resource Manager**

    c.  Soort account: **Storage (algemeen gebruik V1)**

    d.  Locatie: **US - west**

    e.  Replicatie: **Lokaal redundante opslag (LRS)**

    f.  Prestaties: **Standard**

    g.  Veilige overdracht vereist: **Uitgeschakeld**

    h.  Abonnement: Kies een optie

    i.  Resourcegroep: Geef een nieuwe resourcegroep of Selecteer een bestaande resourcegroep

    j.  Virtuele netwerken configureren: **Uitgeschakeld**

4.  Selecteer **maken om te maken van het opslagaccount**.

    ![Alternatieve tekst](media/azure-stack-solution-staged-data-analytics/image1.png)

5.  Nadat u hebt gemaakt, selecteert u de naam van het opslagaccount.

6.  De blade, onder de **BLOB-SERVICE** kop, selecteer **Containers**.

7.  Selecteer boven aan de blade **+ Container.** en selecteer **Container**.

    ![Alternatieve tekst](media/azure-stack-solution-staged-data-analytics/image2.png)

8.  Naam: **Uw keuze**

9.  Openbaar toegangsniveau: **Container** (anonieme leestoegang voor containers en blobs)

10.  Selecteer **OK**.

## <a name="create-an-azure-stack-function"></a>Een Azure Stack-functie maken

Maak een nieuwe Azure Stack-functie schone om gegevens te verplaatsen van Azure Stack in Azure.

### <a name="create-the-azure-stack-function-app"></a>De Azure Stack-functie-app maken

1. Meld u aan bij de [Azure Stack-portal](https://portal.local.azurestack.external).
2. Selecteer **Alle services**.
3. Selecteer **functie-Apps** in de **Web en mobiel** groep.

4.  De functie-app maken met de instellingen die zijn opgegeven in de tabel onder de afbeelding.

    | Instelling | Voorgestelde waarde | Description |
    | ---- | ---- | ---- |
    | Naam van app | Wereldwijd unieke naam | Naam waarmee uw nieuwe functie-app wordt aangeduid. Geldige tekens zijn `a` - `z`, `0``-9`, en `-`. |
    | Abonnement | Uw abonnement | Het abonnement waarmee deze nieuwe functie-app is gemaakt. |
    | **Resourcegroep** |  |  |
    | myResourceGroup | Naam voor de nieuwe resourcegroep waarin uw functie-app moet worden gemaakt. |  |
    | OS | Windows | Hosten zonder server is momenteel alleen beschikbaar wanneer u gebruikmaakt van Windows. |
    | **Hostingabonnement** |  |  |
    | Verbruiksabonnement | Hostingabonnement dat definieert hoe resources worden toegewezen aan uw functie-app. Resources worden in de standaard Verbruiksabonnement behoefte dynamisch door uw functies toegevoegd. In deze zonder server die als host fungeert, betaalt u alleen voor de uitvoering van uw functies. |  |
    | Locatie | Regio dichtstbijzijnde | Kies een regio bij u in de buurt of in de buurt van andere services uw functions-toegang. |
    | **Opslagaccount** |  |  |
    | \<Storage-account dat eerder is gemaakt > | Naam van het nieuwe opslagaccount dat door uw functie-app wordt gebruikt. Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn. De naam mag alleen cijfers en kleine letters gebruiken. U kunt ook een bestaand account gebruiken. |  |

    **Voorbeeld:**

    ![Instellingen voor nieuwe functie-apps definiëren](media/azure-stack-solution-staged-data-analytics/image6.png)

5.  Selecteer **Maken** om de functie-app in te richten en te implementeren.

6.  Selecteer het meldingspictogram in de rechterbovenhoek van de portal en zoek het bericht **Implementatie voltooid**.

    ![Instellingen voor nieuwe functie-apps definiëren](media/azure-stack-solution-staged-data-analytics/image7.png)

7.  Selecteer **naar de resource gaan** om nieuwe functie-app weer te geven.

![De functie-app is gemaakt.](media/azure-stack-solution-staged-data-analytics/image8.png)

### <a name="add-a-function-to-the-azure-stack-function-app"></a>Een functie toevoegen aan de functie-app van Azure Stack

1.  Maak een nieuwe functie door te klikken op **functies**, dan zal de **+ nieuwe functie** knop.

    ![Alternatieve tekst](media/azure-stack-solution-staged-data-analytics/image3.png)

2.  Selecteer **Timertrigger**.

    ![Alternatieve tekst](media/azure-stack-solution-staged-data-analytics/image4.png)

3.  Selecteer **C\#**  als de taal en de naam van de functie: `upload-to-azure`  Stel de planning op `0 0 * * * *`, die in de CRON-notatie is één keer per uur.

    ![Alternatieve tekst](media/azure-stack-solution-staged-data-analytics/image5.png)

## <a name="create-a-blob-storage-triggered-function"></a>Een door Blob Storage geactiveerde functie maken

1.  Vouw de functie-app en selecteer de **+** naast **functies**.

2.  Typ in het zoekveld `blob` en kies vervolgens de gewenste taal voor de **blobtrigger** sjabloon.

  ![Kies de trigger-sjabloon voor blob-opslag.](media/azure-stack-solution-staged-data-analytics/image10.png)

3.  Gebruik de instellingen zoals opgegeven in de onderstaande tabel:

    | Instelling | Voorgestelde waarde | Description |
    | ------- | ------- | ------- |
    | Name | Uniek in uw functie-app | Naam van deze door Blob geactiveerde functie. |
    | Pad | \<het pad van de bovenstaande opslaglocatie > | Locatie in Blob Storage die wordt bewaakt. De bestandsnaam van de blob wordt doorgegeven in de binding als de parameter name. |
    | Opslagaccountverbinding | Functie-App verbinding | U kunt de opslagaccountverbinding al wordt gebruikt door uw functie-app gebruiken of een nieuwe maken. |

    **Voorbeeld:**

    ![Maak de door Blob Storage geactiveerde functie.](media/azure-stack-solution-staged-data-analytics/image11.png)

4.  Selecteer **maken** om de functie te maken.

### <a name="test-the-function"></a>De functie testen

1.  Blader naar de functie in de Azure-portal. Vouw de **logboeken** aan de onderkant van de pagina en zorg ervoor dat logboekstreaming niet is onderbroken.

2.  Open Opslagverkenner en verbinding maken met het opslagaccount dat aan het begin van deze sectie is gemaakt.

3.  Vouw het opslagaccount, **Blob-containers**, en de blob u eerder hebt gemaakt. Selecteer **uploaden** en vervolgens **bestanden uploaden**.

    ![Upload een bestand naar de blob-container.](media/azure-stack-solution-staged-data-analytics/image12.png)

4.  Selecteer het veld bestanden in het dialoogvenster van de bestanden uploaden. Blader naar een bestand op een lokale computer, zoals een afbeeldingsbestand, selecteert u deze en selecteer **Open** en vervolgens **uploaden**.

5.  Ga terug naar de functielogboeken en controleer of dat de blob is gelezen.

    **Voorbeeld:**

    ![Bekijk het bericht in de logboeken.](media/azure-stack-solution-staged-data-analytics/image13.png)

## <a name="create-an-azure-stack-storage-account"></a>Een Azure Stack-storage-account maken

Maak een Azure Stack-storage-account met een blob en een wachtrij.

### <a name="storage-blob--data-archiving"></a>Storage Blob-gegevens archiveren

Dit opslagaccount worden twee containers bevatten. Deze containers zijn één blob die wordt gebruikt voor het opslaan van gegevens archiveren en een wachtrij die wordt gebruikt voor de verwerking van gegevens die zijn toegewezen voor distributie van hoofdkantoor.

Gebruik de stappen en de instellingen die hierboven worden beschreven om te maken van een andere account- en blob-opslagcontainer als onze archiefopslag.

### <a name="storage-queue--filtered-data-holding"></a>Storage Queue gefilterde gegevens bedrijf

1.  Gebruik de stappen en de instellingen voor toegang tot de nieuwe opslagaccount die hierboven worden beschreven.

2.  Selecteer in de sectie overzicht met Opslagaccounts **wachtrij.**

3.  Selecteer de **+ wachtrij** en invullen **naam** veld en een naam op voor de nieuwe wachtrij invullen.

4.  Selecteer **OK.**

    ![Alternatieve tekst](media/azure-stack-solution-staged-data-analytics/image14.png)

    ![Alternatieve tekst](media/azure-stack-solution-staged-data-analytics/image15.png)

## <a name="create-a-queue-triggered-function"></a>Een wachtrij geactiveerde functie maken

1.  Gebruik de stappen in de bovenstaande sectie van de functie maken voor het maken van een extra Azure Stack-functie met een wachtrijtrigger in plaats van een blobtrigger.

2.  Gebruik de instellingen zoals opgegeven in de onderstaande tabel:

    | Instelling | Voorgestelde waarde | Description |
    | ------- | ------- | ------- |
    | Name | Uniek in uw functie-app | Naam van deze door een wachtrij geactiveerde functie. |
    | Pad | \<het pad van de bovenstaande opslaglocatie > | De locatie in de opslag die worden bewaakt. De bestandsnaam van de wachtrij wordt doorgegeven in de binding als de parameter name. |
    | Opslagaccountverbinding | Functie-App verbinding | U kunt de opslagaccountverbinding al wordt gebruikt door uw functie-app gebruiken of een nieuwe maken. |

3.  Selecteer **maken** om de functie te maken.

## <a name="test-the-queue-triggered-function"></a>Test de wachtrij geactiveerde functie

1.  In de Azure Stack-portal, blader naar de functie. Vouw de **logboeken** aan de onderkant van de pagina en zorg ervoor dat logboekstreaming niet is onderbroken.

2.  Open Opslagverkenner en verbinding maken met het opslagaccount dat aan het begin van deze sectie is gemaakt.

3.  Vouw het opslagaccount, **Blob-containers**, en de blob u eerder hebt gemaakt. Selecteer **uploaden** en vervolgens **bestanden uploaden.**

    ![Upload een bestand naar de blob-container.](media/azure-stack-solution-staged-data-analytics/image12.png)

4.  Selecteer het veld bestanden in het dialoogvenster van de bestanden uploaden. Blader naar een bestand op een lokale computer, zoals een afbeeldingsbestand, selecteert u deze en selecteer **Open** en vervolgens **uploaden**.

5.  Ga terug naar de functielogboeken en controleer of dat de blob is gelezen.

  **Voorbeeld:**

    ![Bekijk het bericht in de logboeken.](media/azure-stack-solution-staged-data-analytics/image13.png)

## <a name="securely-stored-and-accessed-compliant-data"></a>Veilig opgeslagen en geopend compatibele gegevens

Wereldwijd opererende Ondernemingsgegevens is veilig opgeslagen, verwerkt, gedistribueerd en toegankelijk zijn via Azure en Azure Stack-gefaseerde gegevensanalyse en aangepast eindpunt richtlijnen. Extern kantoor werknemer en machines activiteiten, faciliteit gegevens en zakelijke metrische gegevens zijn voortdurend gecompileerde, opgeslagen, getest voor naleving en gedistribueerd volgens het bedrijfsbeleid en regionale regelgeving.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over Azure Cloud-patronen, [Cloudontwerppatronen](https://docs.microsoft.com/azure/architecture/patterns).