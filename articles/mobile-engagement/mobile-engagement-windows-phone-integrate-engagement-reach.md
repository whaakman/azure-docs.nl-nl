---
title: Windows Phone Silverlight Reach-SDK-integratie
description: Het integreren van Azure Mobile Engagement bereiken met Windows Phone Silverlight-Apps
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: d3516a6b-db9f-4cdb-a475-4148edf81af1
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 0738f33df94d14fbb393bfaaf09e94c6560213cc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="windows-phone-silverlight-reach-sdk-integration"></a>Windows Phone Silverlight Reach-SDK-integratie
U moet volgen de procedure integratie in de [Windows Phone Silverlight Engagement SDK-integratie](mobile-engagement-windows-phone-integrate-engagement.md) voordat u deze handleiding.

## <a name="embed-the-engagement-reach-sdk-into-your-windows-phone-silverlight-project"></a>De SDK voor het bereiken van Engagement insluiten in uw Windows Phone Silverlight-project
U beschikt niet over iets om toe te voegen. `EngagementReach`verwijzingen en bronnen bevinden zich al in uw project.

> [!TIP]
> U kunt afbeeldingen die zich in de `Resources` map van uw project, met name het merk-pictogram (die standaard ingesteld op het pictogram Engagement).
> 
> 

## <a name="add-the-capabilities"></a>De mogelijkheden toevoegen
De SDK Engagement bereiken moet enkele aanvullende mogelijkheden.

Open uw `WMAppManifest.xml` -bestand en zorg dat de volgende mogelijkheden worden gedefinieerd:

* `ID_CAP_PUSH_NOTIFICATION`
* `ID_CAP_WEBBROWSERCOMPONENT`

De eerste die wordt gebruikt door de service MPNS om toe te staan van de weergave van toast-melding. De tweede waarde wordt gebruikt voor het insluiten van een browser-taak in de SDK.

Bewerk de `WMAppManifest.xml` -bestand en voeg binnen de `<Capabilities />` tag:

    <Capability Name="ID_CAP_PUSH_NOTIFICATION" />
    <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

## <a name="enable-the-microsoft-push-notification-service"></a>De Microsoft Push Notification-Service inschakelen
Als u wilt gebruiken de **Microsoft Push Notification Service** (MPNS genoemd) uw `WMAppManifest.xml` bestand moet hebben een `<App />` tag met een `Publisher` -kenmerk ingesteld op de naam van uw project.

## <a name="initialize-the-engagement-reach-sdk"></a>Initialiseren van het bereik Engagement SDK
### <a name="engagement-configuration"></a>Engagement configuratie
De configuratie van de Engagement is gecentraliseerd in de `Resources\EngagementConfiguration.xml` -bestand van uw project.

Dit bestand om op te geven van reach-configuratie bewerken:

* *Optionele*, geef aan of de native pushberichten (MPNS) is geactiveerd of geen tussen `<enableNativePush>` en `</enableNativePush>` tags (`true` standaard).
* *Optionele*, geef de naam van de push-kanaal tussen `<channelName>` en `</channelName>` tags, bieden dezelfde dat uw toepassing momenteel kan gebruiken of laat het veld leeg.

Als u in plaats daarvan tijdens runtime opgeven wilt, kunt u de volgende methode voordat de initialisatie van de Engagement-agent kunt aanroepen:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    engagementConfiguration.Reach.EnableNativePush = true;                  
    /* [Optional] whether the native push (MPNS) is activated or not. */

    engagementConfiguration.Reach.ChannelName = "YOUR_PUSH_CHANNEL_NAME";   
    /* [Optional] Provide the same channel name that your application may currently use. */

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

> [!TIP]
> U kunt de naam van het kanaal MPNS push van uw toepassing opgeven. Engagement maakt standaard een naam op basis van de appId. U hebben hoeft de naam te geven uzelf, behalve als u van plan bent te gebruiken van het kanaal push buiten Engagement.
> 
> 

### <a name="engagement-initialization"></a>De initialisatie van de engagement
Wijzig de `App.xaml.cs`:

* Toevoegen aan uw `using` instructies:
  
      using Microsoft.Azure.Engagement;
* Invoegen `EngagementReach.Instance.Init` vlak na `EngagementAgent.Instance.Init` in `Application_Launching` :
  
      private void Application_Launching(object sender, LaunchingEventArgs e)
      {
         EngagementAgent.Instance.Init();
         EngagementReach.Instance.Init();
      }
* Invoegen `EngagementReach.Instance.OnActivated` in de `Application_Activated` methode:
  
      private void Application_Activated(object sender, ActivatedEventArgs e)
      {
         EngagementAgent.Instance.OnActivated(e);
         EngagementReach.Instance.OnActivated(e);
      }

> [!IMPORTANT]
> De `EngagementReach.Instance.Init` in een specifieke thread wordt uitgevoerd. U beschikt niet over het zelf te doen.
> 
> 

