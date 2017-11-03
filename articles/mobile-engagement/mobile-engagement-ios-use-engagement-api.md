---
title: Het gebruik van de API Engagement voor iOS
description: Meest recente iOS SDK - het gebruik van de API Engagement voor iOS
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 1fb4509e-3804-46c1-949f-1cf727f91f9f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: a31424da98205e97bdf57010cccfd044360f03dd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-the-engagement-api-on-ios"></a>Het gebruik van de API Engagement voor iOS
Dit document is een invoegtoepassing voor het document hoe integreren Engagement voor iOS: biedt diepte meer informatie over het gebruik van de Engagement-API voor het rapporteren van de toepassingsstatistieken van uw.

Houd er rekening mee dat als u alleen Engagement voor het rapporteren van uw toepassing sessies, activiteiten, crashes en technische informatie wilt, klikt u vervolgens de eenvoudigste manier is om alle uw aangepaste `UIViewController` objecten overnemen van de bijbehorende `EngagementViewController` klasse.

Als u wilt meer doen, bijvoorbeeld als u wilt rapporteren toepassing specifieke gebeurtenissen, fouten en taken, of als u hebt voor het rapporteren van activiteiten van uw toepassing op een andere manier dan de geïmplementeerd in de `EngagementViewController` klassen, moet u de Engagement-API gebruiken.

De Engagement-API wordt geleverd door de `EngagementAgent` klasse. Een exemplaar van deze klasse kan worden opgehaald door het aanroepen van de `[EngagementAgent shared]` statische methode (Houd er rekening mee dat de `EngagementAgent` object dat wordt geretourneerd is een singleton).

Voordat u een API-aanroepen de `EngagementAgent` object moet worden geïnitialiseerd door het aanroepen van de methode`[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`

## <a name="engagement-concepts"></a>Engagement-concepten
De volgende onderdelen verfijnen de algemene [Mobile Engagement-concepten](mobile-engagement-concepts.md) voor het iOS-platform.

### <a name="session-and-activity"></a>`Session` en `Activity`
Een *activiteit* wordt meestal gekoppeld aan één scherm van de toepassing, dat wil zeggen de *activiteit* wordt gestart wanneer het scherm wordt weergegeven en stopt wanneer het scherm is gesloten: dit is het geval wanneer de Engagement SDK is geïntegreerd met behulp van de `EngagementViewController` klassen.

Maar *activiteiten* kunnen ook handmatig met behulp van de Engagement-API worden beheerd. Hierdoor kan het splitsen van een bepaald scherm in verschillende sub-onderdelen voor meer informatie over het gebruik van dit scherm (bijvoorbeeld hoe vaak bekend is en hoe lang dialoogvensters worden gebruikt in dit scherm).

## <a name="reporting-activities"></a>Rapportage
### <a name="user-starts-a-new-activity"></a>Gebruiker start een nieuwe activiteit
            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

U moet aan te roepen `startActivity()` elke keer dat de wijzigingen van de gebruiker-activiteit. De eerste aanroep van deze functie wordt een nieuwe gebruikerssessie gestart.

### <a name="user-ends-his-current-activity"></a>Gebruiker eindigt zijn huidige activiteit
            [[EngagementAgent shared] endActivity];

> [!WARNING]
> U moet **nooit** aanroep van deze functie in het telefoonboek, behalve als u wilt een gebruik van uw toepassing in verschillende sessies splitsen: een aanroep van deze functie uiteindelijk onmiddellijk, de huidige sessie dus een volgende aanroep `startActivity()` een nieuwe sessie wilt starten. Deze functie wordt automatisch door de SDK aangeroepen wanneer uw toepassing wordt gesloten.
> 
> 

## <a name="reporting-events"></a>Melden van gebeurtenissen
### <a name="session-events"></a>Sessiegebeurtenissen
Sessiegebeurtenissen worden meestal gebruikt voor het rapporteren van de acties die worden uitgevoerd door een gebruiker tijdens de sessie.

**Voorbeeld zonder extra gegevens:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:nil];
            ...
       }
       [...]
    }

**Voorbeeld met extra gegevens:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        NSMutableDictionary* extras = [NSMutableDictionary dictionary];
        [extras setObject:[NSNumber numberWithInt:toInterfaceOrientation] forKey:@"to_orientation_id"];
        [extras setObject:[NSNumber numberWithDouble:duration] forKey:@"duration"];
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:extras];
            ...
       }
       [...]
    }

### <a name="standalone-events"></a>Zelfstandige gebeurtenissen
Strijd sessiegebeurtenissen, kunnen zelfstandige gebeurtenissen worden gebruikt buiten de context van een sessie.

**Voorbeeld:**

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

## <a name="reporting-errors"></a>Rapportage van fouten
### <a name="session-errors"></a>Sessie-fouten
Sessie-fouten worden gewoonlijk gebruikt voor het rapporteren van de fouten die invloed hebben op de gebruiker tijdens de sessie.

**Voorbeeld:**

    /** The user has entered invalid data in a form */
    @implementation MyViewController {
      [...]
      -(void)onMyFormSubmitted:(MyForm*)form {
        [...]
        /* The user has entered an invalid email address */
        [[EngagementAgent shared] sendSessionError:@"sign_up_email" extras:nil]
        [...]
      }
      [...]
    }

