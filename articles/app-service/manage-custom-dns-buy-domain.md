---
title: Aangepaste domeinnaam in Azure - App Service kopen
description: Meer informatie over het aanschaffen van een aangepaste domeinnaam met een web-app in Azure App Service.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.assetid: 70fb0e6e-8727-4cca-ba82-98a4d21586ff
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2017
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 0c59e892c8fd5a8bcc74d23e16eaabf1dc1a08f0
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58121533"
---
# <a name="buy-a-custom-domain-name-for-azure-app-service"></a>Een aangepaste domeinnaam voor Azure App Service kopen

App Service-domeinen (preview) zijn op het hoogste niveau van de domeinen die rechtstreeks in Azure worden beheerd. Die het gemakkelijk voor het beheren van aangepaste domeinen voor [Azure App Service](overview.md). Deze zelfstudie leert u hoe u een App Service-domein kopen en DNS-namen toewijzen aan Azure App Service.

Zie voor de virtuele machine van Azure of Azure Storage, [toegewezen App Service-domein naar Azure-VM of Azure Storage](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/31/assign-app-service-domain-to-azure-vm-or-azure-storage/). Zie voor Cloud Services, [configureren van een aangepaste domeinnaam voor een Azure-cloudservice](../cloud-services/cloud-services-custom-domain-name-portal.md).

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

