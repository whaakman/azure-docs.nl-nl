---
title: Aanbevolen procedures voor ondernemingen verplaatsen naar Azure | Microsoft Docs
description: Beschrijft een scaffold die ondernemingen zodat een veilige en beheerbare omgeving kunnen gebruiken.
services: azure-resource-manager
documentationcenter: na
author: rdendtler
manager: timlt
editor: tysonn
ms.assetid: 8692f37e-4d33-4100-b472-a8da37ce628f
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/31/2017
ms.author: rodend;karlku;tomfitz
ms.openlocfilehash: 3b5087faaf3db087b15b77fedac8df0d7e4a899a
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="azure-enterprise-scaffold---prescriptive-subscription-governance"></a>Azure enterprise scaffold - prescriptieve abonnement governance
Ondernemingen zijn steeds meer gebruik nemen van de openbare cloud voor de wendbaarheid en flexibiliteit. Ze met behulp van de sterkte van de cloud inkomsten te genereren of resources voor het bedrijf optimaliseren. Microsoft Azure biedt een groot aantal services dat ondernemingen zoals bouwstenen voor het oplossen van een breed scala aan werkbelastingen en toepassingen kunnen samenstellen. 

Maar weet waar u begint, is vaak moeilijk. Na bepaalt het gebruik van Azure, worden enkele vragen vaak veroorzaakt:

* 'Hoe ik voldoen aan onze wettelijke vereisten voor onafhankelijkheid van de gegevens in bepaalde landen?'
* 'Hoe kan ik ervoor zorgen dat iemand niet per ongeluk een kritieke systeem wijzigt?'
* 'Hoe weet ik elke bron die wordt ondersteund op zodat ik kan account voor het bestand en factuur nauwkeurig weer?'

Het uitvoeren van een leeg abonnement met geen rails guard is uitdaging. Deze spatie kan uw overstap naar Azure belemmeren.

In dit artikel biedt een startpunt voor technische professionals te helpen de behoefte voor toezicht en met de noodzaak van flexibiliteit worden verdeeld. Dit introduceert het concept van een enterprise-scaffold die organisaties helpt in de implementatie en beheer van Azure-abonnementen. 

## <a name="need-for-governance"></a>De noodzaak voor toezicht
Wanneer u naar Azure verplaatst, moet u het onderwerp van toezicht vroeg om te controleren of de succesvol gebruik van de cloud binnen de onderneming houden. Helaas de tijd en bureaucracy van het maken van een systeem uitgebreide governance betekent dat sommige bedrijfsgroepen gaat u rechtstreeks naar leveranciers zonder tussenkomst van bedrijven. Deze aanpak kunt laten de onderneming openen voor beveiligingsproblemen als de bronnen niet correct worden beheerd. De kenmerken van de openbare cloud - flexibiliteit, flexibiliteit en prijzen op basis van verbruik - zijn belangrijk voor bedrijfsonderdelen die moeten snel voldoen aan de eisen van klanten (intern en extern). Maar enterprise IT moet ervoor zorgen dat gegevens en systemen effectief worden beveiligd.

In de praktijk steigers gebruikt voor het maken van de basis van de structuur. De scaffold algemeen overzicht begeleidt en biedt ankerpunten voor meer permanente systemen moet worden gekoppeld. Een enterprise-scaffold is hetzelfde: een reeks flexibele besturingselementen en mogelijkheden van Azure die structuur in de omgeving en ankers voor services is gebaseerd op de openbare cloud bieden. Biedt de opbouwfuncties (IT en bedrijfsgroepen) een basis voor het maken en koppelen van nieuwe services.

De scaffold is gebaseerd op procedures die we hebben die afkomstig zijn uit veel betrokkenheid met clients van verschillende grootte. Deze clients kunnen variëren van kleine organisaties ontwikkelen van oplossingen in de cloud Fortune 500 ondernemingen en onafhankelijke softwareleveranciers die zijn gemigreerd en ontwikkelen van oplossingen in de cloud. De enterprise-scaffold is 'speciaal hiervoor gemaakte' flexibel ondersteuning bieden voor zowel traditionele IT-werkbelastingen en flexibele werkbelastingen; zoals ontwikkelaars maken van software-as-a-service (SaaS)-toepassingen gebaseerd op de mogelijkheden van Azure.

