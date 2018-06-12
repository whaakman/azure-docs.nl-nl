---
title: XML transformeren met maps XSLT - Azure Logic Apps | Microsoft Docs
description: XSLT toegewezen voor het transformeren van XML-gegevens met Azure Logic Apps en de Enterprise Integration Pack toevoegen
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: jeconnoc
editor: cgronlun
ms.assetid: 90f5cfc4-46b2-4ef7-8ac4-486bb0e3f289
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: e0a8e5dfd42d447b4e049574b346c41fe0067c9f
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35299134"
---
# <a name="add-maps-for-xml-data-transform"></a>Toewijzingen voor transformatie van XML-gegevens toevoegen

Enterprise-integratie gebruikt kaarten voor het transformeren van XML-gegevens tussen indelingen. Een kaart is een XML-document waarin de gegevens in een document dat moet worden omgezet in een andere indeling. 

## <a name="why-use-maps"></a>Waarom maps gebruiken?

Stel dat u regelmatig ontvangt B2B orders of facturen van een klant die de indeling YYYMMDD voor datums gebruikt. Echter in uw organisatie slaat u datums in de indeling MMDDYYY. U kunt een kaart om te gebruiken *transformeren* de datumnotatie YYYMMDD in de MMDDYYY voordat de details van de order of factuur wordt opgeslagen in de database van de activiteit klant.


## <a name="how-do-i-create-a-map"></a>Hoe maak ik een kaart

Kunt u de projecten BizTalk-integratie met de [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "meer informatie over het enterprise-integratiepakket") voor Visual Studio 2015. Vervolgens kunt u een Integratiekaart-bestand waarmee u artikelen tussen twee XML-schemabestanden visueel toewijzen. Nadat u dit project bouwen, hebt u een XSLT-document.

Als de kaart een verwijzing naar een externe assembly bevat, moeten vervolgens beide worden geüpload naar de account van de integratie. Moeten ze worden geüpload in een bepaalde volgorde eerst de assembly en klik vervolgens op de kaart die verwijst naar de assembly.


## <a name="how-do-i-add-a-map"></a>Hoe kan ik een kaart toevoegen

1. Selecteer in de Azure-portal **Bladeren**.

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. Voer in het zoekvak filter **integratie**, selecteer daarna **Integratieaccounts** uit de lijst met resultaten.

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. Selecteer het integratie account waar u toe te wijzen.

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Selecteer de **Maps** tegel.

    ![](./media/logic-apps-enterprise-integration-maps/map-1.png)

5. Nadat de toewijzingen pagina wordt geopend, kiest u **toevoegen**.

    ![](./media/logic-apps-enterprise-integration-maps/map-2.png)  

6. Voer een **naam** voor uw kaart. Kies het pictogram van de map aan de rechterkant van de kaart om bestand te uploaden, de **kaart** in het tekstvak. Nadat het uploadproces is voltooid, kiest u **OK**.

    ![](./media/logic-apps-enterprise-integration-maps/map-3.png)

7. Nadat de kaart Azure aan uw account integratie toevoegt, krijgen het bericht op het scherm waarin u of uw toewijzingsbestand of niet is toegevoegd. Nadat u dit bericht ontvangt, kiest u de **Maps** tegel zodat u de zojuist toegevoegde kaart kunt bekijken.

    ![](./media/logic-apps-enterprise-integration-maps/map-4.png)


## <a name="how-do-i-add-an-assembly"></a>Hoe kan ik een assembly toevoegen
Open de integratie-account waarin u wilt uploaden van de assembly.

1. Kies de **assembly's** tegel.

    ![integrationaccount-assembly-tegel](./media/logic-apps-enterprise-integration-maps/assemblytile.png)

2. Nadat de assembly's pagina wordt geopend, kiest u **toevoegen**. Voer een **naam** voor uw assembly. Als u wilt het assembly-bestand uploaden, kies het pictogram van de map aan de rechterkant van de **Assembly** in het tekstvak. Nadat het uploadproces is voltooid, kiest u **OK**.

    ![toevoegen van de assembly](./media/logic-apps-enterprise-integration-maps/assemblyfile.png)


## <a name="how-do-i-edit-a-map"></a>Hoe kan ik een kaart bewerken?

U moet een nieuwe kaart-bestand met de wijzigingen die u wilt uploaden. U kunt de kaart voor het bewerken van eerst downloaden.

Volg deze stappen voor het uploaden van een nieuwe kaart die wordt vervangen door de bestaande toewijzing.

1. Kies de **Maps** tegel.

2. Nadat de pagina Maps wordt geopend, selecteert u de toewijzing die u wilt bewerken.

3. Op de **Maps** pagina **Update**.

    ![](./media/logic-apps-enterprise-integration-maps/edit-1.png)

4. Selecteer het toewijzingsbestand die u wilt uploaden en selecteer vervolgens in de bestandskiezer **Open**.

    ![](./media/logic-apps-enterprise-integration-maps/edit-2.png)

## <a name="how-to-delete-a-map"></a>Het verwijderen van een kaart?

1. Kies de **Maps** tegel.

2. Nadat de pagina Maps wordt geopend, selecteert u de kaart die u wilt verwijderen.

3. Kies **verwijderen**.

    ![](./media/logic-apps-enterprise-integration-maps/delete.png)

4. Bevestig dat u wilt verwijderen van de kaart.

    ![](./media/logic-apps-enterprise-integration-maps/delete-confirmation-1.png)

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over het Enterprise-integratiepakket](logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack")  
* [Meer informatie over de overeenkomsten](../logic-apps/logic-apps-enterprise-integration-agreements.md "meer informatie over enterprise integration-overeenkomsten")  
* [Meer informatie over transformaties](logic-apps-enterprise-integration-transform.md "meer informatie over enterprise integration-transformaties")  

