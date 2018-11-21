---
title: On-premises SQL Server gebruiken in Azure Machine Learning | Microsoft Docs
description: Gegevens uit een on-premises SQL Server-database gebruiken om geavanceerde analyses met Azure Machine Learning uitvoeren.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=hshapiro, author=heatherbshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 08e4610d-02b6-4071-aad7-a2340ad8e2ea
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.openlocfilehash: 83ec812f18e7e47f30db79a6d665d60fae9e0e5d
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52263210"
---
# <a name="perform-advanced-analytics-with-azure-machine-learning-using-data-from-an-on-premises-sql-server-database"></a>Geavanceerde analyses uitvoeren met Azure Machine Learning met gegevens van een on-premises SQL Server-database

Vaak ondernemingen die met on-premises gegevens werken wilt profiteren van de schaal en flexibiliteit van de cloud voor de machine learning-werkbelastingen uitvoeren. Maar ze niet willen hun huidige bedrijfsprocessen en werkstromen worden onderbroken door het verplaatsen van hun on-premises gegevens naar de cloud. Azure Machine Learning biedt nu ondersteuning voor lezen van uw gegevens vanuit een on-premises SQL Server-database en vervolgens training en scoren van een model met deze gegevens. U hebt niet meer handmatig kopiëren en synchroniseren van gegevens tussen de cloud en uw on-premises server. In plaats daarvan de **importgegevens** module in Azure Machine Learning Studio nu rechtstreeks vanuit uw on-premises SQL Server-database voor uw training en scoren van taken kan lezen.

In dit artikel biedt een overzicht van hoe u inkomend verkeer on-premises SQL server-gegevens in Azure Machine Learning. Hierbij wordt ervan uitgegaan dat u bekend met Azure Machine Learning-concepten, zoals werkruimten, modules, gegevenssets, experimenten bent, *enzovoort*.

> [!NOTE]
> Deze functie is niet beschikbaar voor gratis werkruimten. Zie voor meer informatie over Machine Learning-prijzen en lagen [Azure Machine Learning-prijzen](https://azure.microsoft.com/pricing/details/machine-learning/).
>
>

<!-- -->

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="install-the-data-factory-self-hosted-integration-runtime"></a>De Data Factory zelf-hostende Integratieruntime installeren
Voor toegang tot een on-premises SQL Server-database in Azure Machine Learning, die u wilt downloaden en installeren van de Data Factory zelfgehoste Integration Runtime, voorheen bekend als de Data Management Gateway. Wanneer u de verbinding in Machine Learning Studio configureren, hebt u de mogelijkheid om te downloaden en installeer de Integration Runtime (IR) met de **downloaden en registreer gegevensgateway** dialoogvenster die hieronder worden beschreven.


U kunt de IR tevoren ook installeren door te downloaden en uitvoeren van het MSI-installatiepakket van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717). Het MSI-bestand kan ook worden gebruikt om een bestaande IR upgraden naar de nieuwste versie, met alle instellingen behouden.

De Data Factory zelfgehoste Integration Runtime heeft de volgende vereisten:

* De Data Factory zelfgehoste Integration vereist een 64-bits besturingssysteem met .NET Framework 4.6.1 of hoger.
* De ondersteunde versies van de Windows-besturingssysteem zijn Windows 10, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016. 
* De aanbevolen configuratie voor de machine IR is ten minste 2 GHz, 4-Core CPU, 8GB RAM-geheugen en 80GB-schijf.
* Als de hostcomputer in de slaapstand, wordt de IR wordt niet reageert op aanvragen voor gegevens. Daarom een juiste energiebeheerschema configureren op de computer voordat u installeert de IR. Als de machine is geconfigureerd voor de slaapstand, wordt de installatie van de IR een bericht weergegeven.
* Omdat de kopieeractiviteit wordt uitgevoerd op een specifieke frequentie, volgt het Resourcegebruik (CPU, geheugen) op de computer ook hetzelfde patroon met piek- en niet-actieve tijden. Gebruik van resources is ook afhankelijk sterk de hoeveelheid gegevens die wordt verplaatst. Wanneer meerdere kopie-taken uitgevoerd worden, moet u Resourcegebruik tijdens piektijden omhoog gaan zien. De minimale configuratie bovenstaande technisch voldoende is, kunt u een configuratie met meer bronnen dan de minimale configuratie zijn afhankelijk van uw specifieke werkbelasting voor verplaatsing van gegevens.

