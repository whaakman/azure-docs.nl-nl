---
title: Azure AD v2 iOS Getting Started - instellingen | Microsoft Docs
description: Hoe iOS (Swift)-toepassingen met een API waarvoor toegangstokens door Azure Active Directory-v2-eindpunt kunnen aanroepen
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: d25353a61b2a60bff28aa0679d38110e77d19e64
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
## <a name="setting-up-your-ios-application"></a>Uw iOS-toepassing instellen

Deze sectie bevat stapsgewijze instructies voor het maken van een nieuw project te laten zien hoe een toepassing voor iOS (Swift) worden geïntegreerd met *aanmelden met Microsoft* zodat Web-API's waarvoor een token op te vragen.

> Voorkeur voor het downloaden van dit voorbeeld XCode-project in plaats daarvan? [Downloaden van een project](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip) en doorgaan met de [configuratiestap](#create-an-application-express) voor het configureren van het codevoorbeeld voordat wordt uitgevoerd.


## <a name="install-carthage-to-download-and-build-msal"></a>Carthage te downloaden en te bouwen MSAL installeren
Carthage package manager wordt gebruikt tijdens de evaluatieperiode van MSAL – het behoud van de mogelijkheden van Microsoft wijzigingen aanbrengen in de bibliotheek is geïntegreerd met XCode.

- Download en installeer de nieuwste versie van Carthage [hier](https://github.com/Carthage/Carthage/releases "Carthage download-URL")

## <a name="creating-your-application"></a>Maken van de toepassing

1.  Open Xcode en selecteer`Create a new Xcode project`
2.  Selecteer `iOS`  >  `Single view Application` en klik op *volgende*
3.  Geef een productnaam en klik op *volgende*
4.  Selecteer een map voor het maken van uw app en klik op *maken*

## <a name="build-the-msal-framework"></a>Het Framework MSAL bouwen

Volg de instructies hieronder om te halen en vervolgens de nieuwste versie van Carthage MSAL bibliotheken maken:

1.  Open de terminal bash en Ga naar de hoofdmap van de App
2.  Kopieer de hieronder en plak in de terminal bash een 'Cartfile'-bestand te maken:

```bash
echo "github \"AzureAD/microsoft-authentication-library-for-objc\" \"master\"" > Cartfile
```
<!-- Workaround for Docs conversion bug -->
<ol start="3">
<li>
Kopieer en plak de onderstaande. Met deze opdracht haalt afhankelijkheden in een map Carthage/opdrachten om uitchecken en vervolgens maakt de MSAL-bibliotheek:
</li>
</ol>

```bash
carthage update
```

> Het proces dat hierboven wordt gebruikt om te downloaden en bouwen van de Microsoft Authentication Library (MSAL). MSAL verwerkt ophalen, opslaan in cache en vernieuwen van gebruikerstokens gebruikt voor toegang tot API's die zijn beveiligd door de Azure Active Directory-v2.

## <a name="add-the-msal-framework-to-your-application"></a>Het framework MSAL aan uw toepassing toevoegen
1.  Open in Xcode, de `General` tabblad
2.  Ga naar de `Linked Frameworks and Libraries` sectie en klikt u op`+`
3.  Selecteer `Add other…`
4.  Selecteer: `Carthage`  >  `Build`  >  `iOS`  >  `MSAL.framework` en klik op *Open*. U ziet `MSAL.framework` toegevoegd aan de lijst.
5.  Ga naar `Build Phases` tabblad en klik op `+` pictogram kiezen`New Run Script Phase`
6.  Voeg de volgende inhoud naar de *gebied script*:

```text
/usr/local/bin/carthage copy-frameworks
```

<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
Het volgende toevoegen aan <code>Input Files</code> door te klikken op <code>+</code>:
</li>
</ol>

```text
$(SRCROOT)/Carthage/Build/iOS/MSAL.framework
```

## <a name="creating-your-applications-ui"></a>Maken van de gebruikersinterface van uw toepassing
Een bestand Main.storyboard moet automatisch worden gemaakt als onderdeel van uw project-sjabloon. Volg de onderstaande instructies voor het maken van de gebruikersinterface van de app:

1.  Control + klik `Main.storyboard` online zetten van het contextafhankelijke menu en klik vervolgens op:`Open As` > `Source Code`
2.  Vervang de `<scenes>` knooppunt met de volgende code:

```xml
 <scenes>
    <scene sceneID="tne-QT-ifu">
        <objects>
            <viewController id="BYZ-38-t0r" customClass="ViewController" customModule="MSALiOS" customModuleProvider="target" sceneMemberID="viewController">
                <layoutGuides>
                    <viewControllerLayoutGuide type="top" id="y3c-jy-aDJ"/>
                    <viewControllerLayoutGuide type="bottom" id="wfy-db-euE"/>
                </layoutGuides>
                <view key="view" contentMode="scaleToFill" id="8bC-Xf-vdC">
                    <rect key="frame" x="0.0" y="0.0" width="375" height="667"/>
                    <autoresizingMask key="autoresizingMask" widthSizable="YES" heightSizable="YES"/>
                    <subviews>
                        <label opaque="NO" userInteractionEnabled="NO" contentMode="left" horizontalHuggingPriority="251" verticalHuggingPriority="251" fixedFrame="YES" text="Microsoft Authentication Library" textAlignment="natural" lineBreakMode="tailTruncation" baselineAdjustment="alignBaselines" adjustsFontSizeToFit="NO" translatesAutoresizingMaskIntoConstraints="NO" id="ifd-fu-zjm">
                            <rect key="frame" x="64" y="28" width="246" height="21"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <fontDescription key="fontDescription" type="system" pointSize="17"/>
                            <nil key="textColor"/>
                            <nil key="highlightedColor"/>
                        </label>
                        <label opaque="NO" userInteractionEnabled="NO" contentMode="left" horizontalHuggingPriority="251" verticalHuggingPriority="251" fixedFrame="YES" text="Logging" textAlignment="natural" lineBreakMode="tailTruncation" baselineAdjustment="alignBaselines" adjustsFontSizeToFit="NO" translatesAutoresizingMaskIntoConstraints="NO" id="98g-dc-BPL">
                            <rect key="frame" x="16" y="277" width="62" height="21"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <fontDescription key="fontDescription" type="system" pointSize="17"/>
                            <nil key="textColor"/>
                            <nil key="highlightedColor"/>
                        </label>
                        <button opaque="NO" contentMode="scaleToFill" fixedFrame="YES" contentHorizontalAlignment="center" contentVerticalAlignment="center" buttonType="roundedRect" lineBreakMode="middleTruncation" translatesAutoresizingMaskIntoConstraints="NO" id="2rX-Vv-T1i">
                            <rect key="frame" x="87" y="100" width="200" height="30"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <state key="normal" title="Call Microsoft Graph API"/>
                            <connections>
                                <action selector="callGraphButton:" destination="BYZ-38-t0r" eventType="touchUpInside" id="Kx0-JL-Bv9"/>
                            </connections>
                        </button>
                        <textView clipsSubviews="YES" multipleTouchEnabled="YES" contentMode="scaleToFill" fixedFrame="YES" editable="NO" textAlignment="natural" selectable="NO" translatesAutoresizingMaskIntoConstraints="NO" id="qXW-2z-J7K">
                            <rect key="frame" x="16" y="324" width="343" height="291"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <color key="backgroundColor" white="1" alpha="1" colorSpace="calibratedWhite"/>
                            <accessibility key="accessibilityConfiguration">
                                <accessibilityTraits key="traits" updatesFrequently="YES"/>
                            </accessibility>
                            <fontDescription key="fontDescription" type="system" pointSize="14"/>
                            <textInputTraits key="textInputTraits" autocapitalizationType="sentences"/>
                        </textView>
                        <button opaque="NO" contentMode="scaleToFill" fixedFrame="YES" contentHorizontalAlignment="center" contentVerticalAlignment="center" buttonType="roundedRect" lineBreakMode="middleTruncation" translatesAutoresizingMaskIntoConstraints="NO" id="9u4-b8-vmX">
                            <rect key="frame" x="137" y="138" width="100" height="30"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <state key="normal" title="Sign-Out"/>
                            <connections>
                                <action selector="signoutButton:" destination="BYZ-38-t0r" eventType="touchUpInside" id="kZT-P8-0Zy"/>
                            </connections>
                        </button>
                    </subviews>
                    <color key="backgroundColor" red="1" green="1" blue="1" alpha="1" colorSpace="custom" customColorSpace="sRGB"/>
                </view>
                <connections>
                    <outlet property="loggingText" destination="qXW-2z-J7K" id="uqO-Yw-AsK"/>
                    <outlet property="signoutButton" destination="9u4-b8-vmX" id="OCh-qk-ldv"/>
                </connections>
            </viewController>
            <placeholder placeholderIdentifier="IBFirstResponder" id="dkx-z0-nzr" sceneMemberID="firstResponder"/>
        </objects>
        <point key="canvasLocation" x="140" y="137.18140929535232"/>
    </scene>
</scenes>
```
