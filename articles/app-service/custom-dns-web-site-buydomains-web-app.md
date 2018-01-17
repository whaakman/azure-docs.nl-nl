---
title: Een aangepaste domeinnaam voor Azure-Web-Apps kopen
description: Informatie over het aanschaffen van een aangepaste domeinnaam met een web-app in Azure App Service.
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 70fb0e6e-8727-4cca-ba82-98a4d21586ff
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2017
ms.author: cephalin
ms.openlocfilehash: 13a61caf9b4dff8ffc08970d5a4c09efa9c5f117
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="buy-a-custom-domain-name-for-azure-web-apps"></a>Een aangepaste domeinnaam voor Azure-Web-Apps kopen

App Service-domeinen (preview) zijn op het hoogste niveau van de domeinen die worden beheerd rechtstreeks in Azure. Ze gemakkelijk voor het beheren van aangepaste domeinen voor [Azure Web Apps](app-service-web-overview.md). Deze zelfstudie laat zien hoe u een App Service-domein kopen en DNS-namen toewijzen aan Azure Web Apps.

Dit artikel is voor Azure App Service (Web-Apps, API-Apps, Mobile Apps, Logic Apps). Zie voor de virtuele machine in Azure of Azure Storage, [toewijzen App Service-domein aan Azure VM of Azure Storage](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/31/assign-app-service-domain-to-azure-vm-or-azure-storage/). Zie voor Cloud-Services, [configureren van een aangepaste domeinnaam voor een Azure cloudservice](../cloud-services/cloud-services-custom-domain-name-portal.md).

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

