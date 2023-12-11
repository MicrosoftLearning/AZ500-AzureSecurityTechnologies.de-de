---
lab:
  title: "05: Azure\_AD Privileged Identity Management"
  module: Module 01 - Manage Identity and Access
---

# Lab 05: Azure AD Privileged Identity Management
# Lab-Handbuch für Kursteilnehmer

## Labszenario

Sie wurden aufgefordert, einen Proof of Concept zu erstellen, der Azure Privileged Identity Management (PIM) verwendet, um Just-in-Time-Verwaltung zu ermöglichen und die Anzahl der Benutzer zu steuern, die privilegierte Vorgänge ausführen können. Dies sind die spezifischen Anforderungen:

- Erstellen einer permanenten Zuweisung des Azure AD-Benutzers aaduser2 zur Rolle „Sicherheitsadministrator“. 
- Konfigurieren des Azure AD-Benutzers aaduser2 für die Rollen „Abrechnungsadministrator“ und „Globaler Leser“.
- Konfigurieren, dass die Aktivierung der Rolle „Globaler Leser“ eine Genehmigung des Azure AD-Benutzers aaduser3 erfordert
- Konfigurieren einer Zugriffsüberprüfung der Rolle „Globaler Leser“ und Überprüfen der Überwachungsfunktionen.

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

> Stellen Sie vor dem Fortfahren sicher, dass Sie Lab 04: MFA, bedingter Zugriff und AAD Identity Protection abgeschlossen haben. Sie benötigen den Azure AD-Mandanten, AdatumLab500-04 und die Benutzerkonten aaduser1, aaduser2 und aaduser3.

## Ziele des Labs

In diesem Lab führen Sie die folgenden Übungen aus:

- Übung 1: Konfigurieren von PIM-Benutzern und -Rollen.
- Übung 2: Aktivieren von PIM-Rollen mit und ohne Genehmigung.
- Übung 3: Erstellen einer Zugriffsüberprüfung und Überprüfen der PIM-Überwachungsfeatures.

## Diagramm zu Azure AD Privileged Identity Management

