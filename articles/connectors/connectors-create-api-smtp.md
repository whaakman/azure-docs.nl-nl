---
title: Verbinding maken met SMTP-server van Azure Logic Apps | Microsoft Docs
description: Automatiseren van taken en werkstromen die het verzenden van e-mail via uw SMTP (Simple Mail Transfer Protocol)-account met behulp van Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: d4141c08-88d7-4e59-a757-c06d0dc74300
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: 90af33574093cfbe529093c7091ee6988f043aa6
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43052019"
---
# <a name="send-email-from-your-smtp-account-with-azure-logic-apps"></a>E-mail verzenden vanuit uw SMTP-account met Azure Logic Apps

Met Azure Logic Apps en de Simple Mail Transfer Protocol (SMTP)-connector, kunt u geautomatiseerde taken en werkstromen die e-mail vanuit uw SMTP-account verzenden maken. U kunt ook andere acties waarmee de uitvoer van de SMTP-acties hebben. Nadat uw SMTP een e-mailbericht verzendt, kunt u bijvoorbeeld uw team in Slack met de Slack-connector waarschuwen. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* Uw SMTP-account en de gebruikersreferenties

  Uw referenties toestaan dat de logische app een verbinding maken en toegang tot uw SMTP-account.

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app waar u toegang tot uw SMTP-account. Voor het gebruik van een SMTP-actie, start u uw logische app met een trigger, zoals een Salesforce-trigger, hebt u een Salesforce-account.

  Bijvoorbeeld, kun u uw logische app met de **wanneer een record wordt gemaakt** Salesforce-trigger. 
  Deze trigger wordt elke keer dat een nieuwe record, zoals een lead in Salesforce wordt gemaakt. 
  U kunt deze trigger met de SMTP volgen **E-mail verzenden** actie. Op die manier, wanneer de nieuwe record wordt gemaakt, verzendt uw logische app een e-mail van uw SMTP-account over de nieuwe record.

## <a name="connect-to-smtp"></a>Verbinding maken met SMTP-server

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Kies onder de laatste stap waar u een SMTP-actie toevoegen, **nieuwe stap**. 

   Als u wilt toevoegen een actie tussen fasen, de aanwijzer over de pijl tussen fasen. 
   Kies het plusteken (**+**) die wordt weergegeven, en selecteer vervolgens **een actie toevoegen**.

1. Typ 'smtp' als filter in het zoekvak. Selecteer de actie die u wilt onder de lijst met acties.

1. Wanneer u hierom wordt gevraagd, moet u deze verbindingsgegevens opgeven:

   | Eigenschap | Vereist | Beschrijving |
   |----------|----------|-------------|
   | **Verbindingsnaam** | Ja | Een naam op voor de verbinding met de SMTP-server | 
   | **SMTP-serveradres** | Ja | Het adres voor de SMTP-server | 
   | **Naam van gebruiker** | Ja | Uw gebruikersnaam voor de SMTP-account | 
   | **Wachtwoord** | Ja | Uw wachtwoord voor uw SMTP-account | 
   | **SMTP-serverpoort** | Nee | Een specifieke poort op de SMTP-server die u wilt gebruiken | 
   | **SSL inschakelen?** | Nee | Op of SSL-versleuteling uitschakelen. | 
   |||| 

1. Geef de benodigde informatie voor de geselecteerde actie. 

1. Sla uw logische app of doorgaan met het ontwikkelen van uw logische app-werkstroom.

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over triggers en acties limieten die worden beschreven van de connector openapi (voorheen Swagger) beschrijving van de connector controleren [-verwijzingspagina](/connectors/smtpconnector/).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)