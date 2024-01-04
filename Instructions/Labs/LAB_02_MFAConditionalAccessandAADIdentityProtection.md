---
lab:
  title: '02: MFA und bedingter Zugriff'
  module: Module 01 - Manage Identity and Access
---

# Lab 02: MFA und bedingter Zugriff
# Lab-Handbuch für Kursteilnehmer

## Labszenario

Sie wurden gebeten, eine Machbarkeitsstudie für Funktionen zu erstellen, die die Microsoft Entra ID-Authentifizierung verbessern. Insbesondere möchten Sie Folgendes auswerten:

- Multi-Faktor-Authentifizierung von Microsoft Entra ID
- Bedingter Zugriff von Microsoft Entra ID
- risikobasierte Richtlinien für den bedingten Zugriff von Microsoft Entra ID

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

## Lab-Ziele

In diesem Lab führen Sie die folgenden Übungen aus:

- Übung 1: Bereitstellen einer Azure-VM mithilfe einer Azure Resource Manager-Vorlage
- Übung 2: Implementieren von Azure MFA
- Übung 3: Implementieren von Richtlinien für den bedingten Zugriff von Microsoft Entra ID 
- Übung 4: Implementieren von Microsoft Entra ID Identity Protection

## Diagramm zu MFA, bedingter Zugriff, Identity Protection

![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/246a3798-6f50-4a41-99c2-71e9ab6a4c8f)

## Anweisungen

## Lab-Dateien:

- **\\Allfiles\\Labs\\04\\az-500-04_azuredeploy.json**
- **\\Allfiles\\Labs\\04\\az-500-04_azuredeploy.parameters.json** 

### Übung 1: Bereitstellen einer Azure-VM mithilfe einer Azure Resource Manager-Vorlage

### Geschätzte Zeit: 10 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Bereitstellen einer Azure-VM mithilfe einer Azure Resource Manager-Vorlage.

#### Aufgabe 1: Bereitstellen einer Azure-VM mithilfe einer Azure Resource Manager-Vorlage

In dieser Aufgabe erstellen Sie eine VM mithilfe einer ARM-Vorlage. Diese VM wird in der letzten Übung für dieses Lab verwendet. 

1. Melden Sie sich beim Azure-Portal ( **`https://portal.azure.com/`** ) an.

    >**Hinweis**: Melden Sie sich beim Azure-Portal mit einem Konto an, das in dem Azure-Abonnement, das Sie für dieses Lab verwenden, über die Rolle „Besitzer“ oder „Mitwirkender“ und die Rolle „Globaler Administrator“ im Microsoft Entra ID-Mandanten verfügt, der diesem Abonnement zugeordnet ist.

2. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Text **Benutzerdefinierte Vorlage bereitstellen** ein.

    >**Hinweis**: Sie können auch **Vorlagenbereitstellung (Bereitstellen mithilfe benutzerdefinierter Vorlagen)** aus der **Marketplace**-Liste auswählen.

3. Klicken Sie auf dem Blatt **Benutzerdefinierte Bereitstellung** auf die Option **Eigene Vorlage im Editor erstellen**.

4. Klicken Sie auf dem Blatt **Vorlage bearbeiten** auf **Datei laden**, navigieren Sie zu **\\Allfiles\\Labs\\04\\az-500-04_azuredeploy.json**, und klicken Sie auf **Öffnen**.

    >**Hinweis**: Überprüfen Sie den Inhalt der Vorlage, und beachten Sie, dass sie eine Azure-VM bereitstellt, die Windows Server 2019 Datacenter hostet.

5. Klicken Sie auf dem Blatt **Vorlage bearbeiten** auf **Speichern**.

6. Klicken Sie auf dem Blatt **Benutzerdefinierte Bereitstellung** auf **Vorlage bearbeiten**.

7. Klicken Sie auf dem Blatt **Parameter bearbeiten** auf **Datei laden**, navigieren Sie zu **\\Allfiles\\Labs\\04\\az-500-04_azuredeploy.parameters.json**, und klicken Sie auf **Öffnen**.

    >**Hinweis**: Überprüfen Sie den Inhalt der Parameterdatei, und notieren Sie sich die Werte für adminUsername und adminPassword.

8. Klicken Sie auf dem Blatt **Parameter bearbeiten** auf **Speichern**.

