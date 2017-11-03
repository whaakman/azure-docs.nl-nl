---
title: Gebruik het hulpprogramma voor migratie van Power BI Embedded | Microsoft Docs
description: "Het hulpprogramma voor migratie van Power BI Embedded kan worden gebruikt om uw rapporten van de werkruimte-verzamelingen van Power BI te Power BI Embedded kopiëren."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/28/2017
ms.author: asaxton
ms.openlocfilehash: 0b7b5089045daf6dd88fcd84e316b2bd44f8c927
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-power-bi-embedded-migration-tool"></a>Gebruik het hulpprogramma voor migratie van Power BI Embedded

Het hulpprogramma voor migratie van Power BI Embedded kan worden gebruikt om uw rapporten van de werkruimte-verzamelingen van Power BI te Power BI Embedded kopiëren.

Migreren van de inhoud van uw werkruimte-verzamelingen naar de Power BI-service gelijktijdig naar uw huidige oplossing kan worden uitgevoerd en geen uitvaltijd vereist.

## <a name="limitations"></a>Beperkingen

* Pushed gegevenssets kunnen niet worden gedownload en moet opnieuw worden gemaakt met behulp van de Power BI REST-API's voor de Power BI-service.
* PBIX-bestanden geïmporteerd voordat 26 November 2016 niet gedownload worden kunnen.

## <a name="download"></a>Downloaden