![image](https://user-images.githubusercontent.com/91347931/157522920-264ce57e-5c55-4a9d-8f35-e046e1a1e219.png)

## Anweisungen

### Übung 1: Konfigurieren von PIM-Benutzern und -Rollen

#### Geschätzte Zeit: 15 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Festlegen eines Benutzers als „berechtigt“ für eine Rolle.
- Aufgabe 2: Konfigurieren einer Rolle, um eine Genehmigung zum Aktivieren und Hinzufügen eines berechtigten Mitglieds anzufordern.
- Aufgabe 3: Festlegen einer permanente Zuweisung zu einer Rolle für einen Benutzer. 

#### Aufgabe 1: Festlegen eines Benutzers als „berechtigt“ für eine Rolle

In dieser Aufgabe legen Sie einen Benutzer für eine Azure AD-Verzeichnisrolle als „berechtigt“ fest.

1. Melden Sie sich unter **`https://portal.azure.com/`** beim Azure-Portal an.

    >**Hinweis**: Stellen Sie sicher, dass Sie beim Azure AD-Mandanten **AdatumLab500-04** angemeldet sind. Sie können den Filter **Verzeichnis und Abonnement** verwenden, um zwischen Azure AD-Mandanten zu wechseln. Stellen Sie sicher, dass Sie als Benutzer mit der Rolle „Globaler Administrator“ angemeldet sind.
    
    >**Hinweis**: Wenn der Eintrag AdatumLab500-04 nicht angezeigt wird, klicken Sie auf den Link „Verzeichnis wechseln“, wählen Sie die Zeile AdatumLab500-04 aus, und klicken Sie auf die Schaltfläche „Wechseln“.

2. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Azure AD Privileged Identity Management** ein, und drücken Sie die **EINGABETASTE**.

3. Klicken Sie auf dem Blatt **Privileged Identity Management** im Abschnitt **Verwalten** auf **Azure AD-Rollen**.

4. Klicken Sie auf dem Blatt **Schnellstart \| AdatumLab500-04** im Abschnitt **Verwalten** auf **Rollen**.

5. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Rollen** auf **+ Zuweisungen hinzufügen**.

6. Wählen Sie auf dem Blatt **Zuweisungen hinzufügen** in der Dropdownleiste **Rolle auswählen** die Option **Abrechnungsadministrator** aus.

7. Klicken Sie auf den Link **Kein Mitglied ausgewählt**, klicken Sie auf dem Blatt **Mitglied auswählen** auf **aaduser2**, und klicken Sie dann auf **Auswählen**.

8. Klicken Sie auf dem Blatt **Zuweisungen hinzufügen** auf **Weiter**. 

9. Stellen Sie sicher, dass der **Zuweisungstyp** auf **Berechtigt** festgelegt ist, und klicken Sie auf **Zuweisen**.
 
10. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Rollen** im Abschnitt **Verwalten** auf **Zuweisungen**.

11. Beachten Sie auf dem Blatt **AdatumLab500-04 \| Zuweisungen** die Registerkarten für **Berechtigte Zuweisungen**, **Aktive Zuweisungen** und **Abgelaufene Zuweisungen**.

12. Überprüfen Sie auf der Registerkarte **Berechtigte Zuweisungen**, ob **aaduser2** als **Abrechnungsadministrator** angezeigt wird. 

    >**Hinweis**: Während der Anmeldung ist aaduser2 berechtigt, die Rolle „Abrechnungsadministrator“ zu verwenden. 

#### Aufgabe 2: Konfigurieren einer Rolle, um eine Genehmigung zum Aktivieren und Hinzufügen eines berechtigten Mitglieds anzufordern

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Privileged Identity Management**, und klicken Sie auf **Azure AD-Rollen**.

2. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Schnellstart** im Abschnitt **Verwalten** auf **Rollen**.

3. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Rollen** auf den Rolleneintrag **Globaler Leser**. 

4. Klicken Sie auf dem Blatt **Globaler Leser \| Zuweisungen** auf das Symbol **Rolleneinstellungen** in der Symbolleiste des Blatts und überprüfen Sie die Konfigurationseinstellungen für die Rolle, einschließlich der Azure MFA-Anforderungen.

5. Klicken Sie auf **Bearbeiten**.

6. Aktivieren Sie auf der Registerkarte **Aktivierung** das Kontrollkästchen **Genehmigung zum Aktivieren anfordern**.

7. Klicken Sie auf den Link **Genehmigende Person(en) auswählen**, klicken Sie auf dem Blatt **Mitglied auswählen** auf **aaduser3**, und klicken Sie dann auf **Auswählen**.

8. Klicken Sie auf **Weiter: Zuweisung**.

9. Deaktivieren Sie das Kontrollkästchen **Permanente berechtigte Zuweisung zulassen**, und übernehmen Sie alle anderen Einstellungen mit ihren Standardwerten.

10. Klicken Sie auf **Weiter: Benachrichtigung**.

11. Überprüfen Sie die **Benachrichtigungseinstellungen**. Verändern Sie dabei die Standardeinstellungen nicht, und klicken Sie auf **Aktualisieren**.

    >**Hinweis**: Jeder Benutzer, der versucht, die Rolle „Globaler Leser“ zu verwenden, benötigt jetzt die Genehmigung von aaduser3. 

12. Klicken Sie auf dem Blatt **Globaler Leser \| Zuweisungen** auf **+ Zuweisungen hinzufügen**.

13. Klicken Sie auf dem Blatt **Zuweisungen hinzufügen** auf **Kein Mitglied ausgewählt**, klicken Sie auf dem Blatt **Mitglied auswählen** auf **aaduser2**, und klicken Sie dann auf **Auswählen**.

14. Klicken Sie auf **Weiter**. 

15. Stellen Sie sicher, dass der **Zuweisungstyp** **Berechtigt** ist, und überprüfen Sie die Einstellungen für die berechtigte Dauer.

16. Klicken Sie auf **Zuweisen**.

    >**Hinweis**: Der Benutzer aaduser2 ist für die Rolle „Globaler Leser“ berechtigt. 
 
#### Aufgabe 3: Festlegen einer permanente Zuweisung zu einer Rolle für einen Benutzer.

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Privileged Identity Management**, und klicken Sie auf **Azure AD-Rollen**.

2. Klicken Sie auf dem Blatt **Schnellstart \| AdatumLab500-04** im Abschnitt **Verwalten** auf **Rollen**.

3. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Rollen** auf **+ Zuweisungen hinzufügen**.

4. Wählen Sie auf dem Blatt **Zuweisungen hinzufügen** in der Dropdownleiste **Rolle auswählen** die Option **Sicherheitsadministrator** aus.

5. Klicken Sie auf dem Blatt **Zuweisungen hinzufügen** auf **Kein Mitglied ausgewählt**, klicken Sie auf dem Blatt **Mitglied auswählen** auf **aaduser2**, und klicken Sie dann auf **Auswählen**.

6. Klicken Sie auf **Weiter**. 

7. Überprüfen Sie die Einstellungen **für den Zuweisungstyp**, und klicken Sie auf **Zuweisen.**

8. Klicken Sie im linken Navigationsbereich auf **Zuweisungen**. Wählen Sie auf der Registerkarte **Berechtigte Zuweisungen** unter **Sicherheitsadministrator*in** die Option **Update** für die **aaduser2**-Zuweisung aus. Wählen Sie **Permanent berechtigt** und **Speichern** aus.

    >**Hinweis**: Der Benutzer aaduser2 ist jetzt dauerhaft für die Rolle „Sicherheitsadministrator“ berechtigt.
    
### Übung 2: Aktivieren von PIM-Rollen mit und ohne Genehmigung

#### Geschätzte Zeit: 15 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Aktivieren einer Rolle, die keine Genehmigung erfordert. 
- Aufgabe 2: Aktivieren einer Rolle, die Genehmigung erfordert. 

#### Aufgabe 1: Aktivieren einer Rolle, die keine Genehmigung erfordert.

In dieser Aufgabe aktivieren Sie eine Rolle, die keine Genehmigung erfordert.

1. Öffnen Sie ein Browserfenster im InPrivate-Modus.

2. Navigieren Sie im InPrivate-Browserfenster zum Azure-Portal unter **`https://portal.azure.com/`** und melden Sie sich mit dem Benutzerkonto **aaduser2** an.

    >**Hinweis**: Für die Anmeldung müssen Sie einen vollqualifizierten Namen des Benutzerkontos **aaduser2** angeben, einschließlich des DNS-Domänennamens des Azure AD-Mandanten, den Sie sich zuvor in diesem Lab notiert haben. Dieser Benutzername weist das Format aaduser2@`<your_tenant_name>`.onmicrosoft.com auf, wobei `<your_tenant_name>` der Platzhalter ist, der Ihren eindeutigen Azure AD-Mandantennamen darstellt. 

3. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Azure AD Privileged Identity Management** ein, und drücken Sie die **EINGABETASTE**.

4. Klicken Sie auf dem Blatt **Privileged Identity Management** im Abschnitt **Aufgaben** auf **Meine Rollen**.

5. Es sollten drei **Berechtigte Rollen** für **aaduser2** angezeigt werden: **Globaler Leser**, **Sicherheitsadministrator** und **Abrechnungsadministrator**. 

6. Klicken Sie in der Zeile mit dem Rolleneintrag **Abrechnungsadministrator** auf **Aktivieren**.

7. Klicken Sie bei Bedarf auf die Warnung **Zusätzliche Überprüfung erforderlich. Klicken Sie, um fortzufahren**, und befolgen Sie die Anweisungen, um Ihre Identität zu überprüfen.

8. Geben Sie auf dem Blatt **Aktivieren - Abrechnungsadministrator** im Textfeld **Grund** einen Text mit einer Begründung für die Aktivierung ein, und klicken Sie dann auf **Aktivieren**.

    >**Hinweis**: Wenn Sie eine Rolle in PIM aktivieren, kann es bis zu 10 Minuten dauern, bis die Aktivierung wirksam wird. 
    
    >**Hinweis**: Sobald Ihre Rollenzuweisung aktiv ist, wird Ihr Browser aktualisiert (wenn etwas nicht funktioniert, melden Sie sich einfach ab, und melden Sie sich mit dem Benutzerkonto **aaduser2** erneut am Azure-Portal an).

9. Navigieren zurück zum Blatt **Privileged Identity Management**, und klicken Sie dann im Abschnitt **Aufgaben** auf **Meine Rollen**.

10. Wechseln Sie auf dem Blatt **Meine Rollen \| Azure AD-Rollen** zur Registerkarte **Aktive Zuweisungen**. Beachten Sie, dass die Rolle **Abrechnungsadministrator** **aktiviert** ist.

    >**Hinweis**: Wenn eine Rolle aktiviert wurde, wird sie nach Erreichen des Zeitlimits unter **Endzeit** (berechtigte Dauer) automatisch deaktiviert.

    >**Hinweis**: Wenn Sie Ihre Administratoraufgaben frühzeitig abschließen, können Sie eine Rolle manuell deaktivieren.

11.  Klicken Sie in der Liste **Aktive Zuweisungen** in der Zeile, die die Rolle „Abrechnungsadministrator“ darstellt, auf den Link **Deaktivieren**.

12.  Klicken Sie auf dem Blatt **Deaktivieren - Abrechnungsadministrator** erneut auf **Deaktivieren**, um den Vorgang zu bestätigen.


#### Aufgabe 2: Aktivieren einer Rolle, die Genehmigung erfordert. 

In dieser Aufgabe aktivieren Sie eine Rolle, die Genehmigung erfordert.

1. Navigieren Sie im InPrivate-Browserfenster im Azure-Portal, während Sie als Benutzer **aaduser2** angemeldet sind, zurück zum Blatt **Privileged Identity Management \| Schnellstart**. 

2. Klicken Sie auf dem Blatt **Privileged Identity Management \| Schnellstart** im Abschnitt **Aufgaben** auf **Meine Rollen**.

3. Klicken Sie auf dem Blatt **Meine Rollen \| Azure AD Rollen** in der Liste **Berechtigte Zuweisungen**in der Zeile mit der Rolle **Globaler Leser** auf **Aktivieren**. 

4. Geben Sie auf dem Blatt **Aktivieren - Globaler Leser** im Textfeld **Grund** einen Text mit einer Begründung für die Aktivierung ein, und klicken Sie dann auf **Aktivieren**.

5. Klicken Sie auf der Symbolleiste des Azure-Portals auf das **Benachrichtigungssymbol**, und zeigen Sie die Benachrichtigung an, dass die Genehmigung Ihrer Anforderung aussteht.

    >**Hinweis**: Als Administrator für die privilegierte Rolle können Sie Anforderungen jederzeit überprüfen und abbrechen. 

6. Suchen Sie auf dem Blatt **Meine Rollen \| Azure AD-Rollen** die Rolle **Sicherheitsadministrator**, und klicken Sie auf **Aktivieren**. 

7. Klicken Sie bei Bedarf auf die Warnung **Zusätzliche Überprüfung erforderlich. Klicken Sie, um fortzufahren**, und befolgen Sie die Anweisungen, um Ihre Identität zu überprüfen.

    >**Hinweis**: Sie müssen sich nur ein Mal pro Sitzung authentifizieren. 

8. Sobald Sie zurück in der Benutzeroberfläche des Azure-Portals sind, geben Sie auf dem Blatt **Aktivieren - Sicherheitsadministrator** im Textfeld **Grund** einen Text mit einer Begründung für die Aktivierung ein, und klicken Sie dann auf **Aktivieren**.

    >**Hinweis**: Der automatische Genehmigungsprozess sollte abgeschlossen sein.

9. Klicken Sie auf dem Blatt **Meine Rollen \| Azure AD-Rollen** auf die Registerkarte **Aktive Zuweisungen**, und beachten Sie, dass die Liste **Aktive Zuweisungen** die Rolle **Sicherheitsadministrator**, aber nicht die Rolle **Globaler Leser** enthält.

    >**Hinweis**: Sie genehmigen jetzt die Rolle „Globaler Leser“.

10. Melden Sie sich vom Azure-Portal als **aaduser2** ab.

11. Melden Sie sich im InPrivate-Browser am Azure-Portal unter **`https://portal.azure.com/`** als **aaduser3** an.

    >**Hinweis**: Wenn bei der Authentifizierung mit einem der Benutzerkonten Probleme auftreten, können Sie sich beim Azure AD-Mandanten anmelden, indem Sie Ihr Benutzerkonto verwenden, um seine Kennwörter zurückzusetzen oder seine Anmeldeoptionen neu zu konfigurieren.

12. Navigieren Sie im Azure-Portal zu **Azure AD Privileged Identity Management** (Geben Sie im Textfeld „Ressourcen, Dienste und Dokumente durchsuchen“ oben auf der Azure-Portal-Seite „Azure AD Privileged Identity Management“ ein, und drücken Sie die EINGABETASTE).

13. Klicken Sie auf dem Blatt **Privileged Identity Management \| Schnellstart** im Abschnitt **Aufgaben** auf **Anforderungen genehmigen**.

14. Aktivieren Sie auf dem Blatt **Anforderungen genehmigen \| Azure AD-Rollen** im Abschnitt **Anforderungen für Rollenaktivierungen** das Kontrollkästchen für den Eintrag, der die Rollenaktivierungsanforderung für die Rolle **Globaler Leser** von **aaduser2** darstellt.

15. Klicken Sie auf **Approve**. Geben Sie auf dem Blatt **Anforderung genehmigen** im Textfeld **Begründung** einen Grund für die Aktivierung ein, beachten Sie die Start- und Endzeiten, und klicken Sie dann auf **Bestätigen**. 

    >**Hinweis**: Sie haben auch die Möglichkeit, Anforderungen zu verweigern.

16. Melden Sie sich vom Azure-Portal als **aaduser3** ab.

17. Melden Sie sich im InPrivate-Browser am Azure-Portal unter **`https://portal.azure.com/`** als **aaduser2** an.

18. Navigieren Sie im Azure-Portal zu **Azure AD Privileged Identity Management** (Geben Sie im Textfeld „Ressourcen, Dienste und Dokumente durchsuchen“ oben auf der Azure-Portal-Seite „Azure AD Privileged Identity Management“ ein, und drücken Sie die EINGABETASTE).

19. Klicken Sie auf dem Blatt **Privileged Identity Management \| Schnellstart** im Abschnitt **Aufgaben** auf **Meine Rollen**.

20. Klicken Sie auf dem Blatt **Meine Rollen \| Azure AD-Rollen** auf die Registerkarte **Aktive Zuweisungen**, und überprüfen Sie, ob die Rolle „Globaler Leser“ jetzt aktiv ist.

    >**Hinweis**: Möglicherweise müssen Sie die Seite aktualisieren, um die aktualisierte Liste der aktiven Zuweisungen anzuzeigen.

21. Melden Sie sich ab, und schließen Sie das InPrivate-Browserfenster.

> Ergebnis: Sie haben das Aktivieren von PIM-Rollen mit und ohne Genehmigung geübt. 

### Übung 3: Erstellen einer Zugriffsüberprüfung und Überprüfen der PIM-Überwachungsfeatures

#### Geschätzte Zeit: 10 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Konfigurieren von Sicherheitswarnungen für Azure AD-Verzeichnisrollen in PIM
- Aufgabe 2: Überprüfen von PIM-Warnungen, Zusammenfassungsinformationen und detaillierten Überwachungsinformationen

#### Aufgabe 1: Konfigurieren von Sicherheitswarnungen für Azure AD-Verzeichnisrollen in PIM

In dieser Aufgabe reduzieren das Risiko durch „veraltete“ Rollenzuweisungen. Hierzu erstellen Sie eine PIM-Zugriffsüberprüfung, um sicherzustellen, dass zugewiesene Rollen noch gültig sind. Insbesondere überprüfen Sie die Rolle „Globaler Leser“. 

1. Melden Sie sich mit Ihrem Konto am Azure-Portal ( **`https://portal.azure.com/`** ) an.

    >**Hinweis**: Stellen Sie sicher, dass Sie beim Azure AD-Mandanten **AdatumLab500-04** angemeldet sind. Sie können den Filter **Verzeichnis und Abonnement** verwenden, um zwischen Azure AD-Mandanten zu wechseln. Stellen Sie sicher, dass Sie als Benutzer mit der Rolle „Globaler Administrator“ angemeldet sind.
    
    >**Hinweis**: Wenn der Eintrag AdatumLab500-04 nicht angezeigt wird, klicken Sie auf den Link „Verzeichnis wechseln“, wählen Sie die Zeile AdatumLab500-04 aus, und klicken Sie auf die Schaltfläche „Wechseln“.

2. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Azure AD Privileged Identity Management** ein, und drücken Sie die **EINGABETASTE**.

3. Navigieren Sie zum Blatt **Privileged Identity Management**. 

4. Klicken Sie auf dem Blatt **Privileged Identity Management \| Schnellstart** im Abschnitt **Verwalten** auf **Azure AD-Rollen**.

5. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Schnellstart** im Abschnitt **Verwalten** auf **Zugriffsüberprüfungen**.

6. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Zugriffsüberprüfungen** auf **Neu**:

7. Geben Sie auf dem Blatt **Zugriffsüberprüfung erstellen** die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen): 

   |Einstellung|Wert|
   |---|---|
   |Name der Überprüfung|**Überprüfung „Globaler Leser“**|
   |Startdatum|Datum von heute| 
   |Häufigkeit|**Einmal**|
   |Enddatum|Ende des aktuellen Monats|
   |Rolle, Privilegierte Rolle(n) auswählen|**Globaler Leser**|
   |Reviewer|**Ausgewählte Benutzer**|
   |Prüfer auswählen|Ihr Konto|

