---
title: Verbinding maken met SFTP-account - Azure Logic Apps | Microsoft Docs
description: Automatiseren van taken en processen die bewaken, maken, beheren, verzenden en ontvangen van bestanden voor een SFTP-server via SSH met behulp van Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: divswa, klam, LADocs
ms.assetid: 697eb8b0-4a66-40c7-be7b-6aa6b131c7ad
ms.topic: article
tags: connectors
ms.date: 10/26/2018
ms.openlocfilehash: 5d328164ac8ad99db15a12d850327615a9ffd809
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54910281"
---
# <a name="monitor-create-and-manage-sftp-files-by-using-azure-logic-apps"></a>Controleren, maken en beheren van de SFTP-bestanden met behulp van Azure Logic Apps

Voor het automatiseren van taken die bewaken, maken, verzenden en ontvangen van bestanden op een [Secure File Transfer Protocol (SFTP)](https://www.ssh.com/ssh/sftp/) -server die u kunt bouwen en automatiseren van werkstromen voor gegevensintegratie met behulp van Azure Logic Apps en de SFTP-connector. SFTP is een netwerkprotocol dat bestandstoegang, bestandsoverdracht en bestandsbeheer ten opzichte van een betrouwbare gegevensstroom biedt. Hier volgen enkele voorbeelden van taken die kunt u automatiseren: 

* Bewaken wanneer bestanden worden toegevoegd of gewijzigd.
* Ophalen, maken, kopiëren, lijst, bijwerken en verwijderen van bestanden.
* Inhoud van bestanden en metagegevens ophalen.
* Pak archieven voor mappen.

In vergelijking met de [SFTP-SSH-connector](../connectors/connectors-sftp-ssh.md), de SFTP-connector kan lezen of schrijven maximaal 50 MB in grootte van bestanden, tenzij u [bericht opdelen in acties](../logic-apps/logic-apps-handle-large-messages.md). U niet op dit moment gebruiken logische groepen te verdelen voor triggers. Voor bestanden van maximaal 1 GB in grootte, gebruik de [SFTP-SSH-connector](../connectors/connectors-sftp-ssh.md). Voor bestanden die groter zijn dan 1 GB, kunt u de SFTP-SSH connector plus [bericht logische groepen te verdelen](../logic-apps/logic-apps-handle-large-messages.md). 

U kunt triggers die gebeurtenissen op uw SFTP-server bewaken en uitvoer beschikbaar voor andere acties. U kunt acties waarmee verschillende taken worden uitgevoerd op uw SFTP-server gebruiken. U kunt ook andere acties in uw logische app de uitvoer van de SFTP-acties hebben. Bijvoorbeeld, als u regelmatig bestanden uit uw SFTP-server ophalen, kunt u e-mailmeldingen over deze bestanden en hun inhoud verzenden met behulp van de connector voor Office 365 Outlook of Outlook.com-connector.
Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

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

## <a name="connect-to-sftp"></a>Verbinding maken met SFTP

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Typ 'sftp' als filter voor lege, logische apps, in het zoekvak. Selecteer de gewenste trigger onder de lijst met triggers. 

   -of-

   Voor bestaande logische apps, onder de laatste stap waarin u wilt toevoegen van een actie kiezen **nieuwe stap**. 
   Typ 'sftp' als filter in het zoekvak. 
   Selecteer de actie die u wilt onder de lijst met acties.

   Als u wilt toevoegen een actie tussen fasen, de aanwijzer over de pijl tussen fasen. 
   Kies het plusteken (**+**) die wordt weergegeven, en selecteer vervolgens **een actie toevoegen**.

1. Hiermee geeft u de benodigde informatie voor uw verbinding.

   > [!IMPORTANT] 
   >
   > Wanneer u uw persoonlijke SSH-sleutel in de **persoonlijke SSH-sleutel** eigenschap, volgt u deze extra stappen uitvoeren, die ervoor te zorgen dat u de volledige en correcte waarde opgeven voor deze eigenschap. 
   > Een ongeldige sleutel zorgt ervoor dat de verbinding is mislukt.
   
   Hoewel u een teksteditor gebruiken kunt, vindt hier u voorbeeld van de stappen die laten hoe goed Kopieer en plak uw sleutel zien met behulp van Notepad.exe als voorbeeld.
    
   1. Open uw persoonlijke SSH-sleutelbestand in een teksteditor. 
   Kladblok gebruiken als deze stappen als in het voorbeeld.

   1. Op de Kladblok **bewerken** in het menu **Alles selecteren**.

   1. Selecteer **bewerken** > **kopie**.

   1. In de SFTP-trigger of actie die u hebt toegevoegd, plak de *voltooid* sleutel die u hebt gekopieerd in de **persoonlijke SSH-sleutel** eigenschap, die ondersteuning biedt voor meerdere regels. 
   ***Zorg ervoor dat u hebt geplakt*** de sleutel. ***Niet handmatig invoeren of bewerk de sleutel***.

1. Wanneer u klaar bent voor het invoeren van gegevens van de verbinding kiezen **maken**.

1. Geef de benodigde informatie voor uw geselecteerde trigger of actie en doorgaan met het ontwikkelen van uw logische app-werkstroom.

## <a name="trigger-limits"></a>Trigger-limieten

De SFTP-triggers werken door het bestandssysteem SFTP polling en op zoek naar een bestand dat is gewijzigd sinds de laatste polling. Sommige hulpprogramma's kunt u de tijdstempel behouden wanneer de bestanden worden gewijzigd. In dergelijke gevallen moet u deze functie uitschakelen zodat de trigger kunt werken. Hier volgen enkele algemene instellingen:

| SFTP-client | Bewerking | 
|-------------|--------| 
| Winscp | Ga naar **opties** > **voorkeuren** > **Transfer** > **bewerken**  >  **Behouden timestamp** > **uitschakelen** |
| FileZilla | Ga naar **Transfer** > **tijdstempels van overgebrachte bestanden behouden** > **uitschakelen** | 
||| 

Als een trigger een nieuw bestand wordt gevonden, wordt de trigger wordt gecontroleerd dat het nieuwe bestand voltooid en geen gedeeltelijk geschreven is. Bijvoorbeeld, een bestand mogelijk wijzigingen wordt uitgevoerd wanneer de trigger wordt gecontroleerd voor de bestandsserver. Om te voorkomen dat een gedeeltelijk geschreven bestand retourneren, merkt de trigger de tijdstempel voor het bestand dat recente wijzigingen, maar niet direct dat bestand geretourneerd. De trigger retourneert het bestand alleen wanneer u een polling van de server opnieuw. Dit gedrag kan soms leiden tot een vertraging die is maximaal twee keer van de trigger polling-interval. 

Bij het aanvragen van de inhoud van bestand krijg triggers geen bestanden groter zijn dan 50 MB. Als u bestanden groter zijn dan 50 MB, gaat u als volgt dit patroon: 

* Gebruik van een trigger die eigenschappen, zoals retourneert **wanneer een bestand wordt toegevoegd of gewijzigd (alleen eigenschappen)**.

* Ga als volgt de trigger met de actie die het volledige bestand, zoals leest **bestandsinhoud ophalen via pad**, en de actie gebruiken [bericht logische groepen te verdelen](../logic-apps/logic-apps-handle-large-messages.md).

## <a name="examples"></a>Voorbeelden

<a name="file-add-modified"></a>

### <a name="sftp-trigger-when-a-file-is-added-or-modified"></a>SFTP-trigger: Wanneer een bestand is toegevoegd of gewijzigd

Deze trigger wordt een werkstroom voor logische app gestart wanneer een bestand wordt toegevoegd of gewijzigd op een SFTP-server. U kunt bijvoorbeeld een voorwaarde die controleert of de inhoud van het bestand en wordt de inhoud op basis van of de inhoud voldoet aan een opgegeven voorwaarde toevoegen. U kunt vervolgens een actie toevoegen die de bestandsinhoud opgehaald en die inhoud in een map op de SFTP-server geplaatst. 

**Voorbeeld van de onderneming**: U kunt deze trigger gebruiken voor het bewaken van een SFTP-map voor nieuwe bestanden die staan voor klanten en orders. U kunt vervolgens een SFTP-actie zoals gebruiken **bestandsinhoud ophalen** , zodat u de volgorde van de inhoud ophalen voor verdere verwerking en die volgorde opslaan in een orderdatabase.

Bij het aanvragen van de inhoud van bestand krijg triggers geen bestanden groter zijn dan 50 MB. Als u bestanden groter zijn dan 50 MB, gaat u als volgt dit patroon: 

* Gebruik van een trigger die eigenschappen, zoals retourneert **wanneer een bestand wordt toegevoegd of gewijzigd (alleen eigenschappen)**.

* Ga als volgt de trigger met de actie die het volledige bestand, zoals leest **bestandsinhoud ophalen via pad**, en de actie gebruiken [bericht logische groepen te verdelen](../logic-apps/logic-apps-handle-large-messages.md).

<a name="get-content"></a>

### <a name="sftp-action-get-content"></a>SFTP-actie: Inhoud ophalen

Deze actie wordt de inhoud opgehaald van een bestand in een SFTP-server. U kunt bijvoorbeeld de trigger toevoegen uit het vorige voorbeeld en een voorwaarde die moet voldoen aan de inhoud van het bestand. Als de voorwaarde waar is, wordt de actie die wordt de inhoud opgehaald kunt uitvoeren. 

Bij het aanvragen van de inhoud van bestand krijg triggers geen bestanden groter zijn dan 50 MB. Als u bestanden groter zijn dan 50 MB, gaat u als volgt dit patroon: 

* Gebruik van een trigger die eigenschappen, zoals retourneert **wanneer een bestand wordt toegevoegd of gewijzigd (alleen eigenschappen)**.

* Ga als volgt de trigger met de actie die het volledige bestand, zoals leest **bestandsinhoud ophalen via pad**, en de actie gebruiken [bericht logische groepen te verdelen](../logic-apps/logic-apps-handle-large-messages.md).

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over triggers en acties limieten die worden beschreven van de connector openapi (voorheen Swagger) beschrijving van de connector controleren [-verwijzingspagina](/connectors/sftpconnector/).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