De enterprise-scaffold is bedoeld als de basis van elk nieuw abonnement in Azure. Deze kan beheerders om ervoor te zorgen werkbelastingen voldoen aan de minimale beheervereisten van een organisatie zonder zo wordt voorkomen dat bedrijfsgroepen en ontwikkelaars snel voldoen aan hun eigen doelstellingen.

> [!IMPORTANT]
> Toezicht is van cruciaal belang voor het succes van Azure. In dit artikel is bedoeld voor de technische implementatie van een enterprise-scaffold maar alleen raakt op de breder proces en relaties tussen de onderdelen. Beleid voor beheer loopt van boven naar beneden en wordt bepaald door het bedrijf wil bereiken. Natuurlijk, het maken van een model governance voor Azure bevat vertegenwoordigers van IT, maar belangrijker sterk weergave van de groep leidinggevenden en gegevensbeveiliging en risicobeheer beheer heeft. In de end is een enterprise-scaffold over beperkende bedrijfsrisico om de missie en de doelstellingen van een organisatie te vergemakkelijken.
> 
> 

De volgende afbeelding worden de onderdelen van de scaffold beschreven. De foundation afhankelijk van een solide plan voor afdelingen, accounts en -abonnementen. De stijlen bestaan uit het Resource Manager-beleid en sterke naamgevingsstandaarden. De rest van de scaffold afkomstig is van de belangrijkste mogelijkheden van Azure en functies die een veilige en beheerbare omgeving.

![scaffold onderdelen](./media/resource-manager-subscription-governance/components.png)

> [!NOTE]
> Azure is snel toegenomen sinds de introductie in 2008. Deze groei vereist Microsoft engineeringteams na te denken over hun benadering voor het beheren en implementeren van services. Het Azure Resource Manager-model is in 2014 geïntroduceerd en vervangt het klassieke implementatiemodel. Resource Manager kunnen organisaties eenvoudiger implementeren, organiseert en Azure-resources te beheren. Resource Manager bevat garandeert bij het maken van resources voor een snellere implementatie van complexe, onderling afhankelijk oplossingen. Dit omvat ook gedetailleerde toegangsbeheer en de mogelijkheid tot bronnen van de tag met metagegevens. Microsoft raadt u alle resources via de Resource Manager-model te maken. De enterprise-scaffold is expliciet ontworpen voor het Resource Manager-model.
> 
> 

## <a name="define-your-hierarchy"></a>Uw hiërarchie definiëren
De basis van de scaffold is de Azure Enterprise-inschrijving (en de Enterprise Portal). De enterprise-inschrijving definieert de vorm en gebruik van Azure-services binnen een bedrijf en zijn de bestuursstructuur core. In de enterprise agreement zich klanten meer onderverdelen in de omgeving aan de afdelingen, accounts en ten slotte abonnementen. Een Azure-abonnement is de basiseenheid waarbij alle resources zijn opgenomen. Het definieert ook diverse limieten in Azure, zoals het aantal kernen, bronnen, enzovoort.

![hiërarchie](./media/resource-manager-subscription-governance/agreement.png)

Elke enterprise verschilt en de hiërarchie in de vorige afbeelding kunt u aanzienlijke flexibiliteit voor hoe Azure is ingedeeld binnen het bedrijf. Voordat u de instructies in dit document bevat implementeert, moet u model van uw hiërarchie en wat de gevolgen van facturering, toegang tot bedrijfsbronnen en complexiteit.

De drie algemene patronen voor Azure inschrijvingen zijn:

* De **functionele** patroon
  
    ![functionele](./media/resource-manager-subscription-governance/functional.png)
* De **bedrijfseenheid** patroon 
  
    ![bedrijf](./media/resource-manager-subscription-governance/business.png)
* De **geografische** patroon
  
    ![geografische](./media/resource-manager-subscription-governance/geographic.png)

U toepassen de scaffold op het abonnementsniveau uit te breiden de governance vereisten van de onderneming in het abonnement.

