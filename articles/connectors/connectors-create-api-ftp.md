---
title: Verbinding maken met de FTP-server - Azure Logic Apps | Microsoft Docs
description: Maken, bewaken en beheren van bestanden op een FTP-server met Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 10/15/2018
tags: connectors
ms.openlocfilehash: a14f045193c01b8c26019314ddde4c2116d8bad6
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50232814"
---
# <a name="create-monitor-and-manage-ftp-files-by-using-azure-logic-apps"></a>Maken, bewaken en beheren van FTP-bestanden met behulp van Azure Logic Apps

Met Azure Logic Apps en de FTP-connector, kunt u geautomatiseerde taken en werkstromen die maken, bewaken, verzenden en ontvangen van bestanden via uw account op een FTP-server, samen met andere acties, bijvoorbeeld:

* Bewaken wanneer bestanden worden toegevoegd of gewijzigd.
* Ophalen, maken, kopiëren, lijst, bijwerken en verwijderen van bestanden.
* Inhoud van bestanden en metagegevens ophalen.
* Pak archieven voor mappen.

U kunt triggers die te antwoorden krijgen van uw FTP-server en de uitvoer beschikbaar voor andere acties. U kunt acties in uw logische apps gebruiken voor het uitvoeren van taken met bestanden op uw FTP-server. U kunt ook andere acties waarmee de uitvoer van de FTP-acties hebben. Bijvoorbeeld, als u regelmatig bestanden uit uw FTP-server ophalen, kunt u e-mailberichten over deze bestanden en hun inhoud verzenden met behulp van de connector voor Office 365 Outlook of Outlook.com-connector. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

> [!NOTE]
> De FTP-connector ondersteunt alleen de bestanden die zijn dan 50 MB of kleiner, tenzij u [logische groepen te verdelen voor het verwerken van grote berichten](../logic-apps/logic-apps-handle-large-messages.md). 
>
> Ook de FTP-connector ondersteunt alleen expliciete FTP via SSL (FTPS) en is niet compatibel met impliciete FTPS. 

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* Uw FTP-host-server-adres en account-referenties

  De FTP-connector vereist dat uw FTP-server toegankelijk is vanaf het internet en ingesteld is om te werken in *passieve* modus. Uw referenties toestaan dat de logische app een verbinding maken en toegang tot uw FTP-account.

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app waar u toegang tot uw FTP-account. Om te beginnen met een FTP-trigger, [maken van een lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). Voor het gebruik van een FTP-actie beginnen uw logische app met een andere trigger, bijvoorbeeld, de **terugkeerpatroon** trigger.

## <a name="connect-to-ftp"></a>Verbinding maken met FTP

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Typ 'ftp' als filter voor lege, logische apps, in het zoekvak. Selecteer de gewenste trigger onder de lijst met triggers. 

   -of-

   Voor bestaande logische apps, onder de laatste stap waarin u wilt toevoegen van een actie kiezen **nieuwe stap**, en selecteer vervolgens **een actie toevoegen**. 
   Voer 'ftp' als filter in het zoekvak in. 
   Selecteer de actie die u wilt onder de lijst met acties.

   Als u wilt toevoegen een actie tussen fasen, de aanwijzer over de pijl tussen fasen. 
   Kies het plusteken (**+**) die wordt weergegeven, en selecteer vervolgens **een actie toevoegen**.

1. Geeft u de benodigde informatie voor uw verbinding en kies vervolgens **maken**.

1. Geef de benodigde informatie voor uw geselecteerde trigger of actie en doorgaan met het ontwikkelen van uw logische app-werkstroom.

## <a name="examples"></a>Voorbeelden

### <a name="ftp-trigger-when-a-file-is-added-or-modified"></a>FTP-trigger: wanneer een bestand wordt toegevoegd of gewijzigd

Deze trigger start een werkstroom voor logische Apps wanneer de trigger wordt gedetecteerd wanneer een bestand wordt toegevoegd of gewijzigd op een FTP-server. Dus bijvoorbeeld, dat kunt u een voorwaarde die controleert of de inhoud van het bestand en beslist of om op te halen die inhoud toevoegen op basis van of die inhoud voldoet aan een opgegeven voorwaarde. Ten slotte kunt u een actie toevoegen die de inhoud van het bestand opgehaald en die inhoud opslaat in een map op de SFTP-server. 

**Voorbeeld van de onderneming**: U kunt deze trigger gebruiken voor het bewaken van een FTP-map voor nieuwe bestanden die staan voor klanten en orders. U kunt vervolgens een FTP-actie zoals gebruiken **bestandsinhoud ophalen**, zodat u kunt de volgorde van de inhoud voor verdere verwerking ophalen en deze volgorde opslaan in een orderdatabase.

Een geldig en functionele logische app is een trigger en ten minste één actie vereist. Dus zorg ervoor dat u een actie toevoegen nadat u een trigger hebt toegevoegd.

Hier volgt een voorbeeld waarin deze trigger: **wanneer een bestand wordt toegevoegd of gewijzigd**

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Typ 'ftp' als filter voor lege, logische apps, in het zoekvak. Selecteer deze trigger onder de lijst met triggers: **wanneer een gearchiveerde is toegevoegd of gewijzigd - FTP**

   ![Zoek en selecteer de FTP-trigger](./media/connectors-create-api-ftp/select-ftp-trigger.png)  

1. Geeft u de benodigde informatie voor uw verbinding en kies vervolgens **maken**.

   ![FTP-serververbinding maken](./media/connectors-create-api-ftp/create-ftp-connection-trigger.png)  

1. Naast de **map** Kies het pictogram van de map zodat er wordt een lijst weergegeven. Naar de map die u wilt controleren op nieuwe of bewerkte bestanden, selecteer de pijl-rechts hoek (**>**), bladert u naar die map en selecteer vervolgens de map.

   ![Zoek en selecteer de map die u wilt bewaken](./media/connectors-create-api-ftp/select-folder.png)  

   De geselecteerde map wordt weergegeven in de **map** vak.

   ![Geselecteerde map](./media/connectors-create-api-ftp/selected-folder.png)  

Nu dat uw logische app een trigger heeft, voeg de acties die u wilt dat wordt uitgevoerd wanneer de logische app vindt een nieuw of bewerkte bestand. In dit voorbeeld kunt u een FTP-actie die de nieuwe of bijgewerkte inhoud opgehaald toevoegen.

### <a name="ftp-action-get-content"></a>FTP-actie: inhoud ophalen

Deze actie wordt de inhoud opgehaald van een bestand in een FTP-server wanneer dat bestand is toegevoegd of bijgewerkt. U kunt bijvoorbeeld de trigger toevoegen uit het vorige voorbeeld en een actie die de inhoud van het bestand opgehaald nadat dit bestand wordt toegevoegd of bewerkt. 

1. Kies onder de trigger of eventuele andere vereiste acties, **nieuwe stap**. 

1. Voer 'ftp' als filter in het zoekvak in. Onder de lijst met acties, selecteert u deze actie: **bestandsinhoud - FTP ophalen**

   ![FTP-actie selecteren](./media/connectors-create-api-ftp/select-ftp-action.png)  

1. Als u al een verbinding met uw FTP-server en -account hebt, gaat u naar de volgende stap. Anders geeft u de benodigde gegevens voor die verbinding en kies vervolgens **maken**. 

   ![FTP-serververbinding maken](./media/connectors-create-api-ftp/create-ftp-connection-action.png)

1. Na de **bestandsinhoud ophalen** actie wordt geopend, klikt u in de **bestand** vak zodat de lijst met dynamische inhoud wordt weergegeven. U kunt nu de eigenschappen voor de uitvoer selecteren uit de vorige stappen. Selecteer in de lijst met dynamische inhoud, de **bestandsinhoud** eigenschap, die de inhoud van het bestand toegevoegd of bijgewerkt.  

   ![Zoek en selecteer bestand](./media/connectors-create-api-ftp/ftp-action-get-file-content.png)

   De **bestandsinhoud** eigenschap wordt nu weergegeven in de **bestand** vak.

   ![Geselecteerde "Bestandsinhoud" eigenschap](./media/connectors-create-api-ftp/ftp-action-selected-file-content-property.png)

1. Sla uw logische app op. Als u wilt testen van uw werkstroom, moet u een bestand toevoegen aan de FTP-map die uw logische app nu bewaakt.

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over triggers en acties limieten die worden beschreven van de connector openapi (voorheen Swagger) beschrijving van de connector controleren [-verwijzingspagina](/connectors/ftpconnector/).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)