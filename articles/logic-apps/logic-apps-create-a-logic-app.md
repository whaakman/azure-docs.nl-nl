---
title: Werkstromen maken met uw eerste logische app van Azure | Microsoft Docs
description: Aan de slag met het verbinden van apps en SaaS-services met uw eerste logische app
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: ce3582b5-9c58-4637-9379-75ff99878dcd
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/25/2017
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: 6e0ad6b5bec11c5197dd7bded64168a1b8cc2fdd
ms.openlocfilehash: 221f1b9f0985bbaf0553f6ca01f0f048b9976315
ms.lasthandoff: 03/28/2017


---
# <a name="create-a-new-logic-app-connecting-saas-services"></a>Een nieuwe logische app maken waarmee SaaS-services worden verbonden
In dit onderwerp wordt beschreven hoe u in een paar minuten aan de slag kunt met [Azure Logic Apps](logic-apps-what-are-logic-apps.md). We volgen een eenvoudige werkstroom waarmee u interessante tweets naar uw e-mail kunt verzenden.

Voor dit scenario hebt u het volgende nodig:

* Een Azure-abonnement
* Een Twitter-account
* Een Outlook.com- of Office 365 Outlook-account

## <a name="create-a-new-logic-app-to-email-you-tweets"></a>Een nieuwe logische app maken om tweets naar u te e-mailen

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 

2. Kies in het menu links de optie **Nieuw** > **Enterprise Integration** > **Logische app**.

    U kunt ook **Nieuw** kiezen, `logic app` in het zoekvak invoeren en op Enter drukken. Kies **Logische app** > **Maken**.

3. Voer een naam in voor de logische app, selecteer uw Azure-abonnement, maak of selecteer een Azure-resourcegroep, selecteer een locatie en kies **Maken**.

    Als u **Vastmaken aan Dashboard** selecteert, wordt de logische app automatisch geopend na implementatie.

4. Wanneer u de logische app voor het eerst opent, kunt u uit een sjabloon een selectie maken om te starten.
Klik voorlopig op **Lege logische app** om deze vanaf nul op te bouwen. 

5. Het eerste item dat u moet maken, is de trigger. Dit is de gebeurtenis waardoor de logische app wordt gestart. Zoek in het zoekvak naar **twitter** en selecteer **Wanneer er een nieuwe tweet wordt geplaatst**. Meld u aan met de gebruikersnaam en het wachtwoord voor uw Twitter-account.

6. Voer nu een zoekterm in om de logische app te activeren.

   ![Zoeken in Twitter](media/logic-apps-create-a-logic-app/twittersearch.png)

    De **Frequentie** en het **Interval** bepalen hoe vaak de logische app op nieuwe tweets zal controleren. Alle tweets tijdens die periode worden geretourneerd.

7. Selecteer **Nieuwe stap** en kies **Een actie toevoegen** of **Een voorwaarde toevoegen**.

    Wanneer u **Een actie toevoegen** selecteert, kunt in [beschikbare connectors](../connectors/apis-list.md) zoeken om een actie te kiezen. 

8. Zoek in het zoekvak naar **outlook** en selecteer **Een e-mail verzenden** om vanuit uw Outlook-account e-mail te verzenden naar een opgegeven e-mailadres.

   ![Acties](media/logic-apps-create-a-logic-app/actions.png)

9. U moet nu de parameters voor de gewenste e-mail invullen:

   ![Parameters](media/logic-apps-create-a-logic-app/parameters.png)

10. Ten slotte selecteert u **Opslaan** om de logische app te publiceren.

## <a name="manage-your-logic-app-after-creation"></a>Uw logische app beheren na het maken

Nu is uw logische app actief. Deze zal regelmatig controleren op tweets met de ingevoerde zoekterm. Wanneer er een overeenkomstige tweet wordt gevonden, verzendt de logische app een e-mailbericht naar u. Ten slotte ziet u hoe u de app kunt uitschakelen of wat de voortgang hiervan is.

1. Ga naar de [Azure Portal](https://portal.azure.com).

2. Klik in het menu links op **Meer services**. Kies onder **Enterprise Integration** de optie **Logic Apps**. Selecteer uw logische app.

    *    Kies in het menu van de logische app de optie **Overzicht** om de status, uitvoeringsgeschiedenis en algemene informatie van uw app te bekijken. Als de verwachte gegevens niet worden weergegeven, klikt u op de opdrachtbalk op **Vernieuwen**.

    *    Als u de app wilt bewerken, kiest u in het menu van de logische app de optie **Logic App Designer**.

    *    Als u de app tijdelijk wilt uitschakelen, kiest u in het menu van de logische app de optie **Overzicht**. Kies op de opdrachtbalk de optie **Uitschakelen**.

    *    Als u de app wilt verwijderen, kiest u in het menu van de logische app de optie **Overzicht**. 
    Kies op de opdrachtbalk de optie **Verwijderen**. Voer de naam van de logische app in en kies **Verwijderen**.

U hebt nu in minder dan 5 minuten een eenvoudige logische app ingesteld die wordt uitgevoerd in de cloud. Zie [Functies van logische apps gebruiken] voor meer informatie over het gebruik van functies van Logic Apps. Zie [Logic App-definities maken](../logic-apps/logic-apps-author-definitions.md) voor meer informatie over de Logic App-definities zelf.

<!-- Shared links -->
[Azure portal]: https://portal.azure.com
[Functies van logische apps gebruiken]: logic-apps-create-a-logic-app.md