## <a name="naming-standards"></a>Naamgevingsstandaarden
De eerste pillar van de scaffold is naamgevingsvereisten. Goed ontworpen naamgevingsstandaarden kunnen u resources in de portal op een factuur en in scripts voor het identificeren. Zeer waarschijnlijk al naamgevingsvereisten voor on-premises infrastructuur. Wanneer u Azure toevoegt aan uw omgeving, moet u deze naamgevingsstandaarden uitbreiden naar uw Azure-resources. Standaard Naming waardoor efficiënter beheer van de omgeving op alle niveaus.

> [!TIP]
> Voor naamgevingsregels:
> * Bekijk en vaststellen waar mogelijk de [Patterns and practice richtlijnen](../guidance/guidance-naming-conventions.md). In deze richtlijnen kunt u op een zinvolle naamgevingsnorm bepalen.
> * Gebruik camelCasing voor namen van bronnen (zoals myResourceGroup en vnetNetworkName). Opmerking: Er zijn bepaalde bronnen, zoals de storage-accounts, waarbij de enige optie is het gebruik van kleine letters (en geen speciale tekens).
> * Overweeg het gebruik van Azure Resource Manager-beleid (beschreven in de volgende sectie) af te dwingen naamgevingsstandaarden.
> 
> De voorgaande tips helpen u bij het invoeren van een consistente naamgeving.

## <a name="policies-and-auditing"></a>Beleid en controle
De tweede pillar van de scaffold omvat het maken van [Azure beleid](../azure-policy/azure-policy-introduction.md) en [controle van het activiteitenlogboek](resource-group-audit.md). Resource Manager-beleid biedt u de mogelijkheid voor het beheren van risico's in Azure. U kunt beleidsregels die onafhankelijkheid van de gegevens te garanderen door te beperken, afdwingen of controle van bepaalde acties definiëren. 

* Beleid is een standaard **toestaan** system. U kunt acties beheren door te definiëren en beleidsregels toe te wijzen aan resources die weigeren of acties op de bronnen wilt controleren.
* Beleidsregels worden beschreven door de beleidsdefinities in een beleid definition language (als dan voorwaarden).
* U maakt beleidsregels met JSON (Javascript Object Notation) ingedeelde bestanden. Na het definiëren van een beleid voor u deze toewijzen aan een bepaald bereik: abonnement, resourcegroep of resource.

Beleidsregels hebben meerdere acties waarmee een fijnmazig benadering van uw scenario's. De acties zijn:

* **Weigeren**: de resource-aanvraag wordt geblokkeerd
* **Audit**: kan de aanvraag maar voegt u een regel toe aan het activiteitenlogboek (die kan worden gebruikt om waarschuwingen of voor het activeren van runbooks)
* **Toevoeg-**: voegt de opgegeven informatie toe aan de resource. Bijvoorbeeld, als er geen een label 'CostCenter' van een resource, dat label met een standaardwaarde toevoegen.

### <a name="common-uses-of-resource-manager-policies"></a>Veelvoorkomende toepassingen van Resource Manager-beleid
Azure Resource Manager-beleidsregels zijn een krachtig hulpprogramma in de Azure-toolkit. Hiermee kunt u om te voorkomen dat onverwachte kosten, een kostenplaats voor resources via tagging identificeren en om ervoor te zorgen dat compliancy vereisten wordt voldaan. Wanneer beleidsregels worden gecombineerd met de ingebouwde functies voor controle, kunt u complexe en flexibele oplossingen fashion. Beleid kunnen bedrijven die besturingselementen voor werkbelastingen 'Traditionele IT' en 'Agile' werkbelastingen; zoals ontwikkelen van toepassingen van de klant. De meest voorkomende die we voor de beleidsregels zien zijn:

* **Geo-compatibiliteitsgegevens onafhankelijkheid** -Azure regio's biedt over de hele wereld. Ondernemingen willen vaak bepalen waar resources worden gemaakt (ongeacht of dit om ervoor te zorgen onafhankelijkheid van de gegevens of zorg ervoor dat resources dicht bij de consumenten einde van de bronnen worden gemaakt).
* **Kostenbeheer** -een Azure-abonnement kan de resources van veel typen en schaal bevatten. Ondernemingen willen vaak om ervoor te zorgen dat standaard abonnementen Vermijd het gebruik van onnodig groot resources die het kost honderden bedragen een maand of langer.
* **Standaard governance via vereiste tags** -vereisen van labels is een van de meest voorkomende en maximaal gewenste functies. Met behulp van Azure Resource Manager-beleid kunnen ondernemingen om ervoor te zorgen dat een resource op de juiste wijze is gecodeerd. De meest voorkomende labels, zijn: afdeling, Resource-eigenaar en omgeving type (bijvoorbeeld - productie, testen en ontwikkeling)

**Voorbeelden**

"Traditionele IT ' abonnement voor line-of-business-toepassingen

* Afdeling en de eigenaar van tags op alle bronnen afdwingen
* Resource maken voor de regio Noord-Amerika beperken
* Beperkt de mogelijkheid om virtuele machines G-serie en HDInsight-Clusters maken

'Flexibele' omgeving voor een zakelijke eenheid cloudtoepassingen maken

* Toestaan dat het maken van resources om te voldoen aan vereisten voor onafhankelijkheid van gegevens, alleen in een specifieke regio.
* Omgevingstag voor alle resources worden afgedwongen. Toevoegen als een bron wordt gemaakt zonder een label, de **omgeving: onbekende** label op de resource.
* Audit wanneer bronnen buiten Noord-Amerika worden gemaakt, maar niet.
* Audit wanneer hoge kosten voor resources worden gemaakt.

> [!TIP]
> Het meest voorkomende gebruik van Resource Manager-beleid tussen verschillende organisaties is het besturingselement *waar* resources kunnen worden gemaakt en *wat* typen resources kunnen worden gemaakt. Naast het bieden van besturingselementen op *waar* en *wat*, vele ondernemingen beleid gebruiken om ervoor te zorgen resources moeten u de juiste metagegevens factureren voor verbruik. U wordt aangeraden toepassen van beleid op abonnementsniveau voor:
> 
> * Geo-compatibiliteitsgegevens onafhankelijkheid
> * Kostenbeheer
> * Vereiste tags (bepaald door zakelijke behoeften, zoals BillTo, de eigenaar van de toepassing)
> 
> U kunt extra beleidsregels op lagere niveaus van bereik toepassen.
> 
> 

### <a name="audit---what-happened"></a>Audit - wat is er gebeurd?
Om weer te geven hoe u uw omgeving werkt, moet u controleren gebruikersactiviteit. De meeste brontypen in Azure maken diagnostische logboeken die u via een hulpprogramma log of in Azure Operations Management Suite kunt analyseren. U kunt activiteitenlogboeken verzamelen meerdere abonnementen op bieden een afdelingen of enterprise-weergave. Controlerecords zijn zowel een belangrijk hulpprogramma voor Netwerkcontrole en een cruciaal mechanisme voor het activeren van gebeurtenissen in de Azure-omgeving.

Activiteitenlogboeken van implementaties van Resource Manager kunnen u bepalen de **operations** die vond plaats en door wie deze uitgevoerd. Activiteitenlogboeken kunnen worden verzameld en worden geaggregeerd met de hulpprogramma's zoals logboekanalyse.

## <a name="resource-tags"></a>Resourcetags
Als gebruikers in uw organisatie een resource aan het abonnement toevoegt, wordt het steeds belangrijker om resources koppelen aan de juiste afdeling, de klant en de omgeving. U kunt metagegevens koppelen aan resources via [labels](resource-group-using-tags.md). U codes gebruiken om informatie over de bron of de eigenaar te geven. Labels kunnen u niet alleen aggregeren en resources op verschillende manieren groeperen, maar die gegevens gebruiken voor de doeleinden van doorberekening. U kunt labelen resources met maximaal 15: sleutelwaarde-paren. 

Resourcetags is flexibel en moeten worden gekoppeld aan de meeste bronnen. Voorbeelden van algemene resourcelabels zijn:

* BillTo
* Afdeling (of bedrijfseenheid)
* Omgeving (ontwikkeling productie, fase)
* Laag (weblaag, toepassingslaag)
* De eigenaar van de toepassing
* ProjectName

![tags](./media/resource-manager-subscription-governance/resource-group-tagging.png)

Zie voor meer voorbeelden van labels [aanbevolen naamgevingsregels voor Azure-resources](../guidance/guidance-naming-conventions.md).

> [!TIP]
> Overweeg een beleid dat labelen bepaalt voor:
> 
> * Resourcegroepen
> * Storage
> * Virtuele machines
> * Service-omgevingen of web-toepassingsservers
> 
> Deze labels strategie identificeert voor uw abonnementen welke metagegevens nodig is voor de bedrijven, financiën, beveiliging, risicobeheer en algemeen beheer van de omgeving. 

## <a name="resource-group"></a>Resourcegroep
Resource Manager kunt u resources in zinvolle groepen voor beheer, facturerings- of natuurlijke affiniteit plaatsen. Zoals eerder vermeld, wordt Azure heeft twee implementatiemodellen. In de eerdere klassieke model is de basiseenheid voor beheer van het abonnement. Was het moeilijk om op te splitsen resources binnen een abonnement, wat geleid tot het maken van een groot aantal abonnementen heeft. Met het Resource Manager-model hebt gezien de introductie van resourcegroepen. Resourcegroepen zijn containers voor resources die een gemeenschappelijk levenscyclus hebben of delen van een kenmerk zoals 'alle SQL servers' of 'Application A'.

Resourcegroepen kunnen niet worden opgenomen in elkaar en resources kunnen alleen deel uitmaken van een resourcegroep. U kunt bepaalde acties toepassen op alle bronnen in een resourcegroep. Bijvoorbeeld, verwijdert een resourcegroep alle bronnen binnen de resourcegroep. Normaal gesproken schakelt u een volledige toepassings- of verwante in dezelfde resourcegroep. Een toepassing in drie lagen Contoso-webtoepassing aangeroepen zou bijvoorbeeld bevatten de webserver, de toepassingsserver en de SQL server in dezelfde resourcegroep.

> [!TIP]
> Hoe het ordenen van uw resourcegroepen kan afwijken van werkbelastingen 'Traditionele IT' aan 'Flexibele IT' werkbelastingen:
> 
> * "Traditionele IT ' werkbelastingen meestal rapportitems binnen dezelfde levenscyclus, zoals een toepassing worden gegroepeerd. Groepering door toepassing kunt u afzonderlijke Toepassingsbeheer.
> * ' Flexibele IT ' werkbelastingen richt veelal op externe klantgerichte cloud-toepassingen. De resourcegroepen moeten overeenkomen met de lagen van implementatie (zoals weblaag, App-laag) en beheer.
> 
> Inzicht in uw werkbelasting, kunt u een resource group-strategie ontwikkelen.

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer
Waarschijnlijk vraagt u uzelf "wie moet toegang hebben tot bronnen?" en 'hoe ik deze toegang beheren?' Toestaan of verbieden toegang tot de Azure-portal en beheren van toegang tot bronnen in de portal is van cruciaal belang. 

Wanneer Azure in eerste instantie is uitgebracht, toegangsbeheer om een abonnement te basic zijn: Administrator of CO-beheerder. Toegang tot een abonnement in het klassieke model geïmpliceerde toegang tot alle bronnen in de portal. Dit gebrek aan nauwkeuriger beheer leidde tot de komst van abonnementen tot een redelijke toegangsbeheer bieden voor een Azure-inschrijving.

De verspreiding van abonnementen is niet meer nodig. Met op rollen gebaseerde toegangsbeheer, kunt u gebruikers toewijzen aan standaardrollen (zoals 'Lezer' en 'schrijver' voorkomende rollen). U kunt ook aangepaste rollen definiëren.

