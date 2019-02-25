---
title: E-actie binnen het externe controle - Azure | Microsoft Docs
description: In deze gebruiksaanwijzing wordt beschreven hoe u een e-mailactie toevoegen aan een nieuwe of bestaande regel.
author: asdonald
manager: hegate
ms.author: asdonald
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/12/2018
ms.topic: conceptual
ms.openlocfilehash: 3e9d7191af3a870f60b40311be03e0ef9d9bf7ad
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2019
ms.locfileid: "56750205"
---
# <a name="add-an-email-action"></a>Een e-mailactie toevoegen

E-mailbericht acties zorgen ervoor dat u nooit waarschuwingen mist. U kunt een e-mailactie toevoegen aan een bestaande regel, of wanneer u een nieuwe regel maken.

Als u wilt de stappen in deze handleiding hebt voltooid, moet u een geïmplementeerd exemplaar van de oplossingsverbetering voor externe controle in uw Azure-abonnement.

Als u wilt maken of wijzigen van een regel, moet u een [ **beheerder**, of de juiste machtigingen hebt](iot-accelerators-remote-monitoring-rbac.md).

## <a name="edit-an-existing-rule"></a>Een bestaande rol bewerken

Volg deze stappen voor het toevoegen van een e-mailactie aan een bestaande regel:

1. Navigeer naar uw oplossing voor externe controle.

1. Uit de **Dashboard**, gaat u naar de **regels** pagina:

    ![Pagina regels](./media/iot-accelerators-remote-monitoring-email-actions/rules-email.png)

1. Klik op het selectievakje naast de bestaande regel wilt wijzigen en klik vervolgens op **bewerken** aan de bovenkant. Een bewerkbaar **regel** deelvenster wordt weergegeven.

1. In de **actie** sectie, in-/ uitschakelen **waarvoor E-mail is ingeschakeld** naar **op**.

