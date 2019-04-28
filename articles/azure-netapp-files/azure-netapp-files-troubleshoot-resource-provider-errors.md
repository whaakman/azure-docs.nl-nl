---
title: Azure NetApp bestanden Resource Provider-fouten oplossen | Microsoft Docs
description: Hierin wordt beschreven oorzaken, oplossingen en oplossingen voor veelvoorkomende fouten van de Resourceprovider van Azure NetApp bestanden.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: b-juche
ms.openlocfilehash: d4e06429aa1efec7c3301c7d0f0e7e17800fd520
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/24/2019
ms.locfileid: "63769434"
---
# <a name="troubleshoot-azure-netapp-files-resource-provider-errors"></a>Problemen met Azure NetApp Files-resourceproviders oplossen
Dit artikel wordt beschreven algemene Azure NetApp bestanden Resource Provider fouten, hun oorzaken, oplossingen en oplossingen. 

<a name="error_01"></a>***Azure Key Vault is niet geconfigureerd.***   
Azure Key Vault slaat de vereiste referenties voor toegang tot de onderliggende API. Deze fout geeft aan dat Azure Key Vault de volledige referenties voor toegang tot de onderliggende API niet heeft ontvangen.

* Oorzaak  
Azure Key Vault niet de juiste referenties hebt ontvangen, of de referenties zijn onvolledig.  

* Oplossing   
De Azure NetApp Files-service maakt gebruik van Azure Key Vault. Azure Key Vault wordt geverifieerd met behulp van een token uit Azure Active Directory. Daarom moet de eigenaar van de toepassing de toepassing registreren in de Azure Active Directory.

* Tijdelijke oplossing   
Geen.  Azure Key Vault moet correct worden ingesteld voor het gebruik van Azure NetApp bestanden.  

<a name="error_02"></a>***Maken van het Token kan niet worden gewijzigd.***   
Deze fout treedt op wanneer u probeert te wijzigen van het token maken nadat het volume is gemaakt.
Maken van het token moet worden ingesteld wanneer het volume wordt gemaakt en kan later worden gewijzigd.

* Oorzaak   
U probeert te wijzigen van het token maken nadat het volume is gemaakt, dit is geen ondersteunde bewerking.

* Oplossing   
Nadat het Volume is gemaakt, kunt u de parameter verwijderen uit de aanvraag voor het verwijderen van het foutbericht.

* Tijdelijke oplossing   
Als u wijzigen van het token voor het maken wilt, kunt u een nieuw volume maken met een nieuw token voor het maken en vervolgens de gegevens te migreren naar het nieuwe volume.


<a name="error_03"></a>***Maken van het Token moet ten minste 16 tekens lang zijn.***   
Deze fout treedt op wanneer het maken van het token niet aan de vereiste lengte voldoet. De lengte van het maken van het token moet ten minste 16 tekens lang zijn.

* Oorzaak   
Het maken van het token voldoet niet aan de vereiste lengte.  Wanneer u een volume met behulp van de API maakt, is een token voor het maken is vereist. Als u de portal gebruikt, kan het token automatisch worden gegenereerd.

* Oplossing   
Verhoog de lengte van het maken van het token. U kunt bijvoorbeeld een ander woord toevoegen aan het begin of het einde van het maken van het token.

* Tijdelijke oplossing   
De minimaal vereiste lengte van het maken van het token kan niet worden overgeslagen.  U kunt een voorvoegsel of achtervoegsel te verhogen van het maken van het token lengte.


<a name="error_04"></a>***Fout bij het verwijderen van een volume dat niet op Azure NetApp bestanden gevonden is.***   
Deze fout is opgetreden omdat het interne register van resources niet gesynchroniseerd is.

* Oorzaak   
Het volume mogelijk weergegeven in de portal blijven gedurende een bepaalde periode nadat deze is verwijderd. Als u het volume verwijdert met behulp van de API, is het mogelijk dat het volume niet correct is opgegeven. De fout kan ook worden veroorzaakt door verouderde browser-cache.

* Oplossing   
Cache wissen webbrowser als u de portal. Er is ook een interne cache die wordt elke tien minuten vernieuwd.  U kunt proberen opnieuw cache wissen.  Als het probleem zich blijft na 10 minuten voordoen, kunt u een ondersteuningsticket maken.

* Tijdelijke oplossing   
Gebruik een ander volume in de tussentijd en het bestaande bestand negeren.


<a name="error_05"></a>***Fout bij het invoegen van een nieuw Volume op Azure NetApp bestanden gevonden.***   
Deze fout treedt op omdat het interne register van resources niet gesynchroniseerd is.