Overweeg het volgende bij het instellen en gebruiken van een Data Factory zelfgehoste Integration Runtime:

* U kunt slechts één exemplaar van de IR installeren op een enkele computer.
* U kunt een enkel IR gebruiken voor meerdere on-premises gegevensbronnen.
* U kunt meerdere IRs op verschillende computers verbinding maken met dezelfde on-premises gegevensbron.
* U configureert een IRs voor slechts één werkruimte tegelijk. Op dit moment kan niet IRs worden gedeeld tussen werkruimten.
* U kunt meerdere IRs voor één werkruimte configureren. U wilt bijvoorbeeld gebruik van een IR die verbonden met uw gegevensbronnen test tijdens de ontwikkeling en een productie-IR wanneer u bent klaar om in te zetten.
* De IR hoeft niet te worden op dezelfde computer als de gegevensbron. Maar dichter bij de gegevensbron blijft verkort de termijn voor de gateway verbinding maken met de gegevensbron. Het is raadzaam dat u de IR op een computer die verschilt van de map die als host fungeert voor de on-premises gegevensbron installeren zodat de gateway en de gegevensbron niet concurreren om bronnen.
* Als u al een IR geïnstalleerd op uw computer voor scenario's met Power BI of Azure Data Factory, een afzonderlijke IR installeren voor Azure Machine Learning op een andere computer.

  > [!NOTE]
  > U kunt Data Factory zelfgehoste Integration Runtime en Power BI Gateway niet uitvoeren op dezelfde computer.
  >
  >
* U moet de Data Factory zelfgehoste Integration Runtime gebruiken voor Azure Machine Learning, zelfs als u Azure ExpressRoute voor andere gegevens gebruikt. U moet uw gegevensbron behandelen als een on-premises gegevensbron (die zich achter een firewall bevindt), zelfs als u ExpressRoute gebruikt. De Data Factory zelfgehoste Integration Runtime gebruiken om verbinding tussen de Machine Learning en de gegevensbron te maken.

U vindt gedetailleerde informatie over vereisten voor de installatie, installatiestappen en tips voor probleemoplossing in het artikel [Integratieruntime in Gegevensfactory](../../data-factory/concepts-integration-runtime.md).

## <a name="span-idusing-the-data-gateway-step-by-step-walk-classanchorspan-idtoc450838866-classanchorspanspaningress-data-from-your-on-premises-sql-server-database-into-azure-machine-learning"></a><span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>Inkomende gegevens van uw on-premises SQL Server-database in Azure Machine Learning
In dit scenario wordt u instellen van een Azure Data Factory Integration Runtime in een Azure Machine Learning-werkruimte, configureert en lees vervolgens de gegevens uit een on-premises SQL Server-database.

