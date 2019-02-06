---
title: Aanbiedingen in Azure Stack delegeren | Microsoft Docs
description: Informatie over het plaatsen van andere personen die verantwoordelijk is voor het maken van aanbiedingen en ondertekening van gebruikers voor u.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 01/09/2019
ms.openlocfilehash: b7a12494585cedd8f75fd9061f8eb29a0b5dc319
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55747421"
---
# <a name="delegate-offers-in-azure-stack"></a>Aanbiedingen in Azure Stack delegeren

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Als de Azure Stack-operators wilt u meestal om andere personen die verantwoordelijk is voor gebruikers aanmelden en het maken van abonnementen. Bijvoorbeeld, als u een serviceprovider bent, kunt u wederverkopers aan klanten aanmelden en deze beheren namens. Of, als u deel uitmaakt van een centrale IT-groep in een onderneming, wilt u mogelijk gemachtigde gebruiker aanmelden bij andere IT-personeel.

Delegering wordt het gemakkelijker te bereiken en te beheren van meer gebruikers dan u door uzelf doen kunt, zoals wordt weergegeven in de volgende afbeelding:

![Niveaus van overdracht](media/azure-stack-delegated-provider/image1.png)

Met delegering de gedelegeerde provider beheert een aanbieding (gedelegeerde aanbieding) en eindgebruikers merken verkrijgen abonnementen voor deze aanbieding zonder tussenkomst van de systeembeheerder.

## <a name="understand-delegation-roles-and-steps"></a>Functies van de delegatie en stappen begrijpen

### <a name="delegation-roles"></a>De delegatie-rollen

De volgende rollen zijn onderdeel van de delegatie van:

* De *Azure Stack-operators* beheert de Azure Stack-infrastructuur en maakt u een sjabloon van de aanbieding. De operator voor netwerkapparaten delegeert anderen voor aanbiedingen voor hun tenant.

* De gedelegeerde Azure Stack-operators zijn gebruikers met *eigenaar* of *Inzender* rechten in de abonnementen met de naam *gedelegeerde providers*. Ze kunnen deel uitmaken van andere organisaties, zoals andere tenants Azure Active Directory (Azure AD).

* *Gebruikers* zich registreren voor de aanbiedingen en ze gebruiken voor het beheren van hun workloads, het maken van virtuele machines, opslag van gegevens, enzovoort.

### <a name="delegation-steps"></a>Stappen voor overdracht

Er zijn twee eenvoudige stappen voor het instellen van overdracht:

1. **Maken van een gedelegeerde-providerabonnement**: Abonneren op een aanbieding met alleen de service van de abonnementen van een gebruiker. Gebruikers die zijn geabonneerd op deze aanbieding kunnen vervolgens de gedelegeerde aanbiedingen uitbreiden naar andere gebruikers door deze te ondertekenen voor aanbiedingen.

2. **Aanbieding delegeren aan de gedelegeerde provider**: Deze aanbieding kunnen de gedelegeerde provider om abonnementen te maken of uitbreiden van de aanbieding voor hun gebruikers. De gedelegeerde provider kan nu nemen van de aanbieding en aanbieden aan andere gebruikers.

De volgende afbeelding ziet u de stappen voor het instellen van overdracht:

![De gedelegeerde provider maken en ze te registreren van gebruikers](media/azure-stack-delegated-provider/image2.png)

#### <a name="delegated-provider-requirements"></a>Vereisten voor gedelegeerde providers

Om te fungeren als een gedelegeerde provider, een gebruiker tot stand brengt een relatie met de belangrijkste provider door een abonnement te maken. Dit abonnement geeft de gedelegeerde provider als bestaande uit het recht om weer te geven van de gedelegeerde aanbiedingen namens de belangrijkste provider.

Nadat u deze relatie tot stand is gebracht, kunt de Azure Stack-operator een aanbieding delegeren aan de gedelegeerde-provider. De gedelegeerde provider kan duren voordat de aanbieding, wijzig de naam (maar niet de inhoud te wijzigen), en deze te bieden aan klanten.

## <a name="delegation-walkthrough"></a>Overzicht van delegeren

De volgende secties bevatten een praktische scenario voor het instellen van een gedelegeerde provider en verifiëren van gebruikers kunnen zich aanmelden voor de gedelegeerde aanbieding een aanbieding wordt gedelegeerd.

### <a name="set-up-roles"></a>Rollen instellen

Als u wilt gebruiken in dit scenario, moet u twee Azure AD-accounts naast uw Azure Stack-operator-account. Als u deze twee accounts hebt, moet u deze maken. De accounts kunnen deel uitmaken van een Azure AD-gebruiker en worden aangeduid als de gedelegeerd provider en de gebruiker.

| **Rol** | **Organisatie-rechten** |
| --- | --- |
| Gedelegeerde provider |Gebruiker |
| Gebruiker |Gebruiker |

 > [!NOTE]
   > In het geval van een CSP-reseller, voor het maken van deze gedelegeerde provider worden moet deze gebruikers in de Tenant-map (de gebruiker AAD). De Azure Stack-Operator moet [eerste onboarding](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-enable-multitenancy) die AAD-tenant en vervolgens gebruik en facturering configureren door het volgende [stappen](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-csp-howto-register-tenants).

### <a name="identify-the-delegated-provider"></a>De gedelegeerde provider identificeren

1. Meld u aan de administrator-portal als Azure Stack-operators.

1. Maken van een aanbieding waarmee een gebruiker om te worden van een gedelegeerde provider:

   a.  [Maak een plan](azure-stack-create-plan.md).
       Dit plan moet bevatten alleen de abonnementsservice. In dit artikel wordt een abonnement met de naam **PlanForDelegation** als voorbeeld.

   b.  [Maak een aanbieding](azure-stack-create-offer.md) op basis van dit plan. In dit artikel wordt gebruikgemaakt van een aanbieding met de naam **OfferToDP** als voorbeeld.

   c.  De gedelegeerde provider toevoegen als een abonnee op deze aanbieding door te selecteren **abonnementen**, klikt u vervolgens **toevoegen**, klikt u vervolgens **nieuwe Tenantabonnement**.

   ![De gedelegeerde provider toevoegen als een abonnee](media/azure-stack-delegated-provider/image3.png)

   > [!NOTE]
   > Als met alle Azure Stack-aanbiedingen, hebt u de optie van het maken van de aanbieding voor openbare en zodat gebruikers zich aanmelden voor, of het privé houden en laat het beheren van de aanmelding bij Azure Stack-operators. Gedelegeerde providers zijn meestal een kleine groep. U wilt om te bepalen wie is toegelaten, zodat het privé houden van deze aanbieding zinvol in de meeste gevallen.

### <a name="azure-stack-operator-creates-the-delegated-offer"></a>Azure Stack-operators maakt de gedelegeerde aanbieding

De volgende stap is het maken van het plan en aanbieding die u wilt overdragen, en dat uw gebruikers wordt gebruikt. Er is een goed idee om te definiëren van deze aanbieding, precies zoals u wilt dat gebruikers om te zien, omdat de gedelegeerde provider de abonnementen en quota's die het bevat niet wijzigen.

1. Als Azure Stack-operators, [maken van een plan](azure-stack-create-plan.md) en [een aanbieding](azure-stack-create-offer.md) op basis van het abonnement. In dit artikel wordt gebruikgemaakt van een aanbieding met de naam **DelegatedOffer** als voorbeeld.

   > [!NOTE]
   > Deze aanbieding heeft geen openbaar, maar u kunt deze openbaar maken. In de meeste gevallen wilt u echter alleen gedelegeerde providers toegang hebben tot de aanbieding. Nadat u een persoonlijke aanbieding delegeren zoals beschreven in de volgende stappen, heeft de provider van gedelegeerde toegang toe.

2. De aanbieding delegeren. Ga naar **DelegatedOffer**. Onder **instellingen**, selecteer **gedelegeerde Providers**en selecteer vervolgens **toevoegen**.

3. Selecteer het abonnement voor de gedelegeerde provider uit de vervolgkeuzelijst en selecteer vervolgens **gemachtigde**.

   ![Een gedelegeerde provider toevoegen](media/azure-stack-delegated-provider/image4.png)

### <a name="delegated-provider-customizes-the-offer"></a>Gedelegeerde provider past de aanbieding

Meld u aan bij de gebruikersportal aanmeldt als gedelegeerde-provider en maak vervolgens een nieuwe aanbieding met behulp van de gedelegeerde aanbieding als sjabloon.

1. Selecteer **+ een resource maken**, klikt u vervolgens **Tenant aanbiedingen + plannen**en selecteer vervolgens **bieden**.

    ![Een nieuwe aanbieding maken](media/azure-stack-delegated-provider/image5.png)

2. Een naam toewijzen aan de aanbieding. In dit voorbeeld wordt **ResellerOffer**. Selecteer de gedelegeerde aanbieding waarop u wilt baseren, en selecteer vervolgens **maken**.

   ![Een naam toewijzen](media/azure-stack-delegated-provider/image6.png)

   >[!IMPORTANT]
   >Het is belangrijk om te begrijpen dat gedelegeerde providers aanbiedingen die worden overgedragen aan ze alleen kunnen kiezen. Ze aanbrengen geen wijzigingen in aanbiedingen. Alleen een Azure Stack-operator kunt wijzigen deze aanbiedingen, bijvoorbeeld wijzigen van hun abonnementen en quota's. Een gedelegeerde provider heeft niet maken voor een aanbieding van basisplannen en aanvullende plannen.

3. De gedelegeerde provider kunt maken deze aanbiedingen via hun eigen portal openbare URL. Als u de aanbieding voor openbare, schakelt u **Bladeren**, en vervolgens **biedt**. Selecteer de aanbieding en selecteer vervolgens **status wijzigen**.

4. De openbare gedelegeerde aanbiedingen zijn nu zichtbaar zijn alleen door de gedelegeerde-portal. Om te zoeken en wijzig deze URL:

    a.  Selecteer **Bladeren**, klikt u vervolgens **alle services**, en klik vervolgens onder de **algemene** categorie, selecteer **abonnementen**. Selecteer de gedelegeerde-providerabonnement; bijvoorbeeld, **DPSubscription**, klikt u vervolgens **eigenschappen**.

    b.  Kopiëren van de portal URL naar een afzonderlijke locatie, zoals Kladblok.

    ![De gedelegeerde-Providerabonnement selecteren](media/azure-stack-delegated-provider/dpportaluri.png)  

   U klaar bent met het maken van een gedelegeerde aanbieding als een gedelegeerde-provider. Meld u af als gedelegeerde-provider op en sluit het browservenster.

### <a name="sign-up-for-the-offer"></a>Aanmelden voor de aanbieding

1. In een nieuw browservenster geopend, gaat u naar de gedelegeerde portal-URL die u in de vorige stap hebt opgeslagen. Meld u aan de portal aan als een gebruiker.

   >[!NOTE]
   >De gedelegeerde aanbiedingen zijn niet zichtbaar, tenzij u de gedelegeerde-portal.

1. Selecteer in het dashboard, **Neem een abonnement**. U ziet dat alleen de gedelegeerde aanbiedingen die zijn gemaakt door de gedelegeerde provider worden weergegeven voor de gebruiker.

   ![Weergeven en selecteren van aanbiedingen](media/azure-stack-delegated-provider/image8.png)

Het proces van het overdragen van een aanbieding is voltooid. Nu kan een gebruiker zich registreren voor deze aanbieding met het ophalen van een abonnement op.

## <a name="move-subscriptions-between-delegated-providers"></a>Abonnementen verplaatsen tussen gedelegeerde providers

Indien nodig, kan een abonnement worden verplaatst tussen de nieuwe of bestaande gedelegeerde providerabonnementen die deel uitmaken van dezelfde directory-tenant. Dit wordt gedaan met behulp van de PowerShell-cmdlet [verplaatsen AzsSubscription](/powershell/module/azs.subscriptions.admin).

Dit is handig wanneer:

* U onboarding een nieuw teamlid die gaat ondernemen op de rol van gedelegeerde provider en u wilt toewijzen aan dit team lid gebruikersabonnementen die eerder zijn gemaakt in het abonnement van de provider standaard.
* U hebt meerdere abonnementen voor gedelegeerde providers in dezelfde directory-tenant (Azure Active Directory) en moet om gebruikersabonnementen te verplaatsen tussen beide. Dit scenario kan een aanvraag waarin een teamlid worden verplaatst tussen teams en hun abonnement moet worden toegewezen aan het nieuwe team zijn.

## <a name="next-steps"></a>Volgende stappen

* [Een virtuele machine inrichten](azure-stack-provision-vm.md)
