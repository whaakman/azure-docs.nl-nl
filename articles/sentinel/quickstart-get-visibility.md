---
title: Azure Quick Start Sentinel - aan de slag met Azure Sentinel Preview | Microsoft Docs
description: Azure Quick Start Sentinel - aan de slag met Azure Sentinel
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 5a4ae93c-d648-41fb-8fb8-96a025d2f73e
ms.service: sentinel
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/4/2019
ms.author: rkarlin
ms.openlocfilehash: 1d54ac521fb8b81ed5fa135f68af420bda261661
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57246947"
---
# <a name="quickstart-get-started-with-azure-sentinel-preview"></a>Quickstart: Aan de slag met Azure Sentinel Preview

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.


In deze snelstartgids leert u hoe u snel om te kunnen geven en te controleren wat er gebeurt in uw omgeving met behulp van Azure Sentinel. Nadat u uw gegevensbronnen hebt verbonden met Azure Sentinel, krijgt u direct visualisatie en analyse van gegevens zodat u weet wat er gebeurt in uw verbonden gegevensbronnen. Azure Sentinel biedt u dashboards die u voorzien van de volledige kracht van hulpprogramma's die al beschikbaar in zowel Azure als tabellen en grafieken die zijn ingebouwd in zodat u kunt met analytics voor uw logboeken en query's. U kunt ingebouwde dashboards gebruiken of u kunt eenvoudig een nieuw dashboard maken van nieuwe of op basis van een bestaand dashboard. 

## <a name="get-visualization"></a>Visualisatie ophalen

Om te visualiseren en analyseren van wat in uw omgeving gebeurt er, eerst eens in het Overzichtsdashboard voor een beter beeld van de beveiligingsstatus van uw organisatie. U kunt klikken op elk element van deze tegels om te zoomen op de onbewerkte gegevens waarin ze zijn gemaakt. Om te helpen u ruis verlagen en Beperk het aantal waarschuwingen dat u hebt om te controleren en onderzoeken, Azure Sentinel een techniek fusion gebruikt om waarschuwingen in gevallen correleren. **Gevallen** zijn groepen van gerelateerde waarschuwingen die samen een bruikbare incident die u kunt onderzoeken en oplossen van maken.

- In de Azure-portal, selecteer Azure Sentinel en selecteer vervolgens de werkruimte die u wilt bewaken.

  ![Overzicht van Azure Sentinel](./media/qs-get-visibility/overview.png)

- De werkbalk aan de bovenzijde ziet u hoeveel gebeurtenissen die u hebt verkregen via de geselecteerde periode en het vergelijkt deze met de voorgaande 24 uur. De werkbalk vertelt u van deze gebeurtenissen en waarschuwingen die zijn geactiveerd (het kleine aantal vertegenwoordigt wijzigen in de afgelopen 24 uur), krijgt u voor deze gebeurtenissen, hoeveel zijn geopend, wordt uitgevoerd en gesloten. Controleer dat er een aanzienlijke toename niet zien of verwijderen in het aantal gebeurtenissen. Als er een afname, kan het zijn dat een verbinding naar Azure Sentinel rapportage gestopt. Als er een toename, mogelijk iets verdacht is. Controleer of er nieuwe waarschuwingen.

   ![Azure Sentinel trechter](./media/qs-get-visibility/funnel.png)

De hoofdtekst van de overzichtspagina biedt inzicht in één oogopslag in de beveiligingsstatus van uw werkruimte:

- **Gebeurtenissen en waarschuwingen gedurende een periode**: Geeft een lijst van het aantal gebeurtenissen en hoeveel waarschuwingen van de gebeurtenissen die zijn gemaakt. Als u een piek die ongebruikelijke, dat moet u waarschuwingen voor het - zien als er is ongewone activiteit waarbij er bevindt zich een piek in gebeurtenissen, maar er geen waarschuwingen ziet, is het mogelijk oorzaak zouden kunnen gaan maken.

- **Potentiële schadelijke gebeurtenissen**: Wanneer netwerkverkeer wordt gedetecteerd van bronnen die bekend is dat deze schadelijke, waarschuwt Azure Sentinel u op de kaart. Als u oranje ziet, is het binnenkomende verkeer: iemand probeert toegang tot uw organisatie van een bekende schadelijke IP-adres. Als u uitgaande (rood) activiteit ziet, betekent dit dat de gegevens van uw netwerk worden gestreamd buiten uw organisatie naar een bekende schadelijke IP-adres.

   ![Azure Sentinel-kaart](./media/qs-get-visibility/map.png)


