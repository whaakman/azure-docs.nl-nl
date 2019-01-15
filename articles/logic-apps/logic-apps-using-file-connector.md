---
title: Verbinding maken met on-premises - Azure Logic Apps-bestandssystemen | Microsoft Docs
description: Automatiseren van taken en werkstromen die verbinding met on-premises bestandssystemen met de File System-connector via de on-premises gegevensgateway in Azure Logic Apps maken
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan, LADocs
ms.topic: article
ms.date: 01/13/2019
ms.openlocfilehash: b58059727a383e978691bfbbee77a1f6b04692ce
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54264323"
---
# <a name="connect-to-on-premises-file-systems-with-azure-logic-apps"></a>Verbinding maken met on-premises bestandssystemen met Azure Logic Apps

Met de File System-connector en Azure Logic Apps, kunt u geautomatiseerde taken en werkstromen die maken en beheren van bestanden op een on-premises-bestand delen, bijvoorbeeld:  

- Maken, ophalen, toevoegen, bijwerken en verwijderen van bestanden.
- Lijst van bestanden in mappen of hoofdmappen.
- Inhoud van bestanden en metagegevens ophalen.

Dit artikel wordt beschreven hoe u kunt verbinding maken met een on-premises bestandssysteem zoals is beschreven in dit voorbeeldscenario: een bestand dat geüpload naar Dropbox naar een bestandsshare kopiëren en vervolgens een e-mail verzenden. Veilig verbinding maken en toegang tot on-premises systemen, logic apps gebruikt de [on-premises gegevensgateway](../logic-apps/logic-apps-gateway-connection.md). Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Vereisten

Als u wilt volgen in het voorbeeld, moet u deze items:

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* Voordat u logische apps met on-premises systemen, zoals uw bestand system-server verbinden kunt, moet u [installeren en instellen van een on-premises gegevensgateway](../logic-apps/logic-apps-gateway-install.md). Op die manier kunt u opgeven voor het gebruik van de gatewayinstallatie, wanneer u de bestand system-verbinding van uw logische app maakt.

* Een [Drobox account](https://www.dropbox.com/) en referenties van uw account. Uw DropBox-referenties zijn nodig voor het maken van een verbinding tussen uw logische app en uw account Drobox. 

* De referenties van uw account voor de computer die het bestandssysteem dat u wilt openen. Bijvoorbeeld, als u de data gateway op dezelfde computer als uw bestandssysteem installeert, moet u referenties van het account voor die computer. 

* Een e-mailaccount van een provider die wordt ondersteund door Logic Apps, zoals Office 365 Outlook, Outlook.com of Gmail. Voor andere providers [kunt u hier de lijst met connectors bekijken](https://docs.microsoft.com/connectors/). Deze logische app maakt gebruik van een Office 365 Outlook-account. Als u een ander e-mailaccount gebruikt, zijn de algemene stappen hetzelfde, maar ziet de gebruikersinterface er misschien iets anders uit. 

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md). In dit voorbeeld moet u een lege, logische app.

## <a name="add-trigger"></a>Trigger toevoegen

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Typ 'dropbox' als filter in het zoekvak. Selecteer deze trigger uit de lijst met triggers: **Wanneer een bestand wordt gemaakt** 

   ![Dropbox-trigger selecteren](media/logic-apps-using-file-connector/select-dropbox-trigger.png)

1. Meld u aan met uw referenties voor Dropbox-account en verleent toegang tot uw gegevens Dropbox voor Azure Logic Apps. 

1. Geef de vereiste gegevens voor de trigger.

   ![Dropbox-trigger](media/logic-apps-using-file-connector/dropbox-trigger.png)

## <a name="add-actions"></a>Acties toevoegen

1. Kies onder de trigger **volgende stap**. Typ 'file system' als filter in het zoekvak. Selecteer deze actie uit de lijst met acties: **Maken van het bestand - bestandssysteem**

   ![Bestandssysteem connector vinden](media/logic-apps-using-file-connector/find-file-system-action.png)

1. Als u nog een verbinding naar uw bestandssysteem hebt, wordt u gevraagd om een verbinding te maken.

   ![Verbinding maken](media/logic-apps-using-file-connector/file-system-connection.png)

   | Eigenschap | Vereist | Waarde | Description | 
   | -------- | -------- | ----- | ----------- | 
   | **Verbindingsnaam** | Ja | <*naam van de verbinding*> | De naam die u wilt gebruiken voor de verbinding | 
   | **Hoofdmap** | Ja | <*hoofdmap mapnaam*> | De hoofdmap van het bestandssysteem, bijvoorbeeld, als u uw on-premises gegevensgateway, zoals een lokale map geïnstalleerd op de computer waarop de on-premises gegevensgateway is geïnstalleerd, of de map voor een netwerkshare die toegang hebben tot de computer. <p>Bijvoorbeeld: `\\PublicShare\\DropboxFiles` <p>De hoofdmap is de belangrijkste bovenliggende map, dat wordt gebruikt om relatieve paden voor alle bestand gerelateerde acties. | 
   | **Verificatietype** | Nee | <*auth-type*> | Het type verificatie dat uw bestandssysteem, bijvoorbeeld gebruikt **Windows** | 
   | **Gebruikersnaam** | Ja | <*domein*>\\<*gebruikersnaam*> | De gebruikersnaam voor de computer waar u uw bestandssysteem hebt | 
   | **Wachtwoord** | Ja | <*uw wachtwoord*> | Het wachtwoord voor de computer waar u uw bestandssysteem hebt | 
   | **gateway** | Ja | <*installed-gateway-name*> | De naam voor uw eerder geïnstalleerde gateway | 
   ||| 

1. Wanneer u klaar bent, kiest u **Maken**. 

   Logic Apps configureert en test de verbinding, ervoor te zorgen dat de verbinding naar behoren werkt. 
   Als de verbinding correct is ingesteld, wordt er opties weergegeven voor de actie die u eerder hebt geselecteerd. 

1. In de **bestand maken** actie, geef de details op voor het kopiëren van bestanden van Dropbox naar de hoofdmap van uw on-premises bestandsshare. Uitvoer toevoegen uit de vorige stappen, klikt u in de vakken en beschikbare velden selecteren wanneer u de lijst met dynamische inhoud wordt weergegeven.

   ![Actie voor bestanden maken](media/logic-apps-using-file-connector/create-file-filled.png)

1. Nu een Outlook-actie waarmee een e-mailbericht wordt verzonden, zodat de juiste gebruikers over het nieuwe bestand weten toevoegen. Voer de ontvangers, de titel en de hoofdtekst van het e-mailbericht. Voor het testen, kunt u uw eigen e-mailadres.

   ![Actie e-mail verzenden](media/logic-apps-using-file-connector/send-email.png)

1. Sla uw logische app op. Test uw app door een bestand uploaden naar Dropbox. 

   Uw logische app moet het bestand kopiëren naar uw on-premises-bestandsshare en de ontvangers een e-mailbericht verzenden over het gekopieerde bestand.

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over triggers en acties limieten die worden beschreven van de connector openapi (voorheen Swagger) beschrijving van de connector controleren [-verwijzingspagina](/connectors/fileconnector/).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.

* Ter verbetering van Azure Logic Apps en connectors kunt stemmen op ideeën of ideeën indienen op de [Azure Logic Apps User Voice-site](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [verbinding maken met on-premises gegevens](../logic-apps/logic-apps-gateway-connection.md) 
* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
