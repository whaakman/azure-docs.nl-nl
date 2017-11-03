---
title: Beheren van een Machine Learning-werkruimte | Microsoft Docs
description: Toegang tot Azure Machine Learning-werkruimten, beheren en implementeren en beheren van ML API-webservices
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: daf3d413-7a77-4beb-9a7a-6b4bdf717719
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye
ms.openlocfilehash: 2f90234bdd5c917a502d24cd16256bc11c7fbed0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-an-azure-machine-learning-workspace"></a>Een Azure Machine Learning-werkruimte beheren

> [!NOTE]
> Zie voor meer informatie over het beheren van Web-services in de portal voor Machine Learning-webservices [beheren van een webservice via de portal voor Azure Machine Learning-webservices](manage-new-webservice.md).
> 
> 

Machine Learning-werkruimten in de Azure-portal of de klassieke Azure portal, kunt u beheren.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken

Voor het beheren van een werkruimte in de Azure-portal:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) met een administrator-account Azure-abonnement.
2. Voer in het zoekvak boven aan de pagina 'machine learning werkruimten' en selecteer vervolgens **Machine Learning-werkruimten**.
3. Klik op de werkruimte die u wilt beheren.

Naast de standaard resourcegegevens voor beheer en de opties die beschikbaar zijn, kunt u het volgende doen:

- Weergave **eigenschappen** : deze pagina bevat de informatie van de werkruimte en resource en kunt u het abonnement en de resource-groep die deze werkruimte is verbonden met wijzigen.
- **Opslagsleutels Resync** -de werkruimte onderhoudt sleutels naar het opslagaccount. Als het opslagaccount sleutels wijzigt vervolgens kunt u op **sleutels synchroniseren** de sleutels synchroniseren met de werkruimte.

Voor het beheren van de webservices die zijn gekoppeld aan deze werkruimte kunt u de Machine Learning-webservices-portal gebruiken. Zie [beheren van een webservice via de portal voor Azure Machine Learning-webservices](manage-new-webservice.md) voor volledige informatie.

> [!NOTE]
> Als u wilt implementeren of beheren van nieuwe webservices als u een rol Inzender of -beheerder voor het abonnement waarop de webservice is geïmplementeerd zijn toegewezen. Als u een andere gebruiker voor een machine learning-werkruimte uitnodigen, moet u ze toewijzen aan een rol Inzender of -beheerder voor het abonnement voordat ze kunnen implementeren of beheren van webservices. 
> 
>Zie voor meer informatie over toegangsmachtigingen instellen [toegangstoewijzingen weergeven voor gebruikers en groepen in de Azure portal - openbare preview](../../active-directory/role-based-access-control-manage-assignments.md).

## <a name="use-the-azure-classic-portal"></a>Gebruik de klassieke Azure portal

Met de klassieke Azure portal, kunt u uw werkruimten Machine Learning om te beheren:

* Controleren hoe de werkruimte wordt gebruikt
* De werkruimte wilt toestaan of weigeren van toegang configureren
* Webservices die zijn gemaakt in de werkruimte beheren
* Verwijderen van de werkruimte

Bovendien wordt het dashboardtabblad biedt een overzicht van het gebruik van uw werkruimte en een snelle weergave van uw werkruimtegegevens.  

> [!TIP]
> In Azure Machine Learning Studio op de **WEBSERVICES** tabblad, u kunt toevoegen, bijwerken of verwijderen van een Machine Learning-webservice.
> 
> 

Voor het beheren van een werkruimte in de klassieke Azure portal:

