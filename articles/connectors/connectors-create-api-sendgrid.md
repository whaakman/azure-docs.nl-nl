---
title: Verbinding maken met SendGrid van Azure Logic Apps | Microsoft Docs
description: Automatiseren van taken en werkstromen voor het verzenden van e-mailberichten en adressenlijsten SendGrid beheren met behulp van Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: bc4f1fc2-824c-4ed7-8de8-e82baff3b746
ms.topic: article
tags: connectors
ms.date: 08/24/2018
ms.openlocfilehash: 7eecd3908883b195b52755d03e70872afe9180bb
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50228853"
---
# <a name="send-emails-and-manage-mailing-lists-in-sendgrid-by-using-azure-logic-apps"></a>Verzenden van e-mailberichten en adressenlijsten SendGrid beheren met behulp van Azure Logic Apps

Met Azure Logic Apps en de SendGrid-connector, kunt u geautomatiseerde taken en werkstromen die e-mailberichten verzenden en beheren van uw lijsten met ontvangers, bijvoorbeeld:

* E-mail verzenden.
* Geadresseerden toevoegen aan lijsten.
* Ophalen, toevoegen en beheren van de globale onderdrukking.

U kunt SendGrid-acties in uw logische apps gebruiken om uit te voeren van deze taken. U kunt ook andere acties waarmee de uitvoer van de SendGrid-acties hebben. 

Deze connector beschikt u alleen acties, om de logische App, gebruikt u een afzonderlijke trigger, zoals een **terugkeerpatroon** trigger. Bijvoorbeeld, als u regelmatig geadresseerden aan uw lijsten toevoegen, kunt u e-mailberichten over ontvangers en een lijst met behulp van de connector voor Office 365 Outlook of Outlook.com-connector verzenden.
Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* Een [SendGrid-account](https://www.sendgrid.com/) en een [SendGrid-API-sleutel](https://sendgrid.com/docs/ui/account-and-settings/api-keys/)

   Uw API-sleutel machtigt de logische app een verbinding maken en toegang tot uw SendGrid-account.

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app waar u toegang tot uw SendGrid-account. Voor het gebruik van een SendGrid-actie beginnen uw logische app met een andere trigger, bijvoorbeeld, de **terugkeerpatroon** trigger.

## <a name="connect-to-sendgrid"></a>Verbinding maken met SendGrid

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Kies een pad: 

   * Kies onder de laatste stap waar u een actie toevoegen, **nieuwe stap**. 

     -of-

   * Tussen de stappen waar u een actie toevoegen, de aanwijzer over de pijl tussen fasen. 
   Kies het plusteken (**+**) die wordt weergegeven, en selecteer vervolgens **een actie toevoegen**.

1. Typ 'sendgrid' als filter in het zoekvak. Selecteer de actie die u wilt onder de lijst met acties.

1. Geef een naam op voor uw verbinding. 

1. Voer uw SendGrid-API-sleutel en kies vervolgens **maken**.

1. Geef de benodigde informatie voor de geselecteerde actie en doorgaan met het ontwikkelen van uw logische app-werkstroom.

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over triggers en acties limieten die worden beschreven van de connector openapi (voorheen Swagger) beschrijving van de connector controleren [-verwijzingspagina](/connectors/sendgrid/).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)