* Oorzaak   
Het volume, blijven mogelijk weergegeven in de portal voor enige tijd nadat deze is verwijderd. Als u het volume verwijdert met behulp van de API, is het mogelijk dat het volume niet correct is opgegeven.

* Oplossing   
Als u van de portal gebruikmaakt, is het volume al gemaakt.  Het volume moet worden automatisch weergegeven. Als het probleem zich blijft voordoen, kunt u een ondersteuningsticket maken.

* Tijdelijke oplossing   
U kunt een volume maken met een andere naam en een token voor het maken van verschillende.


<a name="error_06"></a>***De bestandsnaam van het pad mag letters, cijfers en afbreekstreepjes bevatten (""-"") alleen.***   
Deze fout treedt op wanneer het bestandspad niet-ondersteunde tekens bevat, bijvoorbeeld een periode ("."), door komma's (","), onderstrepingsteken ('\_'), of dollarteken ("$").

* Oorzaak   
Het bestandspad bevat niet-ondersteunde tekens, bijvoorbeeld een periode ("."), door komma's (","), onderstrepingsteken ('\_'), of dollarteken ("$").

* Oplossing   
Verwijder de tekens die niet alfabetische letters, cijfers en afbreekstreepjes zijn ('-') van het bestandspad dat u hebt ingevoerd.

* Tijdelijke oplossing   
U kunt een onderstrepingsteken vervangen met een afbreekstreepje of kleine letters gebruiken in plaats van spaties aan het begin van de nieuwe woorden (bijvoorbeeld 'NewVolume' gebruiken in plaats van "Nieuw volume").


<a name="error_07"></a>***Volume-ID kan niet worden gewijzigd.***   
Deze fout treedt op wanneer u probeert te wijzigen van de volume-ID.  Wijzigen van de volume-ID is geen ondersteunde bewerking.

* Oorzaak   
De ID van het bestandssysteem wordt ingesteld wanneer het volume wordt gemaakt. Het volume-ID kan niet later worden gewijzigd.

* Oplossing   
Geen.

* Tijdelijke oplossing   
Geen.  Het volume-ID wordt gegenereerd wanneer het volume wordt gemaakt en vervolgens kan niet worden gewijzigd.


<a name="error_08"></a>***Een ongeldige waarde '{0}' is ontvangen voor {1}.***   
Dit bericht geeft aan dat een fout in de velden voor RuleIndex, AllowedClients, UnixReadOnly, UnixReadWrite, Nfsv3 en Nfsv4.

* Oorzaak   
De validatie voor invoer-aanvraag is mislukt voor ten minste één van de volgende velden: RuleIndex, AllowedClients, UnixReadOnly, UnixReadWrite, Nfsv3 en Nfsv4.

* Oplossing   
Zorg ervoor dat alle vereiste en niet-conflicterende parameters ingesteld op de opdrachtregel. U kunt zowel de UnixReadOnly UnixReadWrite parameters en bijvoorbeeld niet instellen op hetzelfde moment.

* Tijdelijke oplossing   
Zie de sectie oplossing.  


<a name="error_09"></a>***Ontbrekende waarde voor '{0}'.***   
Deze fout geeft aan dat een vereist kenmerk in de aanvraag voor ten minste één van de volgende parameters ontbreekt: RuleIndex, AllowedClients, UnixReadOnly, UnixReadWrite, Nfsv3 en Nfsv4.

* Oorzaak   
De validatie voor invoer-aanvraag is mislukt voor ten minste één van de volgende velden: RuleIndex, AllowedClients, UnixReadOnly, UnixReadWrite, Nfsv3 en Nfsv4.

* Oplossing   
Zorg ervoor dat alle vereiste en niet-conflicterende parameters ingesteld op de opdrachtregel. Bijvoorbeeld, instellen u zowel de UnixReadOnly en UnixReadWrite parameters niet op hetzelfde moment

* Tijdelijke oplossing   
Zie de sectie oplossing.  


<a name="error_10"></a> ***{0} al in gebruik.***   
Deze fout geeft aan dat de naam voor de resource is al gebruikt.

* Oorzaak   
U probeert te maken van een volume met een naam die gelijk is aan een bestaand volume.

* Oplossing   
Gebruik een unieke naam bij het maken van een volume.

* Tijdelijke oplossing   
Indien nodig, kunt u de naam van het bestaande volume wijzigen zodat het nieuwe volume de naam van de beoogde kunt gebruiken.


<a name="error_11"></a> ***{0} te kort.***   
Deze fout geeft aan dat de naam van het volume niet voldoen aan de vereiste minimale lengte.