1. Aanmelden bij de [klassieke Azure-portal](https://manage.windowsazure.com/) met uw Microsoft Azure-account - gebruik van het account dat is gekoppeld aan het Azure-abonnement.
2. Klik in het deelvenster Microsoft Azure-services op **MACHINE LEARNING**.
3. Klik op de werkruimte die u wilt beheren.

De pagina voor werkruimte heeft drie tabbladen:

* **DASHBOARD** -kunt u gebruik van de werkruimte weergeven en informatie
* **CONFIGUREER** -kunt u voor het beheren van toegang tot de werkruimte
* **WEBSERVICES** -kunt u Web-services die zijn gepubliceerd vanuit deze werkruimte beheren

### <a name="to-monitor-how-the-workspace-is-being-used"></a>Om te controleren hoe de werkruimte wordt gebruikt
Klik op de **DASHBOARD** tabblad.

Vanuit het dashboard kunt u algemene informatie over het gebruik van uw werkruimte weergeven en ophalen van een snelle weergave van informatie van de werkruimte.

* De **COMPUTE** diagram toont de rekenresources die wordt gebruikt door de werkruimte. U kunt de weergave om relatieve weer te geven of absolute waarden wijzigen en kunt u het tijdskader weergegeven in de grafiek.
* **Overzicht gebruik** Azure-opslag wordt gebruikt door de werkruimte wordt weergegeven.
* **Snelle weergave** bevat een samenvatting van de werkruimte informatie en nuttige koppelingen.

> [!NOTE]
> De **aanmelden bij ML Studio** koppeling met de Microsoft-Account dat u momenteel bent aangemeld bij van Machine Learning Studio wordt geopend. Het Microsoft-Account waarmee u zich aanmeldt bij de klassieke Azure portal om een werkruimte te maken is niet automatisch gemachtigd om deze werkruimte te openen. Om te openen in een werkruimte, moet u zijn aangemeld bij de Microsoft-Account dat is gedefinieerd als eigenaar van de werkruimte of moet u een uitnodiging ontvangen van de eigenaar van het koppelen van de werkruimte.
> 
> 

### <a name="to-grant-or-suspend-access-for-users"></a>Om te verlenen of onderbreken van toegang voor gebruikers
Klik op de **configureren** tabblad.

U kunt op het tabblad configuratie:

* Toegang tot de Machine Learning-werkruimte onderbreken door te klikken op weigeren. Gebruikers niet meer worden kunnen de werkruimte te openen in Machine Learning Studio. Om toegang te herstellen, klikt u op toestaan.

Voor het beheren van extra accounts die toegang tot de werkruimte in Machine Learning Studio hebben, klikt u op **aanmelden bij ML Studio** in de **DASHBOARD** tabblad (Zie de voorgaande opmerking met betrekking tot **bij aanmelden ML Studio**). Hiermee opent u de werkruimte in Machine Learning Studio. Hier kunt klikt u op de **instellingen** tabblad en vervolgens **gebruikers**. U kunt klikken op **meer gebruikers UITNODIGEN** gebruikers toegang geven tot de werkruimte of Selecteer een gebruiker en klik op **verwijderen**.

### <a name="to-manage-web-services-in-this-workspace"></a>Voor het beheren van web-services in deze werkruimte
Klik op de **WEBSERVICES** tabblad.

Dit geeft een lijst van webservices die worden gepubliceerd vanuit deze werkruimte.
Voor het beheren van een webservice, klikt u op de naam in de lijst om de webpagina van de service te openen.

Een webservice heeft misschien een of meer eindpunten zijn gedefinieerd.

* U kunt meer eindpunten naast het eindpunt 'Standaard' definiëren. Klik op als u het eindpunt **eindpunten beheren** aan de onderkant van het dashboard te openen van de portal voor Azure Machine Learning-webservices.
* Verwijderen van een eindpunt (u kunt het eindpunt 'Standaard' niet verwijderen), klik op het selectievakje aan het begin van de rij eindpunt en klik op **verwijderen**. Hiermee verwijdert u het eindpunt van de webservice.
  
  > [!NOTE]
  > Als de webservice-eindpunt is gebruikt door een toepassing wanneer het eindpunt wordt verwijderd, wordt de toepassing een foutbericht wanneer u de volgende keer die wordt geprobeerd toegang tot de service.
  > 
  > 

Klik op de naam van een webservice-eindpunt om dit te openen. 

U kunt algemene informatie over het gebruik van uw Web-service gedurende een periode bekijken vanuit het dashboard. U kunt de periode in het vervolgmenu van de periode in de rechterbovenhoek van de usage-grafieken selecteren. Het dashboard bevat de volgende informatie:

* **Aanvragen gedurende een periode** bevat een grafiek stap van het aantal aanvragen gedurende de geselecteerde tijdsperiode. Kunt u identificeren als er pieken in gebruik.
* **Reactie op aanvragen aanvragen** geeft het totaal aantal aanvragen en antwoorden aanroepen van de service heeft ontvangen via de geselecteerde tijdsperiode en hoeveel is mislukt.
* **Gemiddelde tijd voor aanvragen en antwoorden Compute** gemiddelde van de tijd die nodig zijn voor het uitvoeren van de ontvangen aanvragen weergegeven.
* **Batch-aanvragen** geeft het totale aantal Batch-aanvragen via de geselecteerde tijdsperiode en hoeveel hiervan mislukt de service heeft ontvangen.
* **Gemiddelde latentie van de taak** gemiddelde van de tijd die nodig zijn voor het uitvoeren van de ontvangen aanvragen weergegeven.
* **Fouten** geeft het totale aantal fouten dat is opgetreden op aanroepen naar de webservice.
* **Services-kosten** geeft de kosten voor het abonnement dat is gekoppeld aan de service.

Via de pagina configureren kunt u de volgende eigenschappen bijwerken:

* **Beschrijving** kunt u een beschrijving invoeren voor de webservice. De beschrijving is een verplicht veld.
* **Logboekregistratie** kunt u in- of uitschakelen van de fout bij het aanmelden op het eindpunt. Zie voor meer informatie over logboekregistratie inschakelen [logboekregistratie voor Machine Learning-webservices](web-services-logging.md).
* **Inschakelen van voorbeeldgegevens** kunt u voorbeeldgegevens die u gebruiken kunt voor het testen van de aanvraag / antwoord-service opgeven. Als u de webservice in Machine Learning Studio gemaakt, is de voorbeeldgegevens genomen van de gegevens uw gebruikt voor het model te trainen. Als u de service via een programma hebt gemaakt, worden de gegevens worden opgehaald uit de bijvoorbeeld gegevens die u hebt opgegeven als onderdeel van de JSON-pakket.

