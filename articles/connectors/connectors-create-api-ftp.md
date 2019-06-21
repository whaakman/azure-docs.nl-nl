---
title: Verbinding maken met de FTP-server - Azure Logic Apps
description: Maken, bewaken en beheren van bestanden op een FTP-server met Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: divswa, klam, LADocs
ms.topic: article
ms.date: 06/19/2019
tags: connectors
ms.openlocfilehash: 66f1d726dcfa1a077abbff0d9f028036db43cc25
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67293094"
---
# <a name="create-monitor-and-manage-ftp-files-by-using-azure-logic-apps"></a>Maken, bewaken en beheren van FTP-bestanden met behulp van Azure Logic Apps

Met Azure Logic Apps en de FTP-connector, kunt u geautomatiseerde taken en werkstromen die maken, bewaken, verzenden en ontvangen van bestanden via uw account op een FTP-server, samen met andere acties, bijvoorbeeld:

* Bewaken wanneer bestanden worden toegevoegd of gewijzigd.
* Ophalen, maken, kopiëren, lijst, bijwerken en verwijderen van bestanden.
* Inhoud van bestanden en metagegevens ophalen.
* Pak archieven voor mappen.

U kunt triggers die te antwoorden krijgen van uw FTP-server en de uitvoer beschikbaar voor andere acties. U kunt acties uitvoeren in uw logische apps gebruiken voor het beheren van bestanden op uw FTP-server. U kunt ook andere acties waarmee de uitvoer van de FTP-acties hebben. Als u regelmatig bestanden van uw FTP-server, kunt u e-mailberichten over deze bestanden en hun inhoud verzenden met behulp van de connector voor Office 365 Outlook of Outlook.com-connector. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="limits"></a>Limits

* De FTP-connector ondersteunt alleen expliciete FTP via SSL (FTPS) en is niet compatibel met impliciete FTPS.

* Standaard, FTP-acties kunnen lezen of schrijven van bestanden die zijn *50 MB of kleiner*. FTP-acties ondersteuning voor het afhandelen van bestanden die groter zijn dan 50 MB, [bericht logische groepen te verdelen](../logic-apps/logic-apps-handle-large-messages.md). De **bestandsinhoud ophalen** actie impliciet gebruikt logische groepen te verdelen.

* FTP-triggers bieden geen ondersteuning voor logische groepen te verdelen. Bij het aanvragen van inhoud, selecteert u alleen bestanden die zijn dan 50 MB triggers of kleiner. Als u bestanden groter zijn dan 50 MB, gaat u als volgt dit patroon:

  * Gebruik van een FTP-trigger die als resultaat eigenschappen, zoals geeft **wanneer een bestand wordt toegevoegd of gewijzigd (alleen eigenschappen)** .

  * Ga als volgt de trigger met de FTP **bestandsinhoud ophalen** actie, die het volledige bestand wordt gelezen en impliciet gebruikt logische groepen te verdelen.

## <a name="how-ftp-triggers-work"></a>Hoe werken in FTP wordt geactiveerd

FTP-triggers werk door het bestandssysteem van de FTP-polling en op zoek naar een bestand dat is gewijzigd sinds de laatste polling. Sommige hulpprogramma's kunt u de tijdstempel behouden wanneer de bestanden worden gewijzigd. In dergelijke gevallen moet u deze functie uitschakelen zodat de trigger kunt werken. Hier volgen enkele algemene instellingen:

| SFTP-client | Bewerking |
|-------------|--------|
| Winscp | Ga naar **opties** > **voorkeuren** > **Transfer** > **bewerken**  >  **Behouden timestamp** > **uitschakelen** |
| FileZilla | Ga naar **Transfer** > **tijdstempels van overgebrachte bestanden behouden** > **uitschakelen** |
|||

