---
title: Een on-premises SQL Server gebruiken in Azure Machine Learning | Microsoft Docs
description: Gegevens uit een on-premises SQL Server database gebruiken om uit te voeren geavanceerde analyses met Azure Machine Learning.
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 08e4610d-02b6-4071-aad7-a2340ad8e2ea
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: garye;krishnan
ms.openlocfilehash: 79ae5cd78ce07fcc84be49c2693773d58a15771e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="perform-advanced-analytics-with-azure-machine-learning-using-data-from-an-on-premises-sql-server-database"></a>Geavanceerde analyses uitvoeren met Azure Machine Learning met gegevens van een on-premises SQL Server-database

[!INCLUDE [import-data-into-aml-studio-selector](../../../includes/machine-learning-import-data-into-aml-studio.md)]

Vaak ondernemingen die met on-premises gegevens werken wilt om te profiteren van de schaal en flexibiliteit van de cloud voor hun machine learning-werkbelastingen. Maar deze niet willen hun huidige bedrijfsprocessen en werkstromen verstoren hun on-premises gegevens worden verplaatst naar de cloud. Azure Machine Learning ondersteunt nu uw gegevens leest uit een on-premises SQL Server database en vervolgens training en score berekenen voor een model met deze gegevens. U hebt niet langer handmatig kopiëren en synchroniseren van gegevens tussen de cloud en uw lokale server. In plaats daarvan de **importgegevens** module in Azure Machine Learning Studio nu rechtstreeks vanuit uw lokale SQL Server-database voor uw training en score berekenen voor taken kan lezen.

Dit artikel bevat een overzicht van het inkomend lokale SQL server-gegevens in Azure Machine Learning. Er wordt vanuit gegaan dat u bekend met Azure Machine Learning-concepten zoals werkruimten, modules, gegevenssets, experimenten bent, *enzovoort*.

> [!NOTE]
> Deze functie is niet beschikbaar voor gratis werkruimten. Zie voor meer informatie over Machine Learning-prijzen en lagen [Azure Machine Learning-prijzen](https://azure.microsoft.com/pricing/details/machine-learning/).
>
>

<!-- -->

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="install-the-microsoft-data-management-gateway"></a>Microsoft Data Management Gateway installeren
Voor toegang tot een lokale SQL Server-database in Azure Machine Learning, moet u download en installeer Microsoft Data Management Gateway. Wanneer u de gatewayverbinding in Machine Learning Studio configureert, hebt u de mogelijkheid om te downloaden en installeer de gateway met de **downloaden en registreren gegevensgateway** dialoogvenster die hieronder worden beschreven.

U kunt de Data Management Gateway tevoren ook installeren met het downloaden en uitvoeren van de MSI-installatiepakket van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).
Kies de nieuwste versie, 32-bits of 64-bits geschikt is voor uw computer selecteren. Het MSI-bestand kan ook worden gebruikt om bij te werken van een bestaande Data Management Gateway naar de nieuwste versie, met alle instellingen behouden.

De gateway heeft de volgende vereisten:

* De ondersteunde versies van de Windows-besturingssysteem zijn Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012 en Windows Server 2012 R2.
* De aanbevolen configuratie voor de gatewaycomputer is ten minste 2 GHz, 4 kernen, 8GB RAM-geheugen en schijfruimte 80GB.
* Als de hostmachine in de slaapstand, worden de gateway won't reageren op gegevensaanvragen. Een juiste energiebeheerschema op de computer daarom configureren voordat u de gateway installeert. Als de machine is geconfigureerd voor de sluimerstand, wordt de installatie van de gateway een bericht weergegeven.
* Omdat kopieeractiviteit op een specifieke frequentie plaatsvindt, volgt het brongebruik (CPU, geheugen) op de machine ook hetzelfde patroon met piek- en niet-actieve tijd. Resourcegebruik ook afhankelijk van geheugenlatentie de hoeveelheid gegevens worden verplaatst. Wanneer meerdere kopie taken uitgevoerd worden, moet u Resourcegebruik tijdens piektijden stijgen zien. De minimale configuratie bovenstaande technisch voldoende is, is het raadzaam een configuratie met meer bronnen dan de minimale configuratie afhankelijk van uw specifieke werkbelasting voor de verplaatsing van gegevens.

Overweeg het volgende bij het instellen en gebruiken van een Data Management Gateway:

* U kunt slechts één exemplaar van Data Management Gateway installeren op een enkele computer.
* U kunt één gateway voor meerdere on-premises-gegevensbronnen.
* U kunt meerdere gateways op verschillende computers verbinding maken met dezelfde lokale gegevensbron.
* U configureren een gateway voor slechts één werkruimte tegelijk. Gateways kunnen niet op dit moment worden gedeeld tussen werkruimten.
* U kunt meerdere gateways voor een enkele werkruimte configureren. U wilt bijvoorbeeld een gateway die verbonden met uw gegevensbronnen test tijdens de ontwikkeling en een productie-gateway gebruiken wanneer u klaar bent om operationeel maken.
* De gateway hoeft niet te worden op dezelfde computer als de gegevensbron. Maar bijwerkt dichter bij de gegevensbron minder tijd voor de gateway verbinding maken met de gegevensbron. Het is raadzaam dat u de gateway op een machine die verschilt van die als host fungeert voor de on-premises gegevensbron installeren zodat de gateway en de gegevensbron niet concurreren voor bronnen.
* Als u al een gateway is geïnstalleerd op uw computer voor de Power BI of Azure Data Factory-scenario's, een aparte gateway voor Azure Machine Learning op een andere computer installeren.

  > [!NOTE]
  > Data Management Gateway en de Gateway van Power BI kan niet worden uitgevoerd op dezelfde computer.
  >
  >
* U moet de Data Management Gateway gebruiken voor Azure Machine Learning, zelfs als u Azure ExpressRoute voor andere gegevens gebruikt. U moet uw gegevensbron behandelen als een lokale gegevensbron (die zich achter een firewall) zelfs wanneer u ExpressRoute gebruikt. Data Management Gateway gebruiken om verbinding tussen de Machine Learning en de gegevensbron te maken.

U vindt gedetailleerde informatie over vereisten voor de installatie, installatiestappen en tips voor probleemoplossing in het artikel [Data Management Gateway](../../data-factory/v1/data-factory-data-management-gateway.md).

## <a name="span-idusing-the-data-gateway-step-by-step-walk-classanchorspan-idtoc450838866-classanchorspanspaningress-data-from-your-on-premises-sql-server-database-into-azure-machine-learning"></a><span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>Inkomende gegevens van uw lokale SQL Server-database in Azure Machine Learning
In dit scenario maakt u een Data Management Gateway instellen in een Azure Machine Learning-werkruimte, configureert, en vervolgens gegevens lezen uit een on-premises SQL Server database.

