---
lab:
  title: '04: MFA, bedingter Zugriff und AAD Identity Protection'
  module: Module 01 - Manage Identity and Access
ms.openlocfilehash: ed08b595d5fd8128b5932ed0d88214c0791cc7fd
ms.sourcegitcommit: 2eb153f2856445e5afaa218a012cb92e3d48f24b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2021
ms.locfileid: "132625652"
---
# <a name="lab-04-mfa-conditional-access-and-aad-identity-protection"></a>Lab 04: MFA, bedingter Zugriff und AAD Identity Protection
# <a name="student-lab-manual"></a>Lab-Handbuch für Kursteilnehmer

## <a name="lab-scenario"></a>Labszenario

Sie wurden gebeten, einen Proof of Concept für Features zu erstellen, die die Azure Active Directory-Authentifizierung (Azure AD) verbessern. Insbesondere möchten Sie Folgendes auswerten:

- Azure AD Multi-Factor Authentication
- Bedingter Zugriff auf Azure AD
- Azure AD Identity Protection

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

## <a name="lab-objectives"></a>Lab-Ziele

In diesem Lab führen Sie die folgenden Übungen aus:

- Übung 1: Bereitstellen einer Azure-VM mithilfe einer Azure Resource Manager-Vorlage
- Übung 2: Implementieren von Azure MFA
- Übung 3: Implementieren von Azure AD-Richtlinien für bedingten Zugriff 
- Übung 4: Implementieren von Azure AD Identity Protection

## <a name="lab-files"></a>Lab-Dateien:

- **\\Allfiles\\Labs\\04\\az-500-04_azuredeploy.json**
- **\\Allfiles\\Labs\\04\\az-500-04_azuredeploy.parameters.json** 

### <a name="exercise-1-deploy-an-azure-vm-by-using-an-azure-resource-manager-template"></a>Übung 1: Bereitstellen einer Azure-VM mithilfe einer Azure Resource Manager-Vorlage

### <a name="estimated-timing-10-minutes"></a>Geschätzte Zeit: 10 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Bereitstellen einer Azure-VM mithilfe einer Azure Resource Manager-Vorlage.

#### <a name="task-1-deploy-an-azure-vm-by-using-an-azure-resource-manager-template"></a>Aufgabe 1: Bereitstellen einer Azure-VM mithilfe einer Azure Resource Manager-Vorlage

In dieser Aufgabe erstellen Sie eine VM mithilfe einer ARM-Vorlage. Diese VM wird in der letzten Übung für dieses Lab verwendet. 

1. Melden Sie sich am Azure-Portal **`https://portal.azure.com/`** an.

    >**Hinweis**: Melden Sie sich am Azure-Portal mit einem Konto an, das in dem Azure-Abonnement, das Sie für dieses Lab verwenden, über die Rolle „Besitzer“ oder „Mitwirkender“ und die Rolle „Globaler Administrator“ im Azure AD-Mandanten verfügt, der diesem Abonnement zugeordnet ist.

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Text **Benutzerdefinierte Vorlage bereitstellen** ein.

    >**Hinweis**: Sie können auch **Vorlagenbereitstellung (Bereitstellen mithilfe benutzerdefinierter Vorlagen)** aus der **Marketplace**-Liste auswählen.

1. Klicken Sie auf dem Blatt **Benutzerdefinierte Bereitstellung** auf die Option **Eigene Vorlage im Editor erstellen**.

1. Klicken Sie auf dem Blatt **Vorlage bearbeiten** auf **Datei laden**, navigieren Sie zu **\\Allfiles\\Labs\\04\\az-500-04_azuredeploy.json**, und klicken Sie auf **Öffnen**.

    >**Hinweis**: Überprüfen Sie den Inhalt der Vorlage, und beachten Sie, dass sie eine Azure-VM bereitstellt, die Windows Server 2019 Datacenter hostet.

1. Klicken Sie auf dem Blatt **Vorlage bearbeiten** auf **Speichern**.

1. Klicken Sie auf dem Blatt **Benutzerdefinierte Bereitstellung** auf **Vorlage bearbeiten**.

1. Klicken Sie auf dem Blatt **Parameter bearbeiten** auf **Datei laden**, navigieren Sie zu **\\Allfiles\\Labs\\04\\az-500-04_azuredeploy.parameters.json**, und klicken Sie auf **Öffnen**.

    >**Hinweis**: Überprüfen Sie den Inhalt der Parameterdatei, und notieren Sie sich die Werte für adminUsername und adminPassword.

1. Klicken Sie auf dem Blatt **Parameter bearbeiten** auf **Speichern**.

