---
title: 'Azure Sentinel Snelstartgids: aan de slag met Azure Sentinel preview | Microsoft Docs'
description: 'Azure Sentinel Snelstartgids: aan de slag met Azure Sentinel'
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 5a4ae93c-d648-41fb-8fb8-96a025d2f73e
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/20/2019
ms.author: rkarlin
ms.openlocfilehash: c6434b4f5808c349d5e0ab04dafc5233f6ec63ac
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780469"
---
# <a name="quickstart-get-started-with-azure-sentinel-preview"></a>Quickstart: Aan de slag met Azure Sentinel preview

> [!IMPORTANT]
> Azure Sentinel is momenteel beschikbaar als open bare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.


In deze Quick Start leert u hoe u snel kunt zien wat er in uw omgeving gebeurt met Azure Sentinel. Nadat u uw gegevens bronnen aan Azure Sentinel hebt gekoppeld, krijgt u een snelle visualisatie en analyse van de gegevens, zodat u kunt zien wat er gebeurt in al uw verbonden gegevens bronnen. Azure Sentinel biedt u Dash boards waarmee u beschikt over de volledige kracht van hulpprogram ma's die al beschikbaar zijn in azure, evenals tabellen en grafieken die zijn ingebouwd om u te voorzien van analyses voor uw logboeken en query's. U kunt ingebouwde Dash boards gebruiken of een nieuw dash board eenvoudig maken, helemaal zelf of op basis van een bestaand dash board. 

## <a name="get-visualization"></a>Visualisatie ophalen

Bekijk eerst het overzichts dashboard om een idee te krijgen van de beveiligings postuur van uw organisatie voor het visualiseren en ophalen van analyses van wat er gebeurt in uw omgeving. U kunt op elk element van deze tegels klikken om in te zoomen op de onbewerkte gegevens van waaruit ze zijn gemaakt. Om het geluid te verminderen en het aantal waarschuwingen dat u moet beoordelen en te minimaliseren, gebruikt Azure Sentinel een fusie techniek om waarschuwingen in incidenten te correleren. **incidenten** zijn groepen gerelateerde waarschuwingen die samen een actie kunnen ondernemen die u kunt onderzoeken en oplossen.

- Selecteer in de Azure Portal Azure Sentinel en selecteer vervolgens de werk ruimte die u wilt bewaken.

  ![Overzicht van Azure-Sentinel](./media/qs-get-visibility/overview.png)

- De werk balk aan de bovenkant geeft aan hoeveel gebeurtenissen u tijdens de geselecteerde periode hebt geselecteerd en vergelijkt deze met de voor gaande 24 uur. Op de werk balk wordt u van deze gebeurtenissen verteld, de waarschuwingen die zijn geactiveerd (het kleine getal vertegenwoordigt een wijziging in de afgelopen 24 uur), en vervolgens wordt u op de hoogte gebracht van die gebeurtenissen, hoeveel openstaan, in uitvoering en gesloten. Controleer of het aantal gebeurtenissen geen aanzienlijke toename of drop heeft. Als er een drop is, kan het zijn dat een verbinding is gestopt met het melden van Azure Sentinel. Als er een verhoging is, is er mogelijk een verdacht probleem opgetreden. Controleer of er nieuwe waarschuwingen zijn.

   ![Azure Sentinel trechter](./media/qs-get-visibility/funnel.png)

De hoofd tekst van de overzichts pagina geeft inzicht in de beveiligings status van uw werk ruimte:

- **Gebeurtenissen en waarschuwingen in**de loop van de tijd: Hier wordt het aantal gebeurtenissen weer gegeven en hoeveel waarschuwingen zijn gemaakt op basis van deze gebeurtenissen. Als er sprake is van een ongebruikelijke piek, ziet u er waarschuwingen voor. als er iets ongebruikelijk is waarbij er sprake is van een Prikker, maar u geen waarschuwingen ziet, kan dit de oorzaak van het probleem zijn.

- **Mogelijke schadelijke gebeurtenissen**: Wanneer er verkeer wordt gedetecteerd van bronnen waarvan bekend is dat ze schadelijk zijn, wordt u door Azure Sentinel gewaarschuwd op de kaart. Als oranje wordt weer geven, is het inkomend verkeer: iemand probeert toegang te krijgen tot uw organisatie vanaf een bekend schadelijk IP-adres. Als de activiteit uitgaand (rood) wordt weer gegeven, betekent dit dat de gegevens van uw netwerk worden gestreamd uit uw organisatie naar een bekend schadelijk IP-adres.

   ![Azure-Sentinel-toewijzing](./media/qs-get-visibility/map.png)


