---
title: Maken en koppelen van de service-eindpuntbeleid - Azure portal
titlesuffix: Azure Virtual Network
description: Meer informatie over het instellen van en de bijbehorende service-eindpuntbeleidsregels met behulp van de Azure-portal in dit artikel.
services: virtual-network
documentationcenter: virtual-network
author: anithaa
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 09/18/2018
ms.author: anithaa
ms.openlocfilehash: aaa07759ed8b10578d024d5838ac1d2658778695
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57999891"
---
# <a name="create-change-or-delete-service-endpoint-policy-using-the-azure-portal"></a>Maken, wijzigen of verwijderen van de service-eindpuntbeleid met behulp van de Azure portal

Service-eindpuntbeleid kunnen u voor het filteren van verkeer in virtuele netwerken tot specifieke Azure-resources, via service-eindpunten. Als u niet bekend met service-eindpuntbeleid bent, Zie [overzicht van de beleidsregels van de service-eindpunt](virtual-network-service-endpoint-policies-overview.md) voor meer informatie.

 In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een service-eindpuntbeleid maken
> * Een beleidsdefinitie voor service-eindpunt maken
> * Een virtueel netwerk met een subnet maken
> * Een service-eindpuntbeleid aan een subnet koppelen

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure 

Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="create-a-service-endpoint-policy"></a>Een service-eindpuntbeleid maken

1. Selecteer **+ Een resource maken** in de linkerbovenhoek van Azure Portal.
2. Typ in het deelvenster Zoeken in 'service-eindpuntbeleid' en selecteer **Service-eindpuntbeleid (Preview)** en selecteer vervolgens **maken**.
3. Invoeren of selecteren, de volgende informatie in **grondbeginselen** 

   - Abonnement: Selecteer uw abonnement voor het beleid.    
   - Resourcegroep: Selecteer **Nieuwe maken** en voer *myResourceGroup* in.     
   - Name           : myEndpointPolicy
   - Locatie: US - west-centraal     
 
   ![Grondbeginselen van beleid voor service-eindpunt maken](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-create-startpane.PNG)
   
4. Invoeren of selecteren, de volgende informatie in **beleidsdefinities**

   - Klik op **+ een resource toevoegen**, invoeren, of selecteert, de volgende informatie, accepteer de standaardwaarden voor de overige instellingen en klik op **toevoegen**.  
   - Bereik: Selecteer **één Account** of **alle accounts in het abonnement** of **alle accounts in de resourcegroep**.    
   - Abonnement: Selecteer uw abonnement voor storage-account. Beleid en storage-accounts kunnen zich in verschillende abonnementen.   
   - Resourcegroep: Selecteer de resourcegroep. Vereist als het bereik is ingesteld als 'Alle accounts in de resourcegroep' of 'Één account'.  
   - Bron: mystorageaccountportal    
   - Klik op **+ een resource toevoegen** om door te gaan meer resources toe te voegen.
   
   ![Service-eindpunt beleidsdefinities maken](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-create-policydefinitionspane.PNG)
   
5. Optioneel: Typ of selecteer, de volgende informatie in **Tags**:
   
   - Sleutel: Selecteer de sleutel voor het beleid. Bijvoorbeeld: Schuld     
   - Waarde: Voer de waarde-paar voor de sleutel. Bijvoorbeeld: Finance

6. Selecteer **Controleren + maken**. De informatie en klik op valideren **maken**. Als u wilt meer wijzigingen aanbrengen, klikt u op **vorige**. 

   ![Laatste validaties van beleid voor service-eindpunt maken](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-create-finalcreatereview.PNG)
  
 
## <a name="view-endpoint-policies"></a>Eindpunt-beleid weergeven 

1. In de *alle services* vak in de portal, begint te typen *service-eindpuntbeleid*. Selecteer **Service-eindpunt Policies(Preview)**.
2. Onder **abonnementen**, selecteer uw abonnement en resourcegroep, zoals weergegeven in de volgende afbeelding

   ![Beleid weergeven](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-viewpolicies.PNG)
       