> [!TIP]
> Voordat u begint, uitschakelen van de browser pop-upblokkering voor `studio.azureml.net`. Als u de Google Chrome-browser gebruikt, downloadt en installeert u een van de verschillende invoegtoepassingen beschikbaar zijn op Google Chrome webarchief [Klik eenmaal Appuitbreiding](https://chrome.google.com/webstore/search/clickonce?_category=extensions).
>
>

### <a name="step-1-create-a-gateway"></a>Stap 1: Een gateway maken
De eerste stap is het maken en de gateway instellen voor toegang tot uw lokale SQL-database.

1. Meld u aan bij [Azure Machine Learning Studio](https://studio.azureml.net/Home/) en selecteer de werkruimte die u wilt werken.
2. Klik op de **instellingen** blade aan de linkerkant en klik vervolgens op de **GEGEVENSGATEWAYS** boven op het tabblad.
3. Klik op **nieuwe GEGEVENSGATEWAY** aan de onderkant van het scherm.

    ![Nieuwe gegevensgateway](./media/use-data-from-an-on-premises-sql-server/new-data-gateway-button.png)
4. In de **nieuwe gegevensgateway** dialoogvenster, voer de **gatewaynaam** en voeg desgewenst een **beschrijving**. Klik op de pijl in de rechterbovenhoek om te gaan met de volgende stap van de configuratie.

    ![Gatewaynaam en beschrijving invoeren](./media/use-data-from-an-on-premises-sql-server/new-data-gateway-dialog-enter-name.png)
5. Klik in het downloaden en registreren data gateway dialoogvenster de REGISTRATIESLEUTEL GATEWAY naar het Klembord te kopiëren.

    ![Download gegevensgateway en registreren](./media/use-data-from-an-on-premises-sql-server/download-and-register-data-gateway.png)
6. <span id="note-1" class="anchor"></span>Als u hebt nog niet gedownload en geïnstalleerd van de Microsoft Data Management Gateway, klikt u op **downloaden data management gateway**. Hiermee gaat u naar het Microsoft Download Center waar u kunt selecteren van de gatewayversie die u nodig hebt, downloaden en installeren. Gedetailleerde informatie over vereisten voor de installatie, installatiestappen en tips voor probleemoplossing vindt u in de secties van het begin van het artikel [gegevens verplaatsen tussen lokale bronnen en cloud met Data Management Gateway](../../data-factory/v1/data-factory-move-data-between-onprem-and-cloud.md).
7. Nadat de gateway is geïnstalleerd, de Data Management Gateway Configuration Manager wordt geopend en de **gateway registreren** dialoogvenster wordt weergegeven. Plak de **Gateway registratiesleutel** dat u hebt gekopieerd naar het Klembord en klik op **registreren**.
8. Als u al een gateway is geïnstalleerd, voert u de Data Management Gateway Configuration Manager. Klik op **code wijzigen**, plak de **Gateway registratiesleutel** die u naar het Klembord in de vorige stap gekopieerd en klik op **OK**.
9. Als de installatie voltooid is, de **gateway registreren** voor Microsoft Data Management Gateway Configuration Manager-dialoogvenster wordt weergegeven. Plak de REGISTRATIESLEUTEL GATEWAY die u naar het Klembord in de vorige stap hebt gekopieerd en klik op **registreren**.

    ![Gateway registreren](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-register-gateway.png)
10. Configuratie van de gateway is voltooid als de volgende waarden zijn ingesteld op de **Start** tabblad in Microsoft Data Management Gateway Configuration Manager:

    * **De gatewaynaam van de** en **exemplaarnaam** zijn ingesteld op de naam van de gateway.
    * **Registratie** is ingesteld op **geregistreerde**.
    * **Status** is ingesteld op **gestart**.
    * Op de weergegeven onder de statusbalk **verbonden met Data Management Gateway Cloud Service** samen met een groen vinkje.

      ![Data Management Gateway Manager](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-registered.png)

      Azure Machine Learning Studio wordt ook bijgewerkt wanneer de registratie geslaagd is.

    ![De registratie van de gateway is geslaagd](./media/use-data-from-an-on-premises-sql-server/gateway-registered.png)
11. In de **downloaden en registreren gegevensgateway** dialoogvenster, klikt u op het vinkje om de installatie te voltooien. De **instellingen** pagina wordt weergegeven als 'Online' de status van de gateway. In het rechterdeelvenster vindt u de status en andere nuttige informatie.

    ![Gateway-instellingen](./media/use-data-from-an-on-premises-sql-server/gateway-status.png)
12. In de Microsoft Data Management Gateway Configuration Manager overschakelen naar de **certificaat** tabblad. Het certificaat dat op dit tabblad zijn opgegeven, wordt gebruikt voor versleutelen/ontsleutelen van referenties voor het lokale gegevensarchief die u in de portal opgeeft. Dit certificaat is het standaardcertificaat. Microsoft raadt aan wijzigen in uw eigen certificaat dat u back-up in uw certificate management-systeem. Klik op **wijziging** voor gebruik in plaats hiervan uw eigen certificaat.

    ![Het gatewaycertificaat wijzigen](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-certificate.png)
13. (optioneel) Als u uitgebreide logboekregistratie inschakelen om het oplossen van problemen met de gateway in de Microsoft Data Management Gateway Configuration Manager overschakelen naar de **Diagnostics** tabblad en controleer de **logboekregistratie inschakelen voor het oplossen van problemen** optie. Gegevens in het logboek vindt u in de Windows-Logboeken onder de **logboeken toepassingen en Services**  - &gt; **Data Management Gateway** knooppunt. U kunt ook de **Diagnostics** tabblad om de verbinding met een lokale gegevensbron met behulp van de gateway te testen.

    ![Uitgebreide logboekregistratie inschakelen](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-verbose-logging.png)

Dit is het installatieproces van de gateway in Azure Machine Learning voltooid.
U bent nu klaar voor gebruik van uw on-premises gegevens.

U kunt maken en instellen van meerdere gateways in Studio voor elke werkruimte. Bijvoorbeeld, wellicht u een gateway die u wilt verbinding maken met uw gegevensbronnen test tijdens de ontwikkeling en een andere gateway voor uw productie-gegevensbronnen. Azure Machine Learning biedt u de flexibiliteit voor het instellen van meerdere gateways, afhankelijk van uw zakelijke omgeving. Momenteel kunt u een gateway tussen werkruimten niet delen en kan slechts één gateway worden geïnstalleerd op een enkele computer. Zie voor meer informatie [gegevens verplaatsen tussen lokale bronnen en cloud met Data Management Gateway](../../data-factory/v1/data-factory-move-data-between-onprem-and-cloud.md).

### <a name="step-2-use-the-gateway-to-read-data-from-an-on-premises-data-source"></a>Stap 2: De gateway gebruiken om te lezen van gegevens uit een on-premises gegevensbron
Nadat u de gateway hebt ingesteld, kunt u toevoegen een **importgegevens** module die u wilt een experiment die invoer van de gegevens van de lokale SQL Server-database.

1. In Machine Learning Studio, selecteer de **EXPERIMENTEN** tabblad **+ nieuw** in de linkerbenedenhoek en selecteer **leeg Experiment** (of Selecteer een van de verschillende voorbeeldexperimenten beschikbaar).
2. Zoek en sleep de **importgegevens** module naar het experimentcanvas.
3. Klik op **opslaan als** onder het canvas. Voer 'Azure Machine Learning On-Premises SQL Server-zelfstudie' voor de naam van het experiment, selecteer de werkruimte en klikt u op de **OK** selectievakje is ingeschakeld.

   ![Experiment met een nieuwe naam opslaan](./media/use-data-from-an-on-premises-sql-server/experiment-save-as.png)
4. Klik op de **importgegevens** module die u wilt selecteren, klikt u vervolgens in de **eigenschappen** selecteert u 'On-Premises SQL-Database' in het rechterdeelvenster van het canvas de **gegevensbron** vervolgkeuzelijst.
5. Selecteer de **gegevensgateway** u geïnstalleerd en geregistreerd. U kunt een andere gateway instellen door te selecteren '(toevoegen van nieuwe gegevensgateway...)'.

   ![Selecteer de gegevensgateway voor gegevens importeren-module](./media/use-data-from-an-on-premises-sql-server/import-data-select-on-premises-data-source.png)
6. Voer de SQL **databaseservernaam** en **databasenaam**, samen met de SQL **databasequery** u wilt uitvoeren.
7. Klik op **Voer waarden in** onder **gebruikersnaam en wachtwoord** en voer uw databasereferenties. U kunt Windows-verificatie of SQL Server-verificatie, al naar gelang hoe uw lokale SQL Server is geconfigureerd.

   ![Geef databasereferenties op](./media/use-data-from-an-on-premises-sql-server/database-credentials.png)

   Het bericht 'waarden voor vereiste' de wijzigingen in 'waarden instellen' met een groen vinkje. U moet slechts één keer de referenties invoert tenzij de gegevens van de database of het wachtwoord wijzigt. Azure Machine Learning maakt gebruik van het certificaat dat u hebt opgegeven tijdens de installatie van de gateway voor het versleutelen van de referenties in de cloud. On-premises referenties zonder versleuteling nooit worden opgeslagen in Azure.

   ![Eigenschappen van de module gegevens importeren](./media/use-data-from-an-on-premises-sql-server/import-data-properties-entered.png)
8. Klik op **uitvoeren** het experiment uitvoeren.

Wanneer het experiment is voltooid, kunt u de gegevens die u hebt geïmporteerd uit de database door te klikken op de uitvoerpoort van visualiseren de **importgegevens** module en het selecteren van **Visualize**.

Wanneer u klaar bent met het ontwikkelen van uw experiment, kunt u deze kunt implementeren en uw model operationeel. Met de Service in Batch worden uitgevoerd, gegevens van de lokale SQL Server-database is geconfigureerd de **importgegevens** module worden gelezen en gebruikt voor score berekenen. U kunt de Request Response-Service gebruiken voor score berekenen voor on-premises gegevens, wordt aangeraden met behulp van de [Excel-invoegtoepassing](excel-add-in-for-web-services.md) in plaats daarvan. Op dit moment kunt schrijven naar een lokale SQL Server-database via **gegevens exporteren** wordt niet ondersteund in uw experimenten of gepubliceerde web-services.
