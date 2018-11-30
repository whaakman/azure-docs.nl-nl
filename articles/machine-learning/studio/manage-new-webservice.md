---
title: Gebruik de portal voor Machine Learning-webservices - Azure Machine Learning Studio | Microsoft Docs
description: U kunt beheren op uw Machine Learning nieuwe en klassieke webservices met behulp van de portal voor Microsoft Azure Machine Learning-webservices. Omdat klassieke webservices en nieuwe Web services zijn afhankelijk van verschillende onderliggende technologieën, hebt u iets anders beheermogelijkheden voor elk van deze.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=yahajiza, author=YasinMSFT)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: b62cf2ca-dd2a-4a83-bb54-469f948fb026
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.openlocfilehash: 78e776da17e8e068ff45e426795fa5d86a92cc79
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52308661"
---
# <a name="manage-a-web-service-using-the-azure-machine-learning-web-services-portal"></a>Een webservice met behulp van de portal van Azure Machine Learning-webservices beheren
U kunt beheren op uw Machine Learning nieuwe en klassieke webservices met behulp van de portal voor Microsoft Azure Machine Learning-webservices. Omdat klassieke webservices en nieuwe Web services zijn afhankelijk van verschillende onderliggende technologieën, hebt u iets anders beheermogelijkheden voor elk van deze.

In Machine Learning-webservicesportal kunt u het volgende doen:

* Controleren hoe de webservice wordt gebruikt.
* De beschrijving configureren, bij te werken van de sleutels voor de web service (alleen nieuw), uw storage account key (alleen nieuw), enable logboekregistratie, bijwerken en in- of uitschakelen voorbeeldgegevens.
* De webservice verwijderen.
* Maken, delete of update facturering plannen (alleen nieuw).
* Toevoegen en verwijderen van eindpunten (alleen klassiek)

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="permissions-to-manage-new-resources-manager-based-web-services"></a>Machtigingen voor het beheren van nieuwe Resources Manager op basis van webservices

Nieuwe webservices worden geïmplementeerd als Azure-resources. Daarom moet u de juiste machtigingen om te implementeren en beheren van nieuwe webservices hebben.  Als u wilt implementeren of beheren van nieuwe webservices als u een rol Bijdrager of beheerder zijn op het abonnement waaraan de web-service is geïmplementeerd zijn toegewezen. Als u een andere gebruiker voor een machine learning-werkruimte kunt uitnodigen, moet u deze toewijzen aan een rol Bijdrager of beheerder zijn op het abonnement voordat u ze kunnen implementeren of beheren van webservices. 

Als de gebruiker beschikt niet over de juiste machtigingen voor toegang tot resources in de portal voor Azure Machine Learning-webservices, ontvangt deze de volgende fout tijdens het implementeren van een webservice:

*Implementatie van de webservice is mislukt. Dit account beschikt niet over voldoende toegangsrechten voor het Azure-abonnement met de werkruimte. Als u wilt een webservice implementeert in Azure, hetzelfde account moet worden uitgenodigd voor de werkruimte en toegang krijgen tot het Azure-abonnement met de werkruimte.*

Zie voor meer informatie over het maken van een werkruimte [maken en delen van een Azure Machine Learning-werkruimte](create-workspace.md).

Zie voor meer informatie over toegangsmachtigingen instellen [toegang met RBAC en de Azure-portal beheren](../../role-based-access-control/role-assignments-portal.md).


## <a name="manage-new-web-services"></a>Beheren van nieuwe webservices
Voor het beheren van uw nieuwe Web-services:

1. Aanmelden bij de [Microsoft Azure Machine Learning-webservices](https://services.azureml.net/quickstart) portal met uw Microsoft Azure-account - gebruik van het account dat is gekoppeld aan het Azure-abonnement.
2. In het menu, klikt u op **webservices**.

Dit geeft een lijst van de geïmplementeerde Web-services voor uw abonnement. 

Voor het beheren van een webservice, klikt u op Web Services. Op de pagina Web Services kunt u het volgende doen:

* Klik op de webservice om deze te beheren.
* Klik op de facturering plannen voor de webservice bij te werken.
* Een webservice verwijderen.
* Een webservice kopiëren en deze implementeren naar een andere regio.

Wanneer u een webservice klikt, wordt de web service Quick Start-pagina wordt geopend. De pagina Quick Start-service heeft twee menuopties waarmee u kunt voor het beheren van uw webservice:

* **DASHBOARD** -kunt u gebruik van de Web service weergeven.
* **CONFIGURE** - kunt u beschrijvende tekst toe te voegen bijwerken van de sleutel voor het opslagaccount dat is gekoppeld aan de webservice, en in- of uitschakelen voorbeeldgegevens.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Controleren hoe de webservice wordt gebruikt
Klik op de **DASHBOARD** tabblad.

Vanuit het dashboard vindt u algemene gebruik van uw Web-service gedurende een bepaalde periode. U kunt de periode om weer te geven in de periode vervolgkeuzelijst in de rechterbovenhoek van de grafieken gebruik selecteren. Het dashboard ziet u de volgende informatie:

* **Aanvragen gedurende een periode** wordt een grafiek stap van het aantal aanvragen voor de geselecteerde periode weergegeven. Kunt u bepalen of u met pieken in gebruik ondervindt.
* **Request Response-aanvragen** geeft het totale aantal Request Response-aanroepen die de service heeft ontvangen via de geselecteerde periode en hoe vaak is mislukt.
* **Gemiddelde tijd voor Request Response Compute** het gemiddelde van de tijd die nodig zijn voor het uitvoeren van de ontvangen aanvragen weergegeven.
* **Batch-aanvragen** geeft het totale aantal batchaanvragen de service heeft ontvangen via de geselecteerde periode en hoe vaak is mislukt.
* **Gemiddelde latentie van de taak** het gemiddelde van de tijd die nodig zijn voor het uitvoeren van de ontvangen aanvragen weergegeven.
* **Fouten** geeft het totale aantal fouten dat is opgetreden op aanroepen naar de webservice.
* **Services-kosten** geeft de kosten voor het abonnement dat is gekoppeld aan de service.

### <a name="configuring-the-web-service"></a>De webservice configureren
Klik op de **configureren** menu-optie.

U kunt de volgende eigenschappen bijwerken:

* **Beschrijving** kunt u een beschrijving voor de webservice in te voeren.
* **Titel** Hiermee kunt u een titel invoeren voor de webservice
* **Sleutels** kunt u om uw API primaire en secundaire sleutels te rouleren.
* **Opslagaccountsleutel** kunt u de sleutel voor het opslagaccount dat is gekoppeld aan de wijzigingen in de Web service bijwerken. 
* **Inschakelen van voorbeeldgegevens** kunt u voorbeeldgegevens die u gebruiken kunt voor het testen van de Request Response-service bieden. Als u de webservice in Machine Learning Studio gemaakt, is de voorbeeldgegevens afkomstig uit de gegevens uw gebruikt om uw model te trainen. Als u de service via een programma hebt gemaakt, worden de gegevens worden opgehaald uit de voorbeeldgegevens die u hebt opgegeven als onderdeel van het JSON-pakket.

### <a name="managing-billing-plans"></a>Abonnementen beheren
Klik op de **plannen** menuoptie in de Quick Start-pagina van web services. U kunt ook klikken op het plan dat is gekoppeld aan specifieke Web-service voor het beheren van het abonnement.

* **Nieuwe** kunt u een nieuw plan maken.
* **Toevoegen/verwijderen planexemplaar** kunt u "om uit te schalen" een bestaand plan capaciteit toe te voegen.
* **Upgrade/DownGrade** kunt u "om omhoog te schalen" een bestaand plan capaciteit toe te voegen.
* **Verwijder** kunt u een plan verwijderd.

Klik op een abonnement om het dashboard ervan weer te geven. Het dashboard kunt u een momentopname of plan gebruik gedurende een geselecteerde periode. Selecteer de tijdsperiode om weer te geven, klikt u op de **periode** vervolgkeuzelijst in de rechterbovenhoek van dashboard. 

Het dashboard plan bevat de volgende informatie:

* **Beschrijving van plan bent** geeft informatie weer over de kosten en de capaciteit die zijn gekoppeld aan het abonnement.
* **Gebruik van plan bent** geeft het aantal transacties en rekenuren die in rekening op basis van het abonnement gebracht is.
* **Webservices** geeft het aantal Web-services die van dit plan gebruikmaken.
* **Top Webserviceaanroepen** worden de eerste vier Web-services die aanroepen die in rekening op basis van het abonnement gebracht worden weergegeven.
* **Top webservices door uur Compute** worden de eerste vier Web-services die rekenresources die in rekening op basis van het abonnement gebracht worden weergegeven.

## <a name="manage-classic-web-services"></a>Klassieke webservices beheren
> [!NOTE]
> De procedures in deze sectie zijn relevant zijn voor het beheer van klassieke webservices via de portal van Azure Machine Learning-webservices. Zie voor meer informatie over het beheren van klassieke webservices door middel van de Machine Learning Studio en de Azure-portal [een Azure Machine Learning-werkruimte beheren](manage-workspace.md).
> 
> 

Voor het beheren van uw klassieke webservices:

1. Aanmelden bij de [Microsoft Azure Machine Learning-webservices](https://services.azureml.net/quickstart) portal met uw Microsoft Azure-account - gebruik van het account dat is gekoppeld aan het Azure-abonnement.
2. In het menu, klikt u op **klassieke webservices**.

Voor het beheren van een klassieke webservice, klikt u op **klassieke webservices**. U kunt op de pagina voor klassieke webservices:

* Klik op de webservice om de bijbehorende eindpunten weer te geven.
* Een webservice verwijderen.

Wanneer u een klassieke webservice beheert, beheren u elk van de eindpunten afzonderlijk. Wanneer u een webservice in de Web Services-pagina klikt, wordt de lijst met eindpunten die zijn gekoppeld aan de service wordt geopend. 

U kunt op de pagina klassieke Web Service-eindpunt toevoegen en verwijderen van de eindpunten van de service. Zie voor meer informatie over het toevoegen van eindpunten [eindpunten maken](create-endpoint.md).

Klik op een van de eindpunten om de web service Quick Start-pagina te openen. Er zijn twee menuopties waarmee u kunt voor het beheren van uw web-service op de pagina Quick Start:

* **DASHBOARD** -kunt u gebruik van de Web service weergeven.
* **CONFIGURE** - kunt u beschrijvende tekst toe te voegen foutregistratie in- en uitschakelen, de sleutel voor het opslagaccount dat is gekoppeld aan de webservice, bijwerken en inschakelen en uitschakelen voorbeeldgegevens.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Controleren hoe de webservice wordt gebruikt
Klik op de **DASHBOARD** tabblad.

Vanuit het dashboard vindt u algemene gebruik van uw Web-service gedurende een bepaalde periode. U kunt de periode om weer te geven in de periode vervolgkeuzelijst in de rechterbovenhoek van de grafieken gebruik selecteren. Het dashboard ziet u de volgende informatie:

* **Aanvragen gedurende een periode** wordt een grafiek stap van het aantal aanvragen voor de geselecteerde periode weergegeven. Kunt u bepalen of u met pieken in gebruik ondervindt.
* **Request Response-aanvragen** geeft het totale aantal Request Response-aanroepen die de service heeft ontvangen via de geselecteerde periode en hoe vaak is mislukt.
* **Gemiddelde tijd voor Request Response Compute** het gemiddelde van de tijd die nodig zijn voor het uitvoeren van de ontvangen aanvragen weergegeven.
* **Batch-aanvragen** geeft het totale aantal batchaanvragen de service heeft ontvangen via de geselecteerde periode en hoe vaak is mislukt.
* **Gemiddelde latentie van de taak** het gemiddelde van de tijd die nodig zijn voor het uitvoeren van de ontvangen aanvragen weergegeven.
* **Fouten** geeft het totale aantal fouten dat is opgetreden op aanroepen naar de webservice.
* **Services-kosten** geeft de kosten voor het abonnement dat is gekoppeld aan de service.

### <a name="configuring-the-web-service"></a>De webservice configureren
Klik op de **configureren** menu-optie.

U kunt de volgende eigenschappen bijwerken:

* **Beschrijving** kunt u een beschrijving voor de webservice in te voeren. De beschrijving is een verplicht veld.
* **Logboekregistratie** kunt u in- of uitschakelen van de fout bij het aanmelden op het eindpunt. Zie voor meer informatie over logboekregistratie inschakelen [logboekregistratie voor Machine Learning-webservices](web-services-logging.md).
* **Inschakelen van voorbeeldgegevens** kunt u voorbeeldgegevens die u gebruiken kunt voor het testen van de Request Response-service bieden. Als u de webservice in Machine Learning Studio gemaakt, is de voorbeeldgegevens afkomstig uit de gegevens uw gebruikt om uw model te trainen. Als u de service via een programma hebt gemaakt, worden de gegevens worden opgehaald uit de voorbeeldgegevens die u hebt opgegeven als onderdeel van het JSON-pakket.


