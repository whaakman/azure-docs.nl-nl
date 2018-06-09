---
title: Met behulp van de beheerdersportal in Azure-Stack | Microsoft Docs
description: Informatie over het gebruik van de beheerdersportal als operator Azure-Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 02c7ff03-874e-4951-b591-28166b7a7a79
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.author: mabrigg
ms.openlocfilehash: 673b1144fe927e0619f5f8638d7e8ce9a181f48c
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248517"
---
# <a name="using-the-administrator-portal-in-azure-stack"></a>Met behulp van de beheerdersportal in Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Er zijn twee portals in Azure-Stack; de beheerdersportal en de gebruikersportal (ook wel de *tenant* portal.) Als een Azure-Stack-operator, kunt u de beheerdersportal voor het dagelijkse beheer en de bewerkingen van de Azure-Stack.

## <a name="access-the-administrator-portal"></a>Toegang tot de beheerdersportal

Een ontwikkelingsomgeving kit, moet u eerst controleren of u kunt [verbinding maken met de development kit host](azure-stack-connect-azure-stack.md) via verbinding met extern bureaublad of via een virtueel particulier netwerk (VPN).

Toegang tot de beheerdersportal Blader naar de portal-URL en meld u aan met de referenties van een Azure-Stack-operator. Voor een geïntegreerd systeem is de portal die URL varieert op basis van de regionaam en de externe volledig gekwalificeerde domeinnaam (FQDN) van uw Azure-Stack-implementatie.

| Omgeving | Beheerder Portal-URL |   
| -- | -- | 
| Development kit| https://adminportal.local.azurestack.external  |
| Geïntegreerde systemen | https://adminportal.&lt; *regio*&gt;.&lt; *FQDN-naam*&gt; | 
| | |

 ![De beheerdersportal](media/azure-stack-manage-portals/image1.png)

In de beheerdersportal kunt u doen, zoals:

* beheren van de infrastructuur (inclusief de status van een systeem, updates, capaciteit, enz.)
* De Marketplace vullen
* abonnementen voor gebruikers maken
* plannen en aanbiedingen maken

De **Quick Start-zelfstudie** tegel vindt u koppelingen naar online documentatie voor de meest algemene taken.

Hoewel een operator heeft resources kunt maken zoals virtuele machines, virtuele netwerken en opslagaccounts in de beheerdersportal, moet u [aanmelden bij de gebruikersportal](user/azure-stack-use-portal.md) maken en testen van resources.

>[!NOTE]
>De **maken van een virtuele machine** koppeling in de tegel van Quick Start-zelfstudie moet u een virtuele machine maken in de beheerdersportal, maar dit is alleen bedoeld voor het valideren van Azure-Stack nadat het eerst wordt geïmplementeerd.

## <a name="understand-subscription-behavior"></a>Abonnement gedrag begrijpen

Er is slechts één abonnement vanuit de beheerdersportal kunnen worden gebruikt. Dit abonnement is de *Provider-abonnement standaard*. U kan geen andere abonnementen toevoegen en deze gebruiken in de beheerdersportal.

Als een Azure-Stack-operator, kunt u abonnementen toevoegt voor uw gebruikers (inclusief uzelf) vanuit de beheerdersportal. Gebruikers (inclusief uzelf) kunnen openen en gebruiken van deze abonnementen van de **gebruiker** portal. De gebruikersportal bieden echter geen toegang tot een van de administratieve of operationele mogelijkheden van de beheerdersportal.

De beheerder en gebruiker portals worden ondersteund door de afzonderlijke exemplaren van Azure Resource Manager. Vanwege deze scheiding van Resource Manager overschrijden abonnementen geen portals. Bijvoorbeeld, als u, als een Azure-Stack-operator zich aanmeldt bij de gebruikersportal aanmeldt, u geen toegang tot de *Provider-abonnement standaard*. Hoewel u geen toegang tot alle beheerfuncties hebt, kunt u abonnementen voor uzelf maken in openbare aanbiedingen beschikbaar. Als u bent aangemeld bij de gebruikersportal als de gebruiker van een tenant worden beschouwd.

  >[!NOTE]
  >In de ontwikkelomgeving kit als een gebruiker tot dezelfde tenant directory als de operator Azure Stack behoort worden ze niet geblokkeerd aanmelden bij de beheerdersportal. Echter, hebben geen toegang tot een van de beheerfuncties. Ook vanuit de portal administrator ze kunnen niet toevoegen-abonnementen of toegang biedt die beschikbaar zijn voor deze in de gebruikersportal.

## <a name="administrator-portal-tips"></a>Beheerder portal tips

### <a name="customize-the-dashboard"></a>Het dashboard aanpassen

Het dashboard bevat een reeks standaardtegels. U kunt selecteren **bewerken dashboard** standaarddashboard wijzigen of selecteer **nieuwe dashboard** toevoegen van een aangepast dashboard. U kunt eenvoudig tegels toevoegen aan een dashboard. U kunt bijvoorbeeld selecteren **nieuw**, met de rechtermuisknop op **biedt + plannen**, en selecteer vervolgens **vastmaken aan dashboard**.

### <a name="quick-access-to-online-documentation"></a>Snelle toegang tot de online documentatie

Voor toegang tot de Azure-Stack operator-documentatie, de Help en ondersteuning voor pictogram (vraagteken) in de rechterbovenhoek van de beheerdersportal. De cursor te verplaatsen naar het pictogram en selecteer vervolgens **Help + ondersteuning**.

### <a name="quick-access-to-help-and-support"></a>Snelle toegang tot de help en ondersteuning

Als u het pictogram Help en ondersteuning (vraagteken) in de rechterbovenhoek van de beheerdersportal selecteert en selecteer vervolgens **nieuw ondersteuningsverzoek**, een van de volgende resultaten optreden:

- Als u een geïntegreerd systeem, hiermee een site waar u kunt rechtstreeks een ondersteuningsticket met de klantenondersteuning van Microsoft ondersteuning voor Services (CSS opent). Raadpleeg [waar ondersteuning krijgen](azure-stack-manage-basics.md#where-to-get-support) om te begrijpen wanneer u via Microsoft ondersteuning of uw oorspronkelijke leveranciers (OEM) hardware leverancier ondersteuning moet gaan.
- Als u de development kit, wordt de Azure-Stack-forums site rechtstreeks door deze actie geopend. Deze forums worden regelmatig bewaakt. Omdat de development kit een evaluatieomgeving is, is geen officiële ondersteuning die worden aangeboden via Microsoft CSS.

## <a name="next-steps"></a>Volgende stappen

- [Regio management in Azure-Stack](azure-stack-region-management.md)