> [!TIP]
> Voor het implementeren van toegangsbeheer op basis van rollen:
> * Verbind uw bedrijfsidentiteit store (meestal Active Directory) met Azure Active Directory met het hulpprogramma AD Connect.
> * De beheerder/Co-beheerder van een abonnement met behulp van een beheerde identiteit beheren. **Geen** beheerder/Co-beheerder toewijzen aan de eigenaar van een nieuw abonnement. In plaats daarvan RBAC-rollen te gebruiken om te bieden **eigenaar** rechten aan een groep of persoon.
> * Azure-gebruikers toevoegen aan een groep (bijvoorbeeld X Toepassingseigenaars) in Active Directory. De gesynchroniseerde groep gebruiken voor leden van de juiste rechten heeft voor het beheren van de resourcegroep met de toepassing.
> * Volg het principe van het verlenen van de **minimale bevoegdheden** vereist voor de verwachte werk. Bijvoorbeeld:
>   * Implementatie-groep: Een groep die zich alleen voor het implementeren van resources.
>   * Virtual Machine Management: Een groep die kunnen virtuele machines starten (voor bewerkingen)
> 
> Deze tips helpen u bij het beheren van toegang voor gebruikers in uw abonnement.

## <a name="azure-resource-locks"></a>Azure-resource-vergrendelingen
Als uw organisatie wordt basisservices toegevoegd aan het abonnement, wordt het steeds belangrijker om ervoor te zorgen dat deze services beschikbaar om te voorkomen dat de zakelijke onderbroken zijn. [Resource vergrendelingen](resource-group-lock-resources.md) kunt u beperken van bewerkingen op waardevolle bronnen waar wijzigt of verwijdert deze zou aanzienlijke gevolgen voor uw toepassingen of cloudinfrastructuur hebben. U kunt vergrendelingen toepassen op een abonnement, resourcegroep of resource. U kunt gewoonlijk vergrendelingen toepassen op fundamentele resources, zoals virtuele netwerken, gateways en storage-accounts. 

Resource vergrendelingen momenteel ondersteuning voor twee waarden: CanNotDelete en alleen-lezen. CanNotDelete betekent dat gebruikers (met de juiste rechten) kunnen wel lezen of wijzigen van een bron, maar kan niet worden verwijderd. Alleen-lezen betekent dat gemachtigde gebruikers niet verwijderen of wijzigen van een resource.

Als u wilt maken of verwijderen van management vergrendelingen, u moet toegang hebben tot `Microsoft.Authorization/*` of `Microsoft.Authorization/locks/*` acties.
Van de ingebouwde rollen worden alleen de eigenaar en de beheerder voor gebruikerstoegang die acties verleend.

> [!TIP]
> Belangrijkste netwerkopties moeten worden beveiligd met vergrendelingen. Per ongeluk verwijderen van een gateway, site-naar-site VPN zijn rampzalig zijn voor een Azure-abonnement. Azure is niet toegestaan voor het verwijderen van een virtueel netwerk dat wordt gebruikt, maar meer beperkingen toepassen voorzorgsmaatregel handig is. 
> 
> * Virtueel netwerk: CanNotDelete
> * Netwerkbeveiligingsgroep: CanNotDelete
> * Beleid: CanNotDelete
> 
> Beleidsregels zijn ook essentieel voor het onderhoud van de juiste besturingselementen. Het is raadzaam dat u van toepassing een **CanNotDelete** vergrendelen aan de beleidsregels die worden gebruikt.

## <a name="core-networking-resources"></a>Netwerkresources Core
Toegang tot bronnen kan worden (binnen het bedrijfsnetwerk) interne of externe (via het internet). Het is gemakkelijk voor gebruikers in uw organisatie per ongeluk resources in de verkeerde positie plaatsen en blootstellen aan schadelijke toegang. Net als bij de on-premises apparaten, moeten de ondernemingen juiste besturingselementen voor het Azure-gebruikers moet ervoor zorgen dat de juiste keuzes toevoegen. We identificeren belangrijkste resources die elementaire controle van toegang bieden voor abonnement-toezicht. De belangrijkste resources bestaan uit:

* **Virtuele netwerken** containerobjecten voor subnetten zijn. Hoewel niet strikt noodzakelijk is het vaak gebruikt bij het verbinden van toepassingen tot interne bedrijfsbronnen.
* **Netwerkbeveiligingsgroepen** zijn vergelijkbaar met een firewall en regels bieden voor hoe een resource "" via het netwerk praten kan. Ze bieden gedetailleerde controle over hoe / als een subnet (of de virtuele machine) verbinding met Internet of andere subnetten in hetzelfde virtuele netwerk maken kunt.

![Core-netwerken](./media/resource-manager-subscription-governance/core-network.png)

> [!TIP]
> Voor netwerken:
> * Virtuele netwerken die zijn toegewezen aan een extern gerichte werklast en werkbelastingen van interne maken. Deze aanpak vermindert de kans op onbedoeld plaatsen van virtuele machines die zijn bedoeld voor interne werkbelastingen in een extern gerichte ruimte.
> * Configureer netwerkbeveiligingsgroepen om toegang te beperken. Ten minste toegang tot internet blokkeren vanaf interne virtuele netwerken en blokkeer de toegang tot het bedrijfsnetwerk van de externe virtuele netwerken.
> 
> De volgende tips helpen u beveiligde netwerkresources implementeren.

### <a name="automation"></a>Automatisering
Resources afzonderlijk te beheren is tijdrovend en mogelijk foutgevoelig voor bepaalde bewerkingen. Azure biedt verschillende mogelijkheden tot automatisering met Azure Automation, Logic Apps en Azure Functions. [Azure Automation](../automation/automation-intro.md) kunnen beheerders maken en definiëren van runbooks voor het afhandelen van algemene taken bij het beheren van resources. U kunt runbooks maken met behulp van een PowerShell-code-editor of een grafische editor. U kunt meerdere fasen werkstromen maken. Azure Automation wordt vaak gebruikt voor het afhandelen van algemene taken, zoals het afsluiten van niet-gebruikte resources of het maken van bronnen in reactie op een specifieke trigger zonder menselijke tussenkomst.

> [!TIP]
> Voor automatisering:
> * Een Azure Automation-account maken en bekijk de beschikbare runbooks (grafische en command line) beschikbaar in de [Runbookgalerie](../automation/automation-runbook-gallery.md).
> * Importeren en aanpassen van de belangrijkste runbooks voor uw eigen gebruik.
> 
> Een gebruikelijk scenario is de mogelijkheid om te starten/afsluiten virtuele machines op een planning. Er zijn een voorbeeld van de runbooks die beschikbaar zijn in de galerie die zowel voor het verwerken van dit scenario en leert u hoe u uit te vouwen.
> 
> 

## <a name="azure-security-center"></a>Azure Security Center
Mogelijk is een van de grootste blockers naar cloud acceptatie de problemen die over beveiliging. IT-risicomanagers en beveiliging afdelingen moeten ervoor zorgen dat de bronnen in Azure beveiligd zijn. 

De [Azure Security Center](../security-center/security-center-intro.md) biedt een centrale weergave van de beveiligingsstatus van resources in de abonnementen en aanbevelingen die helpen te voorkomen dat resources waarmee is geknoeid. Ze kan meer gedetailleerd beleid (bijvoorbeeld toepassen beleid aan specifieke resourcegroepen waarmee bedrijven hun houding het risico dat ze adresseert aanpassen) inschakelen. Ten slotte is Azure Security Center een open platform waarmee Microsoft-partners en onafhankelijke softwareleveranciers te maken van de software die in Azure Security Center voor het verbeteren van de mogelijkheden ervan wordt geplaatst. 

> [!TIP]
> Azure Security Center is standaard ingeschakeld in elk abonnement. Echter, moet u het verzamelen van virtuele machines in Azure Security Center voor het installeren van de agent en beginnen met het verzamelen van gegevens toestaan inschakelen.
> 
> ![Verzamelen van gegevens](./media/resource-manager-subscription-governance/data-collection.png)
> 
> 

## <a name="next-steps"></a>Volgende stappen
* U hebt geleerd over abonnement governance, is het tijd om te zien van deze aanbevelingen in de praktijk. Zie [voorbeelden van de implementatie van Azure-abonnement governance](resource-manager-subscription-examples.md).

