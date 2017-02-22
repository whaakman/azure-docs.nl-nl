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
ms.sourcegitcommit: ce1e0cfe7c7f52d521ef3318376af75331323083
ms.openlocfilehash: 178a57624345a4b3e5d73e64ce4ccf81b8b90a88


---
# <a name="create-a-new-logic-app-connecting-saas-services"></a>Een nieuwe logische app maken waarmee SaaS-services worden verbonden
In dit onderwerp wordt beschreven hoe u in een paar minuten aan de slag kunt met [Azure Logic Apps](logic-apps-what-are-logic-apps.md). We volgen een eenvoudige werkstroom waarmee u interessante tweets naar uw e-mail kunt verzenden.

Voor dit scenario hebt u het volgende nodig:

* Een Azure-abonnement
* Een Twitter-account
* Een outlook.com-postvak of een gehost Office 365-postvak

## <a name="create-a-new-logic-app-to-email-you-tweets"></a>Een nieuwe logische app maken om tweets naar u te e-mailen
1. Selecteer **Nieuw** op het [dashboard van Azure Portal](https://portal.azure.com). 
2. Zoek op de zoekbalk naar ‘logische app’ en selecteer vervolgens **Logische apps**. U kunt ook **Nieuw**, **Web en mobiel** selecteren en vervolgens **Logische apps** selecteren. 
3. Voer een naam in voor uw logische app, selecteer een locatie en resourcegroep en selecteer **Maken**.  Als u **Vastmaken aan Dashboard** selecteert, wordt de logische app automatisch geopend na implementatie.  
4. Nadat uw logische app voor het eerst is geopend, kunt u uit een sjabloon een selectie maken om te starten.  Klik voorlopig op **Lege logische app** om deze vanaf nul op te bouwen. 
5. Het eerste item dat u moet maken, is de trigger.  Dit is de gebeurtenis waardoor uw logische app wordt gestart.  Zoek naar **twitter** in het zoekvak voor triggers en selecteer dit.
6. Nu voert u een zoekterm in waarop moet worden geactiveerd.  De **frequentie** en het **interval** bepalen hoe vaak uw logische app op nieuwe tweets zal controleren (en tijdens die periode alle tweets zal retourneren).
    ![Zoeken in Twitter](media/logic-apps-create-a-logic-app/twittersearch.png)
7. Selecteer de knop **Nieuwe stap** en kies daarna **Een actie toevoegen** of **Een voorwaarde toevoegen**.
8. Wanneer u **Een actie toevoegen** selecteert, kunt vanuit de [beschikbare connectoren](../connectors/apis-list.md) zoeken om een actie te kiezen. U kunt bijvoorbeeld **Outlook.com - E-mail verzenden** selecteren om e-mail te verzenden vanuit een outlook.com-adres:  
    ![Acties](media/logic-apps-create-a-logic-app/actions.png)
9. U moet nu de gewenste parameters invullen voor de e-mail:  ![Parameters](media/logic-apps-create-a-logic-app/parameters.png)
10. Ten slotte selecteert u **Opslaan** om de logische app te publiceren.

## <a name="manage-your-logic-app-after-creation"></a>Uw logische app beheren na het maken
Nu is uw logische app actief. Deze zal regelmatig controleren op tweets met de ingevoerde zoekterm. Wanneer er een overeenkomstige tweet wordt gevonden, verzendt de logische app een e-mailbericht naar u. Ten slotte ziet u hoe u de app kunt uitschakelen of wat de voortgang hiervan is.

1. Ga naar de [Azure Portal](https://portal.azure.com).
2. Klik op **Bladeren** links in het scherm en selecteer **Logische apps**.
3. Klik op de nieuwe logische app die u net hebt gemaakt om de huidige status en algemene informatie te bekijken.
4. Als u uw nieuwe logische app wilt bewerken, klikt u op **Bewerken**.
5. Als u de app wilt uitschakelen, klikt u op **Uitschakelen** op de opdrachtbalk.
6. Bekijk de geschiedenisgegevens van uitvoeren en activeren om te controleren wanneer uw logische app wordt uitgevoerd.  U kunt op **Vernieuwen** klikken om de meest recente gegevens te bekijken.

U hebt nu in minder dan 5 minuten een eenvoudige logische app ingesteld die wordt uitgevoerd in de cloud. Zie [Functies van logische apps gebruiken] voor meer informatie over het gebruik van functies van Logic Apps. Zie [Logic App-definities maken](../logic-apps/logic-apps-author-definitions.md) voor meer informatie over de Logic App-definities zelf.

<!-- Shared links -->
[Azure portal]: https://portal.azure.com
[Functies van logische apps gebruiken]: logic-apps-create-a-logic-app.md



<!--HONumber=Jan17_HO4-->