Als een trigger een nieuw bestand wordt gevonden, wordt de trigger wordt gecontroleerd dat het nieuwe bestand voltooid en geen gedeeltelijk geschreven is. Bijvoorbeeld, een bestand mogelijk wijzigingen wordt uitgevoerd wanneer de trigger wordt gecontroleerd voor de bestandsserver. Om te voorkomen dat een gedeeltelijk geschreven bestand retourneren, merkt de trigger de tijdstempel voor het bestand dat recente wijzigingen, maar niet direct dat bestand geretourneerd. De trigger retourneert het bestand alleen wanneer u een polling van de server opnieuw. Dit gedrag kan soms leiden tot een vertraging die is maximaal twee keer van de trigger polling-interval.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, [registreer u dan nu voor een gratis Azure-account](https://azure.microsoft.com/free/).

* Uw FTP-host-server-adres en account-referenties

  De FTP-connector vereist dat uw FTP-server toegankelijk is vanaf het internet en ingesteld is om te werken in *passieve* modus. Uw referenties kunnen uw logische app een verbinding maken en toegang tot uw FTP-account.

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app waar u toegang tot uw FTP-account. Om te beginnen met een FTP-trigger, [maken van een lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). Voor het gebruik van een FTP-actie beginnen uw logische app met een andere trigger, bijvoorbeeld, de **terugkeerpatroon** trigger.

## <a name="connect-to-ftp"></a>Verbinding maken met FTP

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Typ 'ftp' als filter voor lege, logische apps, in het zoekvak. Selecteer de gewenste trigger onder de lijst met triggers.

   -of-

   Voor bestaande logische apps, onder de laatste stap waarin u wilt toevoegen van een actie kiezen **nieuwe stap**, en selecteer vervolgens **een actie toevoegen**. Voer 'ftp' als filter in het zoekvak in. Selecteer de actie die u wilt onder de lijst met acties.

   Als u wilt toevoegen een actie tussen fasen, de aanwijzer over de pijl tussen fasen. Kies het plusteken ( **+** ) die wordt weergegeven, en selecteer **een actie toevoegen**.

1. Geeft u de benodigde informatie voor uw verbinding en kies vervolgens **maken**.

1. Geef de benodigde informatie voor uw geselecteerde trigger of actie en doorgaan met het ontwikkelen van uw logische app-werkstroom.

## <a name="examples"></a>Voorbeelden

<a name="file-added-modified"></a>

### <a name="ftp-trigger-when-a-file-is-added-or-modified"></a>FTP-trigger: Wanneer een bestand wordt toegevoegd of gewijzigd

Deze trigger start een werkstroom voor logische Apps wanneer de trigger wordt gedetecteerd wanneer een bestand wordt toegevoegd of gewijzigd op een FTP-server. Dus bijvoorbeeld, dat kunt u een voorwaarde die controleert of de inhoud van het bestand en beslist of om op te halen die inhoud toevoegen op basis van of die inhoud voldoet aan een opgegeven voorwaarde. Ten slotte kunt u een actie toevoegen die de inhoud van het bestand opgehaald en die inhoud opslaat in een map op de SFTP-server.

**Voorbeeld van de onderneming**: U kunt deze trigger gebruiken voor het bewaken van een FTP-map voor nieuwe bestanden die klanten en orders te beschrijven. U kunt vervolgens een FTP-actie zoals gebruiken **bestandsinhoud ophalen**, zodat u kunt de volgorde van de inhoud voor verdere verwerking ophalen en deze volgorde opslaan in een orderdatabase.

Hier volgt een voorbeeld waarin deze trigger: **Wanneer een bestand wordt toegevoegd of gewijzigd**

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Typ 'ftp' als filter voor lege, logische apps, in het zoekvak. Selecteer deze trigger onder de lijst met triggers: **Wanneer een gearchiveerde is toegevoegd of gewijzigd - FTP**

   ![Zoek en selecteer de FTP-trigger](./media/connectors-create-api-ftp/select-ftp-trigger.png)  

1. Geeft u de benodigde informatie voor uw verbinding en kies vervolgens **maken**.

   Standaard Hiermee deze connector worden bestanden in tekstindeling. Voor de overdracht van bestanden in binaire indeling, bijvoorbeeld, waar en wanneer de codering wordt gebruikt, selecteert u **binair Transport**.

   ![FTP-serververbinding maken](./media/connectors-create-api-ftp/create-ftp-connection-trigger.png)  

1. Naast de **map** Kies het pictogram van de map zodat er wordt een lijst weergegeven. Naar de map die u wilt controleren op nieuwe of bewerkte bestanden, selecteer de pijl-rechts hoek ( **>** ), bladert u naar die map en selecteer vervolgens de map.

   ![Zoek en selecteer de map die u wilt bewaken](./media/connectors-create-api-ftp/select-folder.png)  

   De geselecteerde map wordt weergegeven in de **map** vak.

   ![Geselecteerde map](./media/connectors-create-api-ftp/selected-folder.png)  

Nu dat uw logische app een trigger heeft, voeg de acties die u wilt dat wordt uitgevoerd wanneer de logische app vindt een nieuw of bewerkte bestand. In dit voorbeeld kunt u een FTP-actie die de nieuwe of bijgewerkte inhoud opgehaald toevoegen.

<a name="get-content"></a>

### <a name="ftp-action-get-content"></a>FTP-actie: Inhoud ophalen

Deze actie wordt de inhoud opgehaald van een bestand in een FTP-server wanneer dat bestand is toegevoegd of bijgewerkt. U kunt bijvoorbeeld de trigger toevoegen uit het vorige voorbeeld en een actie die de inhoud van het bestand opgehaald nadat dit bestand wordt toegevoegd of bewerkt.

Hier volgt een voorbeeld waarin deze actie: **Inhoud ophalen**

1. Kies onder de trigger of eventuele andere vereiste acties, **nieuwe stap**.

1. Voer 'ftp' als filter in het zoekvak in. Selecteer deze actie onder de lijst met acties: **Bestandsinhoud - FTP ophalen**

   ![FTP-actie selecteren](./media/connectors-create-api-ftp/select-ftp-action.png)  

1. Als u al een verbinding met uw FTP-server en -account hebt, gaat u naar de volgende stap. Anders geeft u de benodigde gegevens voor die verbinding en kies vervolgens **maken**.

   ![FTP-serververbinding maken](./media/connectors-create-api-ftp/create-ftp-connection-action.png)

1. Na de **bestandsinhoud ophalen** actie wordt geopend, klikt u in de **bestand** vak zodat de lijst met dynamische inhoud wordt weergegeven. U kunt nu de eigenschappen voor de uitvoer selecteren uit de vorige stappen. Selecteer in de lijst met dynamische inhoud, de **bestandsinhoud** eigenschap, die de inhoud van het bestand toegevoegd of bijgewerkt.  

   ![Zoek en selecteer bestand](./media/connectors-create-api-ftp/ftp-action-get-file-content.png)

   De **bestandsinhoud** eigenschap wordt nu weergegeven in de **bestand** vak.

   ![Geselecteerde "Bestandsinhoud" eigenschap](./media/connectors-create-api-ftp/ftp-action-selected-file-content-property.png)

1. Sla uw logische app op. Als u wilt testen van uw werkstroom, moet u een bestand toevoegen aan de FTP-map die uw logische app nu bewaakt.

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over triggers en acties limieten die worden beschreven van de connector openapi (voorheen Swagger) beschrijving, Controleer de [van de connector-verwijzingspagina](/connectors/ftpconnector/).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