9. Stellen Sie auf dem Blatt **Benutzerdefinierte Bereitstellung** sicher, dass die folgenden Einstellungen konfiguriert sind (übernehmen Sie für alle anderen Einstellungen die Standardwerte):

   >**Hinweis**: Sie müssen ein eindeutiges Kennwort erstellen, das für den Rest des Kurses zum Erstellen von VMs (virtuellen Computern) verwendet wird. Das Kennwort muss mindestens 12 Zeichen umfassen und die definierten Komplexitätsanforderungen erfüllen (Das Kennwort muss drei der folgenden Zeichen enthalten: einen Kleinbuchstaben, einen Großbuchstaben, eine Zahl und ein Sonderzeichen). [VM-Kennwortanforderungen](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm-). Notieren Sie sich das Kennwort.

   |Einstellung|Wert|
   |---|---|
   |Subscription|Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden.|
   |Resource group|Klicken Sie auf **Neu erstellen**, und geben Sie den Namen **AZ500LAB04** ein.|
   |Standort|**USA, Osten**|
   |VM-Größe|**Standard_D2s_v3**|
   |VM-Name|**az500-04-vm1**|
   |Administratorbenutzername|**Kursteilnehmer**|
   |Administratorkennwort|**Erstellen Sie Ihr eigenes Kennwort, und notieren Sie es für spätere Zwecke. Sie werden aufgefordert, dieses Kennwort für den erforderlichen Lab-Zugriff einzugeben.**|
   |Name des virtuellen Netzwerks|**az500-04-vnet1**|

   >**Hinweis**: Informationen zum Identifizieren von Azure-Regionen, in denen Sie Azure-VMs bereitstellen können, finden Sie unter [ **https://azure.microsoft.com/en-us/regions/offers/** ](https://azure.microsoft.com/en-us/regions/offers/).

10. Klicken Sie auf **Überprüfen und erstellen** und dann auf **Erstellen**.

    >**Hinweis**: Warten Sie nicht, bis die Bereitstellung abgeschlossen ist, sondern fahren Sie mit der nächsten Übung fort. Sie verwenden die VM, die in dieser Bereitstellung enthalten ist, in der letzten Übung dieses Labs.

> Ergebnis: Sie haben eine Vorlagenbereitstellung einer Azure-VM **az500-04-vm1** initiiert, die Sie in der letzten Übung dieses Labs verwenden werden.


### Übung 2: Implementieren von Azure MFA

### Geschätzte Zeit: 30 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Erstellen eines neuen Microsoft Entra ID-Mandanten
- Aufgabe 2: Aktivieren der Microsoft Entra ID P2-Testversion
- Aufgabe 3: Erstellen von Microsoft Entra ID-Benutzer*innen und -Gruppen
- Aufgabe 4: Zuweisen von Microsoft Entra ID P2-Lizenzen zu Microsoft Entra ID-Benutzer*innen
- Aufgabe 5: Konfigurieren von Azure MFA-Einstellungen.
- Aufgabe 6: Überprüfen der MFA-Konfiguration

#### Aufgabe 1: Erstellen eines neuen Microsoft Entra ID-Mandanten

In dieser Aufgabe erstellen Sie einen neuen Microsoft Entra ID-Mandanten. 

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite in das Textfeld **Ressourcen, Dienste und Dokumente durchsuchen** den Begriff **Microsoft Entra ID** ein und drücken Sie die **Eingabetaste**.

2. Klicken Sie auf dem Blatt mit der **Übersicht** über Ihren aktuellen Microsoft Entra ID-Mandanten auf **Mandanten verwalten** und dann im nächsten Bildschirm auf **+ Erstellen**.

3. Vergewissern Sie sich auf der Registerkarte **Grundlagen** des Blatts **Mandanten erstellen**, dass die Option **Microsoft Entra ID** ausgewählt ist, und klicken Sie auf **Weiter: Konfiguration >**.

4. Geben Sie auf der Registerkarte **Konfiguration** des Blatts **Mandant erstellen** die folgenden Einstellungen an:

   |Einstellung|Wert|
   |---|---|
   |Name der Organisation|**AdatumLab500-04**|
   |Name der Anfangsdomäne|Ein eindeutiger Name, der aus einer Kombination aus Buchstaben und Ziffern besteht|
   |Land oder Region|**USA**|

    >**Hinweis**: Notieren Sie sich den Namen der Anfangsdomäne. Sie benötigen ihn später in diesem Lab.

5. Klicken Sie auf **Überprüfen und erstellen** und dann auf **Erstellen**.
6. Fügen Sie Captcha-Code auf dem Blatt **Helfen Sie uns, zu zeigen, dass Sie kein Bot sind** hinzu, und klicken Sie dann auf die Schaltfläche **Senden**. 

    >**Hinweis**: Warten Sie, bis der neue Mandant erstellt wurde. Verwenden Sie das Symbol **Benachrichtigung** zum Überwachen des Bereitstellungsstatus. 


#### Aufgabe 2: Aktivieren der Microsoft Entra ID P2-Testversion

Mit dieser Aufgabe registrieren Sie sich für die kostenlose Testversion von Microsoft Entra ID P2. 

1. Klicken Sie im Azure-Portal auf der Symbolleiste auf das Symbol **Verzeichnis und Abonnement** rechts neben dem Cloud Shell-Symbol. 

2. Klicken Sie auf dem Blatt **Verzeichnis + Abonnement** auf den neu erstellten Mandanten **AdatumLab500-04**, und klicken Sie auf die Schaltfläche **Wechseln**, um ihn als aktuelles Verzeichnis festzulegen.

    >**Hinweis**: Möglicherweise müssen Sie das Browserfenster aktualisieren, wenn der **Eintrag AdatumLab500-04** nicht in der Filterliste **Verzeichnis und Abonnement** angezeigt wird.

3. Geben Sie im Azure-Portal oben auf der Azure-Portalseite in das Textfeld **Ressourcen, Dienste und Dokumente durchsuchen** den Begriff **Microsoft Entra ID** ein und drücken Sie die **Eingabetaste**. Klicken Sie auf dem Blatt **AdatumLab500-04** im Abschnitt **Verwalten** auf **Lizenzen.**

4. Klicken Sie im Blatt **Lizenzen \|Übersicht** unter **Schnelle Aufgaben** auf **Kostenlose Testversion anfordern**.

5. Erweitern Sie MICROSFT ENTRA ID P2 und klicken Sie dann auf **Aktivieren**.


#### Aufgabe 3: Erstellen von Microsoft Entra ID-Benutzer*innen und -Gruppen

In dieser Aufgabe erstellen Sie drei Benutzer: aaduser1 (Globaler Administrator), aaduser2 (Benutzer) und aaduser3 (Benutzer). Sie benötigen den Benutzerprinzipalnamen und das Kennwort aller Benutzer*innen für spätere Aufgaben. 

1. Navigieren Sie zurück zum Microsoft Entra ID-Blatt **AdatumLab500-04**, und klicken Sie im Abschnitt **Verwalten** auf **Benutzer**.

2. Klicken Sie auf der Seite **Benutzer \| Alle Benutzer** auf **+ Neuer Benutzer** und dann auf **Neuen Benutzer erstellen**. 

3. Stellen Sie auf dem Blatt **Neuer Benutzer** sicher, dass die Option **Benutzer erstellen** ausgewählt ist, und geben Sie die folgenden Einstellungen auf der Registerkarte „Grundlagen“ an (übernehmen Sie die Standardwerte für alle anderen Einstellungen), und klicken Sie auf **Weiter: Eigenschaften >**:

   |Einstellung|Wert|
   |---|---|
   |Benutzerprinzipalname|**aaduser1**|
   |Name|**aaduser1**|
   |Kennwort|Stellen Sie sicher, dass die Option **Kennwort automatisch generieren** ausgewählt ist.|
   
      >**Hinweis:** Notieren Sie sich den vollständigen Benutzerprinzipalnamen. Sie können den Wert kopieren, indem Sie auf der rechten Seite der Dropdownliste, die den Domänennamen anzeigt, auf die Schaltfläche **In Zwischenablage kopieren** klicken. Sie benötigen es später in diesem Lab.
   
      >**Hinweis**: Notieren Sie sich das Kennwort des Benutzers. Sie können den Wert kopieren, indem Sie auf der rechten Seite des Textfelds auf die Schaltfläche **In Zwischenablage kopieren** klicken. Sie benötigen es später in diesem Lab. 
   
4. Scrollen Sie auf der Registerkarte **Eigenschaften** nach unten und geben Sie den Verwendungsort **USA** an (behalten Sie alle anderen Standardwerte bei). Klicken Sie dann auf **Weiter: Zuweisungen >** .

5. Klicken Sie auf der Registerkarte **Zuweisungen** auf **+ Rolle hinzufügen**, suchen Sie nach **Globaler Administrator**, und wählen Sie diese Option aus. Klicken Sie auf **Auswählen**, dann auf **Überprüfen und erstellen** und dann auf **Erstellen**.

6. Klicken Sie auf dem Blatt **Benutzer \| Alle Benutzer** auf **+ Neuer Benutzer**. 

7. Stellen Sie auf dem Blatt **Neuer Benutzer** sicher, dass die Option **Benutzer erstellen** ausgewählt ist, und geben Sie die folgenden Einstellungen an (übernehmen Sie die Standardwerte für alle anderen Einstellungen), und klicken Sie auf **Weiter: Eigenschaften >**:

   |Einstellung|Wert|
   |---|---|
   |Benutzerprinzipalname|**aaduser2**|
   |Name|**aaduser2**|
   |Kennwort|Stellen Sie sicher, dass die Option **Kennwort automatisch generieren** ausgewählt ist.| 

    >**Hinweis:** Notieren Sie sich den vollständigen Benutzerprinzipalnamen und das Kennwort.

8. Scrollen Sie auf der Registerkarte **Eigenschaften** nach unten, und geben Sie den Nutzungsort an: **Vereinigte Staaten** (übernehmen Sie die Standardwerte für alle anderen Einstellungen), und klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**.

9. Klicken Sie auf dem Blatt **Benutzer \| Alle Benutzer** auf **+ Neuer Benutzer**. 

10. Stellen Sie auf dem Blatt **Neuer Benutzer** sicher, dass die Option **Benutzer erstellen** ausgewählt ist, und geben Sie die folgenden Einstellungen an (übernehmen Sie die Standardwerte für alle anderen Einstellungen), und klicken Sie auf **Weiter: Eigenschaften >**:

    |Einstellung|Wert|
    |---|---|
    |Benutzerprinzipalname|**aaduser3**|
    |Name|**aaduser3**|
    |Kennwort|Stellen Sie sicher, dass die Option **Kennwort automatisch generieren** ausgewählt ist.|

    >**Hinweis:** Notieren Sie sich den vollständigen Benutzerprinzipalnamen und das Kennwort.

11. Scrollen Sie auf der Registerkarte **Eigenschaften** nach unten, und geben Sie den Nutzungsort an: **Vereinigte Staaten** (übernehmen Sie die Standardwerte für alle anderen Einstellungen), und klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**.

    >**Hinweis**: Zu diesem Zeitpunkt sollten drei neue Benutzer auf der Seite **Benutzer** aufgeführt werden. 
    
#### Aufgabe 4: Zuweisen von Microsoft Entra ID Premium P2-Lizenzen zu Microsoft Entra ID-Benutzer*innen

In dieser Aufgabe weisen Sie allen Benutzer*innen Microsoft Entra ID Premium P2-Lizenzen zu.

1. Klicken Sie auf dem Blatt **Benutzer \| Alle Benutzer** auf den Eintrag, der Ihr Benutzerkonto darstellt. 

2. Klicken Sie auf dem Blatt, auf dem die Eigenschaften Ihres Benutzerkontos angezeigt werden, auf **Eigenschaften bearbeiten**.  Vergewissern Sie sich, dass der Nutzungsstandort auf **USA** festgelegt ist. Legen Sie andernfalls den Nutzungsstandort fest, und klicken Sie auf **Speichern**.

3. Navigieren Sie zurück zum Microsoft Entra ID-Blatt **AdatumLab500-04**, und klicken Sie im Abschnitt **Verwalten** auf **Lizenzen**.

4. Klicken Sie auf dem Blatt **Lizenzen \| Übersicht** auf **Alle Produkte**, aktivieren Sie das Kontrollkästchen **Microsoft Entra ID Premium P2**, und klicken Sie auf **+ Zuweisen**.

5. Klicken Sie auf dem Blatt **Lizenz zuweisen** auf **+ Benutzer und Gruppen hinzufügen**.

6. Wählen Sie auf dem Blatt **Benutzer** die Optionen **aaduser1**, **aaduser2**, **aaduser3** und Ihr Benutzerkonto aus, und klicken Sie auf **Auswählen**.

7. Klicken Sie auf dem Blatt **Lizenzen zuweisen** auf **Zuweisungsoptionen**, stellen Sie sicher, dass alle Optionen aktiviert sind, klicken Sie auf **Überprüfen und zuweisen**, und klicken Sie dann auf **Zuweisen**.

8. Melden Sie sich vom Azure-Portal ab, und melden Sie sich mit demselben Konto erneut an. Dieser Schritt ist erforderlich, damit die Lizenzzuweisung wirksam wird.

    >**Hinweis**: An diesem Punkt haben Sie allen Benutzerkonten, die Sie in diesem Lab verwenden, Microsoft Entra ID Premium P2-Lizenzen zugewiesen. Achten Sie darauf, dass Sie sich abmelden und dann erneut anmelden. 

#### Aufgabe 5: Konfigurieren von Azure MFA-Einstellungen.

In dieser Aufgabe konfigurieren Sie MFA und aktivieren MFA für aaduser1. 

1. Navigieren Sie im Azure-Portal zurück zum Microsoft Entra ID-Mandantenblatt **AdatumLab500-04**.

    >**Hinweis**: Stellen Sie sicher, dass Sie den Microsoft Entra ID-Mandanten „AdatumLab500-04“ verwenden.

2. Klicken Sie auf dem Microsoft Entra ID-Mandantenblatt **AdatumLab500-04** im Abschnitt **Verwalten** auf **Sicherheit**.

3. Klicken Sie auf dem Blatt **Sicherheit \| Erste Schritte** im Abschnitt **Verwalten** auf **Multi-Faktor-Authentifizierung**.

4. Klicken Sie auf dem Blatt **Multi-Faktor-Authentifizierung \| Erste Schritte** auf den Link **Zusätzliche cloudbasierte Multi-Faktor-Authentifizierung-Einstellungen**. 

    >**Hinweis**: Dadurch wird eine neue Browserregisterkarte geöffnet, auf der die Seite **Multi-Faktor-Authentifizierung** angezeigt wird.

5. Klicken Sie auf der Seite **Multi-Faktor-Authentifizierung** auf die Registerkarte **Diensteinstellungen**. Sehen Sie sich die **Überprüfungsoptionen** an. Beachten Sie, dass die Optionen **Textnachricht an Telefon**, **Benachrichtigung über mobile App** und **Prüfcode aus mobiler App oder Hardwaretoken** aktiviert sind. Klicken Sie auf **Speichern** und dann auf **Schließen**.

6. Wechseln Sie zur Registerkarte **Benutzer**, klicken Sie auf den Eintrag **aaduser1**, klicken Sie auf den Link **Aktivieren**, und klicken Sie, wenn Sie dazu aufgefordert werden, auf **Multi-Faktor-Authentifizierung aktivieren**. Klicken Sie dann auf **Schließen**.

7. Beachten Sie, dass die Spalte **Multi-Factor Auth-Status** für **aaduser1** jetzt **Aktiviert** ist.

8. Klicken Sie auf **aaduser1**, und beachten Sie, dass an diesem Punkt auch die Option **Erzwingen** verfügbar ist. 

    >**Hinweis**: Das Ändern des Benutzerstatus von „Aktiviert“ in „Erzwungen“ wirkt sich nur auf ältere in Microsoft Entra ID integrierte Apps aus, die Azure MFA nicht unterstützen und nach der Änderung des Status in „Erzwungen“ die Verwendung von App-Kennwörtern erfordern.

9. Klicken Sie bei ausgewähltem Eintrag **aaduser1** auf **Benutzereinstellungen verwalten**, und überprüfen Sie die verfügbaren Optionen: 

   - Bereitstellen der Kontaktmethoden bei ausgewählten Benutzern erneut anfordern.

   - Alle vorhandenen App-Kennwörter löschen, die von den ausgewählten Benutzern erstellt wurden.

   - Wiederherstellen von mehrstufiger Authentifizierung für alle gespeicherten Geräte.

10. Klicken Sie auf **Abbrechen**, und wechseln Sie zurück zur Browserregisterkarte, auf der das Blatt **Multi-Faktor-Authentifizierung \| Erste Schritte** im Azure-Portal angezeigt wird.

11. Klicken Sie im Abschnitt **Einstellungen** auf **Betrugswarnung**.

12. Konfigurieren Sie auf dem Blatt **Multi-Factor Authentication \| Betrugswarnung** die folgenden Einstellungen:

    |Einstellung|Wert|
    |---|---|
    |Benutzern das Übermitteln von Betrugswarnungen erlauben|**Ein**|
    |Benutzer, die Betrugsversuche melden, automatisch blockieren|**Ein**|
    |Code zum Melden von Betrugsversuchen während der Begrüßung|**0**|

13. Klicken Sie unten auf der Seite auf **Speichern**.

    >**Hinweis**: An diesem Punkt haben Sie MFA für aaduser1 aktiviert und Einstellungen für Betrugswarnungen eingerichtet. 

14. Navigieren Sie zurück zum Blatt **AdatumLab500-04** des Microsoft Entra ID-Mandanten, und klicken Sie im Abschnitt **Verwalten** auf **Eigenschaften**. Klicken Sie anschließend unten auf dem Blatt auf den Link **Sicherheitseinstellungen verwalten**. Klicken Sie auf dem Blatt **Sicherheitsstandards aktivieren** auf **Deaktiviert**. Wählen Sie **Meine Organisation verwendet bedingten Zugriff** als *Grund für die Deaktivierung* aus, klicken Sie auf **Speichern**, lesen Sie die Warnung, und klicken Sie dann auf **Deaktivieren**.

    >**Hinweis**: Stellen Sie sicher, dass Sie beim Microsoft Entra ID-Mandanten **AdatumLab500-04** angemeldet sind. Sie können den Filter **Verzeichnis und Abonnement** verwenden, um zwischen Microsoft Entra ID-Mandanten zu wechseln. Stellen Sie sicher, dass Sie als Benutzer mit der Rolle „Globaler Administrator“ im Microsoft Entra ID-Mandanten angemeldet sind.

#### Aufgabe 6: Überprüfen der MFA-Konfiguration

In dieser Aufgabe überprüfen Sie die MFA-Konfiguration, indem Sie die Anmeldung des Benutzerkontos aaduser1 testen. 

1. Öffnen Sie ein Browserfenster im InPrivate-Modus.

2. Navigieren Sie zum Azure-Portal (**`https://portal.azure.com/`**), und melden Sie sich mit dem Benutzerkonto **aaduser1** an. 

    >**Hinweis**: Für die Anmeldung müssen Sie einen vollqualifizierten Namen des Benutzerkontos **aaduser1** angeben, einschließlich des DNS-Domänennamens des Microsoft Entra ID-Mandanten, den Sie sich zuvor in diesem Lab notiert haben. Dieser Benutzername weist das Format aaduser1@`<your_tenant_name>`.onmicrosoft.com auf, wobei `<your_tenant_name>` der Platzhalter ist, der Ihren eindeutigen Microsoft Entra ID-Mandantennamen darstellt. 

3. Wenn Sie dazu aufgefordert werden, klicken Sie im Dialogfeld **Weitere Informationen erforderlich** auf **Weiter**.

    >**Hinweis**: Die Browsersitzung wird an die Seite **Zusätzliche Sicherheitsüberprüfung** weitergeleitet.

4. Wählen Sie auf der Seite **Schützen Sie Ihr Konto** den Link **Ich möchte eine andere Methode einrichten** aus. Wählen Sie in der Dropdownliste **Welche Methode möchten Sie verwenden?** die Option **Telefon** und dann **Bestätigen** aus.

5. Wählen Sie auf der Seite **Schützen Sie Ihr Konto** Ihr Land oder Ihre Region aus, geben Sie Ihre Mobiltelefonnummer im Bereich **Telefonnummer eingeben** ein, stellen Sie sicher, dass die Option **Code per SMS an mich senden** ausgewählt ist, und klicken Sie dann auf **Weiter**.
 
6. Geben Sie auf der Seite **Schützen Sie Ihr Konto** den Code ein, den Sie in der SMS auf Ihrem Mobiltelefon erhalten haben, und klicken Sie auf **Weiter**.

7. Stellen Sie auf der Seite **Schützen Sie Ihr Konto** sicher, dass die Überprüfung erfolgreich war, und klicken Sie auf **Weiter**.

8. Wenn Sie aufgefordert werden, eine zusätzliche Authentifizierungsmethode hinzuzufügen, klicken Sie auf **Ich möchte eine andere Methode verwenden**, wählen Sie in der Dropdownliste **E-Mail** aus, klicken Sie auf **Bestätigen**, geben Sie die E-Mail-Adresse an, die Sie verwenden möchten, und klicken Sie dann auf **Weiter**. Nachdem Sie die entsprechende E-Mail erhalten haben, identifizieren Sie den Code im E-Mail-Text, geben Sie ihn an, und klicken Sie dann auf **Fertig**.

9. Ändern Sie Ihr Kennwort, wenn Sie dazu aufgefordert werden. Denken Sie daran, sich das neue Kennwort zu notieren.

10. Vergewissern Sie sich, dass Sie sich erfolgreich am Azure-Portal angemeldet haben.

11. Melden Sie sich als **aaduser1** ab, und schließen Sie das InPrivate-Browserfenster.

> Ergebnis: Sie haben einen neuen AD-Mandanten erstellt, AD-Benutzer konfiguriert, MFA konfiguriert und die MFA-Benutzeroberfläche für einen Benutzer getestet. 


### Übung 3: Implementieren von Richtlinien für den bedingten Zugriff von Microsoft Entra ID 

### Geschätzte Zeit: 15 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus: 

- Aufgabe 1: Konfigurieren einer Richtlinie für bedingten Zugriff.
- Aufgabe 2: Testen der Richtlinie für bedingten Zugriff.

#### Aufgabe 1: Konfigurieren einer Richtlinie für bedingten Zugriff. 

In dieser Aufgabe überprüfen Sie die Einstellungen der Richtlinie für bedingten Zugriff und erstellen eine Richtlinie, die für die Anmeldung am Azure-Portal MFA erfordert. 

1. Navigieren Sie im Azure-Portal zurück zum Microsoft Entra ID-Mandantenblatt **AdatumLab500-04**.

2. Klicken Sie auf dem Blatt **AdatumLab500-04** im Abschnitt **Verwalten** auf **Sicherheit**.

3. Klicken Sie auf dem Blatt **Sicherheit \| Erste Schritte** im Abschnitt **Schützen** auf **Bedingter Zugriff**. Klicken Sie im linken Navigationspanel auf **Richtlinien**.

4. Klicken Sie auf dem Blatt **Bedingter Zugriff \| Richtlinien** Auf **+ Neue Richtlinie**. 

5. Konfigurieren Sie auf dem Blatt **Neu** die folgenden Einstellungen:

   - Geben Sie **AZ500Policy1** im Textfeld **Name** ein.
    
   - Klicken Sie unter **Benutzer** auf **0 Ausgewählte Benutzer und Gruppen**. Wählen Sie auf der rechten Seite unter „Einschließen“ die Option **Benutzer und Gruppen auswählen** aus. Aktivieren Sie das Kontrollkästchen **Benutzer und Gruppen**, und aktivieren Sie auf dem Blatt **Benutzer und Gruppen auswählen** das Kontrollkästchen **aaduser2**. Klicken Sie dann auf **Auswählen**.
    
   - Klicken Sie unter **Zielressourcen** auf **Keine Zielressourcen ausgewählt**. Klicken Sie dann auf **Apps auswählen** und unter „Auswählen“ auf **Keine**. Aktivieren Sie auf dem Blatt **Auswählen** das Kontrollkästchen **Microsoft Azure-Verwaltung**, und klicken Sie auf **Auswählen**. 

     >**Hinweis**: Überprüfen Sie die Warnung, dass sich diese Richtlinie auf den Zugriff auf das Azure-Portal auswirkt.
    
   - Klicken Sie unter **Bedingungen** auf **0 Bedingungen ausgewählt**, und klicken Sie unter **Anmelderisiko** auf **Nicht konfiguriert**. Überprüfen Sie auf dem Blatt **Anmelderisiko** die Risikostufen, nehmen Sie jedoch keine Änderungen vor, und schließen Sie das Blatt **Anmelderisiko**.
    
   - Klicken Sie unter **Geräteplattformen** auf **Nicht konfiguriert**, überprüfen Sie die Geräteplattformen, die eingeschlossen werden können, aber nehmen Sie keine Änderungen vor, und klicken Sie auf **Fertig**.
    
   - Klicken Sie unter **Standorte** auf **Nicht konfiguriert**, und überprüfen Sie die Standortoptionen, ohne Änderungen vorzunehmen.
    
   - Klicken Sie unter **Gewähren** im Abschnitt **Zugriffssteuerungen** auf **0 Steuerungen ausgewählt**. Aktivieren Sie auf dem Blatt **Gewähren** das Kontrollkästchen **Multi-Faktor-Authentifizierung erforderlich**, und klicken Sie auf **Auswählen**.
    
   - Legen Sie **Richtlinie aktivieren** auf **Ein** fest.

6. Klicken Sie auf dem Blatt **Neu** auf **Erstellen**. 

    >**Hinweis**: An diesem Punkt verfügen Sie über eine Richtlinie für bedingten Zugriff, die MFA erfordert, um sich am Azure-Portal anzumelden. 

#### Aufgabe 2: Testen der Richtlinie für bedingten Zugriff.

In dieser Aufgabe melden Sie sich am Azure-Portal als **aaduser2** an und überprüfen, ob MFA erforderlich ist. Sie löschen die Richtlinie außerdem, bevor Sie mit der nächsten Übung fortfahren. 

1. Öffnen Sie ein Microsoft Edge-InPrivate-Fenster.

2. Navigieren Sie im neuen Browserfenster zum Azure-Portal (**`https://portal.azure.com/`**), und melden Sie sich mit dem Benutzerkonto **aaduser2** an.

3. Wenn Sie dazu aufgefordert werden, klicken Sie im Dialogfeld **Weitere Informationen erforderlich** auf **Weiter**.

    >**Hinweis:** Die Browsersitzung wird zur Seite **Schützen Sie Ihr Konto** umgeleitet.
    
4. Wählen Sie auf der Seite **Schützen Sie Ihr Konto** den Link **Ich möchte eine andere Methode einrichten** aus. Wählen Sie in der Dropdownliste **Welche Methode möchten Sie verwenden?** die Option **Telefon** und dann **Bestätigen** aus.

5. Wählen Sie auf der Seite **Schützen Sie Ihr Konto** Ihr Land oder Ihre Region aus, geben Sie Ihre Mobiltelefonnummer im Bereich **Telefonnummer eingeben** ein, stellen Sie sicher, dass die Option **Code per SMS an mich senden** ausgewählt ist, und klicken Sie dann auf **Weiter**.

6. Geben Sie auf der Seite **Schützen Sie Ihr Konto** den Code ein, den Sie in der SMS auf Ihrem Mobiltelefon erhalten haben, und klicken Sie auf **Weiter**.

7. Stellen Sie auf der Seite **Schützen Sie Ihr Konto** sicher, dass die Überprüfung erfolgreich war, und klicken Sie auf **Weiter**.

8. Klicken Sie auf der Seite **Schützen Sie Ihr Konto** auf **Fertig**.

9. Ändern Sie Ihr Kennwort, wenn Sie dazu aufgefordert werden. Denken Sie daran, sich das neue Kennwort zu notieren.

10. Vergewissern Sie sich, dass Sie sich erfolgreich am Azure-Portal angemeldet haben.

11. Melden Sie sich als **aaduser2** ab, und schließen Sie das InPrivate-Browserfenster.

    >**Hinweis**: Sie haben nun überprüft, ob die neu erstellte Richtlinie für bedingten Zugriff MFA erzwingt, wenn sich aaduser2 am Azure-Portal anmeldet.

12. Navigieren Sie im Browserfenster mit dem Azure-Portal zurück zum Microsoft Entra ID-Mandantenblatt **AdatumLab500-04**.

13. Klicken Sie auf dem Blatt **AdatumLab500-04** im Abschnitt **Verwalten** auf **Sicherheit**.

14. Klicken Sie auf dem Blatt **Sicherheit \| Erste Schritte** im Abschnitt **Schützen** auf **Bedingter Zugriff**. Klicken Sie im linken Navigationspanel auf **Richtlinien**.

15. Klicken Sie auf dem Blatt **Bedingter Zugriff \| Richtlinien** auf die Auslassungszeichen neben **AZ500Policy1**, klicken Sie auf **Löschen**, und klicken Sie auf **Ja**, wenn Sie zur Bestätigung aufgefordert werden.

    >**Hinweis**: Ergebnis: In dieser Übung implementieren Sie eine Richtlinie für bedingten Zugriff, um MFA zu erfordern, wenn sich ein Benutzer am Azure-Portal anmeldet. 

>Ergebnis: Sie haben den bedingten Microsoft Entra ID-Zugriff konfiguriert und getestet.

### Übung 4: Bereitstellen risikobasierter Richtlinien im bedingten Zugriff

### Geschätzte Zeit: 30 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Anzeigen der Microsoft Entra ID Identity Protection-Optionen im Azure-Portal
- Aufgabe 2: Konfigurieren einer Benutzerrisiko-Sicherheitsrichtlinie
- Aufgabe 3: Konfigurieren einer Anmelderisiko-Richtlinie
- Aufgabe 4: Simulieren von Risikoereignissen für die Microsoft Entra ID Identity Protection-Richtlinien 
- Aufgabe 5: Überprüfen der Microsoft Entra ID Identity Protection-Berichte

#### Aufgabe 1: Aktivieren von Microsoft Entra ID Identity Protection

In dieser Aufgabe sehen Sie sich die Microsoft Entra ID Identity Protection-Optionen im Azure-Portal an. 

1. Melden Sie sich am Azure-Portal **`https://portal.azure.com/`** an, wenn Sie noch nicht angemeldet sind.

    >**Hinweis**: Stellen Sie sicher, dass Sie beim Microsoft Entra ID-Mandanten **AdatumLab500-04** angemeldet sind. Sie können den Filter **Verzeichnis und Abonnement** verwenden, um zwischen Microsoft Entra ID-Mandanten zu wechseln. Stellen Sie sicher, dass Sie als Benutzer mit der Rolle „Globaler Administrator“ im Microsoft Entra ID-Mandanten angemeldet sind.

#### Aufgabe 2: Konfigurieren einer Benutzerrisiko-Sicherheitsrichtlinie

In dieser Aufgabe erstellen Sie eine neue Benutzerrisiko-Sicherheitsrichtlinie. 

1. Navigieren Sie zu **AdatumLab500-04** Microsoft Entra ID-Mandant > **Sicherheit** > **Bedingter Zugriff** > **Richtlinien**.

2. Klicken Sie auf **+ Neue Richtlinie**.

3. Geben Sie den Richtliniennamen **AZ500Policy2** in das Textfeld **Name** ein.

4. Klicken Sie unter **Zuweisungen** > **Benutzer** auf **0 Benutzer und Gruppen ausgewählt**.

5. Klicken Sie unter **Einschließen** auf **Benutzer und Gruppen auswählen**, klicken Sie auf **Benutzer und Gruppen**, wählen Sie **aaduser2** und **aaduser3** aus, und klicken Sie dann auf **Auswählen**.

6. Klicken Sie unter **Ausschließen** auf **Benutzer und Gruppen**, und wählen Sie **aaduser1** aus. Klicken Sie dann auf **Auswählen**. 

7. Klicken Sie unter **Zielressourcen** auf **Keine Zielressourcen ausgewählt**, vergewissern Sie sich, dass **Cloud-Apps** in der Dropdownliste ausgewählt ist, und wählen Sie unter **Einschließen** die Option **Alle Cloud-Apps** aus.

8. Klicken Sie unter **Bedingungen** auf **0 Bedingungen ausgewählt**, und klicken Sie unter **Benutzerrisiko** auf **Nicht konfiguriert**. Legen Sie auf dem Blatt **Benutzerrisiko** die Option **Konfigurieren** auf **Ja** fest.

9. Wählen Sie unter **Hiermit konfigurieren Sie die Benutzerrisikostufen, die für die Erzwingung der Richtlinie erforderlich sind** die Option **Hoch** aus.

10. Klicke auf **Fertig**.

11. Klicken Sie unter **Gewähren** im Abschnitt **Zugriffssteuerungen** auf **0 Steuerungen ausgewählt**. Vergewissern Sie sich, dass auf dem Blatt **Gewähren** die Option **Zugriff gewähren** ausgewählt ist.

12. Wählen Sie **Multi-Faktor-Authentifizierung erfordern** und **Kennwortänderung erforderlich** aus.

13. Klicken Sie auf **Auswählen**.

14. Klicken Sie unter **Sitzung** auf **0 Steuerungen ausgewählt**. Wählen Sie **Anmeldehäufigkeit** und dann **Jedes Mal** aus.

15. Klicken Sie auf **Auswählen**.

16. Vergewissern Sie sich, dass **Richtlinie aktivieren** auf **Nur melden** festgelegt ist.

17. Klicken Sie auf **Erstellen**, um die Richtlinie zu aktivieren.

#### Aufgabe 3: Konfigurieren einer Anmelderisiko-Richtlinie

1. Navigieren Sie zum Microsoft Entra ID-Mandanten **AdatumLab500-04** und dann zu **Sicherheit** > **Bedingter Zugriff**> **Richtlinien**.

2. Wählen Sie **+ Neue Richtlinie** aus.

3. Geben Sie den Richtliniennamen **AZ500Policy3** in das Textfeld **Name** ein.

4. Klicken Sie unter **Zuweisungen** > **Benutzer** auf **0 Benutzer und Gruppen ausgewählt**.

5. Klicken Sie unter **Einschließen** auf **Benutzer und Gruppen auswählen**, klicken Sie auf **Benutzer und Gruppen**, wählen Sie **aaduser2** und **aaduser3** aus, und klicken Sie auf **Auswählen**.

6. Klicken Sie unter **Ausschließen** auf **Benutzer und Gruppen**, und wählen Sie **aaduser1** aus. Klicken Sie dann auf **Auswählen**. 

7. Klicken Sie unter **Zielressourcen** auf **Keine Zielressourcen ausgewählt**, vergewissern Sie sich, dass **Cloud-Apps** in der Dropdownliste ausgewählt ist, und wählen Sie unter **Einschließen** die Option **Alle Cloud-Apps** aus.

8. Klicken Sie unter **Bedingungen** auf **0 Bedingungen ausgewählt**, und klicken Sie unter **Anmelderisiko** auf **Nicht konfiguriert**. Legen Sie auf dem Blatt **Anmelderisiko** die Option **Konfigurieren** auf **Ja** fest.

9. Wählen Sie unter **Hiermit wählen Sie die Anmelderisikostufe aus, auf die diese Richtlinie angewendet werden soll** die Optionen **Hoch** und **Mittel** aus.

10. Klicke auf **Fertig**.

11. Klicken Sie unter **Gewähren** im Abschnitt **Zugriffssteuerungen** auf **0 Steuerungen ausgewählt**. Vergewissern Sie sich, dass auf dem Blatt **Gewähren** die Option **Zugriff gewähren** ausgewählt ist.   

12. Wählen Sie **Multi-Faktor-Authentifizierung erforderlich** aus.

13. Klicken Sie auf **Auswählen**.

14. Klicken Sie unter **Sitzung** auf **0 Steuerungen ausgewählt**. Wählen Sie **Anmeldehäufigkeit** und dann **Jedes Mal** aus.

15. Klicken Sie auf **Auswählen**.

16. Bestätigen Sie die Einstellungen und legen Sie **Richtlinie aktivieren** auf **Ein** fest.

17. Klicken Sie auf **Erstellen**, um die Richtlinie zu aktivieren.

#### Aufgabe 4: Simulieren von Risikoereignissen für die Microsoft Entra ID Identity Protection-Richtlinien 

> Bevor Sie mit dieser Aufgabe beginnen, stellen Sie sicher, dass die Vorlagenbereitstellung, die Sie in Übung 1 gestartet haben, abgeschlossen wurde. Die Bereitstellung umfasst eine Azure-VM mit dem Namen **az500-04-vm1**. 

1. Legen Sie im Azure-Portal den Filter **Verzeichnis und Abonnement** auf den Microsoft Entra ID-Mandanten fest, der dem Azure-Abonnement zugeordnet ist, in dem Sie die Azure-VM **az500-04-vm1** bereitgestellt haben.

2. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Virtuelle Computer** ein, und drücken Sie die **EINGABETASTE**.

3. Klicken Sie auf dem Blatt **Virtuelle Computer** auf den Eintrag **az500-04-vm1**. 

4. Klicken Sie auf dem Blatt **az500-04-vm1** auf **Verbinden**. Vergewissern Sie sich, dass Sie sich auf der Registerkarte **RDP** befinden.

5. Klicken Sie auf **RDP-Datei herunterladen**, und stellen Sie damit eine Verbindung mit der Azure-VM **az500-04-vm1** über Remotedesktop her. Wenn Sie zur Authentifizierung aufgefordert werden, geben Sie die folgenden Anmeldeinformationen an:

    |Einstellung|Wert|
    |---|---|
    |Benutzername|**Kursteilnehmer**|
    |Kennwort|**Verwenden Sie Ihr persönliches Kennwort, das Sie in Lab 04 > Übung 1 > Aufgabe 1 > Schritt 9 erstellt haben.**|

    >**Hinweis**: Warten Sie, bis die Remotedesktopsitzung und der **Server-Manager** geladen werden.  

    >**Hinweis**: Die folgenden Schritte werden in der Remotedesktopsitzung mit der Azure-Computer-VM **az500-04-vm1** ausgeführt. 

6. Klicken Sie in **Server-Manager** auf **Lokaler Server** und dann auf **Verstärkte Sicherheitskonfiguration für IE**.

7. Legen Sie im Dialogfeld **Verstärkte Sicherheitskonfiguration für Internet Explorer** beide Optionen auf **Aus** fest, und klicken Sie auf **OK**.

8. Starten Sie **Internet Explorer**, klicken Sie auf der Symbolleiste auf das Zahnradsymbol, klicken Sie im Dropdownmenü auf **Sicherheit**, und klicken Sie dann auf **InPrivate-Browsen**.

9. Navigieren Sie im Internet Explorer-InPrivate-Fenster unter  **https://www.torproject.org/projects/torbrowser.html.en**  zum ToR-Browserprojekt.

10. Laden Sie die Windows-Version des ToR-Browsers herunter, und installieren Sie sie mit den Standardeinstellungen. 

11. Starten Sie nach Abschluss der Installation den ToR-Browser, verwenden Sie die Option **Verbinden** auf der Startseite, und navigieren Sie zum Anwendungszugriffsbereich unter **https://myapps.microsoft.com**.

12. Wenn Sie dazu aufgefordert werden, versuchen Sie, sich mit dem Konto **aaduser3** anzumelden. 

    >**Hinweis**: Ihnen wird die Meldung Ihre **Anmeldung wurde gesperrt** angezeigt. Dies ist zu erwarten, da dieses Konto nicht mit Multi-Faktor-Authentifizierung konfiguriert ist, die aufgrund eines erhöhten Anmelderisikos im Zusammenhang mit der Verwendung des ToR-Browsers erforderlich ist.

13. Wählen Sie im ToR-Browser den schwarzen Pfeil aus, um sich mit dem Konto **aaduser1** anzumelden, das Sie zuvor in diesem Lab erstellt und für die Multi-Faktor-Authentifizierung konfiguriert haben.

    >**Hinweis**: Dieses Mal wird die Meldung **Verdächtige Aktivität erkannt** angezeigt. Dies ist ebenfalls zu erwarten, da dieses Konto mit Multi-Faktor-Authentifizierung konfiguriert ist. Angesichts des erhöhten Anmelderisikos im Zusammenhang mit der Verwendung des ToR-Browsers müssen Sie MFA verwenden.

14. Verwenden Sie die Option **Überprüfen**, und geben Sie an, ob Sie Ihre Identität über SMS oder Anruf überprüfen möchten.

15. Schließen Sie die Überprüfung ab, und stellen Sie sicher, dass Sie sich erfolgreich beim Anwendungszugriffsbereich angemeldet haben.

16. Schließen Sie die RDP-Sitzung. 

    >**Hinweis:** An diesem Punkt haben Sie versucht, zwei verschiedene Anmeldungen durchzuführen. Als Nächstes werden Sie die Azure Identity Protection-Berichte überprüfen.

#### Aufgabe 5: Überprüfen der Microsoft Entra ID Identity Protection-Berichte

In dieser Aufgabe überprüfen Sie die Microsoft Entra ID Identity Protection-Berichte, die von den ToR-Browseranmeldungen erstellt wurden.

1. Verwenden Sie im Azure-Portal den Filter **Verzeichnis und Abonnement**, um zum Microsoft Entra ID-Mandanten **AdatumLab500-04** zu wechseln.

2. Klicken Sie auf dem Blatt **AdatumLab500-04** im Abschnitt **Verwalten** auf **Sicherheit**.

3. Klicken Sie auf dem Blatt **Sicherheit \| Erste Schritte** im Abschnitt **Bericht** auf **Riskante Benutzer**. 

4. Überprüfen Sie den Bericht, und identifizieren Sie alle Einträge, die auf das Benutzerkonto **aaduser3** verweisen.

5. Klicken Sie auf dem Blatt **Sicherheit \| Erste Schritte** im Abschnitt **Berichte** auf **Riskante Anmeldungen**. 

6. Überprüfen Sie den Bericht, und identifizieren Sie alle Einträge, die auf eine Anmeldung mit dem Benutzerkonto **aaduser3** verweisen.

7. Klicken Sie unter **Berichte** auf **Risikoerkennung**.

8. Überprüfen Sie den Bericht, und identifizieren Sie alle Einträge, die eine Anmeldung von einer anonymen IP-Adresse darstellen, die vom ToR-Browser generiert wurde. 

    >**Hinweis:** Es kann 10 bis 15 Minuten dauern, bis Risiken in den Berichten angezeigt werden.

> **Ergebnis**: Sie haben Microsoft Entra ID Identity Protection aktiviert, die Benutzerrisiko- und die Anmelderisiko-Richtlinie konfiguriert sowie die Microsoft Entra ID Identity Protection-Konfiguration durch Simulieren von Risikoereignissen überprüft.

**Bereinigen von Ressourcen**

> Wir müssen Identity Protection-Ressourcen entfernen, die Sie nicht mehr verwenden. 

Verwenden Sie die folgenden Schritte, um die Identity Protection-Richtlinien im Microsoft Entra ID-Mandanten **AdatumLab500-04** zu deaktivieren.

1. Navigieren Sie im Azure-Portal zurück zum Microsoft Entra ID-Mandantenblatt **AdatumLab500-04**.

2. Klicken Sie auf dem Blatt **AdatumLab500-04** im Abschnitt **Verwalten** auf **Sicherheit**.

3. Klicken Sie auf dem Blatt **Sicherheit \| Erste Schritte** im Abschnitt **Schützen** auf **Identity Protection**.

4. Klicken Sie auf dem Blatt **Identity Protection \| Übersicht** auf **Benutzerrisiko-Sicherheitsrichtlinie**.

5. Legen Sie auf dem Blatt **Identitätsschutz \| Benutzerrisiko-Richtlinie** die Option **Richtliniendurchsetzung** auf **Aus** fest, und klicken Sie auf **Speichern**.

6. Klicken Sie auf dem Blatt **Identity Protection \| Benutzerrisiko-Sicherheitsrichtlinie** auf **Anmelderisiko-Richtlinie**.

7. Legen Sie auf dem Blatt **Identitätsschutz \| Anmelderisiko-Richtlinie** die Option **Richtliniendurchsetzung** auf **Aus** fest, und klicken Sie auf **Speichern**.

Verwenden Sie die folgenden Schritte, um die Azure-VM zu beenden, die Sie zuvor im Lab bereitgestellt haben.

1. Legen Sie im Azure-Portal den Filter **Verzeichnis und Abonnement** auf den Microsoft Entra ID-Mandanten fest, der dem Azure-Abonnement zugeordnet ist, in dem Sie die Azure-VM **az500-04-vm1** bereitgestellt haben.

2. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Virtuelle Computer** ein, und drücken Sie die **EINGABETASTE**.

3. Klicken Sie auf dem Blatt **Virtuelle Computer** auf den Eintrag **az500-04-vm1**. 
 
4. Klicken Sie auf dem Blatt **az500-04-vm1** auf **Beenden,** und klicken Sie auf **OK**, wenn Sie zur Bestätigung aufgefordert werden. 

>  Entfernen Sie keine in diesem Lab bereitgestellten Ressourcen, da das PIM-Lab von ihnen abhängig ist.
