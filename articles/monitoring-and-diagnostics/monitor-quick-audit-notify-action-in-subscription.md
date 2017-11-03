---
title: Controleren en meldingen ontvangen over belangrijke acties in uw Azure-abonnement | Microsoft Docs
description: Inzicht in de geschiedenis van bronbeheer servicestatus en andere activiteiten abonnement in het gebeurtenissenlogboek en vervolgens een waarschuwing activiteitenlogboek met een e-mailbericht ontvangen wanneer er een bewerking maximaal bevoegdheden wordt uitgevoerd in uw abonnement.
author: johnkemnetz
manager: orenr
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.topic: quickstart
ms.date: 09/25/2017
ms.author: johnkem
ms.custom: mvc
ms.openlocfilehash: 636dc0fcae1bc2647cd59add5957884971015ce2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="audit-and-receive-notifications-about-important-actions-in-your-azure-subscription"></a>Controleren en meldingen ontvangen over belangrijke acties in uw Azure-abonnement

De **Azure Activity Log** biedt een geschiedenis van gebeurtenissen in de Azure-abonnement. Biedt informatie over *die* gemaakt, bijgewerkt of verwijderd *wat* bronnen en *wanneer* ze dit hebben gedaan. Kunt u een **activiteitenlogboek waarschuwing** voor het ontvangen van e-mail, SMS of webhook meldingen wanneer een activiteit die optreedt voldoen aan uw waarschuwing voorwaarden. Met deze stappen Quick Start via een eenvoudige netwerkbeveiligingsgroep maken, bladeren door het activiteitenlogboek voor inzicht in de gebeurtenis die is opgetreden en vervolgens het ontwerpen van een waarschuwing activiteitenlogboek wilt worden gewaarschuwd als netwerkbeveiligingsgroep gaat forwards wordt gemaakt.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-network-security-group"></a>Een netwerkbeveiligingsgroep maken

1. Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.

2. Selecteer **Networking**, selecteer **netwerkbeveiligingsgroep**.

3. Voer 'myNetworkSG' als de **naam** en maak een nieuwe resourcegroep met de naam **myResourceGroup**. Klik op de knop **Maken**.

    ![Een netwerkbeveiligingsgroep maken in de portal](./media/monitor-quick-audit-notify-action-in-subscription/create-network-security-group.png)

## <a name="browse-the-activity-log-in-the-portal"></a>Het activiteitenlogboek in de portal bladeren

Een gebeurtenis is nu toegevoegd aan het activiteitenlogboek die het maken van de netwerkbeveiligingsgroep beschrijft. Gebruik de volgende instructies om te identificeren die gebeurtenis.

1. Klik op de **Monitor** knop gevonden op de linkernavigatiebalk-lijst. Deze wordt geopend aan de sectie activiteitenlogboek. Deze sectie bevat een overzicht van alle acties die gebruikers hebben uitgevoerd op resources in uw abonnement, Filterbaar door een aantal eigenschappen, zoals de **resourcegroep**, **Timespan**, en  **Categorie**.

2. In de **activiteitenlogboek** sectie, klikt u op de **resourcegroep** vervolgkeuzelijst en selecteer **myResourceGroup**. Wijzig de **Timespan** dropdown naar **laatste 1 uur**. Klik op **Toepassen**.

    ![Het activiteitenlogboek filteren](./media/monitor-quick-audit-notify-action-in-subscription/browse-activity-log.png)

3. Klik op de **NetworkSecurityGroups schrijven** gebeurtenis in de tabel van gebeurtenissen die worden weergegeven.

## <a name="browse-an-event-in-the-activity-log"></a>Een gebeurtenis in het gebeurtenissenlogboek bladeren

De sectie die wordt weergegeven, bevat algemene informatie van de bewerking die werd uitgevoerd, met inbegrip van de naam, de tijdstempel, en de gebruiker of toepassing die wordt uitgevoerd.

![Gebeurtenis in het gebeurtenissenlogboek samenvatting weergeven](./media/monitor-quick-audit-notify-action-in-subscription/activity-log-summary.png)

Klik op de **JSON** tabblad om de volledige gebeurtenisdetails te bekijken. Dit omvat de details van hoe de gebruiker of toepassing is gemachtigd om uit te voeren van de bewerking, de gebeurteniscategorie en niveau en de status van de bewerking.