8. Klicken Sie auf dem Blatt **Zugriffsüberprüfung erstellen** auf **Start**:
 
    >**Hinweis**: Es dauert ungefähr eine Minute, bis die Überprüfung bereitgestellt wurde und auf dem Blatt **AdatumLab500-04 \| Zugriffsüberprüfungen** angezeigt wird. Möglicherweise müssen Sie die Webseite aktualisieren. Der Überprüfungsstatus ist **Aktiv**. 

9. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Zugriffsüberprüfungen** unter der Überschrift **Überprüfung „Globaler Leser“** auf den Eintrag **Globaler Leser**. 

10. Überprüfen Sie auf dem Blatt **Überprüfung „Globaler Leser“** die Seite **Übersicht**, und beachten Sie, dass in den **Statusdiagrammen** ein einzelner Benutzer in der Kategorie **Nicht überprüft** angezeigt wird. 

11. Klicken Sie auf dem Blatt **Überprüfung „Globaler Leser“** im Abschnitt **Verwalten** auf **Ergebnisse**. Beachten Sie, dass aaduser2 als zugriffsberechtigt für diese Rolle aufgeführt wird.

12. Klicken Sie in der Zeile **aaduser2** auf **Ansicht**, um ein ausführliches Überwachungsprotokoll mit Einträgen anzuzeigen, die PIM-Aktivitäten darstellen, die diesen Benutzer betreffen.