- **Recente incidenten**: Om uw recente incidenten, hun ernst en het aantal waarschuwingen dat is gekoppeld aan het incident, weer te geven. Als u de onverwachte piek in een specifiek type waarschuwing ziet, kan dit betekenen dat er momenteel een actieve aanval actief is. Als u bijvoorbeeld een plotselinge piek van 20 Pass-the-hash-gebeurtenissen van Azure ATP hebt, is het mogelijk dat iemand een aanval uitvoert.

- **Afwijkingen van de gegevens bron**: De gegevens analisten van micro soft hebben modellen gemaakt die de gegevens van uw gegevens bronnen voortdurend doorzoeken op afwijkingen. Als er geen afwijkingen zijn, wordt er niets weer gegeven. Als er afwijkingen worden gedetecteerd, moet u zich diep vinden om te zien wat er is gebeurd. Klik bijvoorbeeld op de Prikker in azure activity. U kunt klikken op de **grafiek** om te zien wanneer de Prikker zich voordeed en vervolgens filteren op activiteiten die tijdens die periode zijn opgetreden om te zien wat de oorzaak van de Prikker heeft.

   ![Azure-Sentinel-toewijzing](./media/qs-get-visibility/anomolies.png)

## Ingebouwde Dash boards gebruiken<a name="dashboards"></a>

Ingebouwde Dash boards bieden geïntegreerde gegevens van uw verbonden gegevens bronnen, zodat u de gebeurtenissen die in deze services worden gegenereerd, kunt verdiepen. De ingebouwde Dash boards zijn onder andere Azure-ID, gebeurtenissen voor Azure-activiteiten en on-premises. dit kunnen gegevens zijn van Windows-gebeurtenissen van servers, van de eerste partij, van derden, waaronder firewall verkeers logboeken, Office 365 en onveilige protocollen op basis van Windows evenementen.

1. Selecteer onder **instellingen**de optie **Dash boards**. Onder **geïnstalleerd**ziet u alle geïnstalleerde Dash boards. Onder **Alles** ziet u de volledige galerie met ingebouwde Dash boards die beschikbaar zijn voor installatie. 
2. Zoek een specifiek dash board om de hele lijst en de beschrijving van de aanbiedingen te bekijken. 
3. Als u Azure AD gebruikt om aan de slag te gaan met Azure Sentinel, raden we u aan ten minste de volgende Dash boards te installeren:
   - **Azure AD**: Gebruik een van de volgende of beide:
       - Met **Azure AD-** aanmeldingen worden aanmeldingen in de loop van de tijd geanalyseerd om te zien of er afwijkingen zijn. Dit dash board biedt mislukte aanmeldingen per toepassing, apparaat en locatie, zodat u in één oogopslag kunt zien of er iets ongebruikelijk is. Let op meerdere mislukte aanmeldingen. 
       - Met **Azure AD-controle logboeken** worden beheer activiteiten geanalyseerd, zoals wijzigingen in gebruikers (toevoegen, verwijderen, enz.), het maken van groepen en het wijzigen van wijzigingen.  

   - Voeg een dash board toe voor uw firewall. Voeg bijvoorbeeld het Palo Alto-dash board toe. Het dash board analyseert het verkeer van uw firewall, met correlaties tussen uw firewall gegevens en bedreigings gebeurtenissen en verlicht verdachte gebeurtenissen tussen entiteiten. Dash boards biedt u informatie over trends in uw verkeer en kunt u inzoomen op en filteren op resultaten. 

      ![PAL Alto-dash board](./media/qs-get-visibility/palo-alto-week-query.png)


U kunt de Dash boards aanpassen door de knop ![](./media/qs-get-visibility/edit-query-button.png)hoofd query te bewerken. U kunt klikken op de ![knop](./media/qs-get-visibility/go-to-la-button.png) om naar [log Analytics te gaan om de query daar te bewerken](../azure-monitor/log-query/get-started-portal.md), en u kunt het weglatings teken (...) selecteren en **tegel gegevens aanpassen**selecteren, zodat u het filter voor de hoofdtijden kan bewerken of verwijderen. specifieke tegels van het dash board.