> [!TIP]
> Voordat u begint, uitschakelen van uw browser pop-upblokkering voor `studio.azureml.net`. Als u de Google Chrome-browser gebruikt, downloadt en installeert u een van de verschillende invoegtoepassingen beschikbaar zijn op Google Chrome Webstore-webwinkel [klikt u op één keer Appuitbreiding](https://chrome.google.com/webstore/search/clickonce?_category=extensions).
>
> [!NOTE]
> Azure Data Factory zelfgehoste Integration Runtime was voorheen bekend als Data Management Gateway. De stapsgewijze zelfstudie blijven om te verwijzen naar deze als een gateway.  

### <a name="step-1-create-a-gateway"></a>Stap 1: Een gateway maken
De eerste stap is het maken en de gateway instellen voor toegang tot uw on-premises SQL-database.

1. Meld u aan bij [Azure Machine Learning Studio](https://studio.azureml.net/Home/) en selecteer de werkruimte die u wilt werken.
2. Klik op de **instellingen** blade aan de linkerkant en klik vervolgens op de **GEGEVENSGATEWAYS** tabblad bovenaan.
3. Klik op **nieuwe GEGEVENSGATEWAY** aan de onderkant van het scherm.

    ![Nieuwe gegevensgateway](./media/use-data-from-an-on-premises-sql-server/new-data-gateway-button.png)
4. In de **nieuwe gegevensgateway** dialoogvenster, geef de **gatewaynaam** en voeg eventueel een **beschrijving**. Klik op de pijl in de rechterbenedenhoek om naar de volgende stap van de configuratie te gaan.

    ![Gatewaynaam en beschrijving invoeren](./media/use-data-from-an-on-premises-sql-server/new-data-gateway-dialog-enter-name.png)
5. Download en registreer data gateway in het dialoogvenster in, de REGISTRATIESLEUTEL voor de GATEWAY naar het Klembord te kopiëren.

    ![Download en data gateway registreren](./media/use-data-from-an-on-premises-sql-server/download-and-register-data-gateway.png)
6. <span id="note-1" class="anchor"></span>Als u hebt nog niet gedownload en geïnstalleerd, de Microsoft Data Management Gateway, klikt u op **downloaden data management gateway**. Hiermee gaat u naar het Microsoft Download Center waar u kunt de gatewayversie die u nodig hebt, downloadt u deze selecteren en installeren. U vindt gedetailleerde informatie over vereisten voor de installatie, installatiestappen en tips voor probleemoplossing in de secties van het begin van het artikel [gegevens verplaatsen tussen on-premises bronnen en de cloud met Data Management Gateway](../../data-factory/tutorial-hybrid-copy-portal.md) .
7. Nadat de gateway is geïnstalleerd, de Data Management Gateway Configuration Manager wordt geopend en de **registreren gateway** dialoogvenster wordt weergegeven. Plak de **Gateway registratiesleutel** die u hebt gekopieerd naar het Klembord en klik op **registreren**.
8. Als u al een gateway is geïnstalleerd, voert u Data Management Gateway Configuratiebeheer. Klik op **wijzigingssleutel**, plak de **Gateway registratiesleutel** die u naar het Klembord in de vorige stap hebt gekopieerd en klik op **OK**.
9. Wanneer de installatie voltooid is, de **registreren gateway** voor Microsoft Data Management Gateway Configuration Manager-dialoogvenster wordt weergegeven. Plak de REGISTRATIESLEUTEL GATEWAY die u naar het Klembord in de vorige stap hebt gekopieerd en klik op **registreren**.

    ![Gateway registreren](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-register-gateway.png)
10. Configuratie van de gateway is voltooid wanneer de volgende waarden worden ingesteld op de **Start** tabblad in Microsoft Data Management Gateway Configuration Manager:

    * **Gatewaynaam** en **exemplaarnaam** zijn ingesteld op de naam van de gateway.
    * **Registratie** is ingesteld op **geregistreerde**.
    * **Status** is ingesteld op **gestart**.
    * De statusbalk weergegeven als de onderste **verbonden met Data Management Gateway-Cloudservice** samen met een groen vinkje.

      ![Data Management Gateway Manager](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-registered.png)

      Azure Machine Learning Studio wordt ook bijgewerkt wanneer de registratie geslaagd is.

    ![Gateway-registratie is voltooid](./media/use-data-from-an-on-premises-sql-server/gateway-registered.png)
11. In de **downloaden en registreren van de gegevensgateway** dialoogvenster, klikt u op het vinkje om de installatie te voltooien. De **instellingen** pagina wordt de status van de gateway als 'Online' weergegeven. In het rechter deelvenster vindt u de status en andere nuttige informatie.

    ![Gateway-instellingen](./media/use-data-from-an-on-premises-sql-server/gateway-status.png)
12. In de Microsoft Data Management Gateway Configuratiebeheer overschakelen naar de **certificaat** tabblad. Het certificaat dat is opgegeven op dit tabblad wordt gebruikt om referenties voor de on-premises gegevensopslag die u opgeeft in de portal voor versleutelen/ontsleutelen. Dit certificaat is het standaard-certificaat. Microsoft raadt aan wijzigen in uw eigen certificaat dat u back-up in uw certificate management-systeem. Klik op **wijziging** in plaats daarvan uw eigen certificaat gebruiken.

    ![Certificaat van de gateway wijzigen](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-certificate.png)
13. (optioneel) Als u uitgebreide logboekregistratie inschakelen om te kunnen oplossen van problemen met de gateway, in de Microsoft Data Management Gateway Configuratiebeheer overschakelen naar de **Diagnostics** tabblad en controleer de **uitgebreide inschakelen logboekregistratie voor het oplossen van problemen** optie. De logboekregistratie informatie vindt u in de Windows-Logboeken onder de **logboeken toepassingen en Services**  - &gt; **Data Management Gateway** knooppunt. U kunt ook de **Diagnostics** tabblad om de verbinding met een on-premises gegevensbron met behulp van de gateway te testen.

    ![Uitgebreide logboekregistratie inschakelen](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-verbose-logging.png)

Dit is het installatieproces van de gateway in Azure Machine Learning.
U bent nu klaar voor gebruik van uw on-premises gegevens.

U kunt maken en instellen van meerdere gateways in Studio voor elke werkruimte. Bijvoorbeeld, u mogelijk een gateway die u wilt verbinding maken met uw gegevensbronnen test tijdens de ontwikkeling en een andere gateway voor uw productie-gegevensbronnen. Azure Machine Learning biedt u de flexibiliteit voor het instellen van meerdere gateways, afhankelijk van uw zakelijke omgeving. Op dit moment kunt u een gateway tussen werkruimten niet delen en slechts één gateway kan worden geïnstalleerd op een enkele computer. Zie voor meer informatie, [gegevens verplaatsen tussen on-premises bronnen en de cloud met Data Management Gateway](../../data-factory/tutorial-hybrid-copy-portal.md).

### <a name="step-2-use-the-gateway-to-read-data-from-an-on-premises-data-source"></a>Stap 2: De gateway gebruiken om te lezen van gegevens van een on-premises gegevensbron
Nadat u de gateway hebt ingesteld, kunt u toevoegen een **importgegevens** module aan een experiment dat invoer van de gegevens van on-premises SQL Server-database.

1. In Machine Learning Studio, selecteer de **EXPERIMENTEN** tabblad **+ nieuw** in de linkerbenedenhoek en selecteer **leeg Experiment** (of Selecteer een van verschillende voorbeeld experimenten beschikbaar).
2. Zoek en sleep de **importgegevens** module naar het experimentcanvas.
3. Klik op **opslaan als** onder het canvas. Voer 'Azure Machine Learning On-Premises SQL Server-zelfstudie' voor de naam van het experiment, selecteert u de werkruimte en klik op de **OK** selectievakje is ingeschakeld.

   ![Experiment met een nieuwe naam opslaan](./media/use-data-from-an-on-premises-sql-server/experiment-save-as.png)
4. Klik op de **importgegevens** module om deze te selecteren, klikt u vervolgens de **eigenschappen** deelvenster aan de rechterkant van het canvas, selecteert u 'On-Premises SQL-Database' in de **gegevensbron** vervolgkeuzelijst.
5. Selecteer de **gegevensgateway** u geïnstalleerd en geregistreerd. U kunt een andere gateway instellen door te selecteren '(toevoegen van nieuwe gegevensgateway...)'.

   ![Selecteer de gegevensgateway voor de module gegevens importeren](./media/use-data-from-an-on-premises-sql-server/import-data-select-on-premises-data-source.png)
6. Voer de SQL **databaseservernaam** en **databasenaam**, samen met de SQL **databasequery** u wilt uitvoeren.
7. Klik op **waarden invoeren** onder **gebruikersnaam en wachtwoord** en voer de referenties van uw database. U kunt Windows geïntegreerde verificatie of SQL Server-verificatie, afhankelijk van hoe uw on-premises SQL Server is geconfigureerd.

   ![Voer de referenties van database](./media/use-data-from-an-on-premises-sql-server/database-credentials.png)

   Het bericht 'values vereist' wordt overgeschakeld naar 'set waarden' met een groen vinkje. U moet slechts één keer de referenties invoert, tenzij de database-informatie of het wachtwoord wordt gewijzigd. Azure Machine Learning maakt gebruik van het certificaat dat u hebt opgegeven tijdens de installatie van de gateway voor het versleutelen van de referenties in de cloud. Azure slaat nooit on-premises referenties zonder versleuteling.

   ![Eigenschappen van de module gegevens importeren](./media/use-data-from-an-on-premises-sql-server/import-data-properties-entered.png)
8. Klik op **uitvoeren** om uit te voeren van het experiment.

Wanneer het experiment is voltooid, kunt u de gegevens die u hebt geïmporteerd uit de database door te klikken op de uitvoerpoort van visualiseren de **importgegevens** -module en selecteer **Visualize**.

Wanneer u klaar bent met het ontwikkelen van uw experiment, kunt u deze kunt implementeren en operationeel maken van uw model. Met behulp van de Batchuitvoeringsservice gegevens uit de on-premises SQL Server-database is geconfigureerd de **importgegevens** module worden gelezen en gebruikt voor het scoren. U kunt de Request Response-Service gebruiken voor het scoren van on-premises gegevens, Microsoft adviseert om de [Excel-invoegtoepassing](excel-add-in-for-web-services.md) in plaats daarvan. Op dit moment schrijven naar een on-premises SQL Server-database via **gegevens exporteren** wordt niet ondersteund in uw experimenten of gepubliceerde webservices.