* Oorzaak   
Naam van het volume is te kort.

* Oplossing   
De lengte van de naam van het volume vergroten.  

* Tijdelijke oplossing   
U kunt een algemene voorvoegsel of achtervoegsel toevoegen aan de naam van het volume.


<a name="error_12"></a>***NetApp-bestanden-API van Azure niet bereikbaar.***   
De API van Azure, is afhankelijk van de API van Azure-NetApp-bestanden voor het beheren van volumes.  Deze fout wijst op een probleem met de API-verbinding.

* Oorzaak   
De onderliggende API reageert niet, wat resulteert in een interne fout. Deze fout waarschijnlijk tijdelijk.

* Oplossing   
Het probleem waarschijnlijk tijdelijk.  De aanvraag moet slagen na enige tijd.

* Tijdelijke oplossing   
Geen. De onderliggende API is essentieel voor het beheren van volumes.  


<a name="error_13"></a>***Er zijn geen referenties gevonden voor het abonnement '{0}'.***   
Deze fout geeft aan dat de opgegeven referenties ongeldig zijn of niet juist zijn ingesteld in het abonnement.

* Oorzaak   
De referenties die ongeldig of onjuist ingestelde zijn te voorkomen dat toegang tot de service voor het beheren van volumes.

* Oplossing   
Zorg ervoor dat de referenties zijn ingesteld en correct worden ingevoerd op de opdrachtregel.

* Tijdelijke oplossing   
Geen.  Instellen van referenties correct is essentieel voor het gebruik van Azure NetApp bestanden.  


<a name="error_14"></a>***Er is geen bewerkingsresultaat-id gevonden voor '{0}'.***   
Deze fout geeft aan dat een interne fout voorkomt de bewerking niet voltooien dat.

* Oorzaak   
Een interne fout is opgetreden en voorkomt dat de bewerking niet voltooien.

* Oplossing   
Deze fout waarschijnlijk tijdelijk.  Wacht een paar minuten en probeer het opnieuw. Als het probleem zich blijft voordoen, maakt u een ticket voor de technische ondersteuning het probleem onderzoeken.

* Tijdelijke oplossing   
Wacht een paar minuten en controleer of het probleem zich blijft voordoen.


<a name="error_15"></a>***Bewerking '{0}' niet ondersteund.***   
Deze fout geeft aan dat de opdracht niet beschikbaar voor het actieve abonnement of resourcegroep is.

* Oorzaak   
De bewerking is niet beschikbaar voor het abonnement of resourcegroep.

* Oplossing   
Zorg ervoor dat de opdracht correct worden ingevoerd en beschikbaar is voor de bron en het abonnement dat u gebruikt.

* Tijdelijke oplossing   
Zie de sectie oplossing.  


<a name="error_16"></a>***Patch-bewerking wordt niet ondersteund voor dit brontype.***   
Deze fout treedt op wanneer u probeert te wijzigen van de koppeldoel of momentopname.

* Oorzaak   
Het koppeldoel wordt gedefinieerd wanneer deze is gemaakt en deze later niet meer wijzigen.

* Oplossing   
Geen.  Het koppeldoel kan niet worden gewijzigd nadat het volume is gemaakt.

* Tijdelijke oplossing   
Geen.


<a name="error_17"></a>***Ontvangen van een waarde voor alleen-lezen eigenschap '{0}'.***   
Deze fout treedt op wanneer u definieert een waarde voor een eigenschap die niet kan worden gewijzigd. Bijvoorbeeld, u kunt de id van het volume niet wijzigen

* Oorzaak   
U hebt geprobeerd om te wijzigen van een parameter (zoals de volume-ID) die kan niet worden gewijzigd.

* Oplossing   
Geen. De parameter voor de volume-ID kan niet worden gewijzigd.

* Tijdelijke oplossing   
Het volume-ID mag niet moeten worden aangepast.  Een tijdelijke oplossing is daarom niet nodig.

<a name="error_18"></a>***De aangevraagde {0} is niet gevonden.***   
Deze fout treedt op wanneer u probeert te verwijzen naar een niet-bestaande resource, bijvoorbeeld: een volume of de momentopname. De resource is mogelijk verwijderd of de naam van een gespeld resource hebben.

* Oorzaak   
U probeert om te verwijzen naar een niet-bestaande resource (bijvoorbeeld, een volume of een momentopname) die al is verwijderd of heeft een onjuist gespeld resourcenaam.

* Oplossing   
Controleer de aanvraag voor spelfouten om ervoor te zorgen dat deze correct wordt verwezen.

* Tijdelijke oplossing   
Zie de sectie oplossing.

