---
title: Delegeren van aanbiedingen in Azure-Stack | Microsoft Docs
description: Informatie over het plaatsen van andere personen verantwoordelijk aanbiedingen maken en gebruikers melden voor u.
services: azure-stack
documentationcenter: 
author: AlfredoPizzirani
manager: byronr
editor: 
ms.assetid: 157f0207-bddc-42e5-8351-197ec23f9d46
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: alfredop
ms.openlocfilehash: 1a1f2789076b610ee557bde5981a331c55cc1c95
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="delegate-offers-in-azure-stack"></a>Aanbiedingen in Azure Stack delegeren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Als de operator Azure Stack wilt vaak u andere personen verantwoordelijk aanbiedingen maken en gebruikers aanmelden. Bijvoorbeeld, als u een serviceprovider bent, kunt u resellers registreren klanten en deze beheren namens jou. Of als u deel uitmaakt van een centrale IT-groep in een onderneming, kunt u dochterondernemingen aan te melden gebruikers zonder uw tussenkomst.

Overdracht kunt u deze taken door de mogelijkheid om te bereiken en meer gebruikers dan u rechtstreeks kunt beheren. De volgende afbeelding ziet u één niveau van overdracht, maar ondersteunt meerdere niveaus van Azure-Stack. Gedelegeerde providers (DP's) kunnen op zijn beurt delegeren aan andere providers, maximaal vijf niveaus.

![Niveaus van overdracht](media/azure-stack-delegated-provider/image1.png)

Azure Stack-operators kunnen delegeren van het maken van aanbiedingen en gebruikers aan andere gebruikers met behulp van de functionaliteit van de overdracht.

## <a name="roles-and-steps-in-delegation"></a>Functies en de stappen in overdracht
Om te begrijpen overdracht, houd er rekening mee dat er zijn drie rollen die zijn betrokken:

* De *Azure Stack operator* beheert de Stack van Azure-infrastructuur en anderen deze aan te bieden aan hun gebruikers delegeert, maakt u een sjabloon van de aanbieding.

* De gedelegeerde Azure Stack-operators heten *overgedragen providers*. Ze kunnen behoren tot de andere organisaties, zoals de gebruikers van andere Azure Active Directory (Azure AD).

* *Gebruikers* aanmelden voor de aanbiedingen en ze gebruiken voor het beheren van hun werkbelasting, het maken van virtuele machines, het opslaan van gegevens, enzovoort.

Zoals u in de volgende afbeelding, zijn er twee stappen voor het instellen van overdracht.

1. *Identificeren van de gedelegeerde providers*. U doen dit door ze te abonneren op een aanbieding op basis van een plan dat alleen de abonnementen service bevat. Gebruikers die zijn aangemeld bij deze aanbieding aanschaffen enkele mogelijkheden van de Azure-Stack-operator, inclusief de mogelijkheid biedt uitbreiden en meld u aan gebruikers voor deze.

2. *Een aanbieding delegeren aan de gedelegeerde provider*. Deze aanbieding fungeert als een sjabloon voor wat de gedelegeerde provider kunt aanbieden. De gedelegeerde provider kan nu duren voordat de aanbieding. Kies een naam voor het (maar niet de services en quota's), en aan klanten te bieden.

![De gedelegeerde provider maken en deze aan te melden gebruikers inschakelen](media/azure-stack-delegated-provider/image2.png)

Om te fungeren als gedelegeerde providers, moeten gebruikers een relatie tot stand brengen met de belangrijkste provider. Met andere woorden, moeten zij een abonnement maken. In dit scenario identificeert dit abonnement de gedelegeerde providers als bestanden met het recht om te aanwezig aanbiedingen namens de belangrijkste provider.

Nadat deze relatie tot stand is gebracht, kunt een aanbieding in de Azure-Stack-operator delegeren aan de gedelegeerde provider. De gedelegeerde provider nu kan duren voordat de aanbieding, wijzigt u de naam (maar niet de inhoud wijzigen), en aan de klanten te bieden.

De volgende secties wordt beschreven hoe u een gedelegeerde provider tot stand brengen, een aanbieding delegeren, en controleer of gebruikers kunnen zich registreren voor.

## <a name="set-up-roles"></a>Rollen instellen

Als u wilt zien van een gedelegeerd provider op het werk, moet u extra Azure AD-accounts naast uw Azure-Stack operator-account. Als u deze niet hebt, maakt u de twee accounts. De accounts kunnen deel uitmaken van een Azure AD-gebruiker. We noemen deze de gedelegeerde provider en de gebruiker.

| **Rol** | **Organisatie-rechten** |
| --- | --- |
| Gedelegeerde provider |Gebruiker |
| Gebruiker |Gebruiker |

## <a name="identify-the-delegated-providers"></a>De gedelegeerde providers identificeren
1. Meld u aan als een Azure-Stack-operator.

2. Maken van de aanbieding waarmee gebruikers kunnen worden overgedragen providers:
   
   a.  [Maak een plan](azure-stack-create-plan.md).
       Dit abonnement moet alleen de abonnementen service bevatten. In dit artikel gebruiken we een plan genaamd **PlanForDelegation**.
   
   b.  [Maken van een aanbieding](azure-stack-create-offer.md) op basis van dit plan. In dit artikel gebruiken we een aanbieding aangeroepen **OfferToDP**.
   
   c.  Nadat het maken van de aanbieding voltooid is, de gedelegeerde provider toevoegen als een abonnee op deze aanbieding. Hiertoe **abonnementen** > **toevoegen** > **nieuwe Tenantabonnement**.
   
   ![De gedelegeerde provider toevoegen als een abonnee](media/azure-stack-delegated-provider/image3.png)

> [!NOTE]
> Als met alle Azure-Stack-aanbiedingen die u hebt de optie van het maken van de aanbieding openbare te zodat gebruikers aanmelden voor, of het privé houden en laten de Azure-Stack-operator beheren de aanmelding. Gedelegeerde providers zijn meestal een kleine groep. Wilt u bepalen wie is toegelaten, zodat het privé houden van deze aanbieding wel zinvol in de meeste gevallen is.
> 
> 

## <a name="azure-stack-operator-creates-the-delegated-offer"></a>Azure Stack-operator wordt gemaakt van de aanbieding voor gedelegeerd

U hebt nu uw gedelegeerde provider vastgesteld. De volgende stap is het maken van het plan en bieden die u wilt overdragen en uw klanten worden gebruikt. Er is een goed idee om te definiëren van deze aanbieding precies zoals u de klanten om het te bekijken wilt omdat gedelegeerd provider niet kunt wijzigen van de plannen en quota's bevat.

1. Als operator Azure Stack [een plan maken](azure-stack-create-plan.md) en [een aanbieding](azure-stack-create-offer.md) op basis van deze. Voor dit artikel gebruiken we een aanbieding aangeroepen **DelegatedOffer.**
   
   > [!NOTE]
   > Deze aanbieding heeft geen openbaar. Als u kiest, kunt u het openbare. In de meeste gevallen wilt u echter alleen gedelegeerd providers toegang hebben tot deze. Nadat u een persoonlijke aanbieding overdragen zoals beschreven in de volgende stappen, heeft de provider gedelegeerde toegang.
   > 
   > 
1. Delegeren van de aanbieding. Ga naar **DelegatedOffer.** Klik in de **instellingen** deelvenster **overgedragen Providers** > **toevoegen**.

2. Selecteer het abonnement voor de gedelegeerde provider in de vervolgkeuzelijst en selecteer vervolgens **gemachtigde**.

> ![Een gedelegeerde provider toevoegen](media/azure-stack-delegated-provider/image4.png)
> 
> 

## <a name="delegated-provider-customizes-the-offer"></a>Gedelegeerde provider past de aanbieding

Meld u bij de gebruikersportal als de gedelegeerd provider. Vervolgens maakt u een nieuwe aanbieding met behulp van de aanbieding voor gedelegeerd als sjabloon.

1. Selecteer **nieuwe** > **Tenant biedt + plannen** > **bieden**.

    ![Een nieuwe aanbieding maken](media/azure-stack-delegated-provider/image5.png)


1. Een naam toewijzen aan de aanbieding. Hier kiest **ResellerOffer**. Selecteer de gedelegeerde aanbieding waarop moet worden gebaseerd en selecteer vervolgens **maken**.
   
   ![Wijs een naam](media/azure-stack-delegated-provider/image6.png)

    >[!NOTE] 
    > Noteer het verschil vergeleken met bieden maken als ervaren door de Azure-Stack-operator. De gedelegeerde provider heeft de aanbieding van basis-plan en plannen van de invoegtoepassing niet maken. Ze kunnen alleen kiezen uit de aanbiedingen die aan hen zijn toegewezen en geen wijzigingen aanbrengen in aanbiedingen.

1. De aanbieding openbaar maken door het selecteren van **Bladeren**, en vervolgens **biedt**. Selecteer de aanbieding en selecteer vervolgens **wijziging van de status**.

2. De gedelegeerde provider beschrijft deze aanbiedingen via hun eigen portal URL. Deze aanbiedingen zijn zichtbaar alleen via de portal gedelegeerd. Om te zoeken en wijzig deze URL:
   
    a.  Selecteer **Bladeren** > **meer services** >  **abonnementen**. Selecteer vervolgens het gedelegeerde provider-abonnement. In ons geval heeft **DPSubscription** > **eigenschappen**.
   
    b.  Kopiëren van de portal URL naar een afzonderlijke locatie, zoals Kladblok.
   
    ![Selecteer het abonnement gedelegeerd provider](media/azure-stack-delegated-provider/dpportaluri.png)  
   
   U hebt nu een aanbieding voor gedelegeerd gemaakt als een gedelegeerde provider. Meld u af als de gedelegeerd provider. Sluit het browservenster die u hebt gebruikt.

## <a name="sign-up-for-the-offer"></a>Aanmelden voor de aanbieding
1. In een nieuw browservenster en gaat u naar de portal gedelegeerd URL die u hebt opgeslagen in de vorige stap. Meld u aan de portal aan als een gebruiker. 
   
   >[!NOTE]
   > De gedelegeerde portal gebruiken voor deze stap. De gedelegeerde aanbiedingen zijn anders niet zichtbaar.

2. Selecteer in het dashboard **Neem een abonnement op**. U ziet dat alleen de gedelegeerde aanbiedingen die zijn gemaakt door de gedelegeerde provider worden gepresenteerd aan de gebruiker:

> ![Weergeven en selecteren van aanbiedingen](media/azure-stack-delegated-provider/image8.png)
> 
> 

Het proces van aanbieding overdracht is voltooid. De gebruiker kan nu aanmelden voor deze aanbieding met het ophalen van een abonnement voor.

## <a name="multiple-tier-delegation"></a>Overdracht van meerdere lagen

Overdracht van meerdere lagen kunt de gedelegeerde provider te delegeren van de aanbieding voor een andere entiteiten. Hierdoor kan bijvoorbeeld het maken van diepere wederverkoper kanalen, waarbij de provider die wordt beheerd door Azure Stack een aanbieding met een distributieserver delegeert. Deze distributor wordt op zijn beurt delegeert naar een wederverkoper. Azure Stack ondersteunt maximaal vijf niveaus van overdracht.

Voor het maken van meerdere lagen van de aanbieding delegatie delegeert de gedelegeerde provider op zijn beurt de aanbieding met de volgende provider. Het proces is hetzelfde voor de gedelegeerde provider zoals deze voor de Azure-Stack-operator was (Zie [Azure Stack-operator wordt gemaakt van de aanbieding voor gedelegeerd](#cloud-operator-creates-the-delegated-offer)).

## <a name="next-steps"></a>Volgende stappen
[Een virtuele machine inrichten](azure-stack-provision-vm.md)

