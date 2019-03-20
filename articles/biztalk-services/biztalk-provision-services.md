---
title: Azure BizTalk Services maken in Azure Portal | Microsoft Docs
description: Meer informatie over het inrichten of maken van Azure BizTalk Services in de Azure Portal; MABS, WABS
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: 3ad18876-a649-40d6-9aa0-1509c1d62c43
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 05327c05594d38caf5e3d54f8a13eaaaac3588ec
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58097437"
---
# <a name="create-biztalk-services-using-the-azure-portal"></a>BizTalk Services maken met de Azure Portal

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]
> 
> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]
> 
> [!TIP]
> Als u zich wilt aanmelden bij de Azure Portal, moet u een Azure-account en een Azure-abonnement hebben. Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](https://go.microsoft.com/fwlink/p/?LinkID=239738).


## <a name="CreateService"></a>Een BizTalk Service maken

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

Afhankelijk van de status van de BizTalk Service zijn er bepaalde bewerkingen die niet kunnen worden voltooid. Ga naar de [statusgrafiek van BizTalk Services](biztalk-service-state-chart.md) voor een lijst van deze bewerkingen.

## <a name="post-provisioning-steps"></a>Stappen na de inrichting
* [Het certificaat installeren op een lokale computer](#InstallCert)
* [Een certificaat dat gereed is voor productie toevoegen](#AddCert)
* [De Access Control-naamruimte ophalen](#ACS)

#### <a name="InstallCert"></a>Het certificaat installeren op een lokale computer

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

#### <a name="AddCert"></a>Een certificaat dat gereed is voor productie toevoegen

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

#### <a name="ACS"></a>De Access Control-naamruimte ophalen

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

Wanneer u een BizTalk Service-project implementeert vanuit Visual Studio, voert u deze Access Control-naamruimte in. De Access Control-naamruimte wordt automatisch gemaakt voor uw BizTalk Service.

De Access Control-waarden kunnen worden gebruikt met elke gewenste toepassing. Wanneer u Azure BizTalk Services maakt, bepaalt deze Access Control-naamruimte de verificatie met uw BizTalk Service-implementatie. Als u het abonnement wilt wijzigen of de naamruimte wilt beheren, selecteert u **ACTIVE DIRECTORY** in het navigatiedeelvenster links. Selecteer vervolgens de naamruimte. In de taakbalk ziet u de opties.

Als u op **Beheren** klikt, wordt de Access Control-beheerportal geopend. In de Access Control-beheerportal gebruikt de BizTalk Service **service-identiteiten**:  
![De Access Control Service-identiteiten in de Access Control-beheerportal][ACSServiceIdentities]

De Access Control Service-identiteit is een set referenties waarmee toepassingen of clients rechtstreeks via Access Control kunnen verifiëren en een token kunnen ontvangen.

> [!IMPORTANT]
> De BizTalk Service gebruikt **Eigenaar** als standaardservice-identiteit en de waarde **Wachtwoord**. Als u de waarde Symmetrische sleutel gebruikt in plaats van de waarde Wachtwoord, kan de volgende fout optreden.<br/><br/>*Kan geen verbinding maken met het Access Control-beheerserviceaccount met de opgegeven referenties*
> 
> 

[Uw ACS-naamruimte beheren](https://msdn.microsoft.com/library/azure/hh674478.aspx) geeft een lijst weer met de volgende richtlijnen en aanbevelingen.

## <a name="requirements-explained"></a>Uitleg van de vereisten
Deze vereisten zijn niet van toepassing op de editie Free.

<table border="1">
<tr bgcolor="FAF9F9">
        <td><strong>Wat u nodig hebt</strong></td>
        <td><strong>Waarom u dit nodig hebt</strong></td>
</tr>
<tr>
<td>Azure-abonnement</td>
<td>Het abonnement bepaalt wie zich kan aanmelden bij Azure. De accounthouder maakt het abonnement via <a HREF="https://account.windowsazure.com/Subscriptions"> Azure-abonnementen</a>.
<br/><br/>
Het Azure-account kan meerdere abonnementen hebben en worden beheerd door iedereen die is gemachtigd. De houder van uw Azure-account kan bijvoorbeeld een abonnement met de naam <em>BizTalkServiceAbonnement</em> maken en de BizTalk-beheerders binnen uw bedrijf (bijvoorbeeld ContosoBTSAdmins@live.com) toegang geven tot dit abonnement. In dit scenario melden de BizTalk-beheerders zich aan bij Azure en hebben zij volledige beheerdersrechten voor alle gehoste services in het abonnement, waaronder Azure BizTalk Services. De BizTalk-beheerders zijn niet de houders van het Azure-account en hebben daarom geen toegang tot factureringsgegevens.
<br/><br/>
<a HREF="https://go.microsoft.com/fwlink/p/?LinkID=267577"> Abonnementen en opslagaccounts beheren in Azure</a> geeft u meer informatie.
</td>
</tr>
<tr>
<td>Azure SQL Database</td>
<td>Slaat de tabellen, weergaven en opgeslagen procedures op die worden gebruikt door de BizTalk Service, inclusief de traceringsgegevens.
<br/><br/>
Wanneer u een BizTalk Service maakt, kunt u een bestaande Azure SQL Server of Azure SQL Database gebruiken. U kunt ook automatisch een nieuwe server of database maken.
<br/><br/>
De schaal van de SQL Database wordt automatisch geconfigureerd. Meestal is de standaardschaal voldoende voor een BizTalk Service. Als u de schaal wijzigt, heeft dit invloed op de prijzen. Zie <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=234930"> Accounts en facturering in Azure SQL Database</a>
<br/><br/>
<strong>Opmerkingen</strong>
<br/>
<ul>
<li> Wanneer u een nieuwe Azure SQL Server en Database maakt, wordt Azure Services automatisch ingeschakeld. Het is voor de BizTalk Service vereist dat Azure Services is ingeschakeld.</li>
<li>Als u een nieuwe Azure SQL Database maakt op een bestaande Azure SQL Server, worden de firewallregels van de server niet gewijzigd. Als gevolg hiervan hebben andere Azure Services mogelijk geen toegang tot de databases van de server.</li>
</ul>
</td>
</tr>
<tr>
<td>Azure Access Control-naamruimte</td>
<td>Voert de verificatie met Azure BizTalk Services uit. Wanneer u een BizTalk Service-project implementeert vanuit Visual Studio, voert u deze Access Control-naamruimte in. De Access Control-naamruimte wordt automatisch gemaakt wanneer u een BizTalk Service maakt.</td>
</tr>

<tr>
<td>Azure-opslagaccount</td>
<td>Geeft toegang tot tabellen, blobs en wachtrijen die door uw BizTalk Service worden gebruikt om de volgende bestanden op te slaan:

<ul>
<li>Logboekbestanden die de BizTalk Service bewaken. </li>
<li>Bij het maken van een X12- of AS2-overeenkomst tussen partners kunt u de functie Archiveren inschakelen om de eigenschappen van berichten op te slaan. Deze gegevens worden opgeslagen in het opslagaccount.</li>
</ul>
<br/>
Wanneer u een BizTalk Service maakt, kunt u een bestaand opslagaccount gebruiken of automatisch een nieuw opslagaccount maken.
<br/><br/>
Meestal zijn de standaardinstellingen voor opslag voldoende voor een BizTalk Service.
<br/><br/>
Wanneer u een opslagaccount maakt, worden automatisch een primaire en secundaire sleutel gemaakt. Deze sleutels beheren de toegang tot uw opslagaccount. De BizTalk Service gebruikt automatisch de primaire sleutel.
<br/><br/>
Zie <a HREF="https://go.microsoft.com/fwlink/p/?LinkID=285671">Opslag</a> voor meer informatie.
</td>
</tr>

<tr>
<td>Persoonlijk SSL-certificaat</td>
<td>
Wanneer er een Azure BizTalk Service wordt gemaakt, wordt er ook een HTTPS-URL met de naam van uw BizTalk Service gemaakt. Deze URL wordt automatisch geconfigureerd voor het gebruik van een zelfondertekend certificaat dat alleen geschikt is voor ontwikkelingsdoeleinden. Voor de productie hebt u een persoonlijk SSL-certificaat nodig.
<br/><br/>
<strong>Belangrijke informatie over SSL-certificaten</strong>

<ul>
<li>De vervaldatum van het certificaat moet korter zijn dan 5 jaar.</li>
<li>Voor alle persoonlijke certificaten is een wachtwoord vereist. Onthoud dit wachtwoord. U doet er verstandig aan het wachtwoord te delen met uw beheerders.</li>
<li>Zelfondertekende certificaten worden gebruikt in een test-/ontwikkelingsomgeving. Wanneer u zelfondertekende certificaten gebruikt, importeert u het certificaat in uw persoonlijke certificaatarchief en in het certificaatarchief van vertrouwde basiscertificeringsinstanties.</li>
</ul>
<br/>Geef bij het verzenden van de aanvraag voor een productiecertificaat naar uw certificeringsinstantie (CA) de volgende certificaateigenschappen op:
<br/>

<ul>
<li><strong>Enhanced Key Usage</strong>: Azure BizTalk Services is minimaal serververificatie vereist.</li>
<li><strong>Algemene naam</strong>: Voer de volledig gekwalificeerde domeinnaam (FQDN) van uw Azure BizTalk Service-URL. Zie <a HREF="#CreateService">Een BizTalk Service maken</a> in dit artikel.</li>
</ul>
<br/>
Er kan een nieuw of ander certificaat worden toegevoegd nadat de BizTalk Service is gemaakt.
</td>
</tr>
</table>
<!---Loc Comment: Please, check link [Create a BizTalk Service] since it is not redirecting to any location.--->



## <a name="hybrid-connections"></a>Hybride verbindingen
Wanneer u een Azure BizTalk Service maakt, is het tabblad **Hybride verbindingen** beschikbaar:

![Tabblad Hybride verbindingen][HybridConnectionTab]

Hybride verbindingen worden gebruikt om verbinding te maken tussen een Azure-website of mobiele service van Azure en een on-premises resource die gebruikmaakt van een statische TCP-poort, zoals SQL Server, MySQL, HTTP-web-API's, Mobile Services en de meeste aangepaste webservices.  Hybride verbindingen en de BizTalk Adapter Service zijn twee verschillende zaken. De BizTalk Adapter Service wordt gebruikt om verbinding te maken tussen Azure BizTalk Services en een on-premises LOB-systeem (Line of Business).

 Zie [Hybride verbindingen](integration-hybrid-connection-overview.md) voor meer informatie, waaronder het maken en beheren van hybride verbindingen.

## <a name="next-steps"></a>Volgende stappen
Nu er een BizTalk Service is gemaakt, raken met de verschillende [BizTalk Services: Tabbladen dashboard, bewaken en schalen](biztalk-dashboard-monitor-scale-tabs.md). U kunt nu toepassingen maken met uw BizTalk Service. Ga naar [Azure BizTalk Services](https://go.microsoft.com/fwlink/p/?LinkID=235197) om te beginnen met het maken van toepassingen.

## <a name="see-also"></a>Zie ook
* [BizTalk Services: Grafiek van edities](biztalk-editions-feature-chart.md)<br/>
* [BizTalk Services: Statusgrafiek](biztalk-service-state-chart.md)<br/>
* [BizTalk Services: Back-up en herstel](biztalk-backup-restore.md)<br/>
* [BizTalk Services: Beperking](biztalk-throttling-thresholds.md)<br/>
* [BizTalk Services: Naam en sleutel van verlener](biztalk-issuer-name-issuer-key.md)<br/>
* [De Azure BizTalk Services SDK gaan gebruiken](https://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Hybride verbindingen](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png
