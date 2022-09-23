---
lab:
  title: Lab 06 – Implementieren der Verzeichnissynchronisierung
  module: Module 01 - Manage Identity and Access
ms.openlocfilehash: 00c359e1875ab915ab697d8ed33e36d956540529
ms.sourcegitcommit: 1da29a6d959a7f91dbbcbabf5ec06869c98fc1f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2022
ms.locfileid: "141267257"
---
# <a name="lab-06-implement-directory-synchronization"></a>Lab 06: Implementieren der Verzeichnissynchronisierung
# <a name="student-lab-manual"></a>Lab-Handbuch für Kursteilnehmer

## <a name="lab-scenario"></a>Labszenario

Sie wurden gebeten, einen Proof of Concept zu erstellen, der veranschaulicht, wie Sie eine lokale Active Directory Domain Services (AD DS)-Umgebung in einen Azure Active Directory (Azure AD)-Mandanten integrieren. Insbesondere möchten Sie Folgendes erledigen:

- Eine AD DS-Gesamtstruktur mit einer einzelnen Domäne durch Bereitstellen einer Azure-VM implementieren, auf der ein AD DS-Domänencontroller gehostet wird
- Einen Azure AD-Mandanten erstellen und konfigurieren
- Die AD DS-Gesamtstruktur mit dem Azure AD-Mandanten synchronisieren

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

## <a name="lab-objectives"></a>Ziele des Labs

In diesem Lab führen Sie die folgenden Übungen aus:

- Übung 1: Bereitstellen einer Azure-VM, die einen Active Directory-Domänencontroller hostet
- Übung 2: Erstellen und Konfigurieren eines Azure Active Directory-Mandanten
- Übung 3: Synchronisieren einer Active Directory-Gesamtstruktur mit einem Azure Active Directory-Mandanten

## <a name="implement-directory-synchronization"></a>Implementieren der Verzeichnissynchronisierung