3. Selecteer het beleid en klik op **beleidsdefinities** weergeven of toevoegen van meer beleidsdefinities.

   ![Beleidsdefinities weergeven](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-viewpolicy-adddefinitions.PNG)

4. Selecteer **subnetten die zijn gekoppeld** om de subnetten weer te geven het beleid is gekoppeld. Voor het beleid aan een subnet koppelt, klikt u op 'Navigeren naar virtueel netwerk in dezelfde regio'.

   ![Gekoppelde subnetten weergeven](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-view-associatedsubnets.PNG)
 
## <a name="associate-a-policy-to-a-subnet"></a>Een beleid aan een subnet koppelen

>[!WARNING] 
> Zorg ervoor dat alle resources die zijn geopend vanuit het subnet voor de geselecteerde service voordat u koppelt het beleid worden toegevoegd aan het beleid. Zodra het beleid gekoppeld is, alleen de toegang tot de resources die worden vermeld in het beleid zijn toegestaan, voor eindpunt regio's voor de service. 

Voordat u een beleid aan een subnet koppelen kunt, moet u een virtueel netwerk en subnet maken en vervolgens kunt u het beleid aan het subnet koppelen:

1. Selecteer **+ Een resource maken** in de linkerbovenhoek van Azure Portal.
2. Selecteer **Netwerken** en selecteer vervolgens **Virtueel netwerk**.
3. Voer onder **Virtueel netwerk maken** de volgende informatie in of selecteer deze, accepteer de standaardwaarden voor de overige instellingen en selecteer **Maken**:
   - Name           : myVirtualNetwork      
   - Adresruimte: 10.0.0.0/16      
   - Abonnement: Selecteer uw abonnement. Beleid moet zich in hetzelfde abonnement bevinden als het VNet     
   - Resourcegroep: Selecteer **gebruik bestaande** en selecteer vervolgens *myResourceGroup*     
   - Locatie: US - west-centraal     
   - Subnetnaam: persoonlijke     
   - Adresbereik: 10.0.0.0/24
     
4. Begin in het vak *Resources, services en documenten zoeken* bovenaan de portal **myVirtualNetwork** te typen. Wanneer **myVirtualNetwork** wordt weergegeven in de zoekresultaten, selecteert u dit.
5. Onder **instellingen**, selecteer **subnetten** en selecteer vervolgens **persoonlijke**.
6. Zoals weergegeven in de volgende afbeelding, selecteert u **Service-eindpunten**, selecteer **Microsoft.Storage**, selecteer **Service-eindpuntbeleid**, selecteer  **myEndpointPolicy**, en selecteer vervolgens **opslaan**:

   ![Beleid koppelen](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-associatepolicies.PNG)

>[!WARNING] 
>Toegang tot de serviceresources in andere regio's kunnen worden van dit subnet, op basis van Netwerkbeveiligingsgroepen (nsg's). Om toegang te beperken tot alleen eindpunt regio's, door nsg's te beperken tot alleen serviceverkeer in eindpunt regio's. Zie voor meer informatie over nsg's maken met servicetags per regio [servicetags Azure NSG.](manage-network-security-group.md?toc=%2fcreate-a-security-rule%2f.json)

In het onderstaande voorbeeld is de NSG beperkt voor toegang tot alleen Azure Storage-resources in WestCentralUS en WestUS2, met een regel "Deny alle" Als de regel van een lagere prioriteit.

![Alle NSG weigeren](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-nsg-rules.PNG)


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt gemaakt van een service-eindpuntbeleid en die aan een subnet gekoppeld. Zie voor meer informatie over service-eindpuntbeleid [overzicht van de beleidsregels van de service-eindpunt.](virtual-network-service-endpoint-policies-overview.md)

