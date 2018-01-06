---
title: De portal voor Azure Machine Learning-webservices gebruiken | Microsoft Docs
description: Toegang tot Azure Machine Learning-werkruimten, beheren en implementeren en beheren van ML API-webservices
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: b62cf2ca-dd2a-4a83-bb54-469f948fb026
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: v-donglo
ms.openlocfilehash: 2e71fa46f88ce219bee96a223a034078a037b4d3
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="manage-a-web-service-using-the-azure-machine-learning-web-services-portal"></a>Beheren van een webservice via de portal voor Azure Machine Learning-webservices
U kunt uw Machine Learning nieuwe en klassieke webservices met behulp van de Microsoft Azure Machine Learning Web Services-portal beheren. Aangezien klassieke webservices en nieuwe Web-services zijn gebaseerd op verschillende onderliggende technologieën, hebt u iets anders beheermogelijkheden voor elk van deze.

U kunt in de portal voor Machine Learning-webservices:

* Controleren hoe de web-service wordt gebruikt.
* Configureren van de beschrijving, het bijwerken van de sleutels voor de web service (alleen nieuw), uw storage-account key (nieuw alleen) inschakelen aan te melden, bijwerken en in- of uitschakelen voorbeeldgegevens.
* Verwijder de webservice.
* Maak, delete of update facturering plannen (alleen nieuw).
* Toevoegen en verwijderen van eindpunten (alleen klassiek)

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="permissions-to-manage-new-resources-manager-based-web-services"></a>Machtigingen voor het beheren van nieuwe Resources Manager op basis van webservices

Nieuwe webservices worden geïmplementeerd als Azure-resources. Zo moet u de juiste machtigingen te implementeren en beheren van nieuwe webservices hebben.  Als u wilt implementeren of beheren van nieuwe webservices als u een rol Inzender of -beheerder voor het abonnement waarop de webservice is geïmplementeerd zijn toegewezen. Als u een andere gebruiker voor een machine learning-werkruimte uitnodigen, moet u ze toewijzen aan een rol Inzender of -beheerder voor het abonnement voordat ze kunnen implementeren of beheren van webservices. 

Als de gebruiker niet de juiste machtigingen heeft voor toegang tot bronnen in de portal voor Azure Machine Learning-webservices, ontvangt deze de volgende fout is opgetreden bij het implementeren van een webservice:

*Web Service-implementatie is mislukt. Dit account heeft niet voldoende toegangsrechten voor de Azure-abonnement dat in de werkruimte bevat. Om een Web-Service implementeren in Azure, hetzelfde account moet worden uitgenodigd voor de werkruimte en toegang te krijgen tot de Azure-abonnement dat in de werkruimte bevat.*

Zie voor meer informatie over het maken van een werkruimte [maken en delen van een Azure Machine Learning-werkruimte](create-workspace.md).

Zie voor meer informatie over toegangsmachtigingen instellen [toegangstoewijzingen weergeven voor gebruikers en groepen in de Azure portal - openbare preview](../../active-directory/role-based-access-control-manage-assignments.md).


## <a name="manage-new-web-services"></a>Nieuwe Web-services beheren
Voor het beheren van uw nieuwe Web-services:

1. Aanmelden bij de [Microsoft Azure Machine Learning-webservices](https://services.azureml.net/quickstart) portal met behulp van uw Microsoft Azure-account - gebruik van het account dat is gekoppeld aan het Azure-abonnement.
2. Klik op het menu **webservices**.

Dit geeft een lijst met geïmplementeerde Web-services voor uw abonnement. 

Voor het beheren van een webservice, klikt u op Web-Services. U kunt via de pagina Web Services:

* Klik op de webservice voor het beheer ervan.
* Klik op de facturering plannen voor de webservice bij te werken.
* Verwijderen van een webservice.
* Een webservice kopiëren en deze implementeren in een andere regio.

Wanneer u een webservice klikt, wordt de pagina Quick Start-service wordt geopend. De pagina Quick Start webservice heeft twee opties die u voor het beheren van uw webservice toestaan:

* **DASHBOARD** -kunt u gebruik van Web service weergeven.
* **CONFIGUREER** - Hiermee kunt u beschrijvende tekst toevoegen voor het bijwerken van de sleutel voor het opslagaccount die is gekoppeld aan de webservice, en in- of uitschakelen voorbeeldgegevens.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Controleren hoe de web-service wordt gebruikt
Klik op de **DASHBOARD** tabblad.

U kunt algemene informatie over het gebruik van uw Web-service gedurende een periode bekijken vanuit het dashboard. U kunt de periode in het vervolgmenu van de periode in de rechterbovenhoek van de usage-grafieken selecteren. Het dashboard bevat de volgende informatie:

* **Aanvragen gedurende een periode** bevat een grafiek stap van het aantal aanvragen gedurende de geselecteerde tijdsperiode. Kunt u identificeren als er pieken in gebruik.
* **Reactie op aanvragen aanvragen** geeft het totaal aantal aanvragen en antwoorden aanroepen van de service heeft ontvangen via de geselecteerde tijdsperiode en hoeveel is mislukt.
* **Gemiddelde tijd voor aanvragen en antwoorden Compute** gemiddelde van de tijd die nodig zijn voor het uitvoeren van de ontvangen aanvragen weergegeven.
* **Batch-aanvragen** geeft het totale aantal Batch-aanvragen via de geselecteerde tijdsperiode en hoeveel hiervan mislukt de service heeft ontvangen.
* **Gemiddelde latentie van de taak** gemiddelde van de tijd die nodig zijn voor het uitvoeren van de ontvangen aanvragen weergegeven.
* **Fouten** geeft het totale aantal fouten dat is opgetreden op aanroepen naar de webservice.
* **Services-kosten** geeft de kosten voor het abonnement dat is gekoppeld aan de service.

### <a name="configuring-the-web-service"></a>De webservice configureren
Klik op de **configureren** menuoptie.

U kunt de volgende eigenschappen bijwerken:

* **Beschrijving** kunt u een beschrijving invoeren voor de webservice.
* **Titel** Hiermee kunt u een titel voor de webservice invoeren
* **Sleutels** kunt u uw API primaire en secundaire sleutels draaien.
* **Opslagaccountsleutel** kunt u de sleutel voor het opslagaccount dat is gekoppeld aan de wijzigingen van Web service bijwerken. 
* **Inschakelen van voorbeeldgegevens** kunt u voorbeeldgegevens die u gebruiken kunt voor het testen van de aanvraag / antwoord-service opgeven. Als u de webservice in Machine Learning Studio gemaakt, is de voorbeeldgegevens genomen van de gegevens uw gebruikt voor het model te trainen. Als u de service via een programma hebt gemaakt, worden de gegevens worden opgehaald uit de bijvoorbeeld gegevens die u hebt opgegeven als onderdeel van de JSON-pakket.

### <a name="managing-billing-plans"></a>Het beheren van facturerings plannen
Klik op de **plannen** menuoptie van de pagina Quick Start-services. U kunt ook klikken op de planning die is gekoppeld aan specifieke webservice voor het beheren van plan.

* **Nieuwe** kunt u een nieuw plan maken.
* **Abonnement toevoegen of verwijderen exemplaar** kunt u Scale-' een bestaand abonnement capaciteit toe te voegen.
* **Upgrade/DownGrade** kunt u 'opschalen' een bestaand abonnement capaciteit toe te voegen.
* **Verwijder** kunt u een plan te verwijderen.

Klik op een plan het dashboard weer te geven. Het dashboard kunt u een momentopname of plan gebruik gedurende een geselecteerde periode. Selecteer de tijdsperiode om weer te geven, klikt u op de **periode** vervolgkeuzelijst in de rechterbovenhoek van dashboard. 

Het dashboard plan bevat de volgende informatie:

* **Beschrijving van plan bent** geeft informatie weer over de kosten en capaciteit die zijn gekoppeld aan het plan.
* **Informatie over het gebruik van plan bent** geeft het aantal transacties en rekenuren die in rekening tegen het plan gebracht is.
* **Webservices** geeft het aantal Web-services die van dit plan gebruikmaken.
* **Top Webserviceaanroepen** wordt weergegeven de eerste vier webservices die aanroepen die worden berekend op basis van het plan.
* **Web-Services door Compute uur Top** worden de eerste vier Web-services die gebruikmaken van rekenresources die in rekening tegen het plan gebracht worden weergegeven.

## <a name="manage-classic-web-services"></a>Klassieke webservices beheren
> [!NOTE]
> De procedures in deze sectie zijn relevant zijn voor het beheer van klassieke webservices via de portal voor Azure Machine Learning-webservices. Zie voor informatie over het beheer van klassieke Web services via de Machine Learning Studio en de Azure portal, [beheren van een Azure Machine Learning-werkruimte](manage-workspace.md).
> 
> 

Voor het beheren van uw klassieke Web-services:

1. Aanmelden bij de [Microsoft Azure Machine Learning-webservices](https://services.azureml.net/quickstart) portal met behulp van uw Microsoft Azure-account - gebruik van het account dat is gekoppeld aan het Azure-abonnement.
2. Klik op het menu **klassieke webservices**.

Voor het beheren van een klassieke webservice, klikt u op **klassieke webservices**. U kunt via de klassieke Web Services-pagina:

* Klik op de webservice om de bijbehorende eindpunten weer te geven.
* Verwijderen van een webservice.

Wanneer u een klassieke webservice beheert, beheren u elk van de eindpunten afzonderlijk. Wanneer u een web-service op de pagina webservices klikt, wordt de lijst met eindpunten die zijn gekoppeld aan de service wordt geopend. 

U kunt op de pagina klassieke webservice-eindpunt toevoegen en verwijderen van de eindpunten van de service. Zie voor meer informatie over het toevoegen van eindpunten [eindpunten maken](create-endpoint.md).

Klik op een van de eindpunten te openen van de pagina Quick Start webservice. Op de pagina Quick Start zijn er twee opties die u voor het beheren van uw webservice toestaan:

* **DASHBOARD** -kunt u gebruik van Web service weergeven.
* **CONFIGUREER** - Hiermee kunt u beschrijvende tekst toevoegen foutregistratie in- of uitschakelen, de sleutel voor het opslagaccount die is gekoppeld aan de webservice, bijwerken en in- en uitschakelen voorbeeldgegevens.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Controleren hoe de web-service wordt gebruikt
Klik op de **DASHBOARD** tabblad.

U kunt algemene informatie over het gebruik van uw Web-service gedurende een periode bekijken vanuit het dashboard. U kunt de periode in het vervolgmenu van de periode in de rechterbovenhoek van de usage-grafieken selecteren. Het dashboard bevat de volgende informatie:

* **Aanvragen gedurende een periode** bevat een grafiek stap van het aantal aanvragen gedurende de geselecteerde tijdsperiode. Kunt u identificeren als er pieken in gebruik.
* **Reactie op aanvragen aanvragen** geeft het totaal aantal aanvragen en antwoorden aanroepen van de service heeft ontvangen via de geselecteerde tijdsperiode en hoeveel is mislukt.
* **Gemiddelde tijd voor aanvragen en antwoorden Compute** gemiddelde van de tijd die nodig zijn voor het uitvoeren van de ontvangen aanvragen weergegeven.
* **Batch-aanvragen** geeft het totale aantal Batch-aanvragen via de geselecteerde tijdsperiode en hoeveel hiervan mislukt de service heeft ontvangen.
* **Gemiddelde latentie van de taak** gemiddelde van de tijd die nodig zijn voor het uitvoeren van de ontvangen aanvragen weergegeven.
* **Fouten** geeft het totale aantal fouten dat is opgetreden op aanroepen naar de webservice.
* **Services-kosten** geeft de kosten voor het abonnement dat is gekoppeld aan de service.

### <a name="configuring-the-web-service"></a>De webservice configureren
Klik op de **configureren** menuoptie.

U kunt de volgende eigenschappen bijwerken:

* **Beschrijving** kunt u een beschrijving invoeren voor de webservice. De beschrijving is een verplicht veld.
* **Logboekregistratie** kunt u in- of uitschakelen van de fout bij het aanmelden op het eindpunt. Zie voor meer informatie over logboekregistratie inschakelen [logboekregistratie voor Machine Learning-webservices](web-services-logging.md).
* **Inschakelen van voorbeeldgegevens** kunt u voorbeeldgegevens die u gebruiken kunt voor het testen van de aanvraag / antwoord-service opgeven. Als u de webservice in Machine Learning Studio gemaakt, is de voorbeeldgegevens genomen van de gegevens uw gebruikt voor het model te trainen. Als u de service via een programma hebt gemaakt, worden de gegevens worden opgehaald uit de bijvoorbeeld gegevens die u hebt opgegeven als onderdeel van de JSON-pakket.