![image](https://user-images.githubusercontent.com/91347931/157525374-8f740f14-c2db-47b3-98f8-7feb9bc122b5.png)

## <a name="instructions"></a>Anweisungen

### <a name="exercise-1-deploy-an-azure-vm-hosting-an-active-directory-domain-controller"></a>Übung 1: Bereitstellen einer Azure-VM, die einen Active Directory-Domänencontroller hostet

### <a name="estimated-timing-10-minutes"></a>Geschätzte Zeit: 10 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Identifizieren eines verfügbaren DNS-Namens für eine Azure-VM-Bereitstellung
- Aufgabe 2: Verwenden einer ARM-Vorlage zum Bereitstellen einer Azure-VM, die einen Active Directory-Domänencontroller hostet

#### <a name="task-1-identify-an-available-dns-name-for-an-azure-vm-deployment"></a>Aufgabe 1: Identifizieren eines verfügbaren DNS-Namens für eine Azure-VM-Bereitstellung

In dieser Aufgabe identifizieren Sie einen DNS-Namen für Ihre Azure-VM-Bereitstellung. 

1. Melden Sie sich am Azure-Portal ( **`https://portal.azure.com/`** ) an.

    >**Hinweis**: Melden Sie sich am Azure-Portal mit einem Konto an, das über die Rolle „Besitzer“ oder „Mitwirkender“ in dem Azure-Abonnement verfügt, das Sie für dieses Lab nutzen.

2. Öffnen Sie die Cloud Shell, indem Sie oben rechts im Azure-Portal auf das erste Symbol klicken. Klicken Sie bei der entsprechenden Aufforderung auf **PowerShell** und dann auf **Speicher erstellen**.

3. Stellen Sie sicher, dass im Dropdownmenü oben links im Bereich „Cloud Shell“ der Eintrag **PowerShell** ausgewählt ist.

4. Führen Sie in der PowerShell-Sitzung im Bereich „Cloud Shell“ Folgendes aus, um einen verfügbaren DNS-Namen zu identifizieren, den Sie in der nächsten Aufgabe dieser Übung für eine Azure-VM-Bereitstellung verwenden können:

    ```powershell
    Test-AzDnsAvailability -DomainNameLabel <custom-label> -Location '<location>'
    ```

    >**Hinweis**: Ersetzen Sie den Platzhalter `<custom-label>` durch einen gültigen DNS-Namen, der wahrscheinlich global eindeutig ist. Ersetzen Sie den Platzhalter `<location>` durch den Namen der Region, in der Sie die Azure-VM bereitstellen möchten. Sie soll den Active Directory-Domänencontroller hosten, den Sie in diesem Lab verwenden.

    >**Hinweis**: Informationen zum Identifizieren von Azure-Regionen, in denen Sie Azure-VMs bereitstellen können, finden Sie unter [ **https://azure.microsoft.com/en-us/regions/offers/**](https://azure.microsoft.com/en-us/regions/offers/).

5. Überprüfen Sie, ob der Befehl **True** zurückgegeben hat. Wenn das nicht zutrifft, führen Sie denselben Befehl mit einem anderen Wert für `<custom-label>` so lange erneut aus, bis der Befehl **True** zurückgibt.

6. Noten Sie sich den Wert von `<custom-label>`, der zum erfolgreichen Ergebnis geführt hat. Sie werden dies in der nächsten Aufgabe benötigen.

7. Schließen Sie die Cloud Shell.

#### <a name="task-2-use-an-arm-template-to-deploy-an-azure-vm-hosting-an-active-directory-domain-controller"></a>Aufgabe 2: Verwenden einer ARM-Vorlage zum Bereitstellen einer Azure-VM, die einen Active Directory-Domänencontroller hostet

In dieser Aufgabe stellen Sie eine Azure-VM bereit, die einen Active Directory-Domänencontroller hosten soll.

1. Öffnen Sie in demselben Browserfenster eine andere Browserregisterkarte, und navigieren Sie zu **https://github.com/Azure/azure-quickstart-templates/tree/master/application-workloads/active-directory/active-directory-new-domain**.

2. Klicken Sie auf der Seite **Erstellen einer neuen Windows-VM und Erstellen einer neuen AD-Gesamtstruktur, einer Domäne und eines Domänencontrollers** auf **In Azure bereitstellen**. Dadurch wird der Browser automatisch zum Blatt **Azure-VM mit einer neuen AD-Gesamtstruktur erstellen** im Azure-Portal umgeleitet.

3. Klicken Sie auf dem Blatt **Azure-VM mit einer neuen AD-Gesamtstruktur erstellen** auf **Parameter bearbeiten**.

4. Klicken Sie auf dem Blatt **Parameter bearbeiten** auf **Datei laden**. Navigieren Sie anschließend im Dialogfeld **Öffnen** zu **\\\\AllFiles\Labs\\06\\active-directory-new-domain\\azuredeploy.parameters.json**, klicken Sie auf **Öffnen** und dann auf **Speichern**.

5. Geben Sie auf dem Blatt **Azure-VM mit einer neuen AD-Gesamtstruktur erstellen** die folgenden Einstellungen an (übernehmen Sie für andere Einstellungen die vorhandenen Werte):

   |Einstellung|Wert|
   |---|---|
   |Subscription|Der Name Ihres Azure-Abonnements|
   |Resource group|Klicken Sie auf **Neu erstellen**, und geben Sie den Namen **AZ500LAB06** ein.|
   |Region|Die Azure-Region, die Sie in der vorherigen Aufgabe identifiziert haben|
   |Administratorbenutzername|**Kursteilnehmer**|
   |Administratorkennwort|**Verwenden Sie Ihr persönliches Kennwort, das Sie in Lab 04 > Übung 1 > Aufgabe 1 > Schritt 9 erstellt haben.**|
   |Domänenname|**adatum.com**|
   |DNS-Präfix|Der DNS-Hostname, den Sie in der vorherigen Aufgabe identifiziert haben|
   |Größe des virtuellen Computers|**Standard_D2s_v3**|

6. Klicken Sie auf dem Blatt **Azure-VM mit einer neuen AD-Gesamtstruktur erstellen** auf **Überprüfen + erstellen** und dann auf **Erstellen**.

    >**Hinweis**: Warten Sie nicht, bis die Bereitstellung abgeschlossen ist, sondern führen Sie stattdessen die nächste Aufgabe aus. Die Bereitstellung kann ungefähr 15 Minuten dauern. Sie verwenden dafür den virtuellen Computer, der in dieser Aufgabe in der dritten Übung dieses Labs bereitgestellt wurde.

> Ergebnis: Nach Abschluss dieser Übung haben Sie mithilfe einer Azure Resource Manager-Vorlage die Bereitstellung einer Azure-VM initiiert, die einen Active Directory-Domänencontroller hosten soll.


### <a name="exercise-2-create-and-configure-an-azure-active-directory-tenant"></a>Übung 2: Erstellen und Konfigurieren eines Azure Active Directory-Mandanten 

### <a name="estimated-timing-20-minutes"></a>Geschätzte Zeit: 20 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Erstellen eines Azure Active Directory (Azure AD)-Mandanten
- Aufgabe 2: Hinzufügen eines benutzerdefinierten DNS-Namens zum neuen Azure AD-Mandanten
- Aufgabe 3: Erstellen eines Azure AD-Benutzers mit der Rolle „Globaler Administrator“

#### <a name="task-1-create-an-azure-active-directory-ad-tenant"></a>Aufgabe 1: Erstellen eines Azure Active Directory (Azure AD)-Mandanten

In dieser Aufgabe erstellen Sie einen neuen Azure AD-Mandanten, der in diesem Lab verwendet werden soll. 

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Azure Active Directory** ein, und drücken Sie die **EINGABETASTE**.

2. Klicken Sie auf dem Blatt mit der **Übersicht** über Ihren aktuellen Azure AD-Mandanten auf **Mandanten verwalten** und dann im nächsten Bildschirm auf **+ Erstellen**.

3. Stellen Sie auf dem Blatt **Verzeichnis erstellen** auf der Registerkarte **Grundlagen** sicher, dass die Option **Azure Active Directory** ausgewählt ist, und klicken Sie auf **Weiter: Konfiguration >** .

4. Geben Sie auf der Registerkarte **Konfiguration** des Blatts **Verzeichnis erstellen** die folgenden Einstellungen an:

   |Einstellung|Wert|
   |---|---|
   |Name der Organisation|**AdatumSync**|
   |Name der Anfangsdomäne|Ein eindeutiger Name, der aus einer Kombination aus Buchstaben und Ziffern besteht|
   |Land oder Region|**USA**|

    >**Hinweis**: Notieren Sie sich den Namen der Anfangsdomäne. Sie benötigen ihn später in diesem Lab.

    >**Hinweis**: Das grüne Häkchen im Textfeld **Name der Anfangsdomäne** gibt an, dass der eingegebene Domänenname gültig und eindeutig ist. (Notieren Sie sich den Namen Ihrer Anfangsdomäne zur späteren Verwendung.)

5. Klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**.

    >**Hinweis**: Warten Sie, bis der neue Mandant erstellt wurde. Verwenden Sie das Symbol **Benachrichtigung** zum Überwachen des Bereitstellungsstatus. 

#### <a name="task-2-add-a-custom-dns-name-to-the-new-azure-ad-tenant"></a>Aufgabe 2: Hinzufügen eines benutzerdefinierten DNS-Namens zum neuen Azure AD-Mandanten

In dieser Aufgabe fügen Sie ihren benutzerdefinierten DNS-Namen dem neuen Azure AD-Mandanten hinzu. 

1. Klicken Sie im Azure-Portal auf der Symbolleiste rechts neben dem Cloud Shell-Symbol auf das Symbol **Verzeichnis + Abonnement**. 

2. Wählen Sie auf dem Blatt **Verzeichnis + Abonnement** die Zeile mit dem neu erstellten Mandanten **AdatumSync**, und klicken Sie auf die Schaltfläche **Wechseln**.

    >**Hinweis**: Möglicherweise müssen Sie das Browserfenster aktualisieren, wenn der Eintrag **AdatumSync** in der Filterliste **Verzeichnis + Abonnement** nicht angezeigt wird.

3. Klicken Sie auf dem Blatt **AdatumSync \| Azure Active Directory** im Abschnitt **Verwalten** auf **Benutzerdefinierte Domänennamen**.

4. Klicken Sie auf dem Blatt **AdatumSync \| Benutzerdefinierte Domänennamen** auf **+ Benutzerdefinierte Domäne hinzufügen**.

5. Geben Sie auf dem Blatt **Benutzerdefinierter Domänenname** im Textfeld **Benutzerdefinierter Domänenname** den Namen **adatum.com** ein, und klicken Sie auf **Domäne hinzufügen**.

6. Überprüfen Sie auf dem Blatt **adatum.com** die Informationen, die zum Überprüfen des Azure AD-Domänennamens erforderlich sind, und wählen Sie dann zweimal **Löschen** aus. 

    >**Hinweis**: Sie können den Überprüfungsprozess nicht abschließen, weil Sie nicht der Besitzer des DNS-Domänennamens **adatum.com** sind. Dies hindert Sie nicht daran, die AD DS-Domäne **adatum.com** mit dem Azure AD-Mandanten zu synchronisieren. Zu diesem Zweck verwenden Sie den anfänglichen DNS-Namen des Azure AD-Mandanten (der Name endet mit dem Suffix **onmicrosoft.com**), den Sie in der vorherigen Aufgabe identifiziert haben. Bedenken Sie jedoch, dass sich infolgedessen der DNS-Domänenname der AD DS-Domäne und der DNS-Name des Azure AD-Mandanten unterscheiden werden. Dies bedeutet, dass Adatum-Benutzer unterschiedliche Namen verwenden müssen, wenn sie sich bei der AD DS-Domäne und beim Azure AD-Mandanten anmelden.

#### <a name="task-3-create-an-azure-ad-user-with-the-global-administrator-role"></a>Aufgabe 3: Erstellen eines Azure AD-Benutzers mit der Rolle „Globaler Administrator“

In dieser Aufgabe fügen Sie einen neuen Azure AD-Benutzer hinzu und weisen ihn der Rolle „Globaler Administrator“ zu. 

1. Klicken Sie auf dem Blatt **AdatumSync** des Azure AD-Mandanten im Abschnitt **Verwalten** auf **Benutzer**.

2. Klicken Sie auf dem Blatt **Benutzer \| Alle Benutzer** auf **+ Neuer Benutzer**. 

3. Stellen Sie auf dem Blatt **Neuer Benutzer** sicher, dass die Option **Benutzer erstellen** ausgewählt ist, geben Sie die folgenden Einstellungen an (übernehmen Sie für alle anderen Einstellungen die Standardwerte), und klicken Sie auf **Erstellen**:

   |Einstellung|Wert|
   |---|---|
   |Benutzername|**syncadmin**|
   |Name|**syncadmin**|
   |Kennwort|Stellen Sie sicher, dass die Option **Kennwort automatisch generieren** ausgewählt ist, und klicken Sie auf **Kennwort anzeigen**.|
   |Gruppen|**0 Gruppen ausgewählt**|
   |Rollen|Klicken Sie auf **Benutzer**, anschließend auf **Globaler Administrator** und dann auf **Auswählen**.|
   |Nutzungsstandort|**USA**|  

    >**Hinweis**: Notieren Sie sich den vollständigen Benutzernamen. Sie können den Wert kopieren, indem Sie auf der rechten Seite der Dropdownliste, die den Domänennamen anzeigt, auf die Schaltfläche **In Zwischenablage kopieren** klicken. 

    >**Hinweis**: Notieren Sie sich das Kennwort des Benutzers. Sie benötigen es später in diesem Lab. 

    >**Hinweis**: Zum Implementieren von Azure AD Connect ist ein Azure AD-Benutzer mit der Rolle „Globaler Administrator“ erforderlich.

4. Öffnen Sie ein Browserfenster im InPrivate-Modus.

5. Navigieren Sie zum Azure-Portal, und melden Sie sich mit dem Benutzerkonto **syncadmin** an. Ändern Sie bei der entsprechenden Aufforderung das Kennwort, das Sie sich zuvor in dieser Aufgabe notiert haben, in **Pa55w.rd1234**.

    >**Hinweis**: Für die Anmeldung müssen Sie einen vollqualifizierten Namen des Benutzerkontos **syncadmin** angeben, einschließlich des DNS-Domänennamens für den Azure AD-Mandanten, den Sie sich zuvor in dieser Aufgabe notiert haben. Dieser Benutzername hat das Format „syncadmin@`<your_tenant_name>`.onmicrosoft.com“, wobei `<your_tenant_name>` der Platzhalter für Ihren eindeutigen Azure AD-Mandantennamen ist. 

6. Melden Sie sich als **syncadmin** ab, und schließen Sie das InPrivate-Browserfenster.

> **Ergebnis**: Nach Abschluss dieser Übung haben Sie einen Azure AD-Mandanten erstellt, dem neuen Azure AD-Mandanten einen benutzerdefinierten DNS-Namen hinzugefügt und einen Azure AD-Benutzer mit der Rolle „Globaler Administrator“ erstellt.


### <a name="exercise-3-synchronize-active-directory-forest-with-an-azure-active-directory-tenant"></a>Übung 3: Synchronisieren einer Active Directory-Gesamtstruktur mit einem Azure Active Directory-Mandanten

### <a name="estimated-timing-20-minutes"></a>Geschätzte Zeit: 20 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Vorbereiten von AD DS für die Verzeichnissynchronisierung
- Aufgabe 2: Installieren von Azure AD Connect
- Aufgabe 3: Überprüfen der Verzeichnissynchronisierung

#### <a name="task-1-prepare-ad-ds-for-directory-synchronization"></a>Aufgabe 1: Vorbereiten von AD DS für die Verzeichnissynchronisierung

In dieser Aufgabe stellen Sie eine Verbindung mit der Azure-VM her, auf der der AD DS-Domänencontroller ausgeführt wird, und Sie erstellen ein Verzeichnissynchronisierungskonto. 

   > Bevor Sie mit dieser Aufgabe beginnen, vergewissern Sie sich, dass die Vorlagenbereitstellung, die Sie in der ersten Übung dieses Labs gestartet haben, abgeschlossen wurde.

1. Legen Sie im Azure-Portal den Filter **Verzeichnis + Abonnement** auf den Azure AD-Mandanten fest, der dem Azure-Abonnement zugeordnet ist, in dem Sie die Azure-VM in der ersten Übung dieses Labs bereitgestellt haben.

2. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Virtuelle Computer** ein, und drücken Sie die **EINGABETASTE**.

3. Klicken Sie auf dem Blatt **Virtuelle Computer** auf den Eintrag **adVM**. 

4. Klicken Sie auf dem Blatt **adVM** auf **Verbinden** und dann im Dropdownmenü auf **RDP**. 

5. Wählen Sie im Parameter **IP-Adresse** die **öffentliche IP-Adresse des Lastenausgleichs** aus. Klicken Sie dann auf **RDP-Datei herunterladen**, und verwenden Sie diese Datei, um über Remotedesktop eine Verbindung mit der Azure-VM **adVM** herzustellen. Wenn Sie zur Authentifizierung aufgefordert werden, geben Sie die folgenden Anmeldeinformationen an:

   |Einstellung|Wert|
   |---|---|
   |Benutzername|**Kursteilnehmer**|
   |Kennwort|**Verwenden Sie Ihr persönliches Kennwort, das Sie in Lab 04 > Übung 1 > Aufgabe 1 > Schritt 9 erstellt haben.**|

    >**Hinweis**: Warten Sie, bis die Remotedesktop-Sitzung und der **Server-Manager** geladen werden.  

    >**Hinweis**: Die folgenden Schritte werden in der Remotedesktop-Sitzung für die Azure-VM **adVM** ausgeführt. 

6. Klicken Sie in **Server-Manager** auf **Lokaler Server** und dann auf **Verstärkte Sicherheitskonfiguration für IE**.

7. Legen Sie im Dialogfeld **Verstärkte Sicherheitskonfiguration für Internet Explorer** beide Optionen auf **Aus** fest, und klicken Sie auf **OK**.

8. Starten Sie Internet Explorer, navigieren Sie zu **https://www.microsoft.com/en-us/edge/business/download**, laden Sie Microsoft Edge-Installationsbinärdateien herunter, führen Sie die Installation durch, und konfigurieren Sie den Webbrowser mit den Standardeinstellungen.

9. Klicken Sie im **Server-Manager** auf **Tools** und dann im Dropdownmenü auf **Active Directory-Verwaltungscenter**.

10. Klicken Sie im **Active Directory-Verwaltungscenter** auf **adatum (local)** . Klicken Sie im Bereich **Aufgaben** unter dem Domänennamen **adatum (local)** auf **Neu** und im hierarchischen Menü auf **Organisationseinheit**.

11. Geben Sie im Fenster **Organisationseinheit erstellen** im Textfeld **Name** den Namen **ToSync** ein, und klicken Sie auf **OK**.

12. Doppelklicken Sie auf die neu erstellte Organisationseinheit **ToSync**, damit deren Inhalt im Detailbereich der Konsole „Active Directory-Verwaltungscenter“ angezeigt wird. 

13. Klicken Sie im Bereich **Aufgaben** im Abschnitt **ToSync** auf **Neu** und im kaskadierenden Menü auf **Benutzer**.

14. Erstellen Sie im Fenster **Benutzer erstellen** ein neues Benutzerkonto mit den folgenden Einstellungen (übernehmen Sie für andere Einstellungen die vorhandenen Werte), und klicken Sie auf **OK**:

   |Einstellung|Wert|
   |---|---|
   |Vollständiger Name|**aduser1**|
   |Benutzer-UPN-Anmeldung|**aduser1**|
   |SamAccountName-Anmeldung von Benutzer|**aduser1**|
   |Kennwort und Kennwort bestätigen|**Verwenden Sie Ihr persönliches Kennwort, das Sie in Lab 04 > Übung 1 > Aufgabe 1 > Schritt 9 erstellt haben.**|
   |Andere Kennwortoptionen|**Kennwort läuft nie ab**|

#### <a name="task-2-install-azure-ad-connect"></a>Aufgabe 2: Installieren von Azure AD Connect

In dieser Aufgabe installieren Azure AD Connect auf dem virtuellen Computer. 

1. Verwenden Sie in der Remotedesktop-Sitzung für **adVM** Microsoft Edge, um unter **https://portal.azure.com** zum Azure-Portal zu navigieren, und melden Sie sich mit dem Benutzerkonto **syncadmin** an, das Sie in der vorherigen Übung erstellt haben. Geben Sie bei der entsprechenden Aufforderung den vollständigen Benutzernamen, den Sie sich notiert haben, und das Kennwort **Pa55w.rd1234** an.

2. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Azure Active Directory** ein, und drücken Sie die **EINGABETASTE**.

3. Klicken Sie im Azure-Portal auf dem Blatt **AdatumSync \| Übersicht** auf **Azure AD Connect**.

4. Klicken Sie auf dem Blatt **AdatumSync \| Azure AD Connect** auf den Link **Azure AD Connect herunterladen**. Sie werden zur Downloadseite **Microsoft Azure Active Directory Connect** umgeleitet.

5. Klicken Sie auf der Downloadseite **Microsoft Azure Active Directory Connect** auf **Herunterladen**.

6. Klicken Sie bei der entsprechenden Aufforderung auf **Ausführen**, um den Assistenten für **Microsoft Azure Active Directory Connect** zu starten.

7. Klicken Sie auf der Seite **Willkommen bei Azure AD Connect** des Assistenten für **Microsoft Azure Active Directory Connect** auf das Kontrollkästchen **Ich stimme den Lizenzbedingungen und dem Datenschutzhinweis zu** und dann auf **Weiter**.

8. Klicken Sie auf der Seite **Expresseinstellungen** des Assistenten für **Microsoft Azure Active Directory Connect** auf die Option **Anpassen**.

9. Lassen Sie auf der Seite **Erforderliche Komponenten installieren** alle optionalen Konfigurationsoptionen deaktiviert, und klicken Sie auf **Installieren**.

10. Stellen Sie auf der Seite **Benutzeranmeldung** sicher, dass nur die **Kennwort-Hashsynchronisierung** aktiviert ist, und klicken Sie auf **Weiter**.

11. Authentifizieren Sie sich auf der Seite **Mit Azure AD verbinden** mit den Anmeldeinformationen des Benutzerkontos **syncadmin**, das Sie in der vorherigen Übung erstellt haben, und klicken Sie auf **Weiter**. 

12. Klicken Sie auf der Seite **Verzeichnisse verbinden** rechts neben dem Gesamtstruktureintrag **adatum.com** auf die Schaltfläche **Verzeichnis hinzufügen**.

13. Stellen Sie im Fenster **AD-Gesamtstrukturkonto** sicher, dass die Option **Neues AD-Konto erstellen** ausgewählt ist, geben Sie die folgenden Anmeldeinformationen an, und klicken Sie auf **OK**:

   |Einstellung|Wert|
   |---|---|
   |Benutzername|**ADATUM\\Kursteilnehmer**|
   |Kennwort|**Verwenden Sie Ihr persönliches Kennwort, das Sie in Lab 06 > Übung 1 > Aufgabe 2 erstellt haben.**|

14. Stellen Sie auf der Seite **Verzeichnisse verbinden** sicher, dass der Eintrag **adatum.com** als konfiguriertes Verzeichnis angezeigt wird, und klicken Sie auf **Weiter**.

15. Beachten Sie auf der Seite **Azure AD-Anmeldungskonfiguration** die Warnung **Benutzer können sich nicht mit lokalen Anmeldeinformationen bei Azure AD anmelden, wenn das UPN-Suffix keinem überprüften Domänennamen zugeordnet werden kann**, aktivieren Sie das Kontrollkästchen **Ohne Abgleich aller UPN-Suffixe mit überprüfter Domäne fortfahren**, und klicken Sie auf **Weiter**.

    >**Hinweis:** Wie bereits erläutert wurde, wird dies erwartet, da Sie die benutzerdefinierte Azure AD DNS-Domäne **adatum.com** nicht überprüfen konnten.

16. Klicken Sie auf der Seite **Filtern von Domänen und Organisationseinheiten** auf die Option **Ausgewählte Domänen und Organisationseinheiten synchronisieren**. Der Domänenname **adatum.com** wird überprüft. Erweitern Sie **adatum.com**, um die **ToSync**-OE anzuzeigen. Deaktivieren Sie alle Kontrollkästchen. Aktivieren Sie nur das Kontrollkästchen neben der **ToSync**-OE, und klicken Sie auf **Weiter**.

17. Übernehmen Sie auf der Seite **Eindeutige Identifizierung Ihrer Benutzer** die Standardeinstellungen, und klicken Sie auf **Weiter**.

18. Übernehmen Sie auf der Seite **Benutzer und Geräte filtern** die Standardeinstellungen, und klicken Sie auf **Weiter**.

19. Übernehmen Sie auf der Seite **Optionale Features** die Standardeinstellungen, und klicken Sie auf **Weiter**.

20. Vergewissern Sie sich auf der Seite **Bereit zur Konfiguration**, dass das Kontrollkästchen **Starten Sie den Synchronisierungsvorgang, nachdem die Konfiguration abgeschlossen wurde** aktiviert ist, und klicken Sie auf **Installieren**.

    >**Hinweis**: Die Installation sollte ungefähr 2 Minuten dauern.

21. Überprüfen Sie die Informationen auf der Seite **Konfiguration abgeschlossen**, und klicken Sie auf **Beenden**, um das Fenster **Microsoft Azure Active Directory Connect** zu schließen.


#### <a name="task-3-verify-directory-synchronization"></a>Aufgabe 3: Überprüfen der Verzeichnissynchronisierung

In dieser Aufgabe überprüfen Sie, ob die Verzeichnissynchronisierung funktioniert. 

1. Navigieren Sie in der Remotedesktop-Sitzung zu **adVM** im Microsoft Edge-Fenster, in dem das Azure-Portal angezeigt wird, zum Blatt **Benutzer – Alle Benutzer (Vorschau)** des Azure AD-Mandanten „Adatum Lab“.

2. Beachten Sie auf dem Blatt **Benutzer \| Alle Benutzer**, dass die Liste der Benutzerobjekte das Konto **aduser1** enthält. 

>**Hinweis**: Möglicherweise müssen Sie einige Minuten warten und **Aktualisieren** auswählen, damit das Benutzerkonto **aduser1** angezeigt wird.

3. Wählen Sie das Konto **aduser1** aus, und beachten Sie im Abschnitt **Profil > Identität**, dass das Attribut **Quelle** auf **Windows Server AD** festgelegt wurde.

4. Beachten Sie auf dem Blatt **aduser1 \| Profil** im Abschnitt **Jobinformationen**, dass das Attribut **Abteilung** nicht festgelegt wurde.

5. Wechseln Sie in der Remotedesktop-Sitzung für **adVM** zum **Active Directory-Verwaltungscenter**, wählen Sie in der Liste der Objekte in der Organisationseinheit **ToSync** den Eintrag **aduser1** und dann im Bereich **Aufgaben** im Abschnitt **aduser1** die Option **Eigenschaften** aus.

6. Geben Sie im Fenster **aduser1** im Abschnitt **Organisation** im Textfeld **Abteilung** den Begriff **Vertrieb** ein, und wählen Sie **OK** aus.

7. Starten Sie innerhalb der Remotedesktop-Sitzung für **adVM** die **Windows PowerShell**.

8. Führen Sie in der Konsole **Administrator: Windows PowerShell** Folgendes aus, um die Azure AD Connect-Deltasynchronisierung zu starten:

    ```powershell
    Import-Module -Name 'C:\Program Files\Microsoft Azure AD Sync\Bin\ADSync\ADSync.psd1'

    Start-ADSyncSyncCycle -PolicyType Delta
    ```

9. Wechseln Sie zum Microsoft Edge-Fenster mit dem Blatt **aduser1 \| Profil**, aktualisieren Sie die Seite, und beachten Sie, dass die Eigenschaft **Abteilung** auf **Vertrieb** festgelegt wurde.

    >**Hinweis**: Möglicherweise müssen Sie noch eine Minute warten und die Seite erneut aktualisieren, wenn das Attribut **Abteilung** noch nicht festgelegt wurde.

> **Ergebnis**: Nach Abschluss dieser Übung haben Sie AD DS für die Verzeichnissynchronisierung vorbereitet, Azure AD Connect installiert und die Verzeichnissynchronisierung überprüft.


**Bereinigen von Ressourcen**

>**Hinweis**: Deaktivieren Sie zunächst die Azure AD-Synchronisierung.

1. Starten Sie innerhalb der Remotedesktop-Sitzung für **adVM** die Windows PowerShell als Administrator.

2. Installieren Sie in der Windows PowerShell-Konsole das PowerShell-Modul „MsOnline“, indem Sie Folgendes ausführen (geben Sie bei der entsprechenden Aufforderung im Dialogfeld „NuGet provider is required to continue“ (Zum Fortsetzen ist der NuGet-Anbieter erforderlich) **Ja** ein, und drücken Sie die EINGABETASTE.):

    ```powershell
    [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
    Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force
    Install-Module MsOnline -Force
    ```

3. Stellen Sie über die Windows PowerShell-Konsole eine Verbindung mit dem Azure AD-Mandanten „AdatumSync“ her, indem Sie Folgendes ausführen (melden Sie sich bei der entsprechenden Aufforderung mit den Anmeldeinformationen für **syncadmin** an):

    ```powershell
    Connect-MsolService
    ```

4. Deaktivieren Sie in der Windows PowerShell-Konsole die Azure AD Connect-Synchronisierung, indem Sie Folgendes ausführen:

    ```powershell
    Set-MsolDirSyncEnabled -EnableDirSync $false -Force
    ```

5. Überprüfen Sie in der Windows PowerShell-Konsole, ob der Vorgang erfolgreich war, indem Sie Folgendes ausführen:

    ```powershell
    (Get-MSOLCompanyInformation).DirectorySynchronizationEnabled
    ```
    >**Hinweis**: Das Ergebnis sollte `False` lauten. Wenn dies nicht zutrifft, warten Sie eine Minute, und führen Sie den Befehl erneut aus.

    >**Hinweis**: Entfernen Sie als Nächstes die Azure-Ressourcen.
6. Schließen Sie die Remotedesktopsitzung.

7. Legen Sie im Azure-Portal den Filter **Verzeichnis + Abonnement** auf den Azure AD-Mandanten fest, der dem Azure-Abonnement zugeordnet ist, in dem Sie die Azure-VM **adVM** bereitgestellt haben.

8. Öffnen Sie im Azure-Portal die Cloud Shell, indem Sie oben rechts auf das erste Symbol klicken. 

9. Wählen Sie im Dropdownmenü oben links im Bereich „Cloud Shell“ die Option **PowerShell** aus, und klicken Sie bei der entsprechenden Aufforderung auf **Bestätigen**. 

10. Führen Sie im Cloud Shell-Bereich in der PowerShell-Sitzung den folgenden Code aus, um die Ressourcengruppe zu entfernen, die Sie in diesem Lab erstellt haben:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB06" -Force -AsJob
    ```
11. Schließen Sie den **Cloud Shell**-Bereich.

    >**Hinweis**: Entfernen Sie abschließend den Azure AD-Mandanten.
    
    >**Hinweis 2**: Weil das Löschen eines Mandanten ein sehr schwieriger Prozess ist, kann dies nie versehentlich oder böswillig geschehen.  Das bedeutet, dass das Entfernen des Mandanten im Rahmen dieses Labs oft nicht funktioniert.  Wir haben hier zwar die Schritte zum Löschen des Mandanten aufgeführt, aber sie sind für Sie nicht erforderlich zum Abschließen dieses Labs. Wenn Sie jemals einen Mandanten in der realen Welt entfernen müssen, gibt es unter „DOCS.Microsoft.com“ Artikel, die Ihnen helfen sollen.

12. Verwenden Sie im Azure-Portal den Filter **Verzeichnis + Abonnement**, um zum Azure Active Directory-Mandanten **AdatumSync** zu wechseln.

13. Navigieren Sie im Azure-Portal zum Blatt **Benutzer – Alle Benutzer**, und klicken Sie auf den Eintrag für das Benutzerkonto **syncadmin**. Klicken Sie auf dem Blatt **syncadmin – Profil** auf **Löschen** und dann auf **Ja**, wenn Sie zur Bestätigung aufgefordert werden.

14. Wiederholen Sie die gleichen Schritte, um das Benutzerkonto **aduser1** und das **lokale Konto des Verzeichnissynchronisierungsdiensts** zu löschen.

15. Navigieren Sie zum Blatt **AdatumSync – Übersicht** des Azure AD-Mandanten, klicken Sie auf **Mandanten verwalten**, und aktivieren Sie das Kontrollkästchen des **AdatumSync**-Verzeichnisses. Klicken Sie auf **Löschen**, und klicken Sie auf dem Blatt **Delete tenant 'AdatumSync'** (Mandant „AdatumSync“ löschen) auf den Link **Berechtigung zum Löschen von Azure-Ressourcen anfordern**. Legen Sie dann auf dem Azure Active Directory-Blatt **Eigenschaften** die Option **Zugriffsverwaltung für Azure-Ressourcen** auf **Ja** fest, und klicken Sie auf **Speichern**.

    >**Hinweis**: Wenn Ihnen beim Löschen ein Warnzeichen wie **Alle Benutzer löschen** angezeigt wird, dann fahren Sie mit dem Löschen der von Ihnen erstellten Benutzer fort. Wenn das Warnzeichen **Delete LinkedIn applications** (LinkedIn-Anwendungen löschen) angezeigt wird, klicken Sie auf die Warnmeldung, und bestätigen Sie die Löschung der LinkedIn-Anwendung. Alle Warnungen müssen behandelt werde, um den Mandanten zu löschen.

16. Melden Sie sich beim Azure-Portal ab, und melden Sie sich wieder an. 

17. Navigieren Sie zurück zum Blatt **Delete tenant 'AdatumSync'** (Mandant „AdatumSync“ löschen), und klicken Sie auf **Löschen**.

> Weitere Informationen zu dieser Aufgabe finden Sie unter [https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-delete-howto](https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-delete-howto).