![De Gebeurtenisdetails weergeven in het gebeurtenissenlogboek](./media/monitor-quick-audit-notify-action-in-subscription/activity-log-json.png)

## <a name="create-an-activity-log-alert"></a>Een waarschuwing activiteitenlogboek maken

1. Klik op de **samenvatting** om terug te keren naar de samenvatting van de gebeurtenis.

2. Klik in de samenvatting sectie op **toevoegen activiteit logboek waarschuwing**.

    ![Een netwerkbeveiligingsgroep maken in de portal](./media/monitor-quick-audit-notify-action-in-subscription/activity-log-summary.png)

3. In de sectie die wordt weergegeven, geeft u de waarschuwing activiteitenlogboek een naam en beschrijving.

4. Onder **Criteria** ervoor te zorgen dat **gebeurteniscategorie** is ingesteld op **Administrative**, **brontype** is ingesteld op **netwerk beveiligingsgroepen**, **bewerkingsnaam** is ingesteld op **maken of bijwerken Netwerkbeveiligingsgroep**, **Status** is ingesteld op  **Geslaagd** en alle andere velden zijn leeg of is ingesteld op **alle**. De criteria definiëren voor de regels die wordt gebruikt om te bepalen of de waarschuwing moet worden geactiveerd wanneer een nieuwe gebeurtenis wordt weergegeven in het gebeurtenissenlogboek.

    ![Een netwerkbeveiligingsgroep maken in de portal](./media/monitor-quick-audit-notify-action-in-subscription/activity-log-alert-criteria.png)

5. Onder **waarschuwing via** Selecteer **nieuw** actie groeperen en bieden een **naam** en **afkorting** voor de groep in te grijpen. De actiegroep definieert de set met acties die worden uitgevoerd wanneer de waarschuwing is geactiveerd (als de criteria die overeenkomen met een nieuwe gebeurtenis).

6. Onder **acties** 1 of meer acties toevoegen die door een **naam** voor de actie de **actietype** (bijvoorbeeld e-mailbericht of SMS), en **Details**voor dat type bepaalde actie (bijvoorbeeld een webhook-URL, e-mailadres of SMS-nummer).

    ![Een netwerkbeveiligingsgroep maken in de portal](./media/monitor-quick-audit-notify-action-in-subscription/activity-log-alert-actions.png)

7. Klik op **Ok** de waarschuwing activiteitenlogboek opslaan.

## <a name="test-the-activity-log-alert"></a>De waarschuwing activiteitenlogboek testen

> [!NOTE]
> Het duurt ongeveer 10 minuten voor een waarschuwing activiteitenlogboek volledig wordt ingeschakeld. Nieuwe gebeurtenissen die plaatsvinden voordat de waarschuwing activiteitenlogboek is volledig ingeschakeld geen meldingen genereren.
>
>

Herhaal de voorgaande sectie voor het testen van de waarschuwing, **maken van een netwerkbeveiligingsgroep**, maar deze netwerkbeveiligingsgroep een andere naam geven en de bestaande resourcegroep gebruiken. Binnen een paar minuten ontvangt u een melding dat de netwerkbeveiligingsgroep is gemaakt.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, verwijder de resourcegroep en de netwerkbeveiligingsgroep. Om dit te doen, typ de naam van de resourcegroep die u hebt gemaakt in het zoekvak boven aan de portal en klik op de naam van de resourcegroep. Klik in de sectie die wordt weergegeven op de **resourcegroep verwijderen** knop, typ de naam van de resourcegroep en klik op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze snel starten, een bewerking voor het genereren van een gebeurtenis activiteitenlogboek uitgevoerd en wordt gemaakt van een waarschuwing activiteitenlogboek wilt worden gewaarschuwd als deze bewerking in de toekomst opnieuw optreedt. U vervolgens de waarschuwing getest door het uitvoeren van deze bewerking opnieuw uit. Azure maakt beschikbare activiteitenlogboek van gebeurtenissen van de afgelopen 90 dagen. Als u meer dan 90 dagen gebeurtenissen worden bewaard moet, probeert u het archiveren van uw gegevens activiteitenlogboek naast de andere bewakingsgegevens.

> [!div class="nextstepaction"]
> [Bewakingsgegevens archiveren](./monitor-tutorial-archive-monitoring-data.md)