13. Navigieren Sie zurück zum Blatt **AdatumLab500-04 \| Zugriffsüberprüfungen**.

14. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Zugriffsüberprüfungen** im Abschnitt **Aufgaben** auf **Zugriff überprüfen**, und klicken Sie dann auf den Eintrag **Überprüfung „Globaler Leser“** . 

15. Klicken Sie auf dem Blatt **Überprüfung „Globaler Leser“** auf den Eintrag **aaduser2**. 

16. Geben Sie im Textfeld **Grund** eine Begründung für die Genehmigung ein, und klicken Sie dann entweder auf **Genehmigen**, um die aktuelle Rollenmitgliedschaft beizubehalten, oder auf **Verweigern**, um sie zu widerrufen. 

17. Navigieren Sie zurück zum Blatt **Privileged Identity Management**, und klicken Sie im Abschnitt **Verwalten** auf **Azure AD-Rollen**.

18. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Schnellstart** im Abschnitt **Verwalten** auf **Zugriffsüberprüfungen**.

19. Wählen Sie den Eintrag aus, der die Überprüfung **Globaler Leser** darstellt. Beachten Sie, dass das **Statusdiagramm** aktualisiert wurde, um Ihre Überprüfung anzuzeigen. 

#### Aufgabe 2: Überprüfen von PIM-Warnungen, Zusammenfassungsinformationen und detaillierten Überwachungsinformationen. 