<a name="error_19"></a>***Kan geen referenties ophalen voor abonnement '{0}'.***   
Deze fout geeft aan dat de opgegeven referenties ongeldig zijn of niet correct ingesteld in het abonnement.

* Oorzaak   
De referenties die ongeldig zijn of niet correct ingesteld in het abonnement te voorkomen dat toegang tot de service voor het beheren van volumes.

* Oplossing   
Zorg ervoor dat de referenties zijn ingesteld en correct worden ingevoerd op de opdrachtregel.

* Tijdelijke oplossing   
Geen.  Correct zijn set referenties essentieel voor het gebruik van Azure NetApp bestanden.

<a name="error_20"></a>***Onbekende Azure NetApp bestanden fout.***   
De API van Azure, is afhankelijk van de API van Azure-NetApp-bestanden voor het beheren van volumes. De fout geeft aan dat een probleem in de communicatie met de API.

* Oorzaak   
Het verzenden van de onderliggende API is een onbekende fout.  Deze fout waarschijnlijk tijdelijk.

* Oplossing   
Het probleem waarschijnlijk tijdelijk is en de aanvraag moet slagen na enige tijd. Als het probleem zich blijft voordoen, maakt u een ondersteuningsticket om het probleem onderzocht.

* Tijdelijke oplossing   
Geen.  De onderliggende API is essentieel voor het beheren van volumes.

<a name="error_21"></a>***Waarde die is ontvangen voor een onbekende eigenschap '{0}'.***   
Deze fout treedt op wanneer deze wordt niet-bestaande eigenschappen zijn opgegeven voor een resource, zoals het volume, de momentopname of het koppeldoel.

* Oorzaak   
De aanvraag bevat een set eigenschappen die kunnen worden gebruikt met elke resource.  U kunt geen niet-bestaande eigenschappen niet opnemen in de aanvraag.

* Oplossing   
Zorg ervoor dat alle eigenschapnamen goed gespeld zijn en de eigenschappen beschikbaar voor het abonnement en de resource zijn.

* Tijdelijke oplossing   
Verminder het aantal eigenschappen die zijn gedefinieerd in de aanvraag voor het elimineren van de eigenschap die de fout wordt veroorzaakt.


<a name="error_22"></a>***Update-bewerking wordt niet ondersteund voor dit brontype.***   
Alleen volumes kunnen worden bijgewerkt. Deze fout treedt op wanneer u probeert een niet-ondersteunde updatebewerking uit te voeren, bijvoorbeeld een momentopname wordt bijgewerkt.

* Oorzaak   
De resource die u probeert bij te werken biedt geen ondersteuning voor de updatebewerking.  Alleen volumes kunnen hebben hun eigenschappen gewijzigd.

* Oplossing   
Geen.  De resource die u probeert bij te werken biedt geen ondersteuning voor de updatebewerking. Daarom meer deze niet wijzigen.

* Tijdelijke oplossing   
Maak een nieuwe resource met de update in plaats voor een volume en de gegevens migreren.


<a name="error_23"></a>***Aantal items: {0} voor object: {1} buiten min-max bereik is.***   
Deze fout treedt op wanneer de regels van het exporteren niet voldoen aan de vereiste minimum of maximum bereik.  Als u het beleid exporteren definieert, moet een beleidsregel aan de minimale en vijf export-beleidsregels op de maximale hebben.

* Oorzaak   
De export-beleid dat u hebt gedefinieerd voldoet niet aan het vereiste bereik.  

* Oplossing   
Zorg ervoor dat de index niet al wordt gebruikt en die zich in het bereik van 1 tot 5.

* Tijdelijke oplossing   
Het is niet verplicht om te exporteren-beleid gebruiken op de volumes. Daarom kunt u het beleid exporteren weglaten volledig als u niet wilt exporteren-beleidsregels hebt.


<a name="error_24"></a>***Fout van de waarde voor object met dubbele {0}.***   
Deze fout treedt op wanneer het beleid exporteren is niet gedefinieerd met een unieke index.  Wanneer u beleid exporteren definieert, moeten alle beleidsregels voor uitvoer een unieke index tussen 1 en 5 hebben.

* Oorzaak   
Het beleid gedefinieerde export voldoet niet aan de vereisten voor de regels van uitvoer. U moet een beleidsregel ten minste en vijf export-beleidsregels op de maximale hebben.  

* Oplossing   
Zorg ervoor dat de index niet al wordt gebruikt en dat deze zich op het bereik van 1 tot 5.

* Tijdelijke oplossing   
Gebruik een andere index voor de regel die u probeert in te stellen.


