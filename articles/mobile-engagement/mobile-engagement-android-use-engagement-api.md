---
title: Het gebruik van de Engagement-API voor Android
description: Meest recente Android SDK - API van de betrokkenheid op Android gebruiken
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 09b62659-82ae-4a55-8784-fca0b6b22eaf
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: na
ms.topic: article
ms.date: 07/25/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: d353cd2fe47c54a0282cc5bb1b22b4a56e0cd82c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-engagement-api-on-android"></a>Het gebruik van de Engagement-API voor Android
Dit document is een invoegtoepassing voor het document [Reporting geavanceerde opties voor Android Mobile Engagement SDK](mobile-engagement-android-advanced-reporting.md). Het biedt diepte meer informatie over het gebruik van de Engagement-API voor het rapporteren van de toepassingsstatistieken van uw.

Houd er rekening mee dat als u alleen Engagement voor het rapporteren van uw toepassing sessies, activiteiten, crashes en technische informatie wilt, klikt u vervolgens de eenvoudigste manier is om alle uw `Activity` onderliggende klassen overnemen van de bijbehorende `EngagementActivity` klasse.

Als u wilt meer doen, bijvoorbeeld als u wilt rapporteren toepassing specifieke gebeurtenissen, fouten en taken, of als u hebt voor het rapporteren van activiteiten van uw toepassing op een andere manier dan de geïmplementeerd in de `EngagementActivity` klassen, moet u de Engagement-API gebruiken.

De Engagement-API wordt geleverd door de `EngagementAgent` klasse. Een exemplaar van deze klasse kan worden opgehaald door het aanroepen van de `EngagementAgent.getInstance(Context)` statische methode (Houd er rekening mee dat de `EngagementAgent` object dat wordt geretourneerd is een singleton).

## <a name="engagement-concepts"></a>Engagement-concepten
De volgende onderdelen verfijnen de algemene [Mobile Engagement-concepten](mobile-engagement-concepts.md), voor het Android-platform.

### <a name="session-and-activity"></a>`Session` en `Activity`
Als de gebruiker meer dan een paar seconden tussen twee niet-actief blijft *activiteiten*, klikt u vervolgens de volgorde van de *activiteiten* wordt gesplitst in twee afzonderlijke *sessies*. Deze enkele seconden worden de 'sessietime-out' genoemd.

Een *activiteit* wordt meestal gekoppeld aan één scherm van de toepassing, dat wil zeggen de *activiteit* wordt gestart wanneer het scherm wordt weergegeven en stopt wanneer het scherm is gesloten: dit is het geval wanneer de Engagement SDK is geïntegreerd met behulp van de `EngagementActivity` klassen.

Maar *activiteiten* kunnen ook handmatig met behulp van de Engagement-API worden beheerd. Hierdoor kan het splitsen van een bepaald scherm in verschillende sub-onderdelen voor meer informatie over het gebruik van dit scherm (bijvoorbeeld hoe vaak bekend is en hoe lang dialoogvensters worden gebruikt in dit scherm).

## <a name="reporting-activities"></a>Rapportage
> [!IMPORTANT]
> U hoeft niet te rapport activiteiten, zoals beschreven in dit gedeelte als u de `EngagementActivity` klasse en varianten zoals wordt beschreven in de manier waarop integreren engagement voor Android-document.
> 
> 

### <a name="user-starts-a-new-activity"></a>Gebruiker start een nieuwe activiteit
            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing the current activity is required for Reach to display in-app notifications, passing null will postpone such announcements and polls.

U moet aan te roepen `startActivity()` elke keer dat de wijzigingen van de gebruiker-activiteit. De eerste aanroep van deze functie wordt een nieuwe gebruikerssessie gestart.

De beste plaats voor aanroep van deze functie is op elke activiteit `onResume` retouraanroep.

### <a name="user-ends-his-current-activity"></a>Gebruiker eindigt zijn huidige activiteit
            EngagementAgent.getInstance(this).endActivity();