In dieser Aufgabe überprüfen Sie PIM-Warnungen, Zusammenfassungsinformationen und detaillierte Überwachungsinformationen. 

1. Navigieren Sie zurück zum Blatt **Privileged Identity Management**, und klicken Sie im Abschnitt **Verwalten** auf **Azure AD-Rollen**.

2. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Schnellstart** im Abschnitt **Verwalten** auf **Warnungen** und dann auf **Einstellung**.

3. Überprüfen Sie auf dem Blatt **Warnungseinstellungen** die vorkonfigurierten Warnungen und Risikostufen. Klicken Sie auf einen beliebigen Eintrag, um ausführlichere Informationen zu erhalten. 

4. Kehren Sie zum Blatt **AdatumLab500-04 \| Schnellstart** zurück, und klicken Sie auf **Übersicht**. 

5. Überprüfen Sie auf dem Blatt **AdatumLab500-04 \| Übersicht** die Zusammenfassungsinformationen zu Rollenaktivierungen, PIM-Aktivitäten, Warnungen und Rollenzuweisungen.

6. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Übersicht** im Abschnitt **Aktivität** auf **Ressourcenüberwachung**. 

    >**Hinweis**: Der Überwachungsverlauf ist für alle Zuweisungen und Aktivierungen privilegierter Rollen innerhalb der letzten 30 Tage verfügbar.