## <a name="app-store-submission-considerations"></a>Overwegingen voor apps archief verzenden
Microsoft legt sommige regels bij gebruik van de pushmeldingen:

De Microsoft [toepassingsbeleid] -documentatie, punt 2.9:

1) U moet de gebruiker te accepteren om door te ontvangen van pushmeldingen op te vragen. Vervolgens voegt u in uw instellingen voor een manier om de pushmeldingen uitschakelen.

Het object EngagementReach biedt twee methoden voor het beheren van de opt-in/opt-out, `EnableNativePush()` en `DisableNativePush()`. U kunt bijvoorbeeld een optie maken in de instellingen met een in-of uitschakelen MPNS in-of uitschakelen.

U kunt ook bepalen voor het deactiveren van MPNS via de configuratie van de Engagement\<windows phone-sdk-reach-configuratie\>.

> 2.9.1) de toepassing moet eerst beschrijven de meldingen worden opgegeven en **toestemming van de gebruiker snelle (opt-in)**, en **moet bieden een mechanisme waarmee de gebruiker kan afmelden ontvangen van pushmeldingen**. Alle meldingen die worden geleverd met de Microsoft Push Notification-Service moet overeenkomen met de beschrijving voor de gebruiker en moet voldoen aan alle toepasselijke [toepassingsbeleid] [ Content Policies] en [aanvullende vereisten voor specifieke toepassingstypen].
> 
> 

2) U dient niet te veel pushmeldingen gebruiken. Engagement verwerkt door de meldingen voor u.

> 2.9.2) de toepassing en het gebruik van de Microsoft Push Notification-Service moeten niet uitzonderlijk netwerkcapaciteit of bandbreedte van de Microsoft Push Notification-Service gebruiken of anders ten onrechte belasten van een Windows Phone-of andere Microsoft-apparaat of service met buitensporige pushmeldingen, zoals wordt bepaald door Microsoft in alle redelijkheid bepaald en mag niet schadelijk zijn voor of invloed op een Microsoft-netwerken of servers of servers van derden of netwerken die zijn verbonden met de Microsoft Push Notification Service.
> 
> 

3) Vertrouw niet op MPNS criticals gegevens te verzenden. Engagement maakt gebruik van MPNS, zodat deze regel ook voor de campagnes gemaakt binnen de Engagement front-end geldt.

> 2.9.3) de Microsoft Push Notification Service kan niet worden gebruikt voor het verzenden van meldingen die missie kritieke of anderszins zijn kunnen invloed hebben op inzake leven of dood, inclusief en zonder beperking kritieke meldingen gerelateerd aan een medische apparaat of een voorwaarde. WIJST MICROSOFT UITDRUKKELIJK GARANTIE DAT HET GEBRUIK VAN DE MICROSOFT PUSH NOTIFICATION SERVICE OF DE LEVERING VAN MELDINGEN VAN MICROSOFT PUSH NOTIFICATION-SERVICE WORDT ONDERBROKEN, ZONDER FOUTEN, OF ANDERSZINS GEGARANDEERD OM TE WORDEN UITGEVOERD OP BASIS VAN REALTIME.
> 
> 

**Wij garanderen niet dat uw toepassing wordt geaccepteerd door het validatieproces als u bieden geen ondersteuning voor deze aanbevelingen.**

## <a name="handle-data-push-optional"></a>Verwerken van gegevens-push (optioneel)
Als u wilt dat uw toepassing te kunnen ontvangen van gegevens-pushes Reach, hebt u twee gebeurtenissen van de klasse EngagementReach implementeren:

    EngagementReach.Instance.DataPushStringReceived += (body) =>
    {
       Debug.WriteLine("String data push message received: " + body);
       return true;
    };

    EngagementReach.Instance.DataPushBase64Received += (decodedBody, encodedBody) =>
    {
       Debug.WriteLine("Base64 data push message received: " + encodedBody);
       // Do something useful with decodedBody like updating an image view
       return true;
    };

U kunt zien dat de callback van elke methode een Booleaanse waarde retourneert. Engagement verzendt een feedback naar de back-end nadat de gegevens-push. Als de callback ONWAAR is, retourneert de `exit` feedback verzenden zijn. Anders moeten `action`. Als er geen retouraanroep is ingesteld voor de gebeurtenissen de `drop` feedback engagement worden geretourneerd.

> [!WARNING]
> Engagement is niet ontvangen veelvouden feedback voor een gegevens-push. Als u wilt meerdere handlers ingesteld op een gebeurtenis, houd er rekening mee dat de feedback met de laatste overeen wordt een verzonden. In dit geval raden we retourneert altijd dezelfde waarde om te voorkomen dat verwarrend feedback op de front-end.
> 
> 

## <a name="customize-ui-optional"></a>Aanpassen van de gebruikersinterface (optioneel)
### <a name="first-step"></a>Eerste stap
We laten u aanpassen van de gebruikersinterface van het bereik.