U moet aan te roepen `endActivity()` ten minste eenmaal wanneer de gebruiker is voltooid, de laatste activiteit. Dit de Engagement SDK informeert dat de gebruiker die momenteel niet actief is en dat de gebruikerssessie moet worden gesloten eenmaal de sessietime-out verloopt (als u aanroept `startActivity()` voordat de sessietime-out is verlopen, wordt de sessie gewoon hervat).

De beste plaats voor aanroep van deze functie is op elke activiteit `onPause` retouraanroep.

## <a name="reporting-events"></a>Melden van gebeurtenissen
### <a name="session-events"></a>Sessiegebeurtenissen
Sessiegebeurtenissen worden meestal gebruikt voor het rapporteren van de acties die worden uitgevoerd door een gebruiker tijdens de sessie.

**Voorbeeld zonder extra gegevens:**

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

**Voorbeeld met extra gegevens:**

            public MyActivity extends EngagementActivity {
              [...]
              @Override
              public boolean onMenuItemSelected(int featureId, MenuItem item) {
                Bundle extras = new Bundle();
                extras.putInt("id", item.getItemId());
                getEngagementAgent().sendSessionEvent("menu_selected", extras);
              }
              [...]
            }

### <a name="standalone-events"></a>Zelfstandige gebeurtenissen
In tegenstelling tot sessiegebeurtenissen, kunnen zelfstandige gebeurtenissen plaatsvinden buiten de context van een sessie.

**Voorbeeld:**

Stel dat u wilt rapport gebeurtenissen plaatsvinden wanneer een broadcast-ontvanger is geactiveerd:

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

## <a name="reporting-errors"></a>Rapportage van fouten
### <a name="session-errors"></a>Sessie-fouten
Sessie-fouten worden gewoonlijk gebruikt voor het rapporteren van de fouten die invloed hebben op de gebruiker tijdens de sessie.

**Voorbeeld:**

            /** The user has entered invalid data in a form */
            public MyActivity extends EngagementActivity {
              [...]
              public void onMyFormSubmitted(MyForm form) {
                [...]
                /* The user has entered an invalid email address */
                getEngagementAgent().sendSessionError("sign_up_email", null);
                [...]
              }
              [...]
            }

### <a name="standalone-errors"></a>Zelfstandige fouten
Zelfstandige fouten kunnen strijd sessie-fouten optreden buiten de context van een sessie.

**Voorbeeld:**