- **Recente gevallen**: Om weer te geven uw recente cases, hun ernst en het aantal waarschuwingen die zijn gekoppeld aan de aanvraag. Als u als plotselinge piek in een specifiek type waarschuwing ziet, kan dit betekenen dat er een actieve aanval die momenteel actief is. Bijvoorbeeld, als u een plotselinge piek van 20 Pass-the-hash-gebeurtenissen uit Azure ATP hebt, is het mogelijk dat iemand op dit moment probeert u aanvallen.

- **Gegevensbron afwijkingen**: Gegevensanalisten Microsofts gemaakt modellen die continu zoeken naar de gegevens van uw gegevensbronnen op afwijkingen. Als er geen eventuele afwijkingen, wordt niets weergegeven. Als er worden afwijkingen gedetecteerd, moet u uitgebreide informatie over deze om te zien wat is er gebeurd. Bijvoorbeeld, klik op de piek in de Azure-activiteit. U kunt klikken op **grafiek** om te zien wanneer de piek is er gebeurd en filter vervolgens voor activiteiten die tijdens die periode om te zien wat de oorzaak van de piek.

   ![Azure Sentinel-kaart](./media/qs-get-visibility/anomolies.png)

## Ingebouwde dashboards gebruiken<a name="dashboards"></a>

Ingebouwde dashboards bevatten geïntegreerde gegevens van uw verbonden gegevensbronnen waarmee u uitgebreide Duik in de gebeurtenissen die worden gegenereerd in deze services. De ingebouwde dashboards bevatten-ID voor Azure, Azure activiteitsgebeurtenissen, en on-premises, die gegevens van Windows-gebeurtenissen van servers, vanuit het eerste partij waarschuwingen, van derden worden kunnen, waaronder logboeken over webverkeer firewall, Office 365 en onbeveiligde protocollen op basis van Windows gebeurtenissen.

1. Onder **instellingen**, selecteer **Dashboards**. Onder **geïnstalleerde**, ziet u alle geïnstalleerde dashboards. Onder **alle** ziet u de hele galerie met ingebouwde dashboards die beschikbaar voor installatie zijn. 
2. Zoeken naar een specifiek dashboard om te zien van de volledige lijst en de beschrijving van wat elke biedt. 
3. Ervan uitgaande dat u het gebruik van Azure AD, aan de slag kunt en uitvoeren met Azure Sentinel, wordt aangeraden dat u ten minste de volgende dashboards installeert:
   - **Azure AD**: Een of beide van de volgende gebruiken:
       - **Azure AD-aanmeldingen** analyseert aanmeldingen na verloop van tijd om te zien of er afwijkingen. Dit dashboard geeft mislukte aanmeldingen door toepassingen, apparaten en locaties, zodat u, in een oogopslag zien kunt als iets vreemds gebeurt. Let op meerdere mislukte aanmeldingen. 
       - **Azure AD-auditlogboeken** analyseert admin-activiteiten, zoals wijzigingen in gebruikers (toevoegen, verwijderen, enzovoort), het maken van beveiligingsgroepen en wijzigingen.  

   - Een dashboard voor uw firewall toevoegen. Bijvoorbeeld, de Palo Alto-dashboard toevoegen. Het dashboard kunt u uw firewall verkeer, zodat u correlaties tussen uw firewall-gebeurtenissen van gegevens en bedreigingen, analyseren en markeert u verdachte gebeurtenissen binnen verschillende entiteiten. Dashboards vindt u informatie over trends in uw verkeer en kunt u inzoomen op en resultaten te filteren. 

      ![PAL Alto dashboard](./media/qs-get-visibility/palo-alto-week-query.png)


U kunt de dashboards aanpassen door het bewerken van de belangrijkste query ![knop](./media/qs-get-visibility/edit-query-button.png). U kunt klikken op de knop ![knop](./media/qs-get-visibility/go-to-la-button.png) naar [Log Analytics voor het bewerken van de query er](../azure-monitor/log-query/get-started-portal.md), en selecteer het weglatingsteken (...) en selecteer **tegelgegevens aanpassen**, waarmee u kunt het belangrijkste tijdfilter bewerken of verwijderen van de specifieke tegels vanuit het dashboard.