### <a name="standalone-errors"></a>Zelfstandige fouten
In tegenstelling tot sessie fouten, kunnen zelfstandige fouten buiten de context van een sessie worden gebruikt.

**Voorbeeld:**

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

## <a name="reporting-jobs"></a>Rapportage van taken
**Voorbeeld:**

Stel dat u wilt rapporteren van de duur van uw aanmelding:

    [...]
    -(void)signIn
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      [... sign in ...]

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    }
    [...]

### <a name="report-errors-during-a-job"></a>Rapportfouten tijdens een taak
Fouten kunnen zijn gerelateerd aan een actieve taak in plaats van te wijten aan de huidige gebruikerssessie.

**Voorbeeld:**

Stel dat u wilt een fout gemeld tijdens uw aanmelding:

    [...]
    -(void)signin
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      BOOL success = NO;
      while (!success) {
        /* Try to sign in */
        NSError* error = nil;
        [self trySigin:&error];
        success = error == nil;

        /* If an error occured report it */
        if(!success)
        {
          [[EngagementAgent shared] sendJobError:@"sign_in_error"
                         jobName:@"sign_in"
                          extras:[NSDictionary dictionaryWithObject:[error localizedDescription] forKey:@"error"]];

          /* Retry after a moment */
          [NSThread sleepForTimeInterval:20];
        }
      }

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    };
    [...]

### <a name="events-during-a-job"></a>Gebeurtenissen gedurende een project
Gebeurtenissen kunnen zijn gerelateerd aan een actieve taak in plaats van te wijten aan de huidige gebruikerssessie.

**Voorbeeld:**

Stel, we hebben een sociaal netwerk en we gebruiken een taak voor het rapport de totale tijd gedurende welke de gebruiker is verbonden met de server. De gebruiker berichten ontvangen zijn vrienden, dit is een taakgebeurtenis.

    [...]
    - (void) signin
    {
      [...Sign in code...]
      [[EngagementAgent shared] startJob:@"connection" extras:nil];
    }
    [...]
    - (void) signout
    {
      [...Sign out code...]
      [[EngagementAgent shared] endJob:@"connection"];
    }
    [...]
    - (void) onMessageReceived
    {
      [...Notify user...]
      [[EngagementAgent shared] sendJobEvent:@"connection" jobName:@"message_received" extras:nil];
    }
    [...]

## <a name="extra-parameters"></a>Extra parameters
Willekeurige gegevens kunnen worden gekoppeld aan gebeurtenissen, fouten, activiteiten en taken.

Deze gegevens kan worden onderverdeeld, iOS van NSDictionary klasse wordt gebruikt.

Houd er rekening mee dat extra's kunnen bevatten `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` of andere `NSDictionary` exemplaren.

> [!NOTE]
> De extra parameter wordt geserialiseerd in JSON. Als u doorgeven verschillende objecten dan de wilt hierboven beschreven, moet u de volgende methode implementeren in uw klasse:
> 
> -(NSString*) JSONRepresentation;
> 
> De methode moet een JSON-weergave van het object retourneren.
> 
> 

### <a name="example"></a>Voorbeeld
    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### <a name="limits"></a>Limieten
#### <a name="keys"></a>Sleutels
Elke sleutel in de `NSDictionary` moet overeenkomen met de volgende reguliere expressie:

`^[a-zA-Z][a-zA-Z_0-9]*`

Dit betekent dat sleutels met ten minste één letter beginnen moeten, gevolgd door letters, cijfers of onderstrepingstekens (\_).

#### <a name="size"></a>Grootte
Extra's zijn beperkt tot **1024** tekens per aanroep (eenmaal gecodeerd in JSON door de agent Engagement).

In het vorige voorbeeld is de JSON naar de server verzonden 58 tekens bevatten:

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

## <a name="reporting-application-information"></a>Rapportage-toepassingsinformatie
U kunt handmatig rapporteren met behulp van het bijhouden van gegevens (of een andere toepassing specifieke gegevens) de `sendAppInfo:` functie.

Merk op dat deze gegevens stapsgewijs kunnen worden verzonden: alleen de meest recente waarde voor een opgegeven sleutel worden behouden voor een bepaald apparaat.

Gebeurtenis extra's, zoals de `NSDictionary` klasse wordt gebruikt voor abstracte toepassingsinformatie, houd er rekening mee dat-matrices of subquery woordenlijsten worden behandeld als platte tekenreeksen (met behulp van de JSON-serialisatie).

**Voorbeeld:**

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### <a name="limits"></a>Limieten
#### <a name="keys"></a>Sleutels
Elke sleutel in de `NSDictionary` moet overeenkomen met de volgende reguliere expressie:

`^[a-zA-Z][a-zA-Z_0-9]*`

Dit betekent dat sleutels met ten minste één letter beginnen moeten, gevolgd door letters, cijfers of onderstrepingstekens (\_).

#### <a name="size"></a>Grootte
Toepassingsgegevens zijn beperkt tot **1024** tekens per aanroep (eenmaal gecodeerd in JSON door de agent Engagement).

In het vorige voorbeeld is de JSON naar de server verzonden 44 tekens bevatten:

    {"birthdate":"1983-12-07","gender":"female"}
