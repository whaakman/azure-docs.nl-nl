---
title: Azure portal dashboards maken en delen | Microsoft Docs
description: Dit artikel wordt beschreven hoe u kunt maken, aanpassen, publiceren en delen van dashboards in de Azure-portal.
services: azure-portal
documentationcenter: ''
author: sewatson
manager: mtillman
editor: tysonn
ms.assetid: ff422f36-47d2-409b-8a19-02e24b03ffe7
ms.service: azure-portal
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 07/01/2019
ms.author: kfollis
ms.openlocfilehash: 8dd1349ca9ab62484eb6693291e3b869ff079dc1
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67537227"
---
# <a name="create-and-share-dashboards-in-the-azure-portal"></a>Dashboards in de Azure-portal maken en delen

Dashboards bieden een manier om te maken van een weergave gerichte en geordend in de Azure-portal van uw cloudresources. Dashboards gebruiken als een werkruimte waar u kunt snel starten-taken voor dagelijkse taken en resources bewaken.  Bouw aangepaste dashboards op basis van projecten, taken of gebruikersrollen, bijvoorbeeld.  De Azure portal biedt een standaarddashboard als uitgangspunt. U kunt het standaarddashboard bewerken, en meer dashboards, aanpassen en publiceren dashboards maken en delen zodat ze beschikbaar voor andere gebruikers. In dit artikel wordt beschreven hoe u een nieuw dashboard maken, de interface, aanpassen en publiceren en delen van dashboards.

## <a name="create-a-new-dashboard"></a>Een nieuw dashboard maken

In dit voorbeeld wordt een nieuwe, persoonlijk dashboard maken en wijs een naam. Volg deze stappen om aan de slag:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Selecteer **Dashboard** in het bovenste gedeelte van de linkerzijbalk. Uw standaardweergave kan al zijn ingesteld op het dashboard.
1. Selecteer **+ nieuw dashboard**.

    ![Schermopname van het standaarddashboard](./media/azure-portal-dashboards/dashboard-new.png)

4. Deze actie opent u de **Tegelgalerie**, van waaruit u selecteert tegels en een leeg raster waar u de tegels zult rangschikken.

    ![Schermopname van tegelgalerie en een leeg raster](./media/azure-portal-dashboards/dashboard-name.png)

5. Selecteer de **mijn Dashboard** tekst in het dashboard worden gelabeld en voer een naam in waarmee u eenvoudig bepalen het aangepaste dashboard.
1. Selecteer **aanpassen voltooid** modus bewerken in de paginakoptekst om af te sluiten.

De dashboardweergave ziet u nu uw leeg dashboard. Selecteer de vervolgkeuzelijst naast de naam van het dashboard om dashboards die voor u beschikbaar te geven – de lijst met dashboards die andere gebruikers hebt gemaakt en gedeeld kan bevatten.

## <a name="edit-a-dashboard"></a>Een dashboard bewerken

Nu gaan we bewerken van het dashboard dat u wilt toevoegen, vergroten of verkleinen en de tegels die staan voor uw Azure-resources te regelen.

### <a name="add-tiles"></a>Tegels toevoegen

Tegels toevoegen aan een dashboard, de volgende stappen uit:
1. Selecteer ![bewerkpictogram](./media/azure-portal-dashboards/dashboard-edit-icon.png) **bewerken** van koptekst van de pagina.

    ![Schermopname van het dashboard bewerken markeren](./media/azure-portal-dashboards/dashboard-edit.png)

2. Ga de **Tegelgalerie** of gebruik het zoekveld de tegel die u wilt zoeken.
1. Selecteer **toevoegen** de tegel automatisch worden toegevoegd aan het dashboard met een standaard-grootte en locatie. Of, sleept u de tegel op het raster en plaats deze waar u wilt.

Veel resource's (ook wel bekend als ' blades') bevatten een pictogram punaise in de opdrachtbalk. Als u het pictogram selecteert, wordt een tegel voor de bronpagina is vastgemaakt aan het dashboard dat momenteel actief is. Deze methode is een alternatieve manier om tegels toevoegen aan uw dashboard.

![Schermafbeelding van pagina opdrachtbalk met speldpictogram](./media/azure-portal-dashboards/dashboard-pin-blade.png)

> [!TIP]
> Als u met meer dan één organisatie werkt, voegt u toe de **organisatie identity** tegel aan uw dashboard om duidelijk welke organisatie de resources weer te geven tot behoren.
>
>
### <a name="resize-or-rearrange-tiles"></a>Formaat tegels of opnieuw rangschikken
De grootte van een tegel wijzigen of opnieuw rangschikken van de tegels op een dashboard, als volgt te werk:

1. Selecteer ![bewerkpictogram](./media/azure-portal-dashboards/dashboard-edit-icon.png) **bewerken** van koptekst van de pagina.
1. Selecteer het contextmenu in de rechterbovenhoek van een tegel. Kies vervolgens de grootte van een tegel. Tegels die ondersteuning bieden voor elke grootte bevatten ook een 'ingang' in de rechterbenedenhoek waarmee u sleept u de tegel naar de gewenste grootte.

   ![Schermopname van het dashboard met tegel grootte menu openen](./media/azure-portal-dashboards/dashboard-tile-resize.png)

3. Selecteer een tegel en sleep deze naar een nieuwe locatie in het raster rangschikken van uw dashboard.

### <a name="additional-tile-configuration"></a>Aanvullende tegel configuratie

Sommige tegels mogelijk meer configuratie om de gewenste informatie weer te geven. Bijvoorbeeld, de **grafiek met metrische gegevens** tegel moet worden ingesteld voor het weergeven van metrische gegevens van **Azure Monitor**. U kunt ook de gegevens van de tegel om te overschrijven de standaardinstellingen tijd van het dashboard aanpassen.

Een willekeurige tegel die moet worden ingesteld met geeft een **configureren tegel** banner totdat u de tegel aanpassen. Selecteer deze banner en voer vervolgens de vereiste instellingen.

![Schermafbeelding van de tegel die worden geconfigureerd moet](./media/azure-portal-dashboards/dashboard-configure-tile.png)

> [!NOTE]
> Een tegel markdown, kunt u aangepaste, statische inhoud weergeven op uw dashboard. Dit kan worden algemene instructies, een afbeelding, een set hyperlinks of neem contact op met informatie. Zie voor meer informatie over het gebruik van een tegel markdown [gebruiken een aangepaste markdown-tegel](azure-portal-markdown-tile.md).
>
>
### <a name="customize-tile-data"></a>Tegelgegevens aanpassen

Gegevens op het dashboard toont automatisch activiteit voor de afgelopen 24 uur. Als u wilt een andere tijdsduur voor alleen deze tegel weergeven, de volgende stappen uit:

1. Selecteer de ![filterpictogram](./media/azure-portal-dashboards/dashboard-filter.png) filterpictogram in de linkerbovenhoek van de tegel of selecteer **tegelgegevens aanpassen** in het contextmenu.

   ![Schermopname van het contextmenu van tegel](./media/azure-portal-dashboards/dashboard-customize-tile-data.png)

2. Het selectievakje voor het **overschrijf de dashboardinstellingen op het niveau van de tegel**.

   ![Schermopname van het dialoogvenster voor tijd tegelinstellingen configureren](./media/azure-portal-dashboards/dashboard-override-time-settings.png)

3. Kies de tijdsduur voor deze tegel worden weergegeven. U kunt kiezen uit de afgelopen 30 minuten voor de afgelopen 30 dagen of een aangepast datumbereik te definiëren.
1. Kies de tijdgranulatie om weer te geven. U kunt een willekeurige plaats in stappen van één minuut weergeven aan één maand.
1. Selecteer **Toepassen**.

## <a name="delete-a-tile"></a>Een tegel verwijderen

Als u wilt verwijderen een tegel uit een dashboard, de volgende stappen uit:

* Selecteer het contextmenu in de rechterbovenhoek van de tegel en selecteer vervolgens **uit dashboard verwijderen**. Of
* Selecteer ![bewerkpictogram](./media/azure-portal-dashboards/dashboard-edit-icon.png) **bewerken** om aanpassingen te activeren. Beweeg de muisaanwijzer in de rechterbovenhoek van de tegel en selecteer vervolgens de ![verwijderpictogram](./media/azure-portal-dashboards/dashboard-delete-icon.png) verwijderpictogram als u wilt verwijderen van de tegel vanuit het dashboard.

   ![Schermopname die laat zien hoe u een tegel uit dashboard verwijderen](./media/azure-portal-dashboards/dashboard-delete-tile.png)

## <a name="clone-a-dashboard"></a>Een dashboard klonen

Voor het gebruik van een bestaand dashboard als een sjabloon voor een nieuw dashboard, de volgende stappen uit:

1. Zorg ervoor dat de dashboardweergave wordt weergegeven het dashboard dat u wilt kopiëren.
1. Selecteer in de paginakoptekst ![kloon pictogram](./media/azure-portal-dashboards/dashboard-clone.png) **kloon**.
1. Een kopie van het dashboard met de naam ' klonen van *de dashboardnaam van uw*' wordt geopend in de modus bewerken. Gebruik de voorgaande stappen in dit artikel te wijzigen en aanpassen van het dashboard.

## <a name="publish-and-share-a-dashboard"></a>Publiceren en een dashboard delen

Wanneer u een dashboard maakt, is het standaard persoonlijk wat betekent dat u de enige bent die ze kunnen bekijken. Als u dashboards aan anderen, kunt u deze delen met andere gebruikers. U moet eerst publiceren van het dashboard als een Azure-resource. Als u wilt publiceren en delen van een aangepast dashboard, de volgende stappen uit:

1. Selecteer ![deelpictogram](./media/azure-portal-dashboards/dashboard-share-icon.png) **delen** van koptekst van de pagina. De **delen en toegangsbeheer** formulier wordt weergegeven.
1. Controleer of de naam van de juiste dashboard wordt weergegeven.
1. Selecteer een **abonnementsnaam**. Gebruikers met toegang tot het abonnement kunnen het gedeelde dashboard gebruiken. Toegang tot de resources die wordt vertegenwoordigd door de afzonderlijke tegels wordt bepaald door de op rollen gebaseerd toegangsbeheer in Azure.
1. Schakel het selectievakje in voor het publiceren van dit dashboard naar de resourcegroep 'dashboards' voor het geselecteerde abonnement. Of schakel het selectievakje uit en kies publiceren naar een bestaande resourcegroep in plaats daarvan.
1. Kies een locatie voor de resource van het dashboard. U wordt aangeraden dat u het dashboard met andere resources vinden. Opmerking: als u uit bestaande resourcegroepen kiezen, het dashboard bevindt automatisch in die resourcegroep.
1. Selecteer **Publiceren**.

   ![Schermopname van dialoogvenster voor het publiceren van dashboard](./media/azure-portal-dashboards/dashboard-publish.png)

### <a name="set-access-control-on-a-shared-dashboard"></a>Toegangsbeheer instellen op een gedeeld dashboard

Nadat het dashboard is gepubliceerd, kunt u beheren wie toegang heeft tot het dashboard door de volgende stappen:

1. In de **delen en toegangsbeheer** venster **gebruikers beheren**.

   ![Schermopname van het dashboard delen en beheren van dialoogvenster](./media/azure-portal-dashboards/dashboard-share-access-control.png)

2. De **Access Control** pagina wordt geopend. Op deze pagina kunt u het niveau van toegang voor iemand anders te controleren of een nieuwe roltoewijzing toevoegen. Wanneer u een roltoewijzing hier toevoegt, verleent u machtigingen aan het dashboard.

> [!NOTE]
> Tegels zijn representatieve weergaven van bronnen in uw organisatie. Toegang tot resources wordt beheerd door besturingselement toewijzen van rollen gebaseerde toegang en machtigingen worden overgenomen van het abonnement op de resource. Verlenen van toegang aan een dashboard, niet automatisch machtigingen toewijzen aan de resources die worden weergegeven op het dashboard. Zie voor meer informatie over machtigingen voor gedeelde dashboards en op rollen gebaseerd toegangsbeheer voor resources, [dashboards delen met op rollen gebaseerd toegangsbeheer](azure-portal-dashboard-share-access.md).

### <a name="open-a-shared-dashboard"></a>Een gedeeld dashboard openen

Als u wilt zoeken en openen van een gedeeld dashboard, de volgende stappen uit:

1. Selecteer de vervolgkeuzelijst naast de naam van het dashboard.
1. Selecteer een van de weergegeven lijst met dashboards of **alle dashboards doorzoeken** als het dashboard dat u wilt openen, niet wordt weergegeven.

   ![Schermafbeelding van de selectie in het menu](./media/azure-portal-dashboards/dashboard-browse.png)

3. In de **Type** veld **gedeelde dashboards**.
1. Selecteer een of meer abonnementen. U kunt ook tekst dashboards filteren op naam invoeren.
1. Selecteer een dashboard in de lijst met gedeelde dashboards.

## <a name="delete-a-dashboard"></a>Een dashboard verwijderen

Als u wilt een persoonlijke of gedeelde dashboard definitief verwijderen, de volgende stappen uit:

1. Selecteer het dashboard dat u wilt verwijderen uit de vervolgkeuzelijst naast de naam van het dashboard.
1. Selecteer ![verwijderpictogram](./media/azure-portal-dashboards/dashboard-delete-icon.png) **verwijderen** van koptekst van de pagina.
1. Selecteer voor een persoonlijke dashboard **OK** in het bevestigingsvenster op het dashboard verwijderen. Selecteer het selectievakje in om te bevestigen dat het gepubliceerde dashboard niet meer weergegeven door andere gebruikers worden kan voor een gedeeld dashboard, in het bevestigingsvenster. Selecteer vervolgens **OK**.

   ![Schermafbeelding van bevestiging verwijderen](./media/azure-portal-dashboards/dashboard-delete-dash.png)

## <a name="next-steps"></a>Volgende stappen

* [Dashboards delen met op rollen gebaseerd toegangsbeheer](azure-portal-dashboard-share-access.md)
* [Programmatische wijze Azure-dashboards maken](azure-portal-dashboards-create-programmatically.md)