1. De eerste keer dat u inschakelt dat een e-mailactie in de solution accelerator, moet u [aanmelden bij Outlook](#outlook).

1. Een e-mailadres invoeren in het geadresseerden in en druk op de **Enter** sleutel voor elke e-mailadres toe te voegen:

    ![Adres](./media/iot-accelerators-remote-monitoring-email-actions/address-email.png)

1. Voer een onderwerp in voor het e-mailbericht.

1. De aanvullende opmerkingen voor de e-mailontvangers invoeren als tekst zonder opmaak. U kunt HTML-indeling als u [bewerken van de e-mailsjabloon](#htmledit).

1. Zorg ervoor dat de **regelstatus** is ingesteld op **ingeschakeld**.

1. Klik op **Toepassen**.

## <a name="create-a-new-rule"></a>Een nieuwe regel maken

Volg deze stappen voor het toevoegen van een e-mailactie wanneer u een nieuwe regel maken:

1. Navigeer naar uw oplossing voor externe controle.

1. Uit de **Dashboard**, gaat u naar de **regels** pagina:

    ![Pagina regels](./media/iot-accelerators-remote-monitoring-email-actions/rules-email.png)

1. Volg de stappen in de [maakt u een sectie regel](iot-accelerators-remote-monitoring-automate.md#create-a-rule). Het volgen de stappen in de [een geavanceerde regel maken](iot-accelerators-remote-monitoring-automate.md#create-an-advanced-rule) sectie tot het punt waar u instellen een **ernstniveau**. Klik niet op **toepassen** nog.

1. In de **actie** sectie, in-/ uitschakelen **waarvoor E-mail is ingeschakeld** naar **op**.

1. De eerste keer dat u inschakelt dat een e-mailactie in de solution accelerator, moet u [aanmelden bij Outlook](#outlook).

1. Een e-mailadres invoeren in het geadresseerden in en druk op de **Enter** sleutel voor elke e-mailadres toe te voegen:

    ![Adres](./media/iot-accelerators-remote-monitoring-email-actions/address-email.png)

1. Voer een onderwerp in voor het e-mailbericht.

1. De aanvullende opmerkingen voor de e-mailontvangers invoeren als tekst zonder opmaak. U kunt HTML-indeling als u [bewerken van de e-mailsjabloon](#htmledit).

1. Zorg ervoor dat de **regelstatus** is ingesteld op **ingeschakeld**.

1. Klik op **Toepassen**.

De regel met een e-mailactie is nu ingeschakeld. Elke keer dat de actie wordt geactiveerd, wordt een nieuwe e-mail verzonden naar de ontvangers.

## Aanmelden bij Outlook <a name="outlook"></a>

De eerste keer dat u een e-mailactie in uw oplossingenaccelerator inschakelen, moet u zich aanmelden bij Outlook. Hiermee stelt u het e-mailaccount dat de e-mailmeldingen worden verzonden.

> [!NOTE]
> U moet een specifieke Outlook-account alleen voor de solution accelerator meldingen maken en dat account gebruikt als u uw eerste e-mailactie inschakelen.

### <a name="contributor-role-outlook-setup"></a>De installatie van de Outlook-rol Inzender

Als iemand zich in de **Inzender** rol in het abonnement de solution accelerator geïmplementeerd, de toepassing beschikt niet over voldoende machtigingen voor het instellen en controleren van e-mailacties via de Webgebruikersinterface.

Voordat u begint, moet u een Outlook-account te gebruiken voor het verzenden van e-mailmeldingen uit uw oplossingenaccelerator maken.

De volgende stappen laten zien hoe u instellen en de e-mailacties handmatig controleren:

1. Navigeer naar [Azure Portal](https://portal.azure.com).

1. Navigeer naar de resourcegroep voor uw oplossingenaccelerator.

1. Klik op de **Office 365-connector**:

    ![API-verbinding](./media/iot-accelerators-remote-monitoring-email-actions/apiconnector.png)

1. Klik op de banner om te beginnen met het autorisatieproces:

    ![autoriseren](./media/iot-accelerators-remote-monitoring-email-actions/connector.png)

1. Klik op **autoriseren**. U wordt gevraagd of u zich aanmeldt. Het account waarmee u zich aanmeldt moet de toepassing van het e-mailadres wordt gebruikt voor het verzenden van e-mailmeldingen:

    ![Knop autoriseren](./media/iot-accelerators-remote-monitoring-email-actions/authorize.png)

1. Klik op **opslaan** aan de onderkant. De autorisatie is voltooid als de banner verwijderd is.

1. Het e-mailadres waarvan de meldingen worden verzonden vanuit, klikt u op **bewerken API-verbinding**.

    ![e-mailadres wijzigen](./media/iot-accelerators-remote-monitoring-email-actions/editemail.png)

### <a name="owner-role-outlook-setup"></a>De installatie van de Outlook-rol eigenaar

Als iemand zich in de **eigenaar** rol in het abonnement de solution accelerator geïmplementeerd, de toepassing kunt controleren of het e-mailacties zijn correct ingesteld via de Webgebruikersinterface.

Voordat u begint, moet u een Outlook-account te gebruiken voor het verzenden van e-mailmeldingen uit uw oplossingenaccelerator maken.

De volgende stappen helpen u om te melden en e-mailacties instellen:

1. Klik om aan te melden bij Outlook. Er wordt naar de Azure-portal:

  ![Aanmelden bij Outlook](./media/iot-accelerators-remote-monitoring-email-actions/owneroutlook-email.png)

1. Klik op **autoriseren**. U wordt gevraagd of u zich aanmeldt. Het account waarmee u zich aanmeldt moet de toepassing van het e-mailadres wordt gebruikt voor het verzenden van e-mailmeldingen:

1. Klik op **Opslaan**. Ga terug naar uw oplossingenaccelerator en vernieuw de pagina.

1. Als u het e-mailbericht hebt geconfigureerd, kunt u dit bericht ziet:

  ![Geslaagde Outlook aanmelden](./media/iot-accelerators-remote-monitoring-email-actions/success-email.png)

## Stem de e-mail HTML <a name="htmledit"></a>

Out-of-the-box, de oplossingsverbetering voor externe controle biedt een eenvoudige HTML-sjabloon voor actie e-mailberichten. De e-mailsjabloon maakt gebruik van waarden van de actie-instellingen voor e-mailbericht. Hier volgt een voorbeeld van de e-mailbericht:

![Voorbeeld van e-mail](./media/iot-accelerators-remote-monitoring-email-actions/emailtemplate.png)

De volgende stappen laten zien hoe u de HTML-e-mailsjabloon te bewerken. U kunt bijvoorbeeld bevatten meer informatie of toevoegen van aangepaste installatiekopieën:

1. Kloon de Java of .NET Remote Monitoring-GitHub-opslagplaats:

1. Navigeer naar de locatie van de sjabloon e-mailadres:
  
    `Dotnet: device-telemetry\ActionsAgent\data\EmailTemplate.html`
  
    `Java device-telemetry/app/resources/data/EmailTemplate.html`

1. U kunt toevoegen of verwijderen van de parameters in deze sjabloon voor het aanpassen van het bericht. U kunt ook toevoegen, verwijderen of vervangen van aanroepen, zo nodig:

    Bijvoorbeeld, in de .NET-code:  `emailTemplate = emailTemplate.Replace("${subject}", emailAction.GetSubject());`

    Bijvoorbeeld, in de Java-code:  `this.emailTemplate.replace("${subject}", emailAction.GetSubject());`

1. Parameters in de sjabloon hebben de indeling `${...}`. Als u wilt verwijderen een parameter, de vereiste regel te verwijderen. Als u wilt toevoegen een parameter, toevoegen van een regel met de waarde om in te voegen.

1. Installatiekopieën of aangepaste tekst wilt toevoegen, moet u het bestand EmailTemplate.HTML rechtstreeks bijwerken.

## <a name="throttling"></a>Beperken

De oplossingsverbetering voor externe controle wordt Outlook gebruikt voor het verzenden van e-mailmeldingen. Outlook beperkt het aantal e-mailberichten verzonden naar [30 e-mailberichten per één minuut](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-limits#receiving-and-sending-limits). E-mailclients die de e-mailberichten ontvangt mogelijk ook het aantal e-mailberichten ontvangen per minuut vertraging. Neem contact op met uw specifieke e-mailclient op beperkingen. Bij het instellen van e-mailmelding voor een regel, wordt de regel berekend gemiddelde waarden gedurende een periode van ten minste één minuut en niet direct waarden gebruiken:

![Berekening van gemiddelde](./media/iot-accelerators-remote-monitoring-email-actions/calculation-email.png)

## <a name="next-steps"></a>Volgende stappen

Deze handleiding hebt u een e-mailactie toevoegen aan een nieuwe of bestaande regel in een oplossing voor externe controle. De handleiding ook bleek dat u en het bewerken van de HTML-code die de berichtindeling definieert.

De voorgestelde volgende stap is te leren [hoe u met behulp van waarschuwingen en oplossen van problemen met het apparaat](iot-accelerators-remote-monitoring-maintain.md).