Om dit te doen, u moet maken van een subklasse zijn van de `EngagementReachHandler` klasse.

**Voorbeeld van Code:**

    using Microsoft.Azure.Engagement;

    namespace Example
    {
       internal class ExampleReachHandler : EngagementReachHandler
       {
          // Override EngagementReachHandler methods depending on your needs
       }
    }

Vervolgens stelt u de inhoud van de `EngagementReach.Instance.Handler` veld met het aangepaste object in uw `App.xaml.cs` klasse binnen de `Application_Launching` methode.

**Voorbeeld van Code:**

    private void Application_Launching(object sender, LaunchingEventArgs e)
    {
       // your app initialization 
       EngagementReach.Instance.Handler = new ExampleReachHandler();
       // Engagement Agent and Reach initialization
    }

> [!NOTE]
> Engagement maakt standaard gebruik van een eigen implementatie van `EngagementReachHandler`. U hoeft te maken van uw eigen en als u doet dit, hebt u niet elke methode overschrijven. Het standaardgedrag is het selecteren van het basisobject Engagement.
> 
> 

### <a name="layouts"></a>Indelingen
Standaard Reach ingesloten resources van het DLL-bestand gebruikt om de meldingen en pagina's weer te geven.

U kunt echter uw eigen resources gebruiken in overeenstemming met uw huisstijl in deze onderdelen.

U kunt onderdrukken `EngagementReachHandler` methoden in uw subklasse Engagement gebruiken uw indelingen uitgelegd:

**Voorbeeld van Code:**

    // In your subclass of EngagementReachHandler

    public override string GetTextViewAnnouncementUri()
    {
       // return the path of your own xaml
    }

    public override string GetWebViewAnnouncementUri()
    {
       // return the path of your own xaml
    }

    public override string GetPollUri()
    {
       // return the path of your own xaml
    }

    public override EngagementNotificationView CreateNotification(EngagementNotificationViewModel viewModel)
    {
       // return a new instance of your own notification
    }

> [!TIP]
> De `CreateNotification` methode null kan retourneren. De melding niet weergegeven en de reach-campagne wordt verwijderd.
> 
> 

Om te vereenvoudigen de lay-out-implementatie, bieden we ook onze eigen xaml die als basis voor uw code dienen kan. Ze bevinden zich in het archief Engagement SDK (/ src/reach /).

> [!WARNING]
> De bronnen die wij verstrekken zijn exact dezelfde degene die we gebruiken. Dus als u wilt dat ze rechtstreeks wijzigen, vergeet niet om de naamruimte en de naam te wijzigen.
> 
> 

### <a name="notification-position"></a>Positie van de kennisgeving
Standaard wordt een melding in de app weergegeven onder de linkerkant van de toepassing. U kunt dit gedrag wijzigen door het overschrijven van de `GetNotificationPosition` methode van de `EngagementReachHandler` object.

    // In your subclass of EngagementReachHandler

    public override EngagementReachHandler.NotificationPosition GetNotificationPosition(EngagementNotificationViewModel viewModel)
    {
       // return a value of the EngagementReachHandler.NotificationPosition enum (TOP or BOTTOM)
    }

Op dit moment kunt u kiezen uit de `BOTTOM` (standaard) en `TOP` posities.

### <a name="launch-message"></a>Bericht starten
Wanneer een gebruiker klikt op een Systeemmelding (een pop-up), Engagement de app start, de inhoud van de pushberichten laden en de pagina voor de bijbehorende campagne weergegeven.

Er is een vertraging tussen het starten van de toepassing en de weergave van de pagina (afhankelijk van de snelheid van uw netwerk).

Om aan te geven aan de gebruiker iets wordt geladen, moet u een visuele gegevens, zoals een voortgangsbalk zien of een voortgangsindicator opgeven. Engagement kan niet worden verwerkt, maar enkele handlers voor u biedt.

Voor het implementeren van de callback doen:

    /* The application has launched and the content is loading.
     * You should display an indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };

    /* The application has finished loading the content and the page
     * is about to be displayed.
     * You should hide the indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };

    /* The content has been loaded, but an error has occurred.
     * You can provide an information to the user.
     * You should hide the indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

U kunt de callback instellen in uw `Application_Launching` methode van uw `App.xaml.cs` bestand, bij voorkeur voordat de `EngagementReach.Instance.Init()` aanroepen.

> [!TIP]
> Elke handler wordt aangeroepen door de UI-Thread. U hebt geen zorgen te maken wanneer u een MessageBox of iets UI-gerelateerde.
> 
> 

[toepassingsbeleid]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx
[Content Policies]:http://msdn.microsoft.com/library/windows/apps/hh184842(v=vs.105).aspx
[aanvullende vereisten voor specifieke toepassingstypen]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx

