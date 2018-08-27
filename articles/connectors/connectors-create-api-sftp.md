---
title: Verbinding maken met SFTP-account vanuit Azure Logic Apps | Microsoft Docs
description: Automatiseren van taken en werkstromen die bewaken, maken, beheren, verzenden en ontvangen van bestanden voor een SFTP-server met behulp van Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: 697eb8b0-4a66-40c7-be7b-6aa6b131c7ad
ms.topic: article
tags: connectors
ms.date: 08/24/2018
ms.openlocfilehash: 9714a00d070caab9d3a3338329295192e1eb9997
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42887584"
---
# <a name="monitor-create-and-manage-sftp-files-by-using-azure-logic-apps"></a>Controleren, maken en beheren van de SFTP-bestanden met behulp van Azure Logic Apps

Met Azure Logic Apps en de SFTP-connector, kunt u geautomatiseerde taken en werkstromen die worden bewaakt, maken, verzenden en ontvangen van bestanden via uw account op een [SFTP](https://www.ssh.com/ssh/sftp/) server, samen met andere acties, bijvoorbeeld:

* Bewaken wanneer bestanden worden toegevoegd of gewijzigd.
* Ophalen, maken, kopiëren, lijst, bijwerken en verwijderen van bestanden.
* Inhoud van bestanden en metagegevens ophalen.
* Pak archieven voor mappen.

U kunt triggers die te antwoorden krijgen van uw SFTP-server en de uitvoer beschikbaar voor andere acties. U kunt acties in uw logische apps gebruiken voor het uitvoeren van taken met bestanden op uw SFTP-server. U kunt ook andere acties waarmee de uitvoer van de SFTP-acties hebben. Bijvoorbeeld, als u regelmatig bestanden uit uw SFTP-server ophalen, kunt u e-mailberichten over deze bestanden en hun inhoud verzenden met behulp van de connector voor Office 365 Outlook of Outlook.com-connector.
Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Vereiste onderdelen

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* Uw SFTP-host-server-adres en accountreferenties

   Uw referenties toestaan dat de logische app een verbinding maken en toegang tot uw SFTP-account.

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app waar u toegang tot uw SFTP-account. Om te beginnen met een SFTP-trigger, [maken van een lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). Voor het gebruik van een SFTP-actie beginnen uw logische app met een andere trigger, bijvoorbeeld, de **terugkeerpatroon** trigger.

## <a name="connect-to-sftp"></a>Verbinding maken met SFTP

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Kies een pad: 

   * Typ 'salesforce' als filter voor lege, logische apps, in het zoekvak. 
   Selecteer de gewenste trigger onder de lijst met triggers. 

     -of-

   * Voor bestaande logische apps: 
   
     * Kies onder de laatste stap waar u een actie toevoegen, **nieuwe stap**. 

       -of-

     * Tussen de stappen waar u een actie toevoegen, de aanwijzer over de pijl tussen fasen. 
     Kies het plusteken (**+**) die wordt weergegeven, en selecteer vervolgens **een actie toevoegen**.
     
        Typ 'sftp' als filter in het zoekvak. 
        Selecteer de actie die u wilt onder de lijst met acties.

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

## <a name="get-support"></a>Ondersteuning vragen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)