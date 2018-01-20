---
title: Verbinding maken met on-premises - Azure Logic Apps-bestandssystemen | Microsoft Docs
description: Verbinding maken met on-premises bestandssystemen vanuit logic app werkstromen via de lokale gegevensgateway en File System-connector
keywords: bestandssystemen on-premises
services: logic-apps
author: derek1ee
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2017
ms.author: LADocs; deli
ms.openlocfilehash: 32ab5be41a8dee3b1f2c0b1bde076c0d1a844bdd
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="connect-to-on-premises-file-systems-from-logic-apps-with-the-file-system-connector"></a>Verbinding maken met on-premises bestandssystemen vanuit logic apps met de File System-connector

Uw logische apps kunt voor het beheren van gegevens en veilig toegang krijgen tot lokale bronnen, de lokale data gateway. Dit artikel laat zien hoe u verbinding kunt maken met een bestandssysteem on-premises via dit eenvoudige voorbeeldscenario: Kopieer een bestand dat naar Dropbox geüpload naar een bestandsshare en vervolgens een e-mailbericht verzenden.

## <a name="prerequisites"></a>Vereisten

* Download de meest recente [lokale gegevensgateway](https://www.microsoft.com/download/details.aspx?id=53127).

* Installeren en instellen van de meest recente lokale gegevensgateway, versie 1.15.6150.1 of hoger. Zie voor de stappen [verbinding maken met gegevensbronnen on-premises](http://aka.ms/logicapps-gateway). Voordat u met deze stappen kunt doorgaan, moet u de gateway installeren op een on-premises machine.

* Elementaire kennis over [logic apps maken](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="add-trigger-and-actions-for-connecting-to-your-file-system"></a>Trigger en acties voor het verbinden met het bestandssysteem toevoegen

1. Een lege, logische app maken. Deze trigger toevoegen als de eerste stap: **Dropbox - als een bestand is gemaakt** 

2. Kies onder de trigger **+ doornemen** > **een actie toevoegen**. 

3. Voer in het zoekvak 'file system' als filter. Als u de acties voor de connector bestandssysteem ziet, kiest u de **File System - bestand maken** in te grijpen. 

   ![Zoeken naar bestand connector](media/logic-apps-using-file-connector/search-file-connector.png)

4. Als u nog een verbinding naar uw bestandssysteem hebt, wordt u gevraagd om een verbinding te maken. 

5. Selecteer **verbinden via lokale gegevensgateway**. Wanneer de verbindingseigenschappen worden weergegeven, instellen van uw verbinding zoals opgegeven in de tabel.

   ![Configureer de verbinding](media/logic-apps-using-file-connector/create-file.png)

   | Instelling | Beschrijving |
   | ------- | ----------- |
   | **Hoofdmap** | Geef de hoofdmap voor het bestandssysteem. U kunt een lokale map op de computer waar de lokale data gateway is geïnstalleerd, of de map kan zich een netwerkshare die de computer toegang heeft tot opgeven. <p>**Tip:** de hoofdmap is de belangrijkste bovenliggende map, die wordt gebruikt voor het relatieve paden voor alle bestand-gerelateerde acties. | 
   | **Verificatietype** | Het type verificatie dat wordt gebruikt door het bestandssysteem | 
   | **Gebruikersnaam** | Geef uw gebruikersnaam {*domein*\\*gebruikersnaam*} voor uw eerder geïnstalleerde gateway. | 
   | **Wachtwoord** | Uw wachtwoord opgeven voor uw eerder geïnstalleerde gateway. | 
   | **Gateway** | Selecteer uw eerder geïnstalleerde gateway. | 
   ||| 

6. Nadat u de verbindingsgegevens opgeeft, kiest u **maken**. 

   Logic Apps configureert en test uw verbinding, om ervoor te zorgen dat de verbinding goed werkt. 
   Als de verbinding correct is ingesteld, wordt er opties weergegeven voor de actie die u eerder hebt geselecteerd. 
   De file system-connector is nu klaar voor gebruik.

7. Instellen van de **Create file** actie voor het kopiëren van bestanden in Dropbox naar de hoofdmap voor uw on-premises-bestandsshare.

   ![Bestandsactie maken](media/logic-apps-using-file-connector/create-file-filled.png)

8. Na deze actie voor het kopiëren van het bestand, moet u een Outlook-actie die een e-mailbericht verzendt, zodat gebruikers over het nieuwe bestand weten toevoegen. Geef de geadresseerden, de titel en de hoofdtekst van het e-mailbericht. 

   In de **dynamische inhoud** lijst, kunt u gegevens uitvoer van de bestand-connector zodat u meer informatie aan het e-mailbericht toevoegen kunt.

   ![Verzenden van e-actie](media/logic-apps-using-file-connector/send-email.png)

9. Sla uw logische app. Uw app testen door een bestand uploadt naar Dropbox. Het bestand ophalen gekopieerd naar de bestandsshare voor lokale en u ontvangt een e-mail over de werking.

Gefeliciteerd, u hebt nu een werkende logische app die verbinding met uw on-premises bestandssysteem maken kan. 

Probeer een andere functies die de connector bijvoorbeeld biedt verkennen:

- Bestand maken
- Bestanden in de map weergeven
- Toevoegen aan het bestand
- Bestand verwijderen
- Bestandsinhoud ophalen
- Bestandsinhoud ophalen via het pad
- De metagegevens van het bestand ophalen
- De metagegevens van het bestand ophalen via het pad
- Bestanden in de hoofdmap weergeven
- Bestand bijwerken

## <a name="view-the-swagger"></a>De swagger weergeven

Zie de [swagger details](/connectors/fileconnector/). 

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.

* Ter verbetering van de Azure Logic Apps en connectors, stem op of dien ideeën op de [Azure Logic Apps User Voice site](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* [Verbinding maken met on-premises gegevens](../logic-apps/logic-apps-gateway-connection.md) 
* [Uw logische apps bewaken](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Enterprise-integratie voor B2B-scenario 's](../logic-apps/logic-apps-enterprise-integration-overview.md)
