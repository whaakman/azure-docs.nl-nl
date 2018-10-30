---
title: Verbinding maken met SFTP-account vanuit Azure Logic Apps | Microsoft Docs
description: Automatiseren van taken en werkstromen die bewaken, maken, beheren, verzenden en ontvangen van bestanden voor een SFTP-server met behulp van Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
tags: connectors
ms.date: 09/24/2018
ms.openlocfilehash: 2250c6952aeac7b10dcb1a1a35419941e5cad507
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233205"
---
# <a name="monitor-create-and-manage-sftp-files-by-using-azure-logic-apps-and-sftp-ssh-connector"></a>Controleren, maken en beheren van de SFTP-bestanden met behulp van Azure Logic Apps en SFTP-SSH-connector

Met Azure Logic Apps en de SFTP-SSH-connector, kunt u geautomatiseerde taken en werkstromen die worden bewaakt, maken, verzenden en ontvangen van bestanden via uw account op een [SFTP](https://www.ssh.com/ssh/sftp/) server, samen met andere acties, bijvoorbeeld:

* Bewaken wanneer bestanden worden toegevoegd of gewijzigd.
* Ophalen, maken, kopiëren, wijzigen, lijst, bijwerken en verwijderen van bestanden.
* Map maken.
* Inhoud van bestanden en metagegevens ophalen.
* Pak archieven voor mappen.

U kunt triggers die te antwoorden krijgen van uw SFTP-server en de uitvoer beschikbaar voor andere acties. U kunt acties in uw logische apps gebruiken voor het uitvoeren van taken met bestanden op uw SFTP-server. U kunt ook andere acties waarmee de uitvoer van de SFTP-acties hebben. Bijvoorbeeld, als u regelmatig bestanden uit uw SFTP-server ophalen, kunt u e-mailberichten over deze bestanden en hun inhoud verzenden met behulp van de connector voor Office 365 Outlook of Outlook.com-connector.
Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="sftp-ssh-versus-sftp"></a>SFTP-SSH versus SFTP

Hier volgen de enkele belangrijke verschillen tussen de SFTP-SSH-connector en de [SFTP](../connectors/connectors-create-api-sftp.md) connector. De SFTP-SSH-connector biedt deze mogelijkheden:

* Maakt gebruik van de <a href="https://github.com/sshnet/SSH.NET" target="_blank"> **SSH.NET** </a> library, die een open-source Secure Shell (SSH)-bibliotheek voor .NET is.

* Biedt ondersteuning voor grote bestanden tot **1 GB**. De connector kan lezen of schrijven van bestanden die maximaal 1 GB groot zijn.

* Biedt de **map maken** actie maakt u een map op het opgegeven pad op de SFTP-server.

* Biedt de **bestandsnaam wijzigen** actie, die u wijzigt de naam van een bestand op de SFTP-server.

* De verbinding met SFTP-server, die zorgt voor betere prestaties en vermindert het aantal verbindingspogingen op de server in de cache opslaat. 

  U kunt bepalen de duur die wordt gebruikt voor het opslaan van de verbinding door het instellen van de <a href="http://man.openbsd.org/sshd_config#ClientAliveInterval" target="_blank"> **ClientAliveInterval** </a> eigenschap op uw SFTP-server. 

## <a name="how-trigger-polling-works"></a>Hoe werkt polling activeren

De SFTP-SSH-triggers werken door het bestandssysteem SFTP polling en op zoek naar een bestand dat is gewijzigd sinds de laatste polling. Sommige hulpprogramma's kunt u de tijdstempel behouden wanneer de bestanden worden gewijzigd, dus in dergelijke gevallen u deze functie voor de trigger moet werkt uitschakelen. Hier volgen enkele algemene instellingen:

| SFTP-client | Bewerking | 
|-------------|--------| 
| Winscp | Opties voor → voorkeuren... → Overdracht → bewerken... → Behouden timestamp → uitschakelen |
| Filezilla werkt | Overdracht → behouden tijdstempels van overgebrachte bestanden → uitschakelen | 
||| 

Als een trigger een nieuw bestand wordt gevonden, wordt de trigger wordt gecontroleerd dat het nieuwe bestand voltooid en geen gedeeltelijk geschreven is. Bijvoorbeeld, een bestand mogelijk wijzigingen wordt uitgevoerd wanneer de trigger wordt gecontroleerd voor de bestandsserver. Om te voorkomen dat een gedeeltelijk geschreven bestand retourneren, merkt de trigger de tijdstempel voor het bestand dat recente wijzigingen, maar niet direct dat bestand geretourneerd. De trigger retourneert het bestand alleen wanneer u een polling van de server opnieuw. Dit gedrag kan soms leiden tot een vertraging die is maximaal twee keer van de trigger polling-interval. 

Bij het aanvragen van de inhoud van bestand, kan de trigger bestanden groter dan 50 MB niet ophalen. Als u wilt verzamelen van bestanden die groter zijn dan 50 MB, gaat u als volgt dit patroon:

* Gebruik van een trigger die eigenschappen, zoals retourneert **wanneer een bestand wordt toegevoegd of gewijzigd (alleen eigenschappen)**. 
* Ga als volgt de trigger met de actie die het volledige bestand, zoals leest **bestandsinhoud ophalen via pad**.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* Uw SFTP host-server-adres en -account referenties, die uw logische app een verbinding maken en toegang tot uw SFTP-account toestaan.

  Kopieer en plak de volledige inhoud voor de persoonlijke SSH-sleutel in de **persoonlijke SSH-sleutel** eigenschap aan de hand van de indeling met meerdere regels. 
  Hier vindt u een voorbeeld van de stappen die laten hoe de persoonlijke SSH-sleutel opgeven zien met behulp van Notepad.exe:
    
  1. Open het persoonlijke SSH-sleutelbestand in Notepad.exe
  2. Op de **bewerken** in het menu **Alles selecteren**.
  3. Selecteer **bewerken** > **kopie**.
  4. Wanneer u de verbinding maakt de **persoonlijke SSH-sleutel** eigenschap, plak de sleutel. Niet handmatig bewerken de **persoonlijke SSH-sleutel** eigenschap.

     De connector maakt gebruik van de bibliotheek SSH.NET, die ondersteuning biedt voor deze SSH indelingen voor persoonlijke sleutel en de MD5 vingerafdruk:

     * RSA 
     * DSA

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

1. Geeft u de benodigde informatie voor uw verbinding en kies vervolgens **maken**.

1. Geef de benodigde informatie voor uw geselecteerde trigger of actie en doorgaan met het ontwikkelen van uw logische app-werkstroom.

## <a name="examples"></a>Voorbeelden

### <a name="sftp-trigger-when-a-file-is-added-or-modified"></a>SFTP-trigger: wanneer een bestand wordt toegevoegd of gewijzigd

Deze trigger start een werkstroom voor logische Apps wanneer de trigger wordt gedetecteerd wanneer een bestand wordt toegevoegd of gewijzigd op een SFTP-server. Dus bijvoorbeeld, dat kunt u een voorwaarde die controleert of de inhoud van het bestand en beslist of om op te halen die inhoud toevoegen op basis van of die inhoud voldoet aan een opgegeven voorwaarde. Ten slotte kunt u een actie toevoegen die de inhoud van het bestand opgehaald en die inhoud opslaat in een map op de SFTP-server. 

**Voorbeeld van de onderneming**: U kunt deze trigger gebruiken voor het bewaken van een SFTP-map voor nieuwe bestanden die staan voor klanten en orders. U kunt vervolgens een SFTP-actie zoals gebruiken **bestandsinhoud ophalen**, zodat u kunt de volgorde van de inhoud voor verdere verwerking ophalen en deze volgorde opslaan in een orderdatabase.

### <a name="sftp-action-get-content"></a>SFTP-actie: inhoud ophalen

Deze actie wordt de inhoud opgehaald van een bestand in een SFTP-server. U kunt bijvoorbeeld de trigger toevoegen uit het vorige voorbeeld en een voorwaarde die moet voldoen aan de inhoud van het bestand. Als de voorwaarde waar is, wordt de actie die wordt de inhoud opgehaald kunt uitvoeren. 

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over triggers en acties limieten die worden beschreven van de connector openapi (voorheen Swagger) beschrijving van de connector controleren [-verwijzingspagina](/connectors/sftpconnector/).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)