* [Een App Service-app maken](/azure/app-service/), of gebruik een app die u hebt gemaakt voor een andere zelfstudie.
* [Verwijder de bestedingslimiet voor uw abonnement](../billing/billing-spending-limit.md#remove). U kunt App Service-domeinen met een gratis abonnement tegoed kan niet kopen.

## <a name="prepare-the-app"></a>De app voorbereiden

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Gebruik van aangepaste domeinen in Azure Web Apps, uw web-app van [App Service-abonnement](https://azure.microsoft.com/pricing/details/app-service/) moet een betaald laag (**gedeelde**, **Basic**, **standaard**, of  **Premium**). In deze stap maakt ervoor u zorgen dat de web-app is in de ondersteunde prijscategorie.

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Open de [Azure-portal](https://portal.azure.com) en meld u aan met uw Azure-account.

### <a name="navigate-to-the-app-in-the-azure-portal"></a>Navigeer naar de app in de Azure portal

Selecteer in het menu links **App Services**, en selecteer vervolgens de naam van de app.

![Navigatie naar Azure-app in de portal](./media/app-service-web-tutorial-custom-domain/select-app.png)

U ziet de beheerpagina van de App Service-app.  

### <a name="check-the-pricing-tier"></a>Controleer de prijscategorie

Schuif in de navigatiebalk links van de app-pagina naar de **instellingen** sectie en selecteer **opschalen (App Service-abonnement)**.

![Omhoog schalen-menu](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

De huidige tier van de app wordt gemarkeerd door een rand. Controleer of de app is niet in de **vrije** laag. Aangepaste DNS wordt niet ondersteund in de **vrije** laag. 

![Controleer de prijscategorie](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

Als de App Service-abonnement niet **vrije**, sluit de **Kies uw prijscategorie** pagina en doorgaan met [kopen van het domein](#buy-the-domain).

### <a name="scale-up-the-app-service-plan"></a>De App Service-abonnement opschalen

Selecteer een van de lagen niet vrij (**gedeelde**, **Basic**, **standaard**, of **Premium**). 

Klik op **Selecteren**.

![Controleer de prijscategorie](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

Wanneer u de volgende melding ziet, is de schaalbewerking is voltooid.

![Schaal bewerking bevestigen](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

## <a name="buy-the-domain"></a>Het domein kopen

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure
Open de [Azure-portal](https://portal.azure.com/) en meld u aan met uw Azure-account.

### <a name="launch-buy-domains"></a>Kopen domeinen starten
In de **Web-Apps** en klik op de naam van uw web-app, selecteer **instellingen**, en selecteer vervolgens **aangepaste domeinen**
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

In de **aangepaste domeinen** pagina, klikt u op **domein kopen**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

> [!NOTE]
> Als u niet ziet de **Service AppDomains** sectie, moet u de bestedingslimiet op uw Azure-account verwijderen (Zie [vereisten](#prerequisites)).
>
>

### <a name="configure-the-domain-purchase"></a>De aankoop van het domein configureren

In de **Service toepassingsdomein** pagina in de **zoeken naar domein** typt u de domeinnaam die u wilt kopen en typ `Enter`. De voorgestelde beschikbare domeinen worden weergegeven onder het tekstvak. Selecteer een of meer domeinen die u wilt aanschaffen.
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

> [!NOTE]
> De volgende [topleveldomeinen](https://wikipedia.org/wiki/Top-level_domain) worden ondersteund door App Service-domeinen: _com_, _net_, _co.uk_, _org_, _nl_, _in_, _biz_, _org.uk_, en _co.in_.
>
>

Klik op de **contactgegevens** en het domein contactgegevens formulier invullen. Wanneer u klaar bent, klikt u op **OK** om terug te keren naar de pagina App-domein.
   
Het is belangrijk dat u alle vereiste velden met zo nauwkeurig mogelijk invullen. Onjuiste gegevens voor de contactgegevens kan leiden tot mislukte om aan te schaffen domeinen. 

Selecteer vervolgens de gewenste opties voor uw domein. Zie de volgende tabel voor uitleg:

| Instelling | Voorgestelde waarde | Beschrijving |
|-|-|-|
|Privacybescherming | Inschakelen | U meldt zich aan 'Privacy protection', dat van het aankoopbedrag uitmaakt deel _gratis_. Sommige domeinen op het hoogste niveau worden beheerd door registrars die privacybescherming niet ondersteunen, en ze worden weergegeven op de **privacybescherming** pagina. |
| Standaard hostnamen toewijzen | **www** en**@** | Selecteer de gewenste hostnaambindings indien gewenst. Wanneer de aankoop domein voltooid is, kan uw web-app op de geselecteerde hostnamen worden benaderd. Als de web-app zich achter [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/), de optie voor het toewijzen van het hoofddomein niet wordt weergegeven (@), omdat het Traffic Manager biedt geen ondersteuning voor A-records. Nadat de aankoop van het domein is voltooid, kunt u wijzigingen in de toewijzingen van de hostnaam. |

### <a name="accept-terms-and-purchase"></a>Voorwaarden accepteren en kopen

Klik op **juridische voorwaarden** om te controleren van de voorwaarden en de kosten en klik vervolgens op **kopen**.

> [!NOTE]
> Azure DNS-App Service-domeinen gebruiken voor het hosten van de domeinen. Naast de kosten van de registratie van domein toepassing gebruikskosten voor Azure DNS. Zie voor informatie [prijzen van Azure DNS-](https://azure.microsoft.com/pricing/details/dns/).
>
>

Terug in de **Service toepassingsdomein** pagina, klikt u op **OK**. Terwijl de bewerking uitgevoerd wordt, ziet u de volgende meldingen:

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-validate.png)

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-purchase-success.png)

### <a name="test-the-hostnames"></a>De hostnamen testen

Als u een standaard hostnamen aan uw web-app toegewezen hebt, ziet u ook een melding geslaagd voor elke geselecteerde hostnaam. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

U ziet ook de geselecteerde hostnamen in de **aangepaste domeinen** pagina in de **aangepaste hostnamen** sectie. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added.png)

Als u wilt de hostnamen testen, gaat u naar de vermelde hostnamen in de browser. In het voorbeeld in de vorige schermafbeelding, kunt u navigeren naar _kontoso.net_ en _www.kontoso.net_.

## <a name="assign-hostnames-to-web-app"></a>Hostnamen toewijzen aan web-app

Als u niet een of meer standaard hostnamen toewijzen aan uw web-app tijdens het aankoopproces of als u wilt toewijzen van een hostnaam niet wordt vermeld, kunt u een hostnaam op elk gewenst moment toewijzen.

U kunt ook hostnamen in het App Service-domein toewijzen aan andere web-app. De stappen, is afhankelijk van of het App Service-domein en de web-app tot hetzelfde abonnement behoren.

- Ander abonnement: aangepaste DNS-records van het domein van de Service-App in de web-app als een extern gekochte domein toewijzen. Zie voor meer informatie over het toevoegen van aangepaste DNS-namen aan een App-domein [aangepaste DNS-records beheren](#custom). Zie voor informatie over het toewijzen van een extern domein hebt aangeschaft in een web-app [aangepaste DNS-naam van een bestaande toewijzen aan Azure Web Apps](app-service-web-tutorial-custom-domain.md). 
- Hetzelfde abonnement: Gebruik de volgende stappen.

### <a name="launch-add-hostname"></a>Start de hostnaam toevoegen
In de **App Services** pagina, selecteert u de naam van uw web-app die u toewijzen hostnamen wilt te selecteren **instellingen**, en selecteer vervolgens **aangepaste domeinen**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

Controleer of uw aangeschafte domein wordt vermeld de **Service AppDomains** sectie, maar niet selecteren. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-select-domain.png)

> [!NOTE]
> Alle App Service-domeinen in hetzelfde abonnement worden weergegeven in de web-app **aangepaste domeinen** pagina. Als uw domein in de web-app-abonnement, maar u deze niet in de web-app ziet **aangepaste domeinen** pagina, opnieuw de **aangepaste domeinen** pagina of vernieuw de webpagina. Controleer ook de bel melding aan de bovenkant van de Azure-portal voor fouten in de voortgang of maken.
>
>

Selecteer **hostnaam toevoegen**.

### <a name="configure-hostname"></a>Hostname configureren
In de **hostnaam toevoegen** dialoogvenster, typt u de volledig gekwalificeerde domeinnaam van uw App Service-domein of een subdomein. Bijvoorbeeld:

- kontoso.net
- www.kontoso.net
- abc.kontoso.net

Wanneer u klaar bent, selecteert u **valideren**. Het recordtype hostnaam wordt automatisch geselecteerd voor u.

Selecteer **hostnaam toevoegen**.

Als de bewerking voltooid is, ziet u een melding geslaagd voor de hostnaam van de toegewezen.  

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

### <a name="close-add-hostname"></a>Sluit de hostnaam toevoegen
In de **hostnaam toevoegen** pagina, een andere hostnaam toewijzen aan uw web-app, indien gewenst. Wanneer u klaar bent, sluit u de **hostnaam toevoegen** pagina.

U ziet nu de nieuw toegewezen hostname(s) in uw app **aangepaste domeinen** pagina.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added2.png)

### <a name="test-the-hostnames"></a>De hostnamen testen

Navigeer naar de vermelde hostnamen in de browser. In het voorbeeld in de vorige schermafbeelding, kunt u navigeren naar _abc.kontoso.net_.

## <a name="renew-the-domain"></a>Vernieuwen van het domein

Het App Service-domein dat u hebt gekocht is één jaar vanaf het moment van aanschaf geldig. Het domein is standaard geconfigureerd om automatisch vernieuwen door uw betalingsmethode in rekening gebracht voor het volgende jaar. Als u wilt uitschakelen voor de automatische vernieuwing, of als u wilt handmatig vernieuwen van uw domein, voert u de volgende stappen uit.

In de **Web-Apps** en klik op de naam van uw web-app, selecteer **instellingen**, en selecteer vervolgens **aangepaste domeinen**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

In de **Service AppDomains** sectie, selecteert u het domein dat u wilt configureren.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-select-domain.png)

Selecteer in het navigatievenster aan de linkerkant van het domein, **domeinverlenging**. Als u wilt stoppen met het automatisch vernieuwen van uw domein, selecteer **uit**, en vervolgens **opslaan**. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-autorenew.png)

Voor het handmatig vernieuwen van uw domein, selecteer **vernieuwen domein**. Deze knop is echter niet actief tot 90 dagen vóór de vervaldatum van het domein.

<a name="custom"></a>

## <a name="manage-custom-dns-records"></a>Aangepaste DNS-records beheren

In Azure, DNS-records voor een App-domein worden beheerd met [Azure DNS](https://azure.microsoft.com/services/dns/). U kunt toevoegen, verwijderen, en DNS-records bijwerken, net als voor een extern gekochte domein.

### <a name="open-app-service-domain"></a>Open-App Service-domein

Selecteer in de Azure-portal in het menu links **meer Services** > **Service AppDomains**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

Selecteer het domein om te beheren. 

### <a name="access-dns-zone"></a>Toegang tot DNS-zone

Selecteer in het linkermenu van het domein, **DNS-zone**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-dns-zone.png)

Hiermee opent u deze actie de [DNS-zone](../dns/dns-zones-records.md) pagina van uw App Service-domein in Azure DNS. Zie voor meer informatie over het bewerken van DNS-records [DNS-Zones beheren in de Azure portal](../dns/dns-operations-dnszones-portal.md).

## <a name="cancel-purchase-delete-domain"></a>(Domein verwijderen) aankoop annuleren

U hebt vijf dagen om te annuleren van uw aankoop voor een volledige terugbetaling na de aankoop van het App Service-domein. Na vijf dagen, kunt u het domein van de Service-App kunt verwijderen, maar kan geen restitutie ontvangen.

### <a name="open-app-service-domain"></a>Open-App Service-domein

Selecteer in de Azure-portal in het menu links **meer Services** > **Service AppDomains**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

Selecteer het domein dat u wilt annuleren of verwijderen. 

### <a name="delete-hostname-bindings"></a>Hostnaambindings verwijderen

Selecteer in het linkermenu van het domein, **hostnaambindings**. De hostnaambindings van alle Azure-services worden hier weergegeven.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostname-bindings.png)

U kunt het App Service-domein niet verwijderen nadat alle hostnaambindings zijn verwijderd.

De binding voor elke hostnaam verwijderen door het selecteren van **...**   >  **Verwijderen**. Nadat alle bindingen zijn verwijderd, selecteert u **opslaan**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-delete-hostname-bindings.png)

### <a name="cancel-or-delete"></a>Annuleren of verwijderen

Selecteer in het linkermenu van het domein, **overzicht**. 

Als de annulering is op het aangeschafte domein niet verstreken is, selecteert u **aankoop annuleren**. Anders ziet u een **verwijderen** knop in plaats daarvan. Selecteer wilt verwijderen van het domein zonder restitutie **verwijderen**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-cancel.png)

Selecteer om te controleren van de bewerking, **Ja**.

Nadat de bewerking voltooid is, wordt het domein is vrijgegeven voor uw abonnement en beschikbaar zijn voor iedereen om aan te schaffen opnieuw. 

## <a name="direct-default-url-to-a-custom-directory"></a>Standaard-URL naar een aangepaste map omleiden

Standaard stuurt App Service webaanvragen naar de hoofdmap van uw app-code. Om deze zoals leiden naar een submap `public`, Zie [directe standaard-URL naar een aangepaste map](app-service-web-tutorial-custom-domain.md#virtualdir).

## <a name="more-resources"></a>Meer bronnen

[Veelgestelde vragen over: De App Service-domein (preview) en aangepaste domeinen](https://blogs.msdn.microsoft.com/appserviceteam/2017/08/08/faq-app-service-domain-preview-and-custom-domains/)