Zie voor meer informatie over het werken met query's [zelfstudie: Visuele gegevens in Log Analytics](../azure-monitor/learn/tutorial-logs-dashboards.md)

### <a name="add-a-new-tile"></a>Een nieuwe tegel toevoegen

Als u een nieuwe tegel toevoegen wilt, kunt u deze toevoegen aan een bestaand dashboard, een die u maakt of een ingebouwde Azure Sentinel-dashboard. 
1. Maak in Log Analytics, een tegel met behulp van de instructies in [zelfstudie: Visuele gegevens in Log Analytics](../azure-monitor/learn/tutorial-logs-dashboards.md). 
2. Nadat de tegel is gemaakt, onder **pincode**, selecteer het dashboard waarin u wilt dat de tegel wordt weergegeven.

## <a name="create-new-dashboards"></a>Nieuwe dashboards maken
U kunt maken van een nieuw dashboard maken of een ingebouwde dashboard gebruiken als basis voor uw nieuwe dashboard.

1. Voor het maken van een nieuw dashboard helemaal selecteert **Dashboards** en vervolgens **+ nieuw dashboard**.
2. Selecteer het abonnement dat het dashboard is gemaakt in en geef deze een beschrijvende naam. Elk dashboard is een Azure-resource, zoals elk ander en u kunt deze rollen (RBAC) om te definiëren en beperken aan wie toegang heeft tot toewijzen. 
3. U hebt zodat het wordt weergegeven in uw dashboards zodat u visualisaties vastmaken aan om deze te delen. Klik op **Share** en vervolgens **gebruikers beheren**. 
 
1. Gebruik de **toegang controleren** en **roltoewijzingen** zoals u zou voor een andere Azure-resource doen. Zie voor meer informatie, [Share Azure-dashboards met behulp van RBAC](../azure-portal/azure-portal-dashboard-share-access.md).


## <a name="new-dashboard-examples"></a>Nieuwe dashboard-voorbeelden

De volgende voorbeeldquery kunt u trends van verkeer van weken met elkaar vergelijken. U kunt gemakkelijk overschakelen welke leverancier en de gegevensbron waarmee u de query uitvoeren op. In dit voorbeeld SecurityEvent van Windows gebruikt, kunt u functies op AzureActivity of CommonSecurityLog uitgevoerd op een andere firewall.

     |where DeviceVendor = = "Palo Alto Networks":
      // week over week query
      SecurityEvent
      | where TimeGenerated > ago(14d)
      | summarize count() by bin(TimeGenerated, 1d)
      | extend Week = iff(TimeGenerated>ago(7d), "This Week", "Last Week"), TimeGenerated = iff(TimeGenerated>ago(7d), TimeGenerated, TimeGenerated + 7d)


Het is raadzaam om een query waarin gegevens uit meerdere bronnen te maken. U kunt een query maken die kijkt naar Azure Active Directory-auditlogboeken voor nieuwe gebruikers die alleen zijn gemaakt en controles van uw Azure-Logboeken om te zien als de gebruiker gestart rol binnen 24 uur na het maken van wijzigingen aan toewijzingen van maken. Dat verdachte activiteiten op dit dashboard weergegeven zou:

    AuditLogs
    | where OperationName == "Add user"
    | project AddedTime = TimeGenerated, user = tostring(TargetResources[0].userPrincipalName)
    | join (AzureActivity
    | where OperationName == "Create role assignment"
    | project OperationName, RoleAssignmentTime = TimeGenerated, user = Caller) on user
    | project-away user1

U kunt verschillende dashboards op basis van de rol van de persoon kijken naar de gegevens en wat ze nodig hebt. U kunt bijvoorbeeld een dashboard maken voor de beheerder van uw netwerk met de gegevens van de firewall. U kunt ook dashboards die zijn gebaseerd op hoe vaak u bekijken, wilt of er dingen die u wilt bekijken dagelijks maken en andere items die u wilt eenmaal per uur controleren, bijvoorbeeld, kunt u uw Azure AD-aanmeldingen kijken met elk uur om te zoeken naar anomali ES. 


## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids hebt u geleerd hoe u aan de slag met Azure Sentinel. Ga verder met de zelfstudie voor [het vaststellen van bedreigingen](tutorial-detect-threats.md).
> [!div class="nextstepaction"]
> [Detectie van bedreigingen](tutorial-detect-threats.md) voor het automatiseren van uw antwoorden op bedreigingen.

