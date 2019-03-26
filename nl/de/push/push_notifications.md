---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-15"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Benachrichtigungen mit {{site.data.keyword.mobilepushshort}}
senden
{: #push_notifications}

Sie können Ihre Swift-App erweitern, indem Sie mit dem
{{site.data.keyword.mobilepushshort}}-Service in
{{site.data.keyword.cloud}} Echtzeitbenachrichtigungen an Mobilgeräte
und Webanwendungen senden.

 - Benachrichtigungen können entweder an alle Anwendungsbenutzer oder an eine ausgewählte Gruppe von Benutzern oder Geräten zugestellt werden.
 - Benachrichtigungen werden sowohl im interaktiven als auch im unbeaufsichtigten Modus unterstützt.
 - Kunden können bestimmte Tags oder Topics für Benachrichtigungen abonnieren.
 - Der App-Eigner kann die Anzahl der Geräte, die für den Empfang von Benachrichtigungen registriert sind, sowie die Anzahl der gesendeten Benachrichtigungen analysieren.

Der {{site.data.keyword.mobilepushshort}}-Service kann entweder
im Rahmen einer Starter-Boilerplate für MobileFirst-Services oder als
[dedizierter Service](/docs/dedicated/index.html) von
{{site.data.keyword.cloud_notm}} genutzt werden. Des Weiteren können
Sie Ihre Clientanwendungen mithilfe eines
SDK (Software-Development-Kit) und der
[REST-APIs ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://mobile.{DomainName}/imfpush/){: new_window}
weiterentwickeln.

![Push-Operation im Überblick](images/push_notification_lifecycle.jpg) Abbildung 1. Lebenszyklus des {{site.data.keyword.mobilepushshort}}-Service

## Vorbereitende Schritte
{: #prereqs-push}

Stellen Sie zunächst sicher, dass die folgenden Voraussetzungen gegeben
sind:

 - iOS 8.0+
 - Xcode 7.3, 8.0
 - Swift 2.3 - 4.0
 - CocoaPods oder Carthage

## Schritt 1. Instanz von {{site.data.keyword.mobilepushshort}}
erstellen
{: #push_create}

1. Klicken Sie im {{site.data.keyword.cloud_notm}}-Katalog auf
**Mobil** >
**{{site.data.keyword.mobilepushshort}}**. Die Anzeige für die Servicekonfiguration wird geöffnet.
2. Vergeben Sie für die Serviceinstanz einen Namen oder verwenden Sie den voreingestellten Namen.
3. Klicken Sie auf **Erstellen**.
4. Klicken Sie im Navigationsbereich
auf **Verbindungen**, um eine App auszuwählen und an Ihren
Service zu binden. Falls Sie während der Erstellung keine Bindung herstellen,
können Sie Ihre App auch später an die Serviceinstanz binden.


## Schritt 2. Berechtigungsnachweise für Benachrichtigungsprovider
anfordern
{: #get_creds-push}

Zur Einrichtung des Push Notifications-Service müssen Sie die
erforderlichen Berechtigungsnachweise beim Apple Push Notification-Service
(APNs) abrufen. Führen Sie die Schritte aus, die
[hier ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/mobilepush/push_step_1.html#push_step_1_ios){: new_window} beschrieben sind, um Ihre APNs-Berechtigungsnachweise anzufordern und zu
konfigurieren.


## Schritt 3. Serviceinstanz konfigurieren
{: #config-resource-push}

Damit Sie den {{site.data.keyword.mobilepushshort}}-Service zum
Senden von Benachrichtigungen verwenden können, laden Sie das von Ihnen
erstellte Zertifikat des Typs `.p12` hoch, das über den privaten Schlüssel und die SSL-Zertifikate verfügt, die zum Erstellen und Veröffentlichen Ihrer Anwendung erforderlich sind. Zum Hochladen eines
APNs-Zertifikats können Sie auch die REST-API verwenden.

Sobald die Datei `.cer` Bestandteil Ihres
Schlüsselkettenzugriffs ist, exportieren Sie sie auf Ihren Computer, um ein
Zertifikat des Typs `.p12` zu erstellen.

Weitere Informationen zur Verwendung der APNs finden Sie auf der Seite [iOS Developer Library: Local and Push Notification Programming Guide ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1){: new_window}.

Gehen Sie folgendermaßen vor, um APNs in der Konsole des Push Notification-Service zu konfigurieren:

1. Wählen Sie die Option **Konfigurieren** in der
Konsole des
{{site.data.keyword.mobilepushshort}}-Service aus.
2. Wählen Sie die Option **Mobil** aus, um die
Informationen im Formular **APNs-Berechtigungsnachweise für Push**
zu aktualisieren.
3. Wählen Sie eine der folgenden Optionen aus:
	- Option **Mobil**
		1. Wählen Sie **Sandbox** (Entwicklung) oder
**Produktion** (Verteilung) aus und laden Sie anschließend
das von Ihnen erstellte Zertifikat des Typs `p.12` hoch.
		
![Konfiguration der {{site.data.keyword.mobilepushshort}}-Konsole](images/wizard.jpg)

		2. Geben Sie im Feld **Kennwort** das Kennwort ein,
das der Zertifikatsdatei `.p12` zugeordnet ist, und klicken
Sie dann auf
**Speichern**.
	- Option **Web**
		- Aktualisieren Sie das Formular im Abschnitt "Safari Push" mit den
erforderlichen Informationen.
		- **Websitename**: Der im Benachrichtigungszentrum
angegebene Name der Website.
		- **Push-ID für Website**: Aktualisieren Sie dieses
Feld mit Ihrer Push-ID für die Website, bei der es sich um eine Zeichenfolge
mit umgekehrter Domänenreihenfolge handelt. Beispiel: web.com.acmebanks.www.
		- **Website-URL**: Geben Sie die URL der Website
an, die abonniert wurde, um Push-Benachrichtigungen zu erhalten. Beispiel: https://www.acmebanks.com.
		- **Zulässige Domänen**: Dieser optionale Parameter
definiert eine Liste von Websites, die vom Benutzer eine Berechtigung
anfordern. Geben Sie die URLs als durch Kommas getrennte Werte an. Die Werte im
Feld "Website-URL" werden verwendet, wenn die Information nicht angegeben wird.
		- **Zeichenfolge für URL-Format**: Die URL, die
aufgelöst werden soll, wenn auf die Benachrichtigung geklickt wird. Beispiel: ["https://www.acmebanks.com"]. Achten
Sie darauf, dass die URL das HTTP- oder das HTTPS-Schema verwendet.
		-**Web-Push-Zertifikat für Safari**: Laden Sie das
Zertifikat des Typs `.p12` hoch und geben Sie das Kennwort an.
4. Klicken Sie auf **Speichern**.
	![{{site.data.keyword.mobilepushshort}}-Konsole](images/push_configure_safari.jpg)

## Schritt 4. Client-SDK für Service konfigurieren
{: #service-client-push}

Damit iOS-Anwendungen Push-Benachrichtigungen für Ihre Geräte erhalten
können, müssen Sie das iOS-SDK für den {{site.data.keyword.mobilepushshort}}-Service konfigurieren.

Die {{site.data.keyword.cloud_notm}}-Swift-SDKs für Mobile-Services können entweder mit Cocoapods oder Carthage installiert werden. Weitere Informationen finden Sie unter der Adresse
[https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#setup-client-application](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#setup-client-application).

## Schritt 5. Benachrichtigung senden
{: #send-notify-push}

Nachdem Sie Ihre Anwendung entwickelt haben, können Sie grundlegende Push-Benachrichtigungen senden.

Gehen Sie zum Senden von grundlegenden Push-Benachrichtigungen wie folgt
vor:

1. Wählen Sie **Benachrichtigungen senden** aus und
erstellen Sie eine Nachricht durch Auswahl einer Option **Senden an**. Die
unterstützten Optionen lauten **Gerät nach Tag**,
**Geräte-ID**, **Benutzer-ID**,
**iOS-Geräte**, **Webbenachrichtigungen**
und **Alle Geräte**.
**Hinweis**: Wenn Sie die Option **Alle
Geräte** auswählen, werden die Benachrichtigungen von allen Geräten
empfangen, die
{{site.data.keyword.mobilepushshort}} abonniert haben.

	![Anzeige 'Benachrichtigungen'](images/tag_notification.jpg)

2. Erstellen Sie im Feld **Nachricht** Ihre
Nachricht. Konfigurieren Sie die optionalen Einstellungen durch die
entsprechende Auswahl wie benötigt.
3. Klicken Sie auf **Senden**.
3. Überprüfen Sie, ob Ihre Geräte oder Browser die Benachrichtigung erhalten haben.

Der folgende Screenshot zeigt ein Alertfeld, das eine
Push-Benachrichtigung auf dem Gerät im Vordergrund verarbeitet.
	![Push-Benachrichtigung unter Android im Vordergrund](images/Android_Screenshot.jpg)

Der folgende Screenshot zeigt eine Push-Benachrichtigung im Hintergrund.
	![Push-Benachrichtigung unter Android im Hintergrund](images/background.png)

### Optionale Einstellungen
{: #optional-push}

Sie können die {{site.data.keyword.mobilepushshort}}-Einstellungen für das Senden von Benachrichtigungen an iOS-Geräte anpassen. Die folgenden optionalen Anpassungsoptionen werden unterstützt.

- **Ausweis**: Gibt die Nummer an, die auf dem
Anwendungsausweis angezeigt wird. Beim Standardwert Null (0) wird kein Ausweis
angezeigt.
- **Tonsignal**: Gibt einen Audioclip an, der beim
Empfang einer Benachrichtigung wiedergegeben werden soll. Unterstützt den
Standardwert oder den Namen einer Tonsignalressource, die im App-Paket
enthalten ist.
- **Zusätzliche Nutzdaten**: Gibt die angepassten
Nutzdatenwerte für Ihre Benachrichtigungen an.

Sie haben außerdem die Möglichkeit,
[interaktive Benachrichtigungen](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#interactive-notifications) und [Rich-Media-Benachrichtigungen](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#enabling-rich-media-notifications) zu aktivieren.

## Schritt 6. Zustellung der Benachrichtigungen überwachen
{: #monitor-push}

Der {{site.data.keyword.mobilepushshort}}-Service bietet ein
Überwachungsdienstprogramm, mit dem Sie den Status der gesendeten Nachrichten
überprüfen können. Informationen zum Konfigurieren Ihres
Überwachungsdienstprogramms finden Sie im Abschnitt [Überwachung für iOS-Anwendungen aktivieren](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/tree/Doc#enable-monitoring).

## Nächste Schritte
{: #next-push}

 - Weitere Informationen zum Service enthält die zugehörige [Dokumentation](/docs/services/mobilepush/c_overview_push.html#overview-push), in der Sie außerdem erfahren, wie Sie alle Funktionen nutzen können.

 - Eine Einführung in die Arbeit mit Mobile-Services und {{site.data.keyword.cloud_notm}} finden Sie im Abschnitt [Einführung für mobile Apps in {{site.data.keyword.cloud_notm}}](/docs/services/mobile/index.html).

 - Starter-Kits bieten eine der schnellsten Möglichkeiten zur Nutzung des Leistungsspektrums von {{site.data.keyword.cloud_notm}}. Im [Dashboard für Entwickler für Mobilgeräte](https://cloud.ibm.com/developer/mobile/dashboard) werden die verfügbaren Starter-Kits angezeigt. Sie können den Code herunterladen und die App ausführen.

 - Mithilfe der [Swagger-Benutzerschnittstelle](https://mobile.ng.bluemix.net/imfpush/) können Sie die REST-API-Dokumentation schnell überprüfen.
