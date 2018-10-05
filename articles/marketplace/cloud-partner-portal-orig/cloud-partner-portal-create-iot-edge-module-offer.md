---
title: Maak een aanbieding van IoT Edge-Module | Microsoft Docs
description: Het publiceren van een nieuwe IoT Edge-Module voor de Marketplace.
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
ms.date: 09/19/2018
ms.author: pbutlerm
ms.openlocfilehash: d3cc1a09963c5f7fee613af24c63fd15b1cfffee
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48809455"
---
# <a name="how-to-publish-a-new-iot-edge-module-in-the-cloud-partner-portal"></a>Het publiceren van een nieuwe IoT Edge-Module in de Cloud Partner-Portal

In dit artikel beschrijft de stappen voor het publiceren van een nieuwe aanbieding voor IoT Edge-module.

## <a name="prerequisites"></a>Vereisten

De volgende vereisten gelden voor het publiceren van een IoT Edge-module voor Azure Marketplace.

-   Toegang tot de [Cloud Partner-Portal (CPP)](https://cloudpartner.azure.com/#alloffers). Zie voor meer informatie de [publicatiehandleiding voor](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).

-   Uw IoT Edge-module die wordt gehost in een Azure Container Registry hebt.

-   Beschikken over de metagegevens van IoT Edge module gereed (met inbegrip van niet-uitputtende lijst):

    -   Titel

    -   Beschrijving (eenvoudige HTML-indeling)

    -   Een logo in PNG-indeling en in de volgende grootten: 40 pixels x 40 pixels, maximaal 90 pixels x 90 pixels, 115 pixels x 115 pixels, 255 pixels x 115 pixels.

    -   Gebruiksvoorwaarden en privacyverklaring

<a name="prepare-your-iot-edge-module-listing-in-cpp"></a>De vermelding van uw IoT Edge-Module in CPP voorbereiden
-------------------------------------------

### <a name="create-a-new-offer-of-the-type-iot-edge-module"></a>Maak een nieuwe aanbieding van het type IoT Edge-module 

Volg deze stappen voor het voorbereiden van uw aanbieding IoT Edge-Module:

-   Meld u aan uw [CPP account](https://cloudpartner.azure.com/).

>[!Note]
>Voor algemene informatie over de Cloud Partner-Portal, kunt u controleren de [documentatie voor meer informatie](https://cloudpartner.azure.com/#learn)

-   Selecteer **nieuwe aanbieding**, en selecteer vervolgens **IoT Edge-Module**.

>[!NOTE]
>Een IoT Edge-Module is een container die specifiek is gemaakt om uit te voeren op IoT Edge. De scenario's waarvoor een IoT Edge-module moeten zinvol zijn in de context van een IoT Edge. Dit omvat ook standaardconfiguratie-instellingen voor het maken van de implementatie van een eenvoudigere van IoT Edge-apparaat. De container, omvat mogelijk ook de IoT Edge Module SDK om te communiceren met de edgeHub en IoT Hub.

### <a name="define-your-offer-settings"></a>Definieer de instellingen van uw aanbieding

Voer de gegevens voor uw aanbod in het tabblad instellingen bieden.

![Identiteit](./media/cloud-partner-portal-create-iot-edge-module-offer/offer-identity.png)


-   De **aanbiedings-ID** uniek wordt aangeduid uw aanbieding in CPP en kan worden gebruikt in klantgerichte URL's.

-   De **naam** is alleen zichtbaar zijn door u voor deze aanbieding in CPP verwijzende.

### <a name="create-one-or-more-skus"></a>Maak een of meer SKU 's

Elke SKU correspondeert met een containerinstallatiekopie. U moet ten minste één SKU en meer dan één kunt toevoegen. Er zijn twee onderdelen naar een SKU:

-   SKU-metagegevens

-   Metagegevenscontainer

**Een SKU maken:**

Selecteer de **SKU's** tabblad, en selecteer vervolgens **nieuwe Voorraadeenheid**.

![Nieuwe Voorraadeenheid](./media/cloud-partner-portal-create-iot-edge-module-offer/SKUs.png)

De SKU-metagegevens bevat de volgende velden, die vereist zijn:
- SKU-ID: een unieke id.
- Titel: de titel van de SKU, maximaal 50 tekens.
- Overzicht: een korte beschrijving, maximaal 100 tekens.
- Beschrijving: een lange beschrijving.
- Deze SKU verbergen: de standaardwaarde is **Nee**.
   
![SKU-details](./media/cloud-partner-portal-create-iot-edge-module-offer/SKU-settings.png)

#### <a name="iot-edge-module-metadata-and-the-container-registry"></a>De metagegevens van de IoT Edge-module en de container registry

Metagegevens van de IoT Edge-module heeft de installatiekopie van referentie-informatie die opgeslagen in Azure container registry (ACR). De Azure Marketplace kopieert de installatiekopie naar het register van de openbare marketplace en deze na de certificeringsinstantie is beschikbaar voor klanten. Alle gebruikersaanvraag gebruiken voor de installatiekopie van een IoT Edge-module worden uitgevoerd vanuit de Marketplace container registry.

![Containerinstallatiekopieën](./media/cloud-partner-portal-create-iot-edge-module-offer/container-images.png)

**Containerinstallatiekopieën**

De Details van de opslagplaats van de installatiekopie heeft de volgende vereiste velden:

-   **Abonnements-ID** abonnements-id van de Azure waarin de ACR-register zich bevindt.

-   **De naam van resourcegroep** – naam van de resourcegroep van het ACR-register.

-   **Naam van het containerregister** – naam van de ACR-register.

-   **Naam van de opslagplaats** – naam van de opslagplaats. Nadat deze is ingesteld, kan niet deze waarde later worden gewijzigd. De naam moet uniek zijn, zodat er geen ander aanbod voor uw account dezelfde naam heeft.

-   **Gebruikersnaam** – de gebruikersnaam die is gekoppeld aan een ACR (admin gebruikersnaam).

-   **Wachtwoord** : het wachtwoord dat is gekoppeld aan ACR.

    >[!Note]
    >De gebruikersnaam en wachtwoord zijn vereist om ervoor te zorgen dat partners toegang tot de ACR beschreven in het publicatieproces hebben.

Wanneer u de installatiekopie van een IoT Edge-module publiceert, kunt u een of meer installatiekopielabels opgeven. Zorg ervoor dat een 'nieuwste' code (de standaardinstelling) toevoegen aan uw module, zodat u de module eenvoudig tijdens het testen identificeren kunt.

U kunt ook de volgende details van de IoT Edge-module opgeven:

-   **createOptions** -de standaard-createOptions om door te geven zodat deze IoT Edge-module gebruiksklaar kan worden gestart.

-   **dubbele:** -het dubbele standaard om door te geven zodat deze IoT Edge-module kan worden gestart gebruiksklaar bij het gebruik van de IoT-SDK-Module.

-   **routes:** -de standaardroutes om door te geven zodat deze IoT Edge-module gebruiksklaar kan worden gestart als de IoT-SDK-Module.

### <a name="describe-your-iot-edge-module-for-your-customers"></a>Beschrijf uw IoT Edge-module voor uw klanten

Voeg uw marketing-specifieke inhoud op het tabblad Marketplace. Deze informatie is wat worden openbaar zichtbaar en weergegeven op de Azure Marketplace.

![Overzicht van IoT Edge-module](./media/cloud-partner-portal-create-iot-edge-module-offer/overview.png)

-   **Titel** -de titel van uw IoT Edge-module die wordt openbaar.

-   **Samenvatting** -samenvatting van uw IoT Edge-module die wordt openbare in de meeste pagina's, zoals het bladeren door pagina's.

-   **Lange samenvatting** -samenvatting van uw IoT Edge-module die wordt openbare wanneer de module de weergegeven. 

-   **Beschrijving** -de beschrijving van uw IoT Edge-module die wordt openbare op de detailpagina van het product. (U kunt eenvoudige HTML-codes gebruiken om de opmaak van de beschrijving van uw.)

-   **Marketing-id** -een unieke id die wordt gebruikt om de URL van uw product te maken. Deze URL is in de volgende indeling: *azuremarketplace.microsoft.com/enus/marketplace/apps/yourpublisherid.youriotedgemodulemarketingidentifier*.

-   **Preview-abonnement aanleveren** - gebruikers die toegang tot deze abonnementen hebben is mogelijk om te zien van de IoT Edge-module na de certificeringsstap en voordat deze meteen live.

-   **Nuttige koppelingen** -u kunt maximaal 10 koppelingen die wordt weergegeven op de pagina met details van uw product toevoegen.

-   **Aanbevolen categorieën** -maximaal vijf categorieën kiezen. Deze wordt weergegeven op de pagina met details van uw product. Op dit moment alle IoT Edge-modules worden weergegeven onder de *Internet of Things \> IoT Edge-module* categorieën in de's bladeren.

-   **Logo's** -uploaden van uw IoT Edge-module-logo-installatiekopieën in de PNG-indeling. Gebruik de volgende grootten: en precies in de volgende grootten: 40 pixels x 40 pixels, maximaal 90 pixels x 90 pixels, 115 pixels x 115 pixels, 255 pixels x 115 pixels.

-   **Schermafbeeldingen** -schermafbeeldingen worden weergegeven op de pagina met details van uw product. Ze zijn een goede manier om te communiceren visueel wat uw IoT Edge-module doet en hoe het werkt. U kunt architectuurdiagrammen weergeven of case illustraties bijvoorbeeld gebruiken.

-   **Video's** -video's worden weergegeven op de pagina met details van uw product. Ze zijn een goede manier om te communiceren visueel wat uw IoT Edge-module doet en hoe het werkt.

-   **Management leiden** -kunt u een systeem voor het verzamelen van alle potentiële klanten die interesse in uw product weergeven.

-   **Privacy** -hebt u een URL die verwijst naar het privacybeleid.

-   **Gebruiksvoorwaarden** -u moet de gebruiksvoorwaarden hebben. HTML-codes kunt u deze pagina opmaken of verwijst naar een van de andere pagina's.

### <a name="enter-your-support-contact-information"></a>Voer uw contactgegevens voor ondersteuning

In het tabblad ondersteuning bieden **Engineering Contact** en **Customer Support** informatie.

![Contactpersonen voor ondersteuning](./media/cloud-partner-portal-create-iot-edge-module-offer/support-contact-info.png)


## <a name="certify-your-iot-edge-module"></a>Certificeren van uw IoT Edge-module

Nadat u alle informatie die vereist zijn opgegeven, selecteert u **publiceren** voor het verzenden van uw IoT Edge-module voor certificering. U ziet een tijdlijn waarin de stappen in het certificeringsproces's.

**Module-verificatie**

De module wordt gecontroleerd door onze certificering-team. Nadat de module is gecertificeerd krijgt u een persoonlijke verbinding voor het testen van uw module. Als u wijzigingen aanbrengen nadat u hebt getest moet, metagegevens van uw aanbieding te bewerken en de module naar het team van de certificeringsinstantie opnieuw te verzenden. 

## <a name="publish-your-iot-edge-module"></a>Publiceren van uw IoT Edge-module

Nadat u klaar bent met testen en zijn gereed om te publiceren, selecteert u **Go Live** voor het publiceren van uw IoT Edge-module.

>[!Important]
>Als u wilt dat uw IoT Edge-module aangekondigd op de Ignite-gebeurtenis, moet de module openbare 23-09-2018.