U kunt downloaden via het voorbeeld van het hulpprogramma voor migratie van [GitHub](https://github.com/Microsoft/powerbi-migration-sample). U kunt ofwel een zip van de opslagplaats downloaden of u kunt dit lokaal klonen. Als u hebt gedownload, kunt u openen *Power BI-migratie-sample.sln* vanuit Visual Studio het bouwen en uitvoeren van het hulpprogramma voor migratie.

## <a name="migration-plans"></a>Plannen voor de migratie

U van plan bent zijn gewoon metagegevens die catalogus maken van de inhoud van Power BI werkruimte verzamelingen en hoe u wilt deze publiceren in Power BI Embedded.

### <a name="start-with-a-new-migration-plan"></a>Beginnen met een nieuw migratieplan

Een migratieplan is de metagegevens van de items die beschikbaar zijn in Power BI werkruimte verzamelingen die u vervolgens wilt overstappen naar Power BI Embedded. De migratieplan wordt opgeslagen als een XML-bestand.

U wilt beginnen met het maken van een nieuw migratieplan. Ga als volgt te werk voor het maken van een nieuw migratieplan.

1. Selecteer **bestand** > **nieuwe migratieplan**.

    ![Nieuwe migratieplan](media/migrate-tool/migrate-tool-plan.png)

2. In de **Selecteer Power BI werkruimte verzamelingen resourcegroep** dialoogvenster, wilt u selecteert de **omgeving** vervolgkeuzelijst en selecteer de prod.

3. U wordt gevraagd aan te melden. U gebruikt de aanmelding voor uw Azure-abonnement.

    > [!IMPORTANT]
    > Dit is **niet** uw Office 365-organisatie-account dat u bij Power BI met aanmelden.

4. Selecteer het Azure-abonnement die uw Power BI werkruimte verzamelingen resource opslaat.

    ![Selecteer uw Azure-abonnement](media/migrate-tool/migrate-tool-select-resource-group.png)

5. Selecteer onder de abonnementenlijst de **resourcegroep** dat uw werkruimte verzamelingen en selecteer bevat **Selecteer**.

    ![Selecteer de resourcegroep](media/migrate-tool/migrate-tool-select-resource-group2.png)

6. Selecteer **analyseren**. Hiermee verkrijgt u een inventarisatie van de items in uw Azure-abonnement zijn om te beginnen met uw abonnement.

    ![Selecteer de knop analyseren](media/migrate-tool/migrate-tool-analyze-group.png)

    > [!NOTE]
    > Het analyseren-proces kan enige tijd duren, afhankelijk van het aantal verzamelingen werkruimte en hoeveel inhoud bestaat in de werkruimteverzameling.

7. Wanneer **analyseren** is voltooid, wordt u gevraagd om op te slaan u van plan bent.

U hebt op dit moment u van plan bent verbonden met uw Azure-abonnement. Lees hieronder om te begrijpen van de stroom van het werken met u van plan bent. Dit omvat analyseren en migratie plannen, downloaden, groepen maken en uploaden.

### <a name="save-your-migration-plan"></a>U van plan bent opslaan

U kunt uw migratieplan voor later gebruik opslaan. Hiermee maakt u een XML-bestand dat u van plan bent van de informatie uit.

U van plan bent, slaat u het volgende te doen.

1. Selecteer **bestand** > **opslaan migratieplan**.

    ![Menuoptie voor migratie plan opslaan](media/migrate-tool/migrate-tool-save-plan.png)

2. Geef een naam op voor het bestand of gebruik de gegenereerde naam en selecteer **opslaan**.

### <a name="open-an-existing-migration-plan"></a>Open een bestaande migratieplan

U kunt een opgeslagen migratieplan om te blijven werken op uw migratie openen.

Als u wilt uw bestaande migratieplan openen, het volgende te doen.

1. Selecteer **bestand** > **Open bestaande migratieplan**.

    ![Open bestaande menuoptie voor migratie plannen](media/migrate-tool/migrate-tool-open-plan.png)

2. Selecteer uw migratiebestand en selecteer **Open**.

## <a name="step-1-analyze-and-plan-migration"></a>Stap 1: Analyseren en migratie plannen

De **analyseren & migratie plannen** tabblad hebt u een overzicht van wat momenteel in de resourcegroep van uw Azure-abonnement is.

![Analyseren en plannen migratie tabblad](media/migrate-tool/migrate-tool-step1.png)

Kijken we de *SampleResourceGroup* als voorbeeld.

### <a name="paas-topology"></a>PaaS-topologie

Dit is een aanbieding van uw *resourcegroep > werkruimte verzamelingen > werkruimten*. De resource-groep en werkruimte verzamelingen, wordt een beschrijvende naam weergegeven. De werkruimten ziet een GUID zijn.

De items in de lijst wordt ook een kleur en een nummer in de indeling van (#/ #) weergeven. Dit geeft het aantal rapporten dat kan worden gedownload.

Een kleur zwart betekent dat alle rapporten kunnen worden gedownload. Een rood betekent dat enkele rapporten kunnen niet worden gedownload. Het getal links geeft het totale aantal rapporten dat kan worden gedownload. Het nummer op het recht geeft het totale aantal rapporten binnen de groepering.

U kunt een item in de topologie PaaS om de rapporten in de sectie rapporten weer te geven.

### <a name="reports"></a>Rapporten

De sectie rapporten wordt een lijst van de rapporten die beschikbaar zijn en geeft aan of deze kan worden gedownload of niet.

![Lijst met rapporten in Power BI werkruimte verzamelingen](media/migrate-tool/migrate-tool-analyze-reports.png)

### <a name="target-structure"></a>Doelstructuur

De **doel structuur** is waar u het hulpprogramma zien waar dingen worden gedownload en hoe deze te uploaden.

#### <a name="download-plan"></a>Plan downloaden

Een pad wordt automatisch voor u gemaakt. U kunt dit pad wijzigen als u wenst. Als u het pad wijzigt, moet u om te selecteren **bijwerken paden**.

**Dit komt niet downloaden van het daadwerkelijk uitvoeren.** Dit is alleen opgeven van de structuur van waar de rapporten worden gedownload.

#### <a name="upload-plan"></a>Plan uploaden

Hier kunt u een voorvoegsel moet worden gebruikt voor de app-werkruimten die in de Power BI-service worden gemaakt. Nadat het voorvoegsel de GUID van de werkruimte die beschikbaar in Azure is waren.

![Geef het groepsvoorvoegsel](media/migrate-tool/migrate-tool-upload-plan.png)

**Hiermee worden de groepen in de Power BI-service daadwerkelijk niet gemaakt.** Hiermee definieert u alleen de naamgevingsconventie structuur voor de groepen.

Als u het voorvoegsel wijzigen, moet u selecteren **genereren uploaden plannen**.

U kunt Klik met de rechtermuisknop op een groep en kies de naam van de groep binnen het plan uploaden rechtstreeks wijzigen, indien gewenst.

![Wijzig de naam van Groepsoptie contextmenu](media/migrate-tool/migrate-tool-upload-report-rename-item.png)

> [!NOTE]
> De naam van de *groep* mag geen spaties of ongeldige tekens bevatten.

## <a name="step-2-download"></a>Stap 2: downloaden

Op de **downloaden** tabblad ziet u de lijst met rapporten en gekoppelde metagegevens. U kunt zien wat de status van de uitvoer is samen met de status van de vorige export.

![Tabblad downloaden](media/migrate-tool/migrate-tool-download-tab.png)

Hebt u twee opties.

* Selecteer specifieke rapporten en selecteer **downloaden geselecteerd**
* Selecteer **alle downloaden**.

![Geselecteerde knop downloaden](media/migrate-tool/migrate-tool-download-options.png)

Een downloaden voltooid, ziet u de status van *gedaan* en wordt weer dat de PBIX-bestand bestaat.

Nadat het downloaden is voltooid, selecteert u de **groepen maken** tabblad.

## <a name="step-3-create-groups"></a>Stap 3: groepen maken

Nadat u de rapporten die beschikbaar zijn hebt gedownload, gaat u naar de **groepen maken** tabblad. Op dit tabblad maakt de werkruimten app in de Power BI-service op basis van de migratieplan dat u hebt gemaakt. De app-werkruimte wordt gemaakt met de naam die u hebt opgegeven op de **uploaden** tabblad binnen **analyseren & migratie plannen**.

![Tabblad groepen maken](media/migrate-tool/migrate-tool-create-groups.png)

Voor het maken van de app-werkruimten kunt u ofwel **geselecteerd groepen maken** of **alle ontbrekende groepen maken**.

Wanneer u een van deze opties selecteert, wordt u gevraagd aan te melden. *U wilt gebruiken van uw referenties voor de Power BI-service die u maken van de app-werkruimten wilt op.*

![Power BI aanmelden scherm](media/migrate-tool/migrate-tool-create-group-sign-in.png)

Hiermee wordt de app-werkruimte in de Power BI-service gemaakt. Dit biedt de rapporten niet uploaden naar de app-werkruimte.

U kunt controleren of de app-werkruimte is gemaakt door aanmelden bij Power BI en valideren of de werkruimte bestaat. U ziet dat er is niets in de werkruimte is.

![App-werkruimte in Power BI-service](media/migrate-tool/migrate-tool-app-workspace.png)

Nadat de werkruimte is gemaakt, kunt u naar de **uploaden** tabblad.

## <a name="step-4-upload"></a>Stap 4: uploaden

Op de **uploaden** tabblad, dit wordt de rapporten uploaden naar de Power BI-service. U ziet een lijst met rapporten die we gedownload op het tabblad downloaden samen met de naam van het doel-groep op basis van uw migratieplan.

![Tabblad uploaden](media/migrate-tool/migrate-tool-upload-tab.png)

U kunt de geselecteerde rapporten uploaden of u alle rapporten kan uploaden. U kunt ook de uploadstatus opnieuw uploaden items herstellen.

U hebt ook de optie van het selecteren van wat te doen als een rapport met dezelfde naam bestaat. U kunt kiezen tussen **afbreken**, **negeren** en **overschrijven**.

![Optie vervolgkeuzelijst voor wat te doen als rapport bestaat](media/migrate-tool/migrate-tool-upload-report-same-name.png)

![Uploaden van de geselecteerde resultaten](media/migrate-tool/migrate-tool-upload-selected.png)

### <a name="duplicate-report-names"></a>Dubbele namen

Als u een rapport met dezelfde naam hebben, maar u weet dat deze een andere lijst, moet u wijzigen de **TargetName** van het rapport. U kunt de naam wijzigen door de migratieplan XML handmatig te bewerken.

U moet het hulpprogramma voor migratie van de wijziging aanbrengt en opnieuw opent u het hulpprogramma en de migratieplan sluiten.

In het bovenstaande voorbeeld is een van de gekloonde rapporten mislukt die wijzen op dat een rapport met dezelfde naam bestaat. Als we kijken naar de migratieplan XML gaat, zien we het volgende.

```
<ReportMigrationData>
    <PaaSWorkspaceCollectionName>SampleWorkspaceCollection</PaaSWorkspaceCollectionName>
    <PaaSWorkspaceId>4c04147b-d8fc-478b-8dcb-bcf687149823</PaaSWorkspaceId>
    <PaaSReportId>525a8328-b8cc-4f0d-b2cb-c3a9b4ba2efe</PaaSReportId>
    <PaaSReportLastImportTime>1/3/2017 2:10:19 PM</PaaSReportLastImportTime>
    <PaaSReportName>cloned</PaaSReportName>
    <IsPushDataset>false</IsPushDataset>
    <IsBoundToOldDataset>false</IsBoundToOldDataset>
    <PbixPath>C:\MigrationData\SampleResourceGroup\SampleWorkspaceCollection\4c04147b-d8fc-478b-8dcb-bcf687149823\cloned-525a8328-b8cc-4f0d-b2cb-c3a9b4ba2efe.pbix</PbixPath>
    <ExportState>Done</ExportState>
    <LastExportStatus>OK</LastExportStatus>
    <SaaSTargetGroupName>SampleMigrate</SaaSTargetGroupName>
    <SaaSTargetGroupId>6da6f072-0135-4e6c-bc92-0886d8aeb79d</SaaSTargetGroupId>
    <SaaSTargetReportName>cloned</SaaSTargetReportName>
    <SaaSImportState>Failed</SaaSImportState>
    <SaaSImportError>Report with the same name already exists</SaaSImportError>
</ReportMigrationData>
```

Voor het item is mislukt, kunnen we de naam van de SaaSTargetReportName wijzigen.

```
<SaaSTargetReportName>cloned2</SaaSTargetReportName>
```

We kunnen vervolgens opnieuw openen van het plan, in het hulpprogramma voor migratie en het rapport uploaden.

Ga terug naar Power BI, ziet u dat de rapporten en gegevenssets in de werkruimte app zijn geüpload.

![Lijst met rapporten in de Power BI-service](media/migrate-tool/migrate-tool-upload-app-workspace.png)

<a name="upload-local-file"></a>
### <a name="upload-a-local-pbix-file"></a>Een lokaal PBIX-bestand uploaden

U kunt een lokale versie van een Power BI Desktop-bestand uploaden. U moet het hulpprogramma sluiten, bewerken van het XML-bestand en de plaats van het volledige pad naar uw lokale PBIX in de **PbixPath** eigenschap.

```
<PbixPath>[Full Path to PBIX file]</PbixPath>
```

Nadat u het XML-bestand hebt bewerkt, open opnieuw het plan binnen het hulpprogramma voor migratie en het rapport uploaden.

<a name="directquery-reports"></a>
### <a name="directquery-reports"></a>DirectQuery-rapporten

U moet bijwerken voor het bijwerken van de verbindingsreeks voor de DirectQuery-rapporten. Dit kan worden uitgevoerd binnen *powerbi.com*, of u de verbindingsreeks van Power BI Embedded (Paas) programmatisch kunt opvragen. Zie voor een voorbeeld [verbindingsreeks DirectQuery extraheren uit PaaS-rapport](migrate-code-snippets.md#extract-directquery-connection-string-from-power-bi-workspace-collections).

U kunt de verbindingsreeks voor de gegevensset in de Power BI-service bijwerken en stel de referenties voor de gegevensbron. U kunt zoeken op de volgende voorbeelden om te zien hoe u dit doet.

* [Bijwerken van de verbindingsreeks DirectQuery in Power BI Embedded](migrate-code-snippets.md#update-directquery-connection-string-in-power-bi-embedded)
* [DirectQuery referenties instellen in Power BI Embedded](migrate-code-snippets.md#set-directquery-credentials-in-power-bi-embedded)

## <a name="next-steps"></a>Volgende stappen

Nu dat uw rapporten voor Power BI Embedded van Power BI werkruimte verzamelingen zijn gemigreerd, kunt u nu uw toepassing bijwerken en beginnen met de rapporten in deze werkruimte app insluiten.

Zie voor meer informatie [Power BI-Werkruimteverzameling inhoud migreren naar Power BI Embedded](migrate-from-power-bi-workspace-collections.md).

Nog vragen? [Probeer de Power BI-Community vragen](http://community.powerbi.com/)