7. Beachten Sie, dass Sie ausführliche Informationen abrufen können, z. B. **Zeit**, **Anforderer**, **Aktion**, **Ressourcenname**, **Bereich**, **Primäres Ziel** und **Antragsteller**. 

> Ergebnis: Sie haben eine Zugriffsüberprüfung konfiguriert und Überwachungsinformationen überprüft. 

**Bereinigen von Ressourcen**

> Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen nicht verwendeter Ressourcen wird sichergestellt, dass keine unerwarteten Kosten anfallen. 

1. Legen Sie im Azure-Portal den Filter **Verzeichnis und Abonnement** auf den Azure AD-Mandanten fest, der dem Azure-Abonnement zugeordnet ist, in dem Sie die Azure-VM **az500-04-vm1** bereitgestellt haben.

    >**Hinweis**: Wenn der Haupteintrag Ihres Azure AD-Mandanten nicht angezeigt wird, klicken Sie auf den Link „Verzeichnis wechseln“, wählen Sie Ihre Hauptmandantenzeile aus, und klicken Sie auf die Schaltfläche „Wechseln“.

2. Öffnen Sie im Azure-Portal Cloud Shell, indem Sie oben rechts im Azure-Portal auf das erste Symbol klicken. Klicken Sie bei der entsprechenden Aufforderung auf **PowerShell** und dann auf **Speicher erstellen**.