Het volgende voorbeeld laat zien hoe een fout rapporteren wanneer het geheugen op de telefoon lage wordt terwijl het toepassingsproces van uw wordt uitgevoerd.

            public MyApplication extends EngagementApplication {

              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

## <a name="reporting-jobs"></a>Rapportage van taken
### <a name="example"></a>Voorbeeld
Stel dat u wilt rapporteren van de duur van uw aanmelding:

            [...]
            public void signIn(Context context, ...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has started */
              engagementAgent.startJob("sign_in", null);

              [... sign in ...]

              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="report-errors-during-a-job"></a>Rapportfouten tijdens een taak
Fouten kunnen zijn gerelateerd aan een actieve taak in plaats van te wijten aan de huidige gebruikerssessie.

**Voorbeeld:**

Stel dat u wilt rapporteren is een fout tijdens u aanmelden proces:

[...] openbare void aanmelding (Context context,...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has been started */
              engagementAgent.startJob("sign_in", null);

              /* Try to sign in */
              while(true)
                try {
                  trySignin();
                  break;
                }
                catch(Exception e) {
                  /* Report the error to Engagement */
                  engagementAgent.sendJobError("sign_in_error", "sign_in", null);

                  /* Retry after a moment */
                  sleep(2000);
                }
              [...]
              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="reporting-events-during-a-job"></a>Melden van gebeurtenissen gedurende een project
Gebeurtenissen kunnen zijn gerelateerd aan een actieve taak in plaats van te wijten aan de huidige gebruikerssessie.

**Voorbeeld:**

Stel, we hebben een sociaal netwerk en we gebruiken een taak voor het rapport de totale tijd gedurende welke de gebruiker is verbonden met de server. De gebruiker kan Blijf op de achtergrond, zelfs wanneer hij maakt gebruik van een andere toepassing of wanneer de telefoon in de slaapstand staat, dus er geen sessie is.

De gebruiker berichten ontvangen zijn vrienden, dit is een taakgebeurtenis.

            [...]
            public void signin(Context context, ...) {
              [...Sign in code...]
              EngagementAgent.getInstance(context).startJob("connection", null);
            }
            [...]
            public void signout(Context context) {
              [...Sign out code...]
              EngagementAgent.getInstance(context).endJob("connection");
            }
            [...]
            public void onMessageReceived(Context context) {
              [...Notify in status bar...]
              EngagementAgent.getInstance(context).sendJobEvent("message_received", "connection", null);
            }
            [...]

## <a name="extra-parameters"></a>Extra parameters
Willekeurige gegevens kunnen worden gekoppeld aan gebeurtenissen, fouten, activiteiten en taken.

Deze gegevens kan worden onderverdeeld, wordt de Android-bundel klasse (daadwerkelijk, werkt als extra parameters in Android Intents). Houd er rekening mee dat een bundel-matrices of een andere bundel exemplaren kan bevatten.

> [!IMPORTANT]
> Als u in de parcelable of serializable parameters plaatst, controleert u of hun `toString()` methode om te retourneren van een leesbare tekenreeks is geïmplementeerd. Serialiseerbaar klassen die niet tijdelijke velden bevatten die niet geserialiseerd zijn brengt Android vastlopen wanneer u wordt gebeld`bundle.putSerializable("key",value);`
> 
> [!WARNING]
> Sparse matrices in extra parameters worden niet ondersteund, dat wil zeggen won't worden geserialiseerd als een matrix. U moet deze converteren naar standard matrices voordat u deze in extra parameters.
> 
> 

### <a name="example"></a>Voorbeeld
            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### <a name="limits"></a>Limieten
#### <a name="keys"></a>Sleutels
Elke sleutel in de `Bundle` moet overeenkomen met de volgende reguliere expressie:

`^[a-zA-Z][a-zA-Z_0-9]*`

Dit betekent dat sleutels met ten minste één letter beginnen moeten, gevolgd door letters, cijfers of onderstrepingstekens (\_).

#### <a name="size"></a>Grootte
Extra's zijn beperkt tot **1024** tekens per aanroep (eenmaal gecodeerd in JSON door de Engagement-service).

In het vorige voorbeeld is de JSON naar de server verzonden 58 tekens bevatten:

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

## <a name="reporting-application-information"></a>Rapportage-toepassingsinformatie
U kunt handmatig rapporteren met behulp van het bijhouden van gegevens (of een andere toepassing specifieke gegevens) de `sendAppInfo()` functie.

Merk op dat deze gegevens stapsgewijs kunnen worden verzonden: alleen de meest recente waarde voor een opgegeven sleutel worden behouden voor een bepaald apparaat.

Zoals gebeurtenis extra's, wordt de klasse bundel gebruikt voor abstracte toepassingsinformatie, houd er rekening mee dat matrices of subquery bundels worden behandeld als platte tekenreeksen (met behulp van de JSON-serialisatie).

### <a name="example"></a>Voorbeeld
Hier volgt een voorbeeld van code voor het verzenden van de gebruiker geslacht en Geboortedatum:

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### <a name="limits"></a>Limieten
#### <a name="keys"></a>Sleutels
Elke sleutel in de `Bundle` moet overeenkomen met de volgende reguliere expressie:

`^[a-zA-Z][a-zA-Z_0-9]*`

Dit betekent dat sleutels met ten minste één letter beginnen moeten, gevolgd door letters, cijfers of onderstrepingstekens (\_).

#### <a name="size"></a>Grootte
Toepassingsgegevens zijn beperkt tot **1024** tekens per aanroep (eenmaal gecodeerd in JSON door de Engagement-service).

In het vorige voorbeeld is de JSON naar de server verzonden 44 tekens bevatten:

            {"expiration":"2016-12-07","status":"premium"}