* [Maak een App Service-app](/azure/app-service/), of gebruik een app die u hebt gemaakt voor een andere zelfstudie.
* [Verwijder de bestedingslimiet voor uw abonnement](../billing/billing-spending-limit.md#remove). U kunt App Service-domeinen met een gratis abonnementstegoed kan niet kopen.

## <a name="prepare-the-app"></a>De app voorbereiden

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Gebruik van aangepaste domeinen in Azure App Service, uw app van [App Service-plan](https://azure.microsoft.com/pricing/details/app-service/) moet een betaalde laag (**gedeelde**, **Basic**, **Standard**, of  **Premium**). In deze stap maakt ervoor u zorgen dat de app zich in de ondersteunde prijscategorie bevindt.

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Open de [Azure-portal](https://portal.azure.com) en meld u aan met uw Azure-account.

### <a name="navigate-to-the-app-in-the-azure-portal"></a>Navigeer naar de app in de Azure portal

Selecteer in het linkermenu **App Services** en selecteer de naam van uw app.

![Navigatie naar Azure-app in de portal](./media/app-service-web-tutorial-custom-domain/select-app.png)

U ziet de beheerpagina van de App Service-app.  

### <a name="check-the-pricing-tier"></a>Controleer de prijscategorie

Schuif in de navigatiebalk links van de app-pagina naar de sectie **Instellingen** en selecteer **Opschalen (App Service-plan)**.

![Menu Opschalen](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

De huidige laag van de app wordt gemarkeerd door een blauwe rand. Controleer of de app zich niet in de **F1**-laag bevindt. Aangepaste DNS wordt niet ondersteund in de **F1**-laag. 

![Controleer prijscategorie](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

Als de App Service-plan niet in de **F1** tier, sluit de **omhoog schalen** pagina en gaat u naar [kopen van het domein](#buy-the-domain).

### <a name="scale-up-the-app-service-plan"></a>Het App Service-plan opschalen

Selecteer een van de lagen die niet gratis zijn (**D1**, **B1**, **B2**, **B3** of een laag in de categorie **Productie**). Klik op **Aanvullende opties bekijken** voor aanvullende opties.

Klik op **Toepassen**.

![Controleer prijscategorie](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

Wanneer u de volgende melding ziet, is de schaalbewerking voltooid.

![Schaalbewerking bevestigen](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

## <a name="buy-the-domain"></a>Het domein kopen

### <a name="pricing-information"></a>Prijsgegevens
Voor informatie over prijzen Azure App Service-domeinen, gaat u naar de [pagina met prijzen van App Service](https://azure.microsoft.com/pricing/details/app-service/windows/) en blader naar beneden naar App Service-domein.

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure
Open de [Azure-portal](https://portal.azure.com/) en meld u aan met uw Azure-account.

### <a name="launch-buy-domains"></a>Domeinen kopen starten
In de **App Services** en klik op de naam van uw app, selecteer **instellingen**, en selecteer vervolgens **aangepaste domeinen**
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

In de **aangepaste domeinen** pagina, klikt u op **domein kopen**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

> [!NOTE]
> Als u niet zien de **App Service-domeinen** sectie, moet u de bestedingslimiet van uw Azure-account verwijderen (Zie [vereisten](#prerequisites)).
>
>

### <a name="configure-the-domain-purchase"></a>De aankoop van het domein configureren

In de **App Service-domein** pagina, in de **domein zoeken** typt u de domeinnaam die u wilt kopen en typ `Enter`. De voorgestelde beschikbare domeinen worden weergegeven onder het tekstvak. Selecteer een of meer domeinen die u wilt kopen.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

> [!NOTE]
> De volgende [topleveldomeinen](https://wikipedia.org/wiki/Top-level_domain) worden ondersteund door App Service-domeinen: _com_, _net_, _co.uk_, _org_, _nl_, _in_, _biz_, _org.uk_, en _co.in_.
>
>

Klik op de **contactgegevens** en vul het formulier van het domein contactgegevens in. Wanneer u klaar bent, klikt u op **OK** om terug te keren naar de pagina met App Service-domein.

Het is belangrijk dat u alle vereiste velden met zo nauwkeurig mogelijk invullen. Onjuiste gegevens voor de contactgegevens kan resulteren in domeinen kopen is mislukt.

Selecteer vervolgens de gewenste opties voor uw domein. Zie de volgende tabel voor een uitleg:

| Instelling | Voorgestelde waarde | Description |
|-|-|-|
|Privacybescherming | Inschakelen | U meldt zich aan 'Privacy protection', dat van het aankoopbedrag uitmaakt deel _gratis_. Sommige domeinen op het hoogste niveau worden beheerd door registrars die geen ondersteuning voor privacybescherming, en ze worden weergegeven op de **privacybescherming** pagina. |
| Standaard hostnamen toewijzen | **www** en **\@** | Selecteer de gewenste hostnaambindingen, indien gewenst. Wanneer de naamswijziging domein aankoop voltooid is, wordt uw app toegankelijk op de geselecteerde hostnamen. Als de app achter [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/), ziet u niet de optie voor het hoofddomein toewijzen (@), omdat het Traffic Manager biedt geen ondersteuning voor A-records. U kunt wijzigingen aanbrengen aan de hostnaam toewijzingen nadat de domein-aankoop is voltooid. |

### <a name="accept-terms-and-purchase"></a>Voorwaarden accepteren en kopen

Klik op **juridische voorwaarden** aan de voorwaarden en de kosten controleren en klik vervolgens op **kopen**.

> [!NOTE]
> App Service-domeinen worden GoDaddy voor domeinregistratie en Azure DNS gebruiken voor het hosten van de domeinen. Van toepassing naast de kosten van de registratie van domein gebruikskosten voor Azure DNS. Zie voor meer informatie, [prijzen voor Azure DNS](https://azure.microsoft.com/pricing/details/dns/).
>
>

Klik in de **App Service-domein** pagina, klikt u op **OK**. Terwijl de bewerking uitgevoerd wordt, ziet u de volgende meldingen:

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-validate.png)

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-purchase-success.png)

### <a name="test-the-hostnames"></a>De hostnamen testen

Als u standaard hostnamen aan uw app hebt toegewezen, ziet u ook een melding van slagen voor elke geselecteerde hostnaam. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

U ziet ook de geselecteerde hostnamen in de **aangepaste domeinen** pagina, in de **aangepaste hostnamen** sectie. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added.png)

Als u wilt de hostnamen testen, gaat u naar de vermelde hostnamen in de browser. In het voorbeeld in de vorige schermafbeelding, gaat u naar _kontoso.net_ en _www\.kontoso.net_.

## <a name="assign-hostnames-to-app"></a>Hostnamen toewijzen aan app

Als u niet een of meer standaard hostnamen toewijzen aan uw app tijdens het aankoopproces, of als u wilt toewijzen van een hostnaam die niet wordt vermeld, kunt u een hostnaam in op elk gewenst moment toewijzen.

U kunt ook de hostnamen in de App Service-domein toewijzen aan een andere app. De stappen, is afhankelijk van of de App Service-domein en de app tot hetzelfde abonnement behoren.

- Ander abonnement: Kaart aangepaste DNS-records uit de App Service-domein naar de app, zoals een extern aangeschafte-domein. Zie voor meer informatie over het toevoegen van aangepaste DNS-namen aan een App Service-domein [aangepaste DNS-records beheren](#custom). Zie voor informatie over het toewijzen van een extern aangeschafte domein aan een app [een bestaande aangepaste DNS-naam toewijzen in Azure App Service](app-service-web-tutorial-custom-domain.md). 
- Hetzelfde abonnement: Gebruik de volgende stappen.

### <a name="launch-add-hostname"></a>Start hostnaam toevoegen
In de **App Services** pagina, selecteert u de naam van uw app die u toewijzen hostnamen wilt, selecteer **instellingen**, en selecteer vervolgens **aangepaste domeinen**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

Zorg ervoor dat uw aangeschafte domein wordt vermeld de **App Service-domeinen** sectie, maar zonder deze te selecteren. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-select-domain.png)

> [!NOTE]
> Alle App Service-domeinen in hetzelfde abonnement worden weergegeven in de app **aangepaste domeinen** pagina. Als uw domein in van de app-abonnement, maar kunt u deze niet van de App zien **aangepaste domeinen** pagina, probeert u opnieuw de **aangepaste domeinen** pagina of de webpagina vernieuwen. Controleer ook of de melding rinkelende bel aan de bovenkant van de Azure-portal voor fouten bij de uitvoering of maken.
>
>

Selecteer **Hostnaam toevoegen**.

### <a name="configure-hostname"></a>Hostnaam configureren
In de **hostnaam toevoegen** dialoogvenster, typ de volledig gekwalificeerde domeinnaam van uw App Service-domein of elk subdomein. Bijvoorbeeld:

- kontoso.net
- www\.kontoso.net
- abc.kontoso.net

Wanneer u klaar bent, selecteert u **valideren**. De hostnaam recordtype is automatisch voor u geselecteerd.

Selecteer **Hostnaam toevoegen**.

Wanneer de bewerking voltooid is, ziet u een melding van slagen voor de hostnaam van de toegewezen.  

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

### <a name="close-add-hostname"></a>Sluit hostnaam toevoegen
In de **hostnaam toevoegen** pagina, een andere hostnaam toewijzen aan uw app, zoals gewenst. Wanneer u klaar bent, sluit u de **hostnaam toevoegen** pagina.

U ziet nu de zojuist toegewezen hostname(s) van uw App **aangepaste domeinen** pagina.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added2.png)

### <a name="test-the-hostnames"></a>De hostnamen testen

Navigeer naar de vermelde hostnamen in de browser. In het voorbeeld in de vorige schermafbeelding, gaat u naar _abc.kontoso.net_.

## <a name="renew-the-domain"></a>Vernieuwen van het domein

Het App Service-domein dat u hebt gekocht is geldig gedurende één jaar vanaf het moment van aankoop. Het domein is standaard geconfigureerd om te vernieuwen automatisch uw betalingswijze wordt gebruikt voor het komende jaar in rekening gebracht. Als u wilt uitschakelen voor de automatische vernieuwing, of als u wilt handmatig verlengen van uw domein, volgt u deze stappen.

In de **App Services** en klik op de naam van uw app, selecteer **instellingen**, en selecteer vervolgens **aangepaste domeinen**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

In de **App Service-domeinen** sectie, selecteer het domein dat u wilt configureren.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-select-domain.png)

Selecteer in het linkernavigatievenster van het domein, **domeinverlenging**. Als u wilt stoppen met het automatisch vernieuwen van uw domein, selecteer **uit**, en vervolgens **opslaan**. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-autorenew.png)

Als u wilt uw domein handmatig vernieuwen, selecteert u **vernieuwen domein**. Deze knop is echter niet actief tot 90 dagen vóór de vervaldatum van het domein.

<a name="custom"></a>

## <a name="manage-custom-dns-records"></a>Aangepaste DNS-records beheren

In Azure, DNS-records voor een App Service-domein worden beheerd met behulp van [Azure DNS](https://azure.microsoft.com/services/dns/). U kunt toevoegen, verwijderen, en DNS-records bijwerken, net als voor een extern aangeschafte domein.

### <a name="open-app-service-domain"></a>Open App Service Domain

Selecteer in de Azure portal, in het menu links **alle services** > **App Service-domeinen**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

Selecteer het domein om te beheren. 

### <a name="access-dns-zone"></a>Toegang tot DNS-zone

Selecteer in het linkermenu van het domein, **DNS-zone**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-dns-zone.png)

Deze actie opent u de [DNS-zone](../dns/dns-zones-records.md) pagina van uw App Service-domein in Azure DNS. Zie voor meer informatie over het bewerken van DNS-records [over het beheren van DNS-Zones in Azure portal](../dns/dns-operations-dnszones-portal.md).

## <a name="cancel-purchase-delete-domain"></a>Aankoop annuleren (domein verwijderen)

Als u de App Service-domein kopen, hebt u vijf dagen om uw aankoop voor een volledige terugbetaling te annuleren. Na vijf dagen, kunt u de App Service-domein kunt verwijderen, maar kan niet een restitutie ontvangen.

### <a name="open-app-service-domain"></a>Open App Service Domain

Selecteer in de Azure portal, in het menu links **alle services** > **App Service-domeinen**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

Selecteer het domein aan dat u wilt annuleren of verwijderen. 

### <a name="delete-hostname-bindings"></a>Hostnaambindingen verwijderen

Selecteer in het linkermenu van het domein, **hostnaambindingen**. De hostnaambindingen uit alle Azure-services worden hier weergegeven.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostname-bindings.png)

U kunt de App Service-domein niet verwijderen totdat alle hostnaambindingen worden verwijderd.

De binding voor elke hostnaam verwijderen door het selecteren van **...**   >  **Verwijderen**. Nadat alle bindingen zijn verwijderd, selecteert u **opslaan**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-delete-hostname-bindings.png)

### <a name="cancel-or-delete"></a>Annuleren of verwijderen

Selecteer in het linkermenu van het domein, **overzicht**. 

Als de annulering is in de aangeschafte domein niet verstreken is, selecteert u **aankoop annuleren**. Anders is, ziet u een **verwijderen** knop in plaats daarvan. Selecteer wilt verwijderen van het domein zonder een restitutie **verwijderen**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-cancel.png)

Selecteer om te bevestigen dat de bewerking, **Ja**.

Nadat de bewerking voltooid is, wordt het domein is vrijgegeven voor uw abonnement en beschikbaar is voor iedereen opnieuw te kopen. 

## <a name="direct-default-url-to-a-custom-directory"></a>Standaard-URL naar een aangepaste map omleiden

Standaard stuurt App Service webaanvragen naar de hoofdmap van uw app-code. Om te leiden ze naar een submap zoals `public`, Zie [directe standaard-URL naar een aangepaste map](app-service-web-tutorial-custom-domain.md#virtualdir).

## <a name="more-resources"></a>Meer bronnen

[FAQ: App Service-domein (preview) en aangepaste domeinen](https://blogs.msdn.microsoft.com/appserviceteam/2017/08/08/faq-app-service-domain-preview-and-custom-domains/)