3. Stellen Sie sicher, dass oben links im Cloud Shell-Bereich im Dropdownmenü der Eintrag **PowerShell** ausgewählt ist.

4. Führen Sie im Cloud Shell-Bereich in der PowerShell-Sitzung den folgenden Code aus, um die Ressourcengruppe zu entfernen, die Sie im vorherigen Lab erstellt haben:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB04" -Force -AsJob
    ```

5. Schließen Sie den **Cloud Shell**-Bereich. 

6. Verwenden Sie im Azure-Portal den Filter **Verzeichnis und Abonnement**, um zum Azure Active Directory-Mandanten **AdatumLab500-04** zu wechseln.

7. Navigieren Sie zum Azure Active Directory-Blatt **AdatumLab500-04**, und klicken Sie im Abschnitt **Verwalten** auf **Lizenzen**.

8. Klicken Sie auf dem Blatt „**Lizenzen**  | Übersicht“ auf **Alle Produkte**, aktivieren Sie das Kontrollkästchen **Azure Active Directory Premium P2**, und klicken Sie darauf, um es zu öffnen.

    >**Hinweis**: In Lab 4, Übung 2, Aufgabe 4 **Zuweisen von Azure AD Premium P2-Lizenzen an Azure AD-Benutzer** sollten Sie die Premium-Lizenzen an die Benutzer **aaduser1, aaduser2 und aaduser3** vergeben. Stellen Sie sicher, dass Sie diese Lizenzen von den zugewiesenen Benutzern entfernen.

9. Aktivieren Sie auf dem Blatt **Azure Active Directory Premium P2 – Lizenzierte Benutzer** die Kontrollkästchen der Benutzerkonten, denen Sie **Azure Active Directory Premium P2**-Lizenzen zugewiesen haben. Klicken Sie im oberen Bereich auf **Lizenz entfernen**, und wählen Sie **Ja** aus, wenn Sie zur Bestätigung aufgefordert werden.

10. Navigieren Sie im Azure-Portal zum Blatt **Benutzer - Alle Benutzer**, und klicken Sie auf den Eintrag, der das Benutzerkonto **aaduser1** darstellt. Klicken Sie auf dem Blatt **aaduser1 - Profil** auf **Löschen**, und wählen Sie **Ja** aus, wenn Sie zur Bestätigung aufgefordert werden.

11. Wiederholen Sie diese Schrittfolge, um die übrigen Benutzerkonten zu löschen, die Sie erstellt haben.

12. Navigieren Sie zum Blatt **AdatumLab500-04 – Übersicht** des Azure AD-Mandanten, wählen Sie **Mandanten verwalten** aus, und aktivieren Sie dann auf dem nächsten Bildschirm das Kontrollkästchen neben **AdatumLab500-04**, und wählen Sie **Löschen** aus. Wählen Sie auf dem Blatt **Delete tenant 'AdatumLab500-04'** (Mandant „AdatumLab500-04“ löschen) den Link **Berechtigung zum Löschen von Azure-Ressourcen anfordern** aus. Legen Sie auf dem Azure Active Directory-Blatt **Eigenschaften** die Option **Zugriffsverwaltung für Azure-Ressourcen** auf **Ja** fest, und wählen Sie **Speichern** aus.

13. Melden Sie sich beim Azure-Portal ab, und melden Sie sich erneut an. 

14. Navigieren Sie zurück zum Blatt **Verzeichnis "AdatumLab500-04"** löschen, und klicken Sie auf **Löschen**.

    >**Hinweis**: Der Mandant kann weiterhin nicht gelöscht werden, und es wird der Fehler **Alle auf Lizenzen basierenden Abonnements löschen** ausgegeben. Dies kann durch Abonnements verursacht werden, die mit dem Mandanten verknüpft wurden. In diesem Fall könnte die **kostenlose Premium P2-Lizenz** den Validierungsfehler auslösen. Sie können dieses Problem lösen, indem Sie das Testabonnement der Premium P2-Lizenz mithilfe der globalen Administrator-ID aus dem M365-Administrator >> **Ihre Produkte** und aus dem **Business Store**-Portal löschen. Beachten Sie auch, dass das Löschen des Mandanten länger dauert. Überprüfen Sie das Enddatum des Abonnements. Rufen Sie nach dem Ende des Testzeitraums erneut Azure Active Directory auf, und versuchen Sie dann, den Mandanten zu löschen.    

> Weitere Informationen zu dieser Aufgabe finden Sie unter [https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-delete-howto](https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-delete-howto).
