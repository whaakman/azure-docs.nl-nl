---
title: De instellingen van het hulpprogramma voor Content Moderator-controle configureren | Microsoft Docs
description: Configureren of vraag uw team, labels, connectors, werkstromen en referenties.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/25/2017
ms.author: sajagtap
ms.openlocfilehash: 2ffed5e561cf9988ec10ecb7e10318d148281057
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2018
ms.locfileid: "51851534"
---
# <a name="review-tool-settings"></a>Controleer de instellingen van het hulpprogramma #

Met behulp van het tabblad instellingen op het dashboard van de hulpprogramma voor beoordeling, is het gemakkelijk om te definiëren en te wijzigen van veel onderdelen.

![Content Moderator controle-instellingen](images/settings-1.png)

## <a name="team-and-subteams"></a>Team en Subteams ## 

Beheer uw team en subteams op dit tabblad. U kunt slechts één team hebben, maar u kunt [maken van meerdere subteams](subteams.md) en uitnodigingen verzenden naar toekomstige leden. Nadat u uitnodigingen zijn verzonden, kunt u ze te controleren, wijzigen van machtigingen voor teamleden en extra gebruikers uitnodigen. Nadat de teamleden hebben uw uitnodiging geaccepteerd, kunt u die leden toewijzen aan verschillende subteams. U kunt functies van de teamleden beheerders of revisoren instellen: beheerders kunnen andere gebruikers uitnodigen terwijl revisoren kunnen niet.

![Content Moderator Team-instellingen](images/settings-2-team.png)

## <a name="tags"></a>Tags ##

Dit is waar u kunt [definiëren van aangepaste labels](tags.md) door in te voeren van de korte code, naam en beschrijving voor de labels. Nadat u deze hebt gemaakt, is beschikbaar tijdens de beoordelingen. U kunt verschillende codes gebruiken voor verschillende beoordelingen door de zichtbaarheid uit en weer in te schakelen.

![Content Moderator Tags instellingen](images/settings-3-tags.png)

## <a name="connectors"></a>Connectors ##

Werkstromen toevoegen functionaliteit met behulp van connectors om te communiceren met het hulpprogramma voor beoordeling. Het beoordelingsprogramma roept de inhoud Moderator-API's met de standaardwerkstroom voor toezicht houden op inhoud. Wanneer u zich aanmeldt voor het hulpprogramma voor beoordeling, het automatisch-bepalingen de Moderator-API-referenties voor u. Het ondersteunt ook integratie van andere connector-API's, zolang een connector beschikbaar is. We hebben een paar connectors beschikbaar gesteld buiten het vak.

De [tabblad Connectors is](connectors.md) is waar u connectors beheren. U kunt toevoegen of verwijderen van connectors en uw abonnementssleutel voor een specifieke connector vinden. Klik op verbinden om toe te voegen aan uw aangepaste werkstromen. 

![Content Moderator Connectors instellingen](images/settings-4-connectors.png)

## <a name="workflows"></a>Werkstromen ##

Werkstromen beheren op het tabblad werkstromen. U kunt werkstromen testen door een voorbeeldbestand te uploaden. U kunt ook [definiëren van aangepaste werkstromen](workflows.md) voor afbeeldingen en tekst met behulp van de beschikbare API-connectors (gevonden op het tabblad Connectors is). 

![Content Moderator Werkstroominstellingen](images/settings-5-workflows.png)

## <a name="credentials"></a>Referenties ##

Op dit tabblad biedt snel toegang tot uw abonnementssleutel Content Moderator, die u moet de API's inbegrepen met Content Moderator (beheer van afbeeldingen, beheer van tekst, beheer, werkstroom en revisie API's) gebruiken.
 
![Content Moderator-referenties](images/settings-6-credentials.png)
