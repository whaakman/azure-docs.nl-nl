---
title: App publiceren stappen | Microsoft Docs
description: .
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: pbutlerm
ms.openlocfilehash: 0fc82229e158ed35b97203b11d08841c683c45ce
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48809919"
---
<a name="app-publishing-steps"></a>App publiceren stappen
====================

Voor het starten van het proces van publiceren, klikt u op 'Publiceren' onder het tabblad Editor.

![Knop CPP-App publiceren](./media/d365-financials/image014.jpg)


Klik op het tabblad Status ziet u de publicatie stappen waarmee wordt aangegeven waar uw aanbieding in het publicatieproces. Op enig moment gedurende het publicatieproces, kunt u ook Meld u aan en klik op het tabblad alle biedt om de meest recente status voor het gebruik van uw aanbiedingen weer te geven. U kunt op rechtstreeks op de status van uw aanbieding en bekijk de details voor waar uw aanbieding in het publicatieproces is.

Laten we elk van de publicatie stappen doorlopen, bespreken wat er gebeurt bij elke stap en hoe lang u moet een schatting maken dat elke stap duurt.

![Publiceren procesdiagram](./media/d365-financials/image017.png)


**Valideer de vereisten**

Als u 'Publiceren' klikt, is een geautomatiseerde controle vindt plaats om te controleren of dat u alle vereiste velden in uw aanbieding hebt ingevuld. Als alle velden zijn niet gevuld, een waarschuwing wordt weergegeven naast het veld en u moet voor het vullen van het nauwkeurig klikt u op 'Publiceren' opnieuw.

Nadat u deze stap correct hebt voltooid, wordt een pop-upvenster weergegeven waarin wordt gevraagd om een e-mailadres dat wordt gebruikt om u voor het publiceren van meldingen te sturen. Nadat u uw e-mailadres hebt ingediend, wordt deze stap is voltooid.


**Automatische toepassing validatie**

In deze stap controleert onze geautomatiseerde certificeringsservice toepassingsextensies voorzien van een aanbieding die de inhoud ervan worden uitgelijnd met metagegevens bieden. Altijd voor zorgen dat uw app-naam, versie, uitgever en -ID overeenkomt met opgegeven in het extensiemanifest met de naam `app.json`.


**Test Drive niet valideren**

Als u ervoor hebt gekozen voor het instellen van Test Drive, wordt in deze fase is waar uw Test Drive-instellingen worden gevalideerd.


**Validatie van de lead management en de registratie**

In deze fase is als u de functie generatie leiden, geconfigureerd zullen we dit valideren of uw CRM-integratie werkt door een lead test verzenden naar uw CRM. Hier ziet u een record met de valse gegevens in uw CRM- of Azure Table vullen nadat deze stap voltooid is. Alle documentatie voor het genereren van potentiële klant zich bevindt.


**AppSource-pakketten**

Uw webwinkel details artefacten worden gecontroleerd en AppSource preview pakket wordt gegenereerd.


**Afmeldings-uitgever**

In deze fase is de **Go Live** knop wordt nu actief wordt. U hebt nu ook een koppeling om een voorbeeld van uw aanbieding (met uw hidekey). Wanneer u tevreden bent met het uiterlijk van de Preview-versie, klikt u op de knop Go Live.
Houd er rekening mee, deze aanvraag zorgt dat uw app te publiceren op Appsource, maar in plaats daarvan het onze interne validatieproces activeert.


**Marketing en technische validatie**

Deze stap is waar we de marketing- en technische validaties parallel uitvoeren. Raadpleeg de [controlelijst voor het indienen van uw App](https://aka.ms/CheckBeforeYouSubmit) en [ontwikkelen van Apps voor Dynamics 365 voor Finance and Operations whitepaper](https://go.microsoft.com/fwlink/?linkid=841518) richtlijnen documenten voor verplichte vereisten en aanbevelingen. Tijdens het validatieproces we het volgende doen:
-  samen met u op alle openstaande vragen en problemen.  
- u voorzien van een app publiceren datum en een melding wanneer uw app wordt gepubliceerd. 
- bieden u een eerste feedback over de technische en de marketing validatie binnen 5-7 werkdagen.

Deze stappen doorgaans een week kunnen overnemen en hoeven niet voor u om te blijven continu geregistreerd in de Cloud Partner-Portal.


**Met de service-toepassing publiceren**

Uw aanbieding ondergaat laatste verwerking. Uw app marketing- en technische validatie is verstreken, maar nu laatste verwerking zodat deze gereed voor App-bron moet doorlopen.


**Live**

Uw aanbieding is nu Live op AppSource en klanten kunnen weergeven en uw app implementeren in hun Microsoft Dynamics 365 Business Central-abonnementen. U ontvangt een e-mailbericht van ons melding dat uw app openbaar is gemaakt op de bron van de App. Op elk gewenst moment kunt u klikt u op het tabblad alle aanbiedingen en de status bekijken voor uw aanbiedingen die zijn vermeld in de rechterkolom. U kunt klikken op de status om te zien van de publicatiestatus van de stroom in de details voor uw aanbieding.


<a name="error-handling"></a>Foutafhandeling
--------------

Tijdens het publicatieproces, kan een fout optreden. Als er een fout is opgetreden, ontvangt u een e-mailmelding waarin wordt gemeld dat er een fout met instructies voor de volgende stappen opgetreden. U ziet ook fouten op elk gewenst moment tijdens dit proces door te klikken op het tabblad Status. U ziet dat punt in het proces dat de fout is opgetreden, samen met een foutbericht geeft een overzicht van wat er moet worden omgezet.

Als er fouten tijdens het publicatieproces optreden, bent u vereist voor het oplossen van deze problemen en klik vervolgens op **publiceren** het proces opnieuw starten. U moet beginnen aan het begin van de publicatie stappen in **vereisten valideren** bij het opnieuw na elke fout-oplossing te publiceren.

Als u een fout bij het oplossen van problemen, moet u een ondersteuningsaanvraag om hulp te krijgen van onze ondersteuningsmedewerkers openen.


<a name="canceling-the-publishing-request"></a>Het publicatieverzoek annuleren
--------------------------------

U kunt start het proces van de publicatie en een nodig voor het annuleren van uw aanvraag. U kunt alleen een aanvraag voor publicatie annuleren nadat de aanvraag publiceren de stap aftekening van de uitgever bereikt. Als u wilt annuleren, klikt u op **publiceren annuleren**. De status van de publicatie wordt opnieuw ingesteld naar stap 1, en als u wilt opnieuw publiceert, moet u publiceren op en volg de stappen in de status.

![Publicatie van de knop Annuleren](./media/d365-financials/image013.png)