Zie [zelf studie voor meer informatie over het werken met query's: Visuele gegevens in Log Analytics](../azure-monitor/learn/tutorial-logs-dashboards.md)

### <a name="add-a-new-tile"></a>Een nieuwe tegel toevoegen

Als u een nieuwe tegel wilt toevoegen, kunt u deze toevoegen aan een bestaand dash board, dat u hebt gemaakt of een Azure Sentinel ingebouwd dash board. 
1. Maak in log Analytics een tegel met behulp van de instructies [in de zelf studie: Visuele gegevens in Log Analytics](../azure-monitor/learn/tutorial-logs-dashboards.md). 
2. Nadat de tegel is gemaakt, selecteert u onder **pincode**het dash board waarin u de tegel wilt weer geven.

## <a name="create-new-dashboards"></a>Nieuwe Dash boards maken
U kunt een volledig nieuw dash board maken of een ingebouwd dash board gebruiken als basis voor uw nieuwe dash board.

1. Als u een nieuw dash board wilt maken, selecteert u **Dash boards** en vervolgens **+ Nieuw dash board**.
2. Selecteer het abonnement waarin het dash board is gemaakt en geef het een beschrijvende naam. Elk dash board is een Azure-resource, zoals andere, en u kunt IT-rollen (RBAC) toewijzen om te bepalen wie toegang heeft. 
3. Als u wilt dat deze wordt weer gegeven in uw Dash boards om visualisaties vast te maken, moet u deze delen. Klik op **delen** en vervolgens op **gebruikers beheren**. 
 
1. Gebruik de **optie toegang** en **roltoewijzingen** controleren, net zoals u zou doen voor andere Azure-resources. Zie [Azure-Dash boards delen](../azure-portal/azure-portal-dashboard-share-access.md)met behulp van RBAC voor meer informatie.


## <a name="new-dashboard-examples"></a>Nieuwe voor beelden van Dash boards

Met de volgende voorbeeld query kunt u trends van verkeer tussen weken vergelijken. U kunt eenvoudig overschakelen naar de leverancier en gegevens bron van het apparaat waarop u de query uitvoert. In dit voor beeld maakt gebruik van SecurityEvent van Windows. u kunt deze switch zo wijzigen dat deze wordt uitgevoerd op AzureActivity of CommonSecurityLog op elke andere firewall.

     |where DeviceVendor = = "Palo Alto Networks":
      // week over week query
      SecurityEvent
      | where TimeGenerated > ago(14d)
      | summarize count() by bin(TimeGenerated, 1d)
      | extend Week = iff(TimeGenerated>ago(7d), "This Week", "Last Week"), TimeGenerated = iff(TimeGenerated>ago(7d), TimeGenerated, TimeGenerated + 7d)


Mogelijk wilt u een query maken die gegevens uit meerdere bronnen bevat. U kunt een query maken om Azure Active Directory audit logboeken te bekijken voor nieuwe gebruikers die zojuist zijn gemaakt. vervolgens controleert u de logboeken van uw Azure om na te gaan of de gebruiker is begonnen met het wijzigen van de roltoewijzing binnen 24 uur na het maken van de toewijzing. Deze verdachte activiteit zou op dit dash board worden weer gegeven:

    AuditLogs
    | where OperationName == "Add user"
    | project AddedTime = TimeGenerated, user = tostring(TargetResources[0].userPrincipalName)
    | join (AzureActivity
    | where OperationName == "Create role assignment"
    | project OperationName, RoleAssignmentTime = TimeGenerated, user = Caller) on user
    | project-away user1

U kunt verschillende Dash boards maken op basis van de rol van persoon die de gegevens bekijkt en wat ze zoeken. U kunt bijvoorbeeld een dash board maken voor uw netwerk beheerder die de firewall gegevens bevat. U kunt ook Dash boards maken op basis van hoe vaak u deze wilt bekijken, of u nu dagelijks wilt controleren en andere items die u eenmaal per uur wilt controleren. u kunt bijvoorbeeld elk uur uw Azure AD-aanmeldingen bekijken om te zoeken naar Anomali s. 

## <a name="create-new-detections"></a>Nieuwe detecties maken

Genereer detecties op de [gegevens bronnen die u hebt verbonden met Azure Sentinel](connect-data-sources.md) om bedreigingen in uw organisatie te onderzoeken.

Wanneer u een nieuwe detectie maakt, moet u gebruikmaken van de ingebouwde detecties die zijn gemaakt door micro soft-beveiligings onderzoekers die zijn afgestemd op de gegevens bronnen die u hebt verbonden.

1. Ga [in de GitHub-Community](https://github.com/Azure/Azure-Sentinel/tree/master/Detections) naar de map **detecties** en selecteer de relevante mappen.
   ![relevante mappen](./media/qs-get-visibility/detection-folders.png)
 
3.  Ga naar het tabblad **Analytics** en selecteer **toevoegen**.
   ![regel maken in Log Analytics](./media/qs-get-visibility/query-params.png)

3.  Kopieer alle para meters naar de regel en klik op **maken**.
   ![waarschuwings regel maken](./media/qs-get-visibility/create-alert-rule.png)

 
## <a name="next-steps"></a>Volgende stappen
In deze Snelstartgids hebt u geleerd hoe u aan de slag kunt met Azure Sentinel. Ga door naar de zelf studie voor [het detecteren van bedreigingen](tutorial-detect-threats.md).
> [!div class="nextstepaction"]
> [Spoor bedreigingen](tutorial-detect-threats.md) op om uw reacties op bedreigingen te automatiseren.

