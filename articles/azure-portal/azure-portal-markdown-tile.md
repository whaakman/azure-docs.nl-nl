---
title: Gebruik een aangepaste markdown-tegel op Azure-dashboards
description: Meer informatie over het toevoegen van een markdown-tegel aan een Azure-dashboard om weer te geven van statische inhoud
services: azure-portal
keywords: ''
author: kfollis
ms.author: kfollis
ms.date: 01/25/2019
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 29c6dc0f7e71508e7b4ecb4e0f1d5c849df7a68c
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54904845"
---
# <a name="use-a-markdown-tile-on-azure-dashboards-to-show-custom-content"></a>Een tegel markdown gebruiken op Azure-dashboards om aangepaste inhoud weer te geven

U kunt een tegel markdown toevoegen aan uw Azure-dashboards om aangepaste, statische inhoud weer te geven. Bijvoorbeeld, kunt u eenvoudige instructies, een installatiekopie of een set hyperlinks met een tegel markdown weergeven.

## <a name="add-a-markdown-tile-to-your-dashboard"></a>Een markdown-tegel aan uw dashboard toevoegen

1. Selecteer **Dashboard** in de Azure portal zijbalk. Als u geen aangepaste dashboards in de dashboardweergave hebt gemaakt, gebruikt u de vervolgkeuzelijst te selecteren van het dashboard waarin de aangepaste markdown-tegel moet worden weergegeven. Selecteer het bewerkingspictogram openen de **Tegelgalerie**.

  ![Schermafbeelding waarin de weergave dashboard bewerken](./media/azure-portal-markdown-tile/azure-portal-dashboard-edit.png)

2. In de **Tegelgalerie**, zoek de tegel met de naam **Markdown** en klikt u op **toevoegen**. De tegel wordt toegevoegd aan het dashboard en de **Markdown bewerken** deelvenster wordt geopend.

1. Bewerk de **titel**, **subtitel**, en **inhoud** velden voor het aanpassen van de tegel. In het voorbeeld hieronder wordt is de tegel markdown bewerkt om aangepaste help helpdesk-informatie weer te geven.

  ![Schermafbeelding van de weergave voor het bewerken van tegel markdown](./media/azure-portal-markdown-tile/azure-portal-edit-markdown-tile.png)

4. Selecteer **gedaan** voor het verwijderen van de **Markdown bewerken** deelvenster. Uw inhoud wordt weergegeven op de tegel Markdown, die vervolgens kan worden gewijzigd door het schuifblokje in de rechterbenedenhoek.

  ![Schermopname die laat zien aangepaste markdown-tegel](./media/azure-portal-markdown-tile/azure-portal-custom-markdown-tile.png)

## <a name="markdown-content-capabilities-and-limitations"></a>Markdown-inhoud mogelijkheden en beperkingen

U kunt een willekeurige combinatie van tekst zonder opmaak, Markdown-syntaxis en HTML-inhoud op de tegel markdown gebruiken. De Azure portal maakt gebruik van een open-source-bibliotheek met de naam _gemarkeerd_ omzetten van uw inhoud in HTML-code die wordt weergegeven op de tegel. De HTML-code die wordt geproduceerd door _gemarkeerd_ vooraf is verwerkt door de portal voordat deze wordt weergegeven. Deze stap zorgt dat uw aanpassingen geen invloed op de beveiliging of de indeling van de portal. Tijdens de verwerking van die vooraf, wordt een deel van de HTML-code die een mogelijke bedreiging vormt verwijderd. De volgende typen inhoud zijn niet toegestaan door de portal:

* JavaScript- `<script>` tags en inline JavaScript-evaluaties, worden verwijderd.
* IFRAMES - `<iframe>` codes worden verwijderd.
* Style - `<style>` codes worden verwijderd. Stijlkenmerken inline HTML-elementen worden niet officieel ondersteund. U merkt mogelijk dat sommige inline style-elementen voor u werkt, maar als zij leiden tot met de indeling van de portal problemen, kunnen ze werken op elk gewenst moment te stoppen. De tegel Markdown is bedoeld voor basic-, statische inhoud die gebruikmaakt van de standaardstijlen van de portal.

## <a name="next-steps"></a>Volgende stappen

* Zie voor het maken van een aangepast dashboard [maken en delen van dashboards in de Azure-portal](../azure-portal/azure-portal-dashboards.md)
