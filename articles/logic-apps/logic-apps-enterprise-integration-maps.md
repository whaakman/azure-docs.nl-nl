---
title: XML transformeren met XSLT-toewijzingen - Azure Logic Apps | Microsoft Docs
description: XSLT-toewijzingen die transformeer XML in Azure Logic Apps met Enterprise Integration Pack toevoegen
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 90f5cfc4-46b2-4ef7-8ac4-486bb0e3f289
ms.date: 07/08/2016
ms.openlocfilehash: c5e5e0a0a3f8bd5feedc00d5bbfb76a1453ccc84
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43123553"
---
# <a name="add-maps-for-xml-transformation-in-azure-logic-apps-with-enterprise-integration-pack"></a>Toewijzingen voor XML-transformatie toevoegen in Azure Logic Apps met Enterprise Integration Pack

Bedrijfsintegratie gebruikt-kaarten om XML-gegevens tussen indelingen te transformeren. Een kaart is een XML-document waarin de gegevens in een document dat moet worden omgezet in een andere indeling. 

## <a name="why-use-maps"></a>Waarom kaarten gebruiken?

Stel dat u regelmatig B2B orders of ontvangt facturen van een klant die gebruikmaakt van de indeling YYYMMDD voor datums. Echter, in uw organisatie slaat u datums in de indeling MMDDYYY. U kunt een kaart om te gebruiken *transformeren* de datumnotatie YYYMMDD in de MMDDYYY voordat u de volgorde of per factuur gegevens opslaat in uw klantendatabase van de activiteit.


## <a name="how-do-i-create-a-map"></a>Hoe kan ik een kaart maken?

Kunt u BizTalk Integration-projecten met de [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "meer informatie over het enterprise integration pack") voor Visual Studio 2015. Vervolgens kunt u een integratie van toewijzingsbestand waarmee u visueel items tussen twee bestanden van de XML-schema worden toegewezen. Nadat u dit project bouwen, hebt u een XSLT-document.

Als de kaart een verwijzing naar een externe assembly bevat, moeten klikt u vervolgens zowel worden geüpload naar het integratieaccount. Ze moeten worden geüpload in een specifieke volgorde eerst de assembly en klik vervolgens op de kaart die verwijst naar de assembly.


## <a name="how-do-i-add-a-map"></a>Hoe kan ik een kaart toevoegen

1. Selecteer in de Azure portal, **Bladeren**.

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. Voer in het zoekvak filter **integratie**en selecteer vervolgens **Integratieaccounts** in de lijst met resultaten.

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. Selecteer het integratieaccount waaraan u wilt toevoegen van de kaart.

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Selecteer de **Maps** tegel.

    ![](./media/logic-apps-enterprise-integration-maps/map-1.png)

5. Nadat de kaarten pagina wordt geopend, kiest u **toevoegen**.

    ![](./media/logic-apps-enterprise-integration-maps/map-2.png)  

6. Voer een **naam** voor uw kaart. De kaart om bestand te uploaden, kies het pictogram van de map aan de rechterkant van de **kaart** in het tekstvak. Nadat de upload is voltooid, kiest u **OK**.

    ![](./media/logic-apps-enterprise-integration-maps/map-3.png)

7. Nadat de kaart Azure aan uw integratie-account toevoegt, krijgt u een op het scherm weergegeven waarin wordt weergegeven of uw toewijzingsbestand is toegevoegd of niet. Nadat u dit bericht ontvangt, kiest u de **Maps** tegel, zodat u de zojuist toegevoegde kaart kunt bekijken.

    ![](./media/logic-apps-enterprise-integration-maps/map-4.png)


## <a name="how-do-i-add-an-assembly"></a>Hoe kan ik een assembly toevoegen
Open het integratieaccount waar u het uploaden van de assembly.

1. Kies de **assembly's** tegel.

    ![integrationaccount-assembly-tegel](./media/logic-apps-enterprise-integration-maps/assemblytile.png)

2. Nadat de assembly's pagina wordt geopend, kiest u **toevoegen**. Voer een **naam** voor uw assembly. Als u wilt uploaden van het assembly-bestand, kiest u het mappictogram aan de rechterkant van de **Assembly** in het tekstvak. Nadat de upload is voltooid, kiest u **OK**.

    ![toevoegen-assembly](./media/logic-apps-enterprise-integration-maps/assemblyfile.png)


## <a name="how-do-i-edit-a-map"></a>Hoe kan ik een kaart bewerken?

U moet een nieuw bestand in de map met de wijzigingen die u wilt uploaden. U kunt de kaart voor het bewerken van het eerst downloaden.

Volg deze stappen voor het uploaden van een nieuwe kaart die wordt vervangen door de bestaande toewijzing.

1. Kies de **Maps** tegel.

2. Nadat de Maps-pagina wordt geopend, selecteert u de toewijzing die u wilt bewerken.

3. Op de **Maps** pagina, kies **Update**.

    ![](./media/logic-apps-enterprise-integration-maps/edit-1.png)

4. Selecteer de kaart-bestand dat u wilt uploaden, en selecteer vervolgens in de bestandenkiezer **Open**.

    ![](./media/logic-apps-enterprise-integration-maps/edit-2.png)

## <a name="how-to-delete-a-map"></a>Hoe kan ik een toewijzing verwijderen?

1. Kies de **Maps** tegel.

2. Nadat de Maps-pagina wordt geopend, selecteert u de kaart die u wilt verwijderen.

3. Kies **verwijderen**.

    ![](./media/logic-apps-enterprise-integration-maps/delete.png)

4. Bevestig dat u wilt verwijderen van de kaart.

    ![](./media/logic-apps-enterprise-integration-maps/delete-confirmation-1.png)

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over het Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack")  
* [Meer informatie over de overeenkomsten](../logic-apps/logic-apps-enterprise-integration-agreements.md "meer informatie over enterprise integration-overeenkomsten")  
* [Meer informatie over transformaties](logic-apps-enterprise-integration-transform.md "meer informatie over enterprise integration-transformaties")  