1. Stellen Sie auf dem Blatt **Benutzerdefinierte Bereitstellung** sicher, dass die folgenden Einstellungen konfiguriert sind (übernehmen Sie für alle anderen Einstellungen die Standardwerte):

   |Einstellung|Wert|
   |---|---|
   |Subscription|Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden.|
   |Resource group|Klicken Sie auf **Neu erstellen**, und geben Sie den Namen **AZ500LAB04** ein.|
   |Standort|**(USA) USA, Osten**|
   |VM-Größe|**Standard_D2s_v3**|
   |VM-Name|**az500-04-vm1**|
   |Administratorbenutzername|**Kursteilnehmer**|
   |Administratorkennwort|**Pa55w.rd1234**|
   |Name des virtuellen Netzwerks|**az500-04-vnet1**|

    >**Hinweis**: Informationen zum Identifizieren von Azure-Regionen, in denen Sie Azure-VMs bereitstellen können, finden Sie unter [ **https://azure.microsoft.com/en-us/regions/offers/** ](https://azure.microsoft.com/en-us/regions/offers/).

1. Klicken Sie auf **Überprüfen und erstellen** und dann auf **Erstellen**.

    >**Hinweis**: Warten Sie nicht, bis die Bereitstellung abgeschlossen ist, sondern fahren Sie mit der nächsten Übung fort. Sie verwenden die VM, die in dieser Bereitstellung enthalten ist, in der letzten Übung dieses Labs.

> Ergebnis: Sie haben eine Vorlagenbereitstellung einer Azure-VM **az500-04-vm1** initiiert, die Sie in der letzten Übung dieses Labs verwenden werden.


### <a name="exercise-2-implement-azure-mfa"></a>Übung 2: Implementieren von Azure MFA

### <a name="estimated-timing-30-minutes"></a>Geschätzte Zeit: 30 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Erstellen eines neuen Azure AD-Mandanten.
- Aufgabe 2: Aktivieren der Azure AD Premium P2-Testversion.
- Aufgabe 3: Erstellen von Azure AD-Benutzern und -Gruppen.
- Aufgabe 4: Zuweisen von Azure AD Premium P2-Lizenzen zu Azure AD-Benutzern.
- Aufgabe 5: Konfigurieren von Azure MFA-Einstellungen.
- Aufgabe 6: Überprüfen der MFA-Konfiguration

#### <a name="task-1-create-a-new-azure-ad-tenant"></a>Aufgabe 1: Erstellen eines neuen Azure AD-Mandanten

In dieser Aufgabe erstellen Sie einen neuen Azure AD-Mandanten. 

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Azure Active Directory** ein, und drücken Sie die **EINGABETASTE**.

1. Klicken Sie auf dem Blatt mit der **Übersicht** über Ihren aktuellen Azure AD-Mandanten auf **Mandant verwalten**, und klicken Sie dann auf dem nächsten Bildschirm auf **+ Erstellen**.

1. Stellen Sie auf dem Blatt **Mandant erstellen** auf der Registerkarte **Grundlagen** sicher, dass die Option **Azure Active Directory** ausgewählt ist, und klicken Sie dann auf **Weiter: Konfiguration >** .

1. Geben Sie auf der Registerkarte **Konfiguration** des Blatts **Mandant erstellen** die folgenden Einstellungen an:

   |Einstellung|Wert|
   |---|---|
   |Name der Organisation|**AdatumLab500-04**|
   |Name der Anfangsdomäne|Ein eindeutiger Name, der aus einer Kombination aus Buchstaben und Ziffern besteht|
   |Land oder Region|**USA**|

    >**Hinweis**: Notieren Sie sich den anfänglichen Domänennamen. Sie benötigen ihn später in diesem Lab.

1. Klicken Sie auf **Überprüfen und erstellen** und dann auf **Erstellen**.
2. Fügen Sie Captcha-Code auf dem Blatt **Helfen Sie uns, zu zeigen, dass Sie kein Bot sind** hinzu, und klicken Sie dann auf die Schaltfläche **Senden**. 

    >**Hinweis**: Warten Sie, bis der neue Mandant erstellt wurde. Verwenden Sie das **Benachrichtigungssymbol**, um den Bereitstellungsstatus zu überwachen. 


#### <a name="task-2-activate-azure-ad-premium-p2-trial"></a>Aufgabe 2: Aktivieren der Azure AD Premium P2-Testversion

In dieser Aufgabe registrieren Sie sich für die kostenlose Azure AD Premium P2-Testversion. 

1. Klicken Sie im Azure-Portal auf der Symbolleiste auf das Symbol **Verzeichnis und Abonnement** rechts neben dem Cloud Shell-Symbol. 

1. Klicken Sie auf dem Blatt **Verzeichnis und Abonnement** auf den neu erstellten Mandanten **AdatumLab500-04**.

    >**Hinweis**: Möglicherweise müssen Sie das Browserfenster aktualisieren, wenn der **Eintrag AdatumLab500-04** nicht in der Filterliste **Verzeichnis und Abonnement** angezeigt wird.

1. Klicken Sie auf dem Blatt **AdatumLab500-04** im Abschnitt **Verwalten** auf **Lizenzen.**

1. Klicken Sie auf dem Blatt **Lizenzen \| Übersicht** im Abschnitt **Verwalten** auf **Alle Produkte** und dann auf **+ Ausprobieren/Kaufen**.

1. Klicken Sie auf dem Blatt **Aktivieren** im Abschnitt „Azure AD Premium P2“ auf **Kostenlose Testversion** und dann auf **Aktivieren**.


#### <a name="task-3-create-azure-ad-users-and-groups"></a>Aufgabe 3: Erstellen von Azure AD-Benutzern und -Gruppen.

In dieser Aufgabe erstellen Sie drei Benutzer: aaduser1 (Globaler Administrator), aaduser2 (Benutzer) und aaduser3 (Benutzer). Sie benötigen den Prinzipalnamen und das Kennwort jedes Benutzers für spätere Aufgaben. 

1. Navigieren Sie zurück zum Azure Active Directory-Blatt **AdatumLab500-04**, und klicken Sie im Abschnitt **Verwalten** auf **Benutzer**.

1. Klicken Sie auf dem Blatt **Benutzer \| Alle Benutzer** auf **+ Neuer Benutzer**. 

1. Stellen Sie auf dem Blatt **Neuer Benutzer** sicher, dass die Option **Benutzer erstellen** ausgewählt ist, und geben Sie die folgenden Einstellungen an (übernehmen Sie die Standardwerte für alle anderen Einstellungen), und klicken Sie auf **Erstellen**:

   |Einstellung|Wert|
   |---|---|
   |Benutzername|**aaduser1**|
   |Name|**aaduser1**|
   |Kennwort|Stellen Sie sicher, dass die Option **Kennwort automatisch generieren** ausgewählt ist, und klicken Sie auf Kennwort **anzeigen**.|
   |Gruppen|**0 Gruppen ausgewählt**|
   |Rollen|Klicken Sie auf **Benutzer** und dann auf **Globaler Administrator**, und klicken Sie dann auf **Auswählen**.|
   |Nutzungsstandort|**USA**|  

    >**Hinweis**: Notieren Sie sich den vollständigen Benutzernamen. Sie können den Wert kopieren, indem Sie auf der rechten Seite der Dropdownliste, die den Domänennamen anzeigt, auf die Schaltfläche **In Zwischenablage kopieren** klicken. 

    >**Hinweis**: Notieren Sie sich das Kennwort des Benutzers. Sie benötigen es später in diesem Lab. 

1. Klicken Sie auf dem Blatt **Benutzer \| Alle Benutzer** auf **+ Neuer Benutzer**. 

1. Stellen Sie auf dem Blatt **Neuer Benutzer** sicher, dass die Option **Benutzer erstellen** ausgewählt ist, und geben Sie die folgenden Einstellungen an (übernehmen Sie die Standardwerte für alle anderen Einstellungen):

   |Einstellung|Wert|
   |---|---|
   |Benutzername|**aaduser2**|
   |Name|**aaduser2**|
   |Kennwort|Stellen Sie sicher, dass die Option **Kennwort automatisch generieren** ausgewählt ist, und klicken Sie auf Kennwort **anzeigen**.|
   |Gruppen|**0 Gruppen ausgewählt**|
   |Rollen|**Benutzer**|
   |Nutzungsstandort|**USA**|  

    >**Hinweis**: Notieren Sie sich den vollständigen Benutzernamen und das Kennwort.

1. Klicken Sie auf dem Blatt **Benutzer \| Alle Benutzer** auf **+ Neuer Benutzer**. 

1. Klicken Sie auf **Neuer Benutzer**, vervollständigen Sie die neuen Benutzerkonfigurationseinstellungen, und klicken Sie dann auf **Erstellen**.

   |Einstellung|Wert|
   |---|---|
   |Benutzername|**aaduser3**|
   |Name|**aaduser3**|
   |Kennwort|Stellen Sie sicher, dass die Option **Kennwort automatisch generieren** ausgewählt ist, und klicken Sie auf Kennwort **anzeigen**.|
   |Gruppen|**0 Gruppen ausgewählt**|
   |Rollen|**Benutzer**|
   |Nutzungsstandort|**USA**|  

    >**Hinweis**: Notieren Sie sich den vollständigen Benutzernamen und das Kennwort.

    >**Hinweis**: Zu diesem Zeitpunkt sollten drei neue Benutzer auf der Seite **Benutzer** aufgeführt werden. 
    
#### <a name="task-4-assign-azure-ad-premium-p2-licenses-to-azure-ad-users"></a>Aufgabe 4: Zuweisen von Azure AD Premium P2-Lizenzen zu Azure AD-Benutzern

In dieser Aufgabe weisen Sie jeden Benutzer der Azure Active Directory Premium P2-Lizenz zu.

1. Klicken Sie auf dem Blatt **Benutzer \| Alle Benutzer** auf den Eintrag, der Ihr Benutzerkonto darstellt. 

1. Klicken Sie auf dem Blatt, auf dem die Eigenschaften Ihres Benutzerkontos angezeigt werden, auf **Bearbeiten**. 

1. Navigieren Sie zurück zum Azure Active Directory-Blatt **AdatumLab500-04**, und klicken Sie im Abschnitt **Verwalten** auf **Lizenz**.

1. Klicken Sie auf dem Blatt **Lizenzen \| Übersicht** auf **Alle Produkte**, aktivieren Sie das Kontrollkästchen **Azure Active Directory Premium P2**, und klicken Sie auf **+ Zuweisen**.

1. Klicken Sie auf dem Blatt **Lizenzen zuweisen** auf **+ Benutzer und Gruppen hinzufügen**.

1. Wählen Sie auf dem Blatt **Benutzer** die Optionen **aaduser1**, **aaduser2**, **aaduser3** und Ihr Benutzerkonto aus, und klicken Sie auf **Auswählen**.

1. Klicken Sie auf dem Blatt **Lizenzen zuweisen** auf **Zuweisungsoptionen**, stellen Sie sicher, dass alle Optionen aktiviert sind, klicken Sie auf **Überprüfen und zuweisen**, und klicken Sie auf **Zuweisen**.

1. Melden Sie sich vom Azure-Portal ab, und melden Sie sich mit demselben Konto erneut an. Dieser Schritt ist erforderlich, damit die Lizenzzuweisung wirksam wird.

    >**Hinweis**: An diesem Punkt haben Sie allen Benutzerkonten, die Sie in diesem Lab verwenden, Azure Active Directory Premium P2-Lizenzen zugewiesen. Achten Sie darauf, dass Sie sich abmelden und dann erneut anmelden. 

#### <a name="task-5-configure-azure-mfa-settings"></a>Aufgabe 5: Konfigurieren von Azure MFA-Einstellungen.

In dieser Aufgabe konfigurieren Sie MFA und aktivieren MFA für aaduser1. 

1. Navigieren Sie im Azure-Portal zurück zum Blatt **AdatumLab500-04** des Azure Active Directory-Mandanten.

    >**Hinweis**: Stellen Sie sicher, dass Sie den Azure AD-Mandanten AdatumLab500-04 verwenden.

1. Klicken Sie auf dem Blatt **AdatumLab500-04** des Azure Active Directory-Mandanten im Abschnitt **Verwalten** auf **Sicherheit**.

1. Klicken Sie auf dem Blatt **Sicherheit \| Erste Schritte** im Abschnitt **Verwalten** auf **MFA**.

1. Klicken Sie auf dem Blatt **MFA \|Erste Schritte** auf den Link **Zusätzliche cloudbasierte MFA-Einstellungen**. 

    >**Hinweis**: Dadurch wird eine neue Browserregisterkarte geöffnet, auf der die Seite für **Multi-Factor Authentication** angezeigt wird.

1. Klicken Sie auf der Seite **Multi-Factor Authentication** auf die Registerkarte **Diensteinstellungen**. Sehen Sie sich die **Überprüfungsoptionen** an. Beachten Sie, dass die Optionen **Textnachricht an Telefon**, **Benachrichtigung über mobile App** und **Prüfcode aus mobiler App oder Hardwaretoken** aktiviert sind. Klicken Sie auf **Speichern** und dann auf **Schließen**.

1. Wechseln Sie zur Registerkarte **Benutzer**, klicken Sie auf den Eintrag **aaduser1**, klicken Sie auf den Link **Aktivieren**, und klicken Sie, wenn Sie dazu aufgefordert werden, auf **Multi-Factor Auth aktivieren**.

1. Beachten Sie, dass die Spalte **Multi-Factor Auth-Status** für **aaduser1** jetzt **Aktiviert** ist.

1. Klicken Sie auf **aaduser1**, und beachten Sie, dass an diesem Punkt auch die Option **Erzwingen** verfügbar ist. 

    >**Hinweis**: Das Ändern des Benutzerstatus von „Aktiviert“ in „Erzwungen“ wirkt sich nur auf ältere in Azure AD integrierte Apps aus, die Azure MFA nicht unterstützen und nach der Änderung des Status in „Erzwungen“ die Verwendung von App-Kennwörtern erfordern.

1. Klicken Sie bei ausgewähltem Eintrag **aaduser1** auf **Benutzereinstellungen verwalten**, und überprüfen Sie die verfügbaren Optionen: 

   - Bereitstellen der Kontaktmethoden bei ausgewählten Benutzern erneut anfordern.

   - Alle vorhandenen App-Kennwörter löschen, die von den ausgewählten Benutzern erstellt wurden.

   - Wiederherstellen von mehrstufiger Authentifizierung für alle gespeicherten Geräte.

1. Klicken Sie auf **Abbrechen**, und wechseln Sie zurück zur Browserregisterkarte, auf der das Blatt **Multi-Factor Authentication \| Erste Schritte** im Azure-Portal angezeigt wird.

1. Klicken Sie im Abschnitt **Einstellungen** auf **Betrugswarnung**.

1. Konfigurieren Sie auf dem Blatt **Multi-Factor Authentication \| Betrugswarnung** die folgenden Einstellungen:

   |Einstellung|Wert|
   |---|---|
   |Benutzern das Übermitteln von Betrugswarnungen erlauben|**Ein**|
   |Benutzer, die Betrugsversuche melden, automatisch blockieren|**Ein**|
   |Code zum Melden von Betrugsversuchen während der Begrüßung|**0**|

1. Klicken Sie unten auf der Seite auf **Speichern**.

    >**Hinweis**: An diesem Punkt haben Sie MFA für aaduser1 aktiviert und Einstellungen für Betrugswarnungen eingerichtet. 

1. Navigieren Sie zurück zum Blatt **AdatumLab500-04** des Azure Active Directory-Mandanten, und klicken Sie im Abschnitt **Verwalten** auf **Eigenschaften**. Klicken Sie anschließend unten auf dem Blatt auf den Link **Sicherheitseinstellungen verwalten**. Klicken Sie auf dem Blatt **Sicherheitsstandards aktivieren** auf **Nein**. Wählen Sie **Meine Organisation verwendet bedingten Zugriff** als Grund aus, und klicken Sie dann auf **Speichern**.

    >**Hinweis**: Stellen Sie sicher, dass Sie beim Azure AD-Mandanten **AdatumLab500-04** angemeldet sind. Sie können den Filter **Verzeichnis und Abonnement** verwenden, um zwischen Azure AD-Mandanten zu wechseln. Stellen Sie sicher, dass Sie als Benutzer mit der Rolle „Globaler Administrator“ im Azure AD-Mandanten angemeldet sind.

#### <a name="task-6-validate-mfa-configuration"></a>Aufgabe 6: Überprüfen der MFA-Konfiguration

In dieser Aufgabe überprüfen Sie die MFA-Konfiguration, indem Sie die Anmeldung des Benutzerkontos aaduser1 testen. 

1. Öffnen Sie ein Browserfenster im InPrivate-Modus.

1. Navigieren Sie zum Azure-Portal, und melden Sie sich mit dem Benutzerkonto **aaduser1** an. 

    >**Hinweis**: Für die Anmeldung müssen Sie einen vollqualifizierten Namen des Benutzerkontos **aaduser1** angeben, einschließlich des DNS-Domänennamens des Azure AD-Mandanten, den Sie sich zuvor in diesem Lab notiert haben. Dieser Benutzername weist das Format aaduser1@`<your_tenant_name>`.onmicrosoft.com auf, wobei `<your_tenant_name>` der Platzhalter ist, der Ihren eindeutigen Azure AD-Mandantennamen darstellt. 

1. Wenn Sie dazu aufgefordert werden, klicken Sie im Dialogfeld **Weitere Informationen erforderlich** auf **Weiter**.

    >**Hinweis**: Die Browsersitzung wird an die Seite **Zusätzliche Sicherheitsüberprüfung** weitergeleitet.

1. Wählen Sie auf der Seite **Schützen Sie Ihr Konto** den Link **Ich möchte eine andere Methode einrichten** aus. Wählen Sie in der Dropdownliste **Welche Methode möchten Sie verwenden?** die Option **Telefon** und dann **Bestätigen** aus.

1. Wählen Sie auf der Seite **Schützen Sie Ihr Konto** Ihr Land oder Ihre Region aus, geben Sie Ihre Mobiltelefonnummer im Bereich **Telefonnummer eingeben** ein, stellen Sie sicher, dass die Option **Code per SMS an mich senden** ausgewählt ist, und klicken Sie dann auf **Weiter**.
 
1. Geben Sie auf der Seite **Schützen Sie Ihr Konto** den Code ein, den Sie in der SMS auf Ihrem Mobiltelefon erhalten haben, und klicken Sie auf **Weiter**.

1. Stellen Sie auf der Seite **Schützen Sie Ihr Konto** sicher, dass die Überprüfung erfolgreich war, und klicken Sie auf **Weiter**.

1. Klicken Sie auf der Seite **Schützen Sie Ihr Konto** auf **Ich möchte eine andere Methode verwenden**, wählen Sie in der Dropdownliste **E-Mail** aus, klicken Sie auf **Bestätigen**, geben Sie die E-Mail-Adresse an, die Sie verwenden möchten, und klicken Sie dann auf **Weiter**. Nachdem Sie die entsprechende E-Mail erhalten haben, identifizieren Sie den Code im E-Mail-Text, geben Sie ihn an, und klicken Sie dann auf **Fertig**.

1. Ändern Sie Ihr Kennwort, wenn Sie dazu aufgefordert werden. Denken Sie daran, sich das neue Kennwort zu notieren.

1. Vergewissern Sie sich, dass Sie sich erfolgreich am Azure-Portal angemeldet haben.

1. Melden Sie sich als **aaduser1** ab, und schließen Sie das InPrivate-Browserfenster.

> Ergebnis: Sie haben einen neuen AD-Mandanten erstellt, AD-Benutzer konfiguriert, MFA konfiguriert und die MFA-Benutzeroberfläche für einen Benutzer getestet. 


### <a name="exercise-3-implement-azure-ad-conditional-access-policies"></a>Übung 3: Implementieren von Azure AD-Richtlinien für bedingten Zugriff 

### <a name="estimated-timing-15-minutes"></a>Geschätzte Zeit: 15 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus: 

- Aufgabe 1: Konfigurieren einer Richtlinie für bedingten Zugriff.
- Aufgabe 2: Testen der Richtlinie für bedingten Zugriff.

#### <a name="task-1---configure-a-conditional-access-policy"></a>Aufgabe 1: Konfigurieren einer Richtlinie für bedingten Zugriff. 

In dieser Aufgabe überprüfen Sie die Einstellungen der Richtlinie für bedingten Zugriff und erstellen eine Richtlinie, die für die Anmeldung am Azure-Portal MFA erfordert. 

1. Navigieren Sie im Azure-Portal zurück zum Blatt **AdatumLab500-04** des Azure Active Directory-Mandanten.

1. Klicken Sie auf dem Blatt **AdatumLab500-04** im Abschnitt **Verwalten** auf **Sicherheit**.

1. Klicken Sie auf dem Blatt **Sicherheit \| Erste Schritte** im Abschnitt **Schützen** auf **Bedingter Zugriff**.

1. Klicken Sie auf dem Blatt **Bedingter Zugriff \| Richtlinien** Auf **+ Neue Richtlinie**. 

1. Konfigurieren Sie auf dem Blatt **Neu** die folgenden Einstellungen:

   - Geben Sie **AZ500Policy1** im Textfeld **Name** ein.
    
   - Klicken Sie auf **Benutzer und Gruppen**, aktivieren Sie das Kontrollkästchen **Benutzer und Gruppen**, klicken Sie auf dem Blatt **Auswählen** auf **aaduser2**,und klicken Sie dann auf **Auswählen**.
    
   - Klicken Sie auf **Cloud-Apps oder Aktionen**, klicken Sie auf **Apps auswählen**, klicken Sie auf dem Blatt **Auswählen** auf **Microsoft Azure-Verwaltung**, und klicken Sie dann auf **Auswählen**. 

    >**Hinweis**: Überprüfen Sie die Warnung, dass sich diese Richtlinie auf den Zugriff auf das Azure-Portal auswirkt.
    
   - Klicken Sie auf **Bedingungen**, klicken Sie auf **Anmelderisiko**. Überprüfen Sie auf dem Blatt **Anmelderisiko** die Risikostufen, nehmen Sie jedoch keine Änderungen vor, und schließen Sie das Blatt **Anmelderisiko**.
    
   - Klicken Sie auf **Geräteplattformen**, überprüfen Sie die Geräteplattformen, die eingeschlossen werden können, und klicken Sie auf **Fertig**.
    
   - Klicken Sie auf **Standorte**, und überprüfen Sie die Standortoptionen, ohne Änderungen vorzunehmen.
    
   - Klicken Sie im Abschnitt **Zugriffssteuerungen** auf **Erteilen**, aktivieren Sie auf dem Blatt **Erteilen** das Kontrollkästchen **Mehrstufige Authentifizierung erforderlich**, und klicken Sie auf **Auswählen**.
    
   - Legen Sie **Richtlinie aktivieren** auf **Ein** fest.

1. Klicken Sie auf dem Blatt **Neu** auf **Erstellen**. 

    >**Hinweis**: An diesem Punkt verfügen Sie über eine Richtlinie für bedingten Zugriff, die MFA erfordert, um sich am Azure-Portal anzumelden. 

#### <a name="task-2---test-the-conditional-access-policy"></a>Aufgabe 2: Testen der Richtlinie für bedingten Zugriff.

In dieser Aufgabe melden Sie sich am Azure-Portal als **aaduser2** an und überprüfen, ob MFA erforderlich ist. Sie löschen die Richtlinie außerdem, bevor Sie mit der nächsten Übung fortfahren. 

1. Öffnen Sie ein Microsoft Edge-InPrivate-Fenster.

1. Navigieren Sie im neuen Browserfenster zum Azure-Portal, und melden Sie sich mit dem Benutzerkonto **aaduser2** an.

1. Wenn Sie dazu aufgefordert werden, klicken Sie im Dialogfeld **Weitere Informationen erforderlich** auf **Weiter**.

    >**Hinweis**: Die Browsersitzung wird zur Seite **Schützen Sie Ihr Konto** umgeleitet.
    
1. Wählen Sie auf der Seite **Schützen Sie Ihr Konto** den Link **Ich möchte eine andere Methode einrichten** aus. Wählen Sie in der Dropdownliste **Welche Methode möchten Sie verwenden?** die Option **Telefon** und dann **Bestätigen** aus.

1. Wählen Sie auf der Seite **Schützen Sie Ihr Konto** Ihr Land oder Ihre Region aus, geben Sie Ihre Mobiltelefonnummer im Bereich **Telefonnummer eingeben** ein, stellen Sie sicher, dass die Option **Code per SMS an mich senden** ausgewählt ist, und klicken Sie dann auf **Weiter**.

1. Geben Sie auf der Seite **Schützen Sie Ihr Konto** den Code ein, den Sie in der SMS auf Ihrem Mobiltelefon erhalten haben, und klicken Sie auf **Weiter**.

1. Stellen Sie auf der Seite **Schützen Sie Ihr Konto** sicher, dass die Überprüfung erfolgreich war, und klicken Sie auf **Weiter**.

1. Klicken Sie auf der Seite **Schützen Sie Ihr Konto** auf **Fertig**.

1. Ändern Sie Ihr Kennwort, wenn Sie dazu aufgefordert werden. Denken Sie daran, sich das neue Kennwort zu notieren.

1. Vergewissern Sie sich, dass Sie sich erfolgreich am Azure-Portal angemeldet haben.

1. Melden Sie sich als **aaduser2** ab, und schließen Sie das InPrivate-Browserfenster.

    >**Hinweis**: Sie haben nun überprüft, ob die neu erstellte Richtlinie für bedingten Zugriff MFA erzwingt, wenn sich aaduser2 am Azure-Portal anmeldet.

1. Navigieren Sie im Browserfenster, das das Azure-Portal anzeigt, zurück zum Blatt **AdatumLab500-04** des Azure Active Directory-Mandanten.

1. Klicken Sie auf dem Blatt **AdatumLab500-04** im Abschnitt **Verwalten** auf **Sicherheit**.

1. Klicken Sie auf dem Blatt **Sicherheit \| Erste Schritte** im Abschnitt **Schützen** auf **Bedingter Zugriff**.

1. Klicken Sie auf dem Blatt **Bedingter Zugriff \| Richtlinien** auf die Auslassungszeichen neben **AZ500Policy1**, klicken Sie auf **Löschen**, und klicken Sie auf **Ja**, wenn Sie zur Bestätigung aufgefordert werden.

    >**Hinweis**: Ergebnis: In dieser Übung implementieren Sie eine Richtlinie für bedingten Zugriff, um MFA zu erfordern, wenn sich ein Benutzer am Azure-Portal anmeldet. 

>Ergebnis: Sie haben bedingten Azure AD-Zugriff konfiguriert und getestet.

### <a name="exercise-4-implement-azure-ad-identity-protection"></a>Übung 4: Implementieren von Azure AD Identity Protection

### <a name="estimated-timing-30-minutes"></a>Geschätzte Zeit: 30 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus: 

- Aufgabe 1: Anzeigen der Azure AD Identity Protection-Optionen im Azure-Portal
- Aufgabe 2: Konfigurieren einer Benutzerrisiko-Sicherheitsrichtlinie
- Aufgabe 3: Konfigurieren einer Anmelderisiko-Richtlinie
- Aufgabe 4: Simulieren von Risikoereignissen für die Azure AD Identity Protection-Richtlinien 
- Aufgabe 5: Überprüfen der Azure AD Identity Protection-Berichte

#### <a name="task-1-enable-azure-ad-identity-protection"></a>Aufgabe 1: Aktivieren von Azure AD Identity Protection

In dieser Aufgabe zeigen Sie die Azure AD Identity Protection-Optionen im Azure-Portal an. 

1. Melden Sie sich am Azure-Portal **`https://portal.azure.com/`** an, wenn Sie noch nicht angemeldet sind.

    >**Hinweis**: Stellen Sie sicher, dass Sie beim Azure AD-Mandanten **AdatumLab500-04** angemeldet sind. Sie können den Filter **Verzeichnis und Abonnement** verwenden, um zwischen Azure AD-Mandanten zu wechseln. Stellen Sie sicher, dass Sie als Benutzer mit der Rolle „Globaler Administrator“ im Azure AD-Mandanten angemeldet sind.

1. Klicken Sie auf dem Blatt **AdatumLab500-04** im Abschnitt **Verwalten** auf **Sicherheit**.

1. Klicken Sie auf dem Blatt **Sicherheit \| Erste Schritte** im Abschnitt **Schützen** auf **Identity Protection**.

1. Überprüfen Sie auf dem Blatt **Identity Protection \| Übersicht** die Optionen **Schützen**, **Bericht** und **Benachrichtigen**. 

#### <a name="task-2-configure-a-user-risk-policy"></a>Aufgabe 2: Konfigurieren einer Benutzerrisiko-Sicherheitsrichtlinie

In dieser Aufgabe erstellen Sie eine neue Benutzerrisiko-Sicherheitsrichtlinie. 

1. Klicken Sie auf dem Blatt **Identity Protection \| Übersicht** im Abschnitt **Schützen** auf **Benutzerrisiko-Sicherheitsrichtlinie**.

1. Konfigurieren Sie die **Richtlinie zum Beheben des Benutzerrisikos** mit den folgenden Einstellungen: 

   - Klicken Sie auf **Benutzer**. Stellen Sie auf der Registerkarte **Einschließen** des Blatts **Benutzer** sicher, dass die Option **Alle Benutzer** ausgewählt ist.

   - Wechseln Sie auf dem Blatt **Benutzer** zur Registerkarte **Ausschließen**, klicken Sie auf **Ausgeschlossene Benutzer auswählen**, wählen Sie Ihr Benutzerkonto aus, und klicken Sie dann auf **Auswählen**. 

   - Klicken Sie auf **Benutzerrisiko**. Wählen Sie auf dem Blatt **Benutzerrisiko** die Option **Niedrig und höher** aus,und klicken Sie dann auf **Fertig**. 

   - Klicken Sie auf **Zugriff**. Stellen Sie auf dem Blatt **Zugriff** sicher, dass die Option **Zugriff zulassen** und das Kontrollkästchen **Kennwortänderung erforderlich** aktiviert sind, und klicken Sie auf **Fertig**.

   - Legen Sie **Richtlinie erzwingen** auf **Ein** fest, und klicken Sie auf **Speichern**.

#### <a name="task-3-configure-sign-in-risk-policy"></a>Aufgabe 3: Konfigurieren der Anmelderisiko-Richtlinie

In dieser Aufgabe konfigurieren Sie eine Anmelderisiko-Richtlinie. 

1. Klicken Sie auf dem Blatt **Identity Protection \| Benutzerrisiko-Sicherheitsrichtlinie** im Abschnitt **Schützen** auf **Anmelderisiko-Richtlinie**.

1. Konfigurieren Sie die **Richtlinie zum Beheben des Anmelderisikos** mit den folgenden Einstellungen: 

   - Klicken Sie auf **Benutzer**. Stellen Sie auf der Registerkarte **Einschließen** des Blatts **Benutzer** sicher, dass die Option **Alle Benutzer** ausgewählt ist.

   - Klicken Sie auf **Anmelderisiko**. Wählen Sie auf dem Blatt **Anmelderisiko** die Option **Mittel und höher** aus,und klicken Sie dann auf **Fertig**. 

   - Klicken Sie auf **Zugriff**. Stellen Sie auf dem Blatt **Zugriff** sicher, dass die Option **Zugriff zulassen** und das Kontrollkästchen **Mehrstufige Authentifizierung erforderlich** aktiviert sind, und klicken Sie auf **Fertig**.

   - Legen Sie **Richtlinie erzwingen** auf **Ein** fest, und klicken Sie auf **Speichern**.

#### <a name="task-4-simulate-risk-events-against-the-azure-ad-identity-protection-policies"></a>Aufgabe 4: Simulieren von Risikoereignissen für die Azure AD Identity Protection-Richtlinien 

> Bevor Sie mit dieser Aufgabe beginnen, stellen Sie sicher, dass die Vorlagenbereitstellung, die Sie in Übung 1 gestartet haben, abgeschlossen wurde. Die Bereitstellung umfasst eine Azure-VM mit dem Namen **az500-04-vm1**. 

1. Legen Sie im Azure-Portal den Filter **Verzeichnis und Abonnement** auf den Azure AD-Mandanten fest, der dem Azure-Abonnement zugeordnet ist, in dem Sie die Azure-VM **az500-04-vm1** bereitgestellt haben.

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Virtuelle Computer** ein, und drücken Sie die **EINGABETASTE**.

1. Klicken Sie auf dem Blatt **Virtuelle Computer** auf den Eintrag **az500-04-vm1**. 

1. Klicken Sie auf dem Blatt **az500-04-vm1** auf **Verbinden**, und klicken Sie im Dropdownmenü auf **RDP**. 

1. Klicken Sie auf **RDP-Datei herunterladen**, und stellen Sie damit eine Verbindung mit der Azure-VM **az500-04-vm1** über Remotedesktop her. Wenn Sie zur Authentifizierung aufgefordert werden, geben Sie die folgenden Anmeldeinformationen an:

   |Einstellung|Wert|
   |---|---|
   |Benutzername|**Kursteilnehmer**|
   |Kennwort|**Pa55w.rd1234**|

    >**Hinweis**: Warten Sie, bis die Remotedesktopsitzung und der **Server-Manager** geladen werden.  

    >**Hinweis**: Die folgenden Schritte werden in der Remotedesktopsitzung mit der Azure-Computer-VM **az500-04-vm1** ausgeführt. 

1. Klicken Sie in **Server-Manager** auf **Lokaler Server** und dann auf **Verstärkte Sicherheitskonfiguration für IE**.

1. Legen Sie im Dialogfeld **Verstärkte Sicherheitskonfiguration für Internet Explorer** beide Optionen auf **Aus** fest, und klicken Sie auf **OK**.

1. Starten Sie **Internet Explorer**, klicken Sie auf der Symbolleiste auf das Zahnradsymbol, klicken Sie im Dropdownmenü auf **Sicherheit**, und klicken Sie dann auf **InPrivate-Browsen**.

1. Navigieren Sie im Internet Explorer-InPrivate-Fenster unter [ **https://www.torproject.org/projects/torbrowser.html.en** ](https://www.torproject.org/projects/torbrowser.html.en) zum ToR-Browserprojekt.

1. Laden Sie die Windows-Version des ToR-Browsers herunter, und installieren Sie sie mit den Standardeinstellungen. 

1. Starten Sie nach Abschluss der Installation den ToR-Browser, verwenden Sie die Option **Verbinden** auf der anfänglichen Seite, und navigieren Sie zum Anwendungszugriffsbereich unter [ **https://myapps.microsoft.com** ](https://myapps.microsoft.com).

1. Wenn Sie dazu aufgefordert werden, versuchen Sie, sich mit dem Konto **aaduser3** anzumelden. 

    >**Hinweis**: Ihnen wird die Meldung Ihre **Anmeldung wurde gesperrt** angezeigt. Dies ist zu erwarten, da dieses Konto nicht mit MFA konfiguriert ist, die aufgrund eines erhöhten Anmelderisikos im Zusammenhang mit der Verwendung des ToR-Browsers erforderlich ist.

1. Verwenden Sie die Option **Melden Sie sich ab, und melden Sie sich mit einem anderen Konto an**, um sich als Konto **aaduser1** anzumelden, das Sie zuvor in diesem Lab erstellt und für MFA konfiguriert haben.

    >**Hinweis**: Dieses Mal wird die Meldung **Verdächtige Aktivität erkannt** angezeigt. Dies ist ebenfalls zu erwarten, da dieses Konto mit MFA konfiguriert ist. Angesichts des erhöhten Anmelderisikos im Zusammenhang mit der Verwendung des ToR-Browsers müssen Sie MFA verwenden.

1. Verwenden Sie die Option **Überprüfen**, und geben Sie an, ob Sie Ihre Identität über SMS oder Anruf überprüfen möchten.

1. Schließen Sie die Überprüfung ab, und stellen Sie sicher, dass Sie sich erfolgreich beim Anwendungszugriffsbereich angemeldet haben.

1. Schließen Sie die RDP-Sitzung. 

    >**Hinweis**: An diesem Punkt haben Sie zwei verschiedene Anmeldungen versucht. Im nächsten Schritt überprüfen Sie die Azure Identity Protection-Berichte.

#### <a name="task-5-review-the-azure-ad-identity-protection-reports"></a>Aufgabe 5: Überprüfen der Azure AD Identity Protection-Berichte

In dieser Aufgabe überprüfen Sie die Azure AD Identity Protection-Berichte, die aus den ToR-Browseranmeldungen generiert wurden.

1. Verwenden Sie im Azure-Portal den Filter **Verzeichnis und Abonnement**, um zum Azure Active Directory-Mandanten **AdatumLab500-04** zu wechseln.

1. Klicken Sie auf dem Blatt **AdatumLab500-04** im Abschnitt **Verwalten** auf **Sicherheit**.

1. Klicken Sie auf dem Blatt **Sicherheit \| Erste Schritte** im Abschnitt **Berichte** auf **Riskante Benutzer**. 

1. Überprüfen Sie den Bericht, und identifizieren Sie alle Einträge, die auf das Benutzerkonto **aaduser3** verweisen.

1. Klicken Sie auf dem Blatt **Sicherheit \| Erste Schritte** im Abschnitt **Berichte** auf **Riskante Anmeldungen**. 

1. Überprüfen Sie den Bericht, und identifizieren Sie alle Einträge, die auf eine Anmeldung mit dem Benutzerkonto **aaduser3** verweisen.

1. Klicken Sie unter **Berichte** auf **Risikoerkennung**.

1. Überprüfen Sie den Bericht, und identifizieren Sie alle Einträge, die eine Anmeldung von einer anonymen IP-Adresse darstellen, die vom ToR-Browser generiert wurde. 

 >**Hinweis**: Es kann 10 bis 15 Minuten dauern, bis Risiken in den Berichten angezeigt werden.

> **Ergebnis**: Sie haben Azure AD Identity Protection aktiviert, die Benutzerrisiko-Sicherheitsrichtlinie und die Anmelderisiko-Richtlinie konfiguriert sowie die Azure AD Identity Protection-Konfiguration durch Simulieren von Risikoereignissen überprüft.

**Bereinigen von Ressourcen**

> Wir müssen Identity Protection-Ressourcen entfernen, die Sie nicht mehr verwenden. 

Verwenden Sie die folgenden Schritte, um die Identity Protection-Richtlinien im Azure AD-Mandanten **AdatumLab500-04** zu deaktivieren.

1. Navigieren Sie im Azure-Portal zurück zum Blatt **AdatumLab500-04** des Azure Active Directory-Mandanten.

1. Klicken Sie auf dem Blatt **AdatumLab500-04** im Abschnitt **Verwalten** auf **Sicherheit**.

1. Klicken Sie auf dem Blatt **Sicherheit \| Erste Schritte** im Abschnitt **Schützen** auf **Identity Protection**.

1. Klicken Sie auf dem Blatt **Identity Protection \| Übersicht** auf **Benutzerrisiko-Sicherheitsrichtlinie**.

1. Legen Sie auf dem Blatt **Identity Protection \| Benutzerrisiko-Sicherheitsrichtlinie** die Option **Richtlinie erzwingen** auf **Aus** fest, und klicken Sie dann auf **Speichern**.

1. Klicken Sie auf dem Blatt **Identity Protection \| Benutzerrisiko-Sicherheitsrichtlinie** auf **Anmelderisiko-Richtlinie**.

1. Legen Sie auf dem Blatt **Identity Protection \| Anmelderisiko-Richtlinie** die Option **Richtlinie erzwingen** auf **Aus** fest, und klicken Sie dann auf **Speichern**.

Verwenden Sie die folgenden Schritte, um die Azure-VM zu beenden, die Sie zuvor im Lab bereitgestellt haben.

1. Legen Sie im Azure-Portal den Filter **Verzeichnis und Abonnement** auf den Azure AD-Mandanten fest, der dem Azure-Abonnement zugeordnet ist, in dem Sie die Azure-VM **az500-04-vm1** bereitgestellt haben.

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Virtuelle Computer** ein, und drücken Sie die **EINGABETASTE**.

1. Klicken Sie auf dem Blatt **Virtuelle Computer** auf den Eintrag **az500-04-vm1**. 
 
1. Klicken Sie auf dem Blatt **az500-04-vm1** auf **Beenden,** und klicken Sie auf **OK**, wenn Sie zur Bestätigung aufgefordert werden. 

>  Entfernen Sie keine in diesem Lab bereitgestellten Ressourcen, da das PIM-Lab von ihnen abhängig ist.
