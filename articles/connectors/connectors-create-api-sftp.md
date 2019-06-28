---
title: Verbinding maken met SFTP-account - Azure Logic Apps
description: Automatiseren van taken en processen die bewaken, maken, beheren, verzenden en ontvangen van bestanden voor een SFTP-server via SSH met behulp van Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: divswa, klam, LADocs
ms.topic: article
ms.date: 06/18/2019
tags: connectors
ms.openlocfilehash: 5b80339e90947b7cc45b1edb5df00652a13291ac
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67293464"
---
# <a name="monitor-create-and-manage-sftp-files-by-using-azure-logic-apps"></a>Controleren, maken en beheren van de SFTP-bestanden met behulp van Azure Logic Apps

Voor het automatiseren van taken die bewaken, maken, verzenden en ontvangen van bestanden op een [Secure File Transfer Protocol (SFTP)](https://www.ssh.com/ssh/sftp/) -server die u kunt bouwen en automatiseren van werkstromen voor gegevensintegratie met behulp van Azure Logic Apps en de SFTP-connector. SFTP is een netwerkprotocol dat bestandstoegang, bestandsoverdracht en bestandsbeheer ten opzichte van een betrouwbare gegevensstroom biedt. Hier volgen enkele voorbeelden van taken die kunt u automatiseren:

* Bewaken wanneer bestanden worden toegevoegd of gewijzigd.
* Ophalen, maken, kopiëren, lijst, bijwerken en verwijderen van bestanden.
* Inhoud van bestanden en metagegevens ophalen.
* Pak archieven voor mappen.

U kunt triggers die gebeurtenissen op uw SFTP-server bewaken en uitvoer beschikbaar voor andere acties. U kunt acties waarmee verschillende taken worden uitgevoerd op uw SFTP-server gebruiken. U kunt ook andere acties in uw logische app de uitvoer van de SFTP-acties hebben. Bijvoorbeeld, als u regelmatig bestanden uit uw SFTP-server ophalen, kunt u e-mailmeldingen over deze bestanden en hun inhoud verzenden met behulp van de connector voor Office 365 Outlook of Outlook.com-connector. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="limits"></a>Limits

De SFTP-connector worden alleen bestanden die zijn verwerkt *50 MB of kleiner* en biedt geen ondersteuning voor [bericht logische groepen te verdelen](../logic-apps/logic-apps-handle-large-messages.md). Voor grotere bestanden, gebruikt u de [SFTP-SSH-connector](../connectors/connectors-sftp-ssh.md). Raadpleeg voor verschillen tussen de SFTP-connector en de SFTP-SSH-connector, [vergelijken SFTP-SSH versus SFTP](../connectors/connectors-sftp-ssh.md#comparison) in de SFTP-SSH-artikel.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, [registreer u dan nu voor een gratis Azure-account](https://azure.microsoft.com/free/).

* Uw SFTP-server-adres en -account referenties, zodat uw logische app toegang tot uw SFTP-account. Gebruik de [Secure Shell (SSH)](https://www.ssh.com/ssh/protocol/) -protocol, moet u ook toegang tot een persoonlijke SSH-sleutel en de SSH-privésleutelwachtwoord.

  > [!NOTE]
  >
  > De SFTP-connector ondersteunt deze persoonlijke sleutel indelingen: OpenSSH ssh.com en PuTTY
  >
  > Wanneer u uw logische app, maakt nadat u de SFTP-trigger of actie die u wilt toevoegen, moet u de verbindingsgegevens voor uw SFTP-server opgeven. 
  > Als u een persoonlijke SSH-sleutel gebruikt, zorg ervoor dat u ***kopie*** de sleutel van uw SSH bestand met persoonlijke sleutel en ***plakken*** die sleutel in de details van de verbinding, ***niet handmatig invoeren of de sleutelbewerken***, waardoor de verbinding is mislukt. 
  > Zie de volgende stappen in dit artikel voor meer informatie.

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app waar u toegang tot uw SFTP-account. Om te beginnen met een SFTP-trigger, [maken van een lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). Voor het gebruik van een SFTP-actie beginnen uw logische app met een andere trigger, bijvoorbeeld, de **terugkeerpatroon** trigger.

## <a name="how-sftp-triggers-work"></a>Hoe werken in SFTP wordt geactiveerd

SFTP triggers werk door het bestandssysteem SFTP polling en op zoek naar een bestand dat is gewijzigd sinds de laatste polling. Sommige hulpprogramma's kunt u de tijdstempel behouden wanneer de bestanden worden gewijzigd. In dergelijke gevallen moet u deze functie uitschakelen zodat de trigger kunt werken. Hier volgen enkele algemene instellingen:

| SFTP-client | Bewerking |
|-------------|--------|
| Winscp | Ga naar **opties** > **voorkeuren** > **Transfer** > **bewerken**  >  **Behouden timestamp** > **uitschakelen** |
| FileZilla | Ga naar **Transfer** > **tijdstempels van overgebrachte bestanden behouden** > **uitschakelen** |
|||

Als een trigger een nieuw bestand wordt gevonden, wordt de trigger wordt gecontroleerd dat het nieuwe bestand voltooid en geen gedeeltelijk geschreven is. Bijvoorbeeld, een bestand mogelijk wijzigingen wordt uitgevoerd wanneer de trigger wordt gecontroleerd voor de bestandsserver. Om te voorkomen dat een gedeeltelijk geschreven bestand retourneren, merkt de trigger de tijdstempel voor het bestand dat recente wijzigingen, maar niet direct dat bestand geretourneerd. De trigger retourneert het bestand alleen wanneer u een polling van de server opnieuw. Dit gedrag kan soms leiden tot een vertraging die is maximaal twee keer van de trigger polling-interval.

## <a name="connect-to-sftp"></a>Verbinding maken met SFTP

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Typ 'sftp' als filter voor lege, logische apps, in het zoekvak. Selecteer de gewenste trigger onder de lijst met triggers.

   -of-

   Voor bestaande logische apps, onder de laatste stap waarin u wilt toevoegen van een actie kiezen **nieuwe stap**. Typ 'sftp' als filter in het zoekvak. Selecteer de actie die u wilt onder de lijst met acties.

   Als u wilt toevoegen een actie tussen fasen, de aanwijzer over de pijl tussen fasen. Kies het plusteken ( **+** ) die wordt weergegeven, en selecteer vervolgens **een actie toevoegen**.

1. Hiermee geeft u de benodigde informatie voor uw verbinding.

   > [!IMPORTANT]
   >
   > Wanneer u uw persoonlijke SSH-sleutel in de **persoonlijke SSH-sleutel** eigenschap, volgt u deze extra stappen uitvoeren, die ervoor te zorgen dat u de volledige en correcte waarde opgeven voor deze eigenschap. 
   > Een ongeldige sleutel zorgt ervoor dat de verbinding is mislukt.

   Hoewel u een teksteditor gebruiken kunt, vindt hier u voorbeeld van de stappen die laten hoe goed Kopieer en plak uw sleutel zien met behulp van Notepad.exe als voorbeeld.

   1. Open uw persoonlijke SSH-sleutelbestand in een teksteditor. Kladblok gebruiken als deze stappen als in het voorbeeld.

   1. Op de Kladblok **bewerken** in het menu **Alles selecteren**.

   1. Selecteer **bewerken** > **kopie**.

   1. In de SFTP-trigger of actie die u hebt toegevoegd, plak de *voltooid* sleutel die u hebt gekopieerd in de **persoonlijke SSH-sleutel** eigenschap, die ondersteuning biedt voor meerdere regels. ***Zorg ervoor dat u hebt geplakt*** de sleutel. ***Niet handmatig invoeren of bewerk de sleutel***.

1. Wanneer u klaar bent voor het invoeren van gegevens van de verbinding kiezen **maken**.

1. Geef de benodigde informatie voor uw geselecteerde trigger of actie en doorgaan met het ontwikkelen van uw logische app-werkstroom.

## <a name="examples"></a>Voorbeelden

<a name="file-add-modified"></a>

### <a name="sftp-trigger-when-a-file-is-added-or-modified"></a>SFTP-trigger: Wanneer een bestand wordt toegevoegd of gewijzigd

Deze trigger wordt een werkstroom voor logische app gestart wanneer een bestand wordt toegevoegd of gewijzigd op een SFTP-server. U kunt bijvoorbeeld een voorwaarde die controleert of de inhoud van het bestand en wordt de inhoud op basis van of de inhoud voldoet aan een opgegeven voorwaarde toevoegen. U kunt vervolgens een actie toevoegen die de bestandsinhoud opgehaald en die inhoud in een map op de SFTP-server geplaatst.

**Voorbeeld van de onderneming**: U kunt deze trigger gebruiken voor het bewaken van een SFTP-map voor nieuwe bestanden die staan voor klanten en orders. U kunt vervolgens een SFTP-actie zoals gebruiken **bestandsinhoud ophalen** , zodat u de volgorde van de inhoud ophalen voor verdere verwerking en die volgorde opslaan in een orderdatabase.

<a name="get-content"></a>

### <a name="sftp-action-get-content"></a>SFTP-actie: Inhoud ophalen

Deze actie wordt de inhoud opgehaald van een bestand in een SFTP-server. U kunt bijvoorbeeld de trigger toevoegen uit het vorige voorbeeld en een voorwaarde die moet voldoen aan de inhoud van het bestand. Als de voorwaarde waar is, wordt de actie die wordt de inhoud opgehaald kunt uitvoeren.

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over triggers en acties limieten die worden beschreven van de connector openapi (voorheen Swagger) beschrijving van de connector controleren [-verwijzingspagina](/connectors/sftpconnector/).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
