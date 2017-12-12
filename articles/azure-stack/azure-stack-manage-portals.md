---
title: Met behulp van de beheerdersportal in Azure-Stack | Microsoft Docs
description: Informatie over het gebruik van de beheerdersportal als operator Azure-Stack.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 02c7ff03-874e-4951-b591-28166b7a7a79
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: 34d9d207225327758d535a11c870019f3db31cb0
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="using-the-administrator-portal-in-azure-stack"></a>Met behulp van de beheerdersportal in Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Er zijn twee portals in Azure-Stack; de beheerdersportal en de gebruikersportal (ook wel de *tenant* portal). Als een Azure-Stack-operator, kunt u de beheerdersportal voor het dagelijkse beheer en de bewerkingen van de Azure-Stack. 

## <a name="access-the-administrator-portal"></a>Toegang tot de beheerdersportal

Een ontwikkelingsomgeving kit, moet u eerst controleren of u kunt [verbinding maken met de development kit host](azure-stack-connect-azure-stack.md) via verbinding met extern bureaublad of via een virtueel particulier netwerk (VPN).

Toegang tot de beheerdersportal Blader naar de portal-URL en meld u aan met de referenties van een Azure-Stack-operator. Voor een geïntegreerd systeem is de portal die URL varieert op basis van de regionaam en de externe volledig gekwalificeerde domeinnaam (FQDN) van uw Azure-Stack-implementatie.

| Omgeving | Beheerder Portal-URL |   
| -- | -- | 
| Development kit| https://adminportal.local.azurestack.external  |
| Geïntegreerde systemen | https://adminportal. &lt; *regio*&gt;.&lt; *FQDN-naam*&gt; | 
| | |

 ![De beheerdersportal](media/azure-stack-manage-portals/image1.png)

In de beheerdersportal kunt u doen, zoals:

* beheren van de infrastructuur (inclusief de status van een systeem, updates, capaciteit, enz.)
* Vullen van de marketplace
* plannen en aanbiedingen maken
* abonnementen voor gebruikers maken

In de **Quick Start-zelfstudie** tegel, zijn er koppelingen naar online documentatie voor de meest algemene taken.
 
Hoewel er voor een operator resources, zoals virtuele machines, virtuele netwerken en storage-accounts maken in de beheerdersportal de mogelijkheid, moet u [aanmelden bij de gebruikersportal](user/azure-stack-use-portal.md) maken en testen van resources. (De **maken van een virtuele machine** koppeling in de tegel van Quick Start-zelfstudie moet u een virtuele machine maken in de beheerdersportal, maar deze procedure is alleen voor het valideren van Azure-Stack na de initiële implementatie.)

## <a name="subscription-behavior"></a>Abonnement-gedrag
 
Er is slechts één abonnement die beschikbaar is in de beheerdersportal. Dit abonnement is de *Provider-abonnement standaard*. U kunt geen abonnementen voor gebruik in de beheerdersportal toevoegen.

Als een Azure-Stack-operator, kunt u abonnementen toevoegt voor uw gebruikers (inclusief uzelf) vanuit de beheerdersportal. Gebruikers (inclusief uzelf) kunnen openen en gebruiken van deze abonnementen van de gebruikersportal. De gebruikersportal biedt geen toegang tot een van de administratieve of operationele mogelijkheden van de beheerdersportal.

De beheerder en gebruiker portals worden ondersteund door de afzonderlijke exemplaren van Azure Resource Manager. Vanwege de Resource Manager-scheiding tussen overschrijden abonnementen geen portals. Bijvoorbeeld, als u een Azure-Stack-operator zich aanmeldt bij de gebruikersportal aanmeldt, u geen toegang tot de standaard Provider-abonnement. Daarom hebt u geen toegang tot alle beheerfuncties. U kunt abonnementen voor uzelf maken van openbare aanbiedingen, maar u worden beschouwd als een gebruiker van de tenant.

  >[!NOTE]
  In de ontwikkelomgeving kit als een gebruiker tot dezelfde tenant directory als de operator Azure Stack behoort worden ze niet geblokkeerd aanmelden bij de beheerdersportal. Echter, hebben geen toegang tot een van de beheerfuncties. Ook vanuit de portal administrator ze kunnen niet toevoegen-abonnementen of toegang biedt die zijn aangebracht die voor hen beschikbaar in de gebruikersportal.

## <a name="administrator-portal-tips"></a>Beheerder portal tips

### <a name="customize-the-dashboard"></a>Het dashboard aanpassen

Het dashboard bevat een reeks standaardtegels. U kunt klikken op **bewerken dashboard** standaarddashboard wijzigen of klik op **nieuwe dashboard** toevoegen van aangepaste dashboards. U kunt eenvoudig tegels toevoegen aan het dashboard. U kunt bijvoorbeeld klikken op **nieuw**, met de rechtermuisknop op **biedt + plannen**, en klik vervolgens op **vastmaken aan dashboard**.

### <a name="quick-access-to-online-documentation"></a>Snelle toegang tot de online documentatie

Voor toegang tot de Azure-Stack operator-documentatie, klikt u op het pictogram Help en ondersteuning (vraagteken) in de rechterbovenhoek van de beheerdersportal en klik vervolgens op **Help + ondersteuning**.

### <a name="quick-access-to-help-and-support"></a>Snelle toegang tot de help en ondersteuning

Als u klikt u op het pictogram Help en ondersteuning (vraagteken) in de rechterbovenhoek van de beheerdersportal en klik vervolgens op **nieuw ondersteuningsverzoek**, dit heeft een van de volgende:

- Als u een geïntegreerd systeem, hiermee een site waar u kunt rechtstreeks een ondersteuningsticket met de klantenondersteuning van Microsoft ondersteuning voor Services (CSS opent). Raadpleeg het gedeelte 'Where ondersteuning krijgen' [basisbeginselen van Azure-Stack beheer](azure-stack-manage-basics.md) om te begrijpen wanneer u via Microsoft ondersteuning of uw oorspronkelijke leveranciers (OEM) hardware leverancier ondersteuning moet gaan.
- Als u de development kit, wordt de Azure-Stack-forums site rechtstreeks door deze actie geopend. Deze forums worden regelmatig bewaakt. Omdat de development kit een evaluatieomgeving is, is geen officiële ondersteuning die worden aangeboden via Microsoft CSS.

## <a name="next-steps"></a>Volgende stappen

- [Regio management in Azure-Stack](azure-stack-region-management.md)
