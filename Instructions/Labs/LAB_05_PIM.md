---
lab:
  title: '05: Azure AD Privileged Identity Management'
  module: Module 01 - Manage Identity and Access
ms.openlocfilehash: 9e0b57125345a57e9f050667c7df18c6b8585ef9
ms.sourcegitcommit: 2eb153f2856445e5afaa218a012cb92e3d48f24b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2021
ms.locfileid: "132625650"
---
# <a name="lab-05-azure-ad-privileged-identity-management"></a>Lab 05: Azure AD Privileged Identity Management
# <a name="student-lab-manual"></a>Lab-Handbuch für Kursteilnehmer

## <a name="lab-scenario"></a>Labszenario

Sie wurden aufgefordert, einen Proof of Concept zu erstellen, der Azure Privileged Identity Management (PIM) verwendet, um Just-in-Time-Verwaltung zu ermöglichen und die Anzahl der Benutzer zu steuern, die privilegierte Vorgänge ausführen können. Dies sind die spezifischen Anforderungen:

- Erstellen einer permanenten Zuweisung des Azure AD-Benutzers aaduser2 zur Rolle „Sicherheitsadministrator“. 
- Konfigurieren des Azure AD-Benutzers aaduser2 für die Rollen „Abrechnungsadministrator“ und „Globaler Leser“.
- Konfigurieren, dass die Aktivierung der Rolle „Globaler Leser“ eine Genehmigung des Azure AD-Benutzers aaduser3 erfordert
- Konfigurieren einer Zugriffsüberprüfung der Rolle „Globaler Leser“ und Überprüfen der Überwachungsfunktionen.

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

> Stellen Sie vor dem Fortfahren sicher, dass Sie Lab 04: MFA, bedingter Zugriff und AAD Identity Protection abgeschlossen haben. Sie benötigen den Azure AD-Mandanten, AdatumLab500-04 und die Benutzerkonten aaduser1, aaduser2 und aaduser3.

## <a name="lab-objectives"></a>Ziele des Labs

In diesem Lab führen Sie die folgenden Übungen aus:

- Übung 1: Konfigurieren von PIM-Benutzern und -Rollen.
- Übung 2: Aktivieren von PIM-Rollen mit und ohne Genehmigung.
- Übung 3: Erstellen einer Zugriffsüberprüfung und Überprüfen der PIM-Überwachungsfeatures.

### <a name="exercise-1---configure-pim-users-and-roles"></a>Übung 1: Konfigurieren von PIM-Benutzern und -Rollen

#### <a name="estimated-timing-15-minutes"></a>Geschätzte Zeit: 15 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Festlegen eines Benutzers als „berechtigt“ für eine Rolle.
- Aufgabe 2: Konfigurieren einer Rolle, um eine Genehmigung zum Aktivieren und Hinzufügen eines berechtigten Mitglieds anzufordern.
- Aufgabe 3: Festlegen einer permanente Zuweisung zu einer Rolle für einen Benutzer. 

#### <a name="task-1-make-a-user-eligible-for-a-role"></a>Aufgabe 1: Festlegen eines Benutzers als „berechtigt“ für eine Rolle

In dieser Aufgabe legen Sie einen Benutzer für eine Azure AD-Verzeichnisrolle als „berechtigt“ fest.

1. Melden Sie sich am Azure-Portal ( **`https://portal.azure.com/`** ) an.

    >**Hinweis**: Stellen Sie sicher, dass Sie beim Azure AD-Mandanten **AdatumLab500-04** angemeldet sind. Sie können den Filter **Verzeichnis und Abonnement** verwenden, um zwischen Azure AD-Mandanten zu wechseln. Stellen Sie sicher, dass Sie als Benutzer mit der Rolle „Globaler Administrator“ angemeldet sind.
    
    >**Hinweis**: Wenn der Eintrag AdatumLab500-04 nicht angezeigt wird, klicken Sie auf den Link „Verzeichnis wechseln“, wählen Sie die Zeile AdatumLab500-04 aus, und klicken Sie auf die Schaltfläche „Wechseln“.

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Azure AD Privileged Identity Management** ein, und drücken Sie die **EINGABETASTE**.

1. Klicken Sie auf dem Blatt **Azure AD Privileged Identity Management** im Abschnitt **Verwalten** auf **Azure AD-Rollen**.

1. Klicken Sie auf dem Blatt **Schnellstart \| AdatumLab500-04** im Abschnitt **Verwalten** auf **Rollen**.

1. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Rollen** auf **+ Zuweisungen hinzufügen**.

1. Wählen Sie auf dem Blatt **Zuweisungen hinzufügen** in der Dropdownleiste **Rolle auswählen** die Option **Abrechnungsadministrator** aus.

1. Klicken Sie auf den Link **Kein Mitglied ausgewählt**, klicken Sie auf dem Blatt **Mitglied auswählen** auf **aaduser2**, und klicken Sie dann auf **Auswählen**.

1. Klicken Sie auf dem Blatt **Zuweisungen hinzufügen** auf **Weiter**. 

1. Stellen Sie sicher, dass der **Zuweisungstyp** auf **Berechtigt** festgelegt ist, und klicken Sie auf **Zuweisen**.
 
1. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Rollen** im Abschnitt **Verwalten** auf **Zuweisungen**.

1. Beachten Sie auf dem Blatt **AdatumLab500-04 \| Zuweisungen** die Registerkarten für **Berechtigte Zuweisungen**, **Aktive Zuweisungen** und **Abgelaufene Zuweisungen**.

1. Überprüfen Sie auf der Registerkarte **Berechtigte Zuweisungen**, ob **aaduser2** als **Abrechnungsadministrator** angezeigt wird. 

    >**Hinweis**: Während der Anmeldung ist aaduser2 berechtigt, die Rolle „Abrechnungsadministrator“ zu verwenden. 

#### <a name="task-2-configure-a-role-to-require-approval-to-activate-and-add-an-eligible-member"></a>Aufgabe 2: Konfigurieren einer Rolle, um eine Genehmigung zum Aktivieren und Hinzufügen eines berechtigten Mitglieds anzufordern

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Azure AD Privileged Identity Management**, und klicken Sie auf **Azure AD-Rollen**.

1. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Schnellstart** im Abschnitt **Verwalten** auf **Rollen**.

1. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Rollen** auf den Rolleneintrag **Globaler Leser**. 

1. Klicken Sie auf dem Blatt **Globaler Leser \| Zuweisungen** auf das **Einstellungssymbol** in der Symbolleiste des Blatts, und überprüfen Sie die Konfigurationseinstellungen für die Rolle, einschließlich der Azure MFA-Anforderungen.

1. Klicken Sie auf **Bearbeiten**.

1. Aktivieren Sie auf der Registerkarte **Aktivierung** das Kontrollkästchen **Genehmigung zum Aktivieren anfordern**.

1. Klicken Sie auf den Link **Genehmigende Person(en) auswählen**, klicken Sie auf dem Blatt **Mitglied auswählen** auf **aaduser3**, und klicken Sie dann auf **Auswählen**.

1. Klicken Sie auf **Weiter: Zuweisung**.

1. Deaktivieren Sie das Kontrollkästchen **Permanente berechtigte Zuweisung zulassen**, und übernehmen Sie alle anderen Einstellungen mit ihren Standardwerten.

1. Klicken Sie auf **Weiter: Benachrichtigung**.

1. Überprüfen Sie auf dem Blatt **Rolleneinstellung bearbeiten - Globaler Leser** die Einstellungen, und klicken Sie auf **Aktualisieren**.

    >**Hinweis**: Jeder Benutzer, der versucht, die Rolle „Globaler Leser“ zu verwenden, benötigt jetzt die Genehmigung von aaduser3. 

1. Klicken Sie auf dem Blatt **Globaler Leser \| Zuweisungen** auf **+ Zuweisungen hinzufügen**.

1. Klicken Sie auf dem Blatt **Zuweisungen hinzufügen** auf **Kein Mitglied ausgewählt**, klicken Sie auf dem Blatt **Mitglied auswählen** auf **aaduser2**, und klicken Sie dann auf **Auswählen**.

1. Klicken Sie auf **Weiter**. 

1. Stellen Sie sicher, dass der **Zuweisungstyp** **Berechtigt** ist, und überprüfen Sie die Einstellungen für die berechtigte Dauer.

1. Klicken Sie auf **Zuweisen**.

    >**Hinweis**: Der Benutzer aaduser2 ist für die Rolle „Globaler Leser“ berechtigt. 
 
#### <a name="task-3-give-a-user-permanent-assignment-to-a-role"></a>Aufgabe 3: Festlegen einer permanente Zuweisung zu einer Rolle für einen Benutzer.

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Azure AD Privileged Identity Management**, und klicken Sie auf **Azure AD-Rollen**.

1. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Schnellstart** im Abschnitt **Verwalten** auf **Rollen**.

1. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Rollen** auf **+ Zuweisungen hinzufügen**.

1. Wählen Sie auf dem Blatt **Zuweisungen hinzufügen** in der Dropdownleiste **Rolle auswählen** die Option **Sicherheitsadministrator** aus.

1. Klicken Sie auf dem Blatt **Zuweisungen hinzufügen** auf **Kein Mitglied ausgewählt**, klicken Sie auf dem Blatt **Mitglied auswählen** auf **aaduser2**, und klicken Sie dann auf **Auswählen**.

1. Klicken Sie auf **Weiter**. 

1. Überprüfen Sie die Einstellungen **für den Zuweisungstyp**, und klicken Sie auf **Zuweisen.**

    >**Hinweis**: Der Benutzer aaduser2 ist jetzt dauerhaft für die Rolle „Sicherheitsadministrator“ berechtigt.
    
### <a name="exercise-2---activate-pim-roles-with-and-without-approval"></a>Übung 2: Aktivieren von PIM-Rollen mit und ohne Genehmigung

#### <a name="estimated-timing-15-minutes"></a>Geschätzte Zeit: 15 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Aktivieren einer Rolle, die keine Genehmigung erfordert. 
- Aufgabe 2: Aktivieren einer Rolle, die Genehmigung erfordert. 

#### <a name="task-1-activate-a-role-that-does-not-require-approval"></a>Aufgabe 1: Aktivieren einer Rolle, die keine Genehmigung erfordert.

In dieser Aufgabe aktivieren Sie eine Rolle, die keine Genehmigung erfordert.

1. Öffnen Sie ein Browserfenster im InPrivate-Modus.

1. Navigieren Sie im InPrivate-Browserfenster zum Azure-Portal, und melden Sie sich mit dem Benutzerkonto **aaduser2** an.

    >**Hinweis**: Für die Anmeldung müssen Sie einen vollqualifizierten Namen des Benutzerkontos **aaduser2** angeben, einschließlich des DNS-Domänennamens des Azure AD-Mandanten, den Sie sich zuvor in diesem Lab notiert haben. Dieser Benutzername weist das Format aaduser2@`<your_tenant_name>`.onmicrosoft.com auf, wobei `<your_tenant_name>` der Platzhalter ist, der Ihren eindeutigen Azure AD-Mandantennamen darstellt. 

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Azure AD Privileged Identity Management** ein, und drücken Sie die **EINGABETASTE**.

1. Klicken Sie auf dem Blatt **Azure AD Privileged Identity Management** im Abschnitt **Aufgaben** auf **Meine Rollen**.

1. Es sollten drei **Berechtigte Rollen** für **aaduser2** angezeigt werden: **Globaler Leser**, **Sicherheitsadministrator** und **Abrechnungsadministrator**. 

1. Klicken Sie in der Zeile mit dem Rolleneintrag **Abrechnungsadministrator** auf **Aktivieren**.

1. Klicken Sie bei Bedarf auf die Warnung **Zusätzliche Überprüfung erforderlich. Klicken Sie, um fortzufahren**, und befolgen Sie die Anweisungen, um Ihre Identität zu überprüfen.

1. Geben Sie auf dem Blatt **Aktivieren - Abrechnungsadministrator** im Textfeld **Grund** einen Text mit einer Begründung für die Aktivierung ein, und klicken Sie dann auf **Aktivieren**.

    >**Hinweis**: Wenn Sie eine Rolle in PIM aktivieren, kann es bis zu 10 Minuten dauern, bis die Aktivierung wirksam wird. 
    
    >**Hinweis**: Sobald Ihre Rollenzuweisung aktiv ist, wird Ihr Browser aktualisiert (wenn etwas nicht funktioniert, melden Sie sich einfach ab, und melden Sie sich mit dem Benutzerkonto **aaduser2** erneut am Azure-Portal an).

1. Navigieren zurück zum Blatt **Azure AD Privileged Identity Management**, und klicken Sie dann im Abschnitt **Aufgaben** auf **Meine Rollen**.

1. Wechseln Sie auf dem Blatt **Meine Rollen \| Azure AD-Rollen** zur Registerkarte **Aktive Zuweisungen**. Beachten Sie, dass die Rolle **Abrechnungsadministrator** **aktiviert** ist.

    >**Hinweis**: Wenn eine Rolle aktiviert wurde, wird sie nach Erreichen des Zeitlimits unter **Endzeit** (berechtigte Dauer) automatisch deaktiviert.

    >**Hinweis**: Wenn Sie Ihre Administratoraufgaben frühzeitig abschließen, können Sie eine Rolle manuell deaktivieren.

1.  Klicken Sie in der Liste **Aktive Zuweisungen** in der Zeile, die die Rolle „Abrechnungsadministrator“ darstellt, auf den Link **Deaktivieren**.

1.  Klicken Sie auf dem Blatt **Deaktivieren - Abrechnungsadministrator** erneut auf **Deaktivieren**, um den Vorgang zu bestätigen.


#### <a name="task-2-activate-a-role-that-requires-approval"></a>Aufgabe 2: Aktivieren einer Rolle, die Genehmigung erfordert. 

In dieser Aufgabe aktivieren Sie eine Rolle, die Genehmigung erfordert.

1. Navigieren Sie im InPrivate-Browserfenster im Azure-Portal, während Sie als Benutzer **aaduser2** angemeldet sind, zurück zum Blatt **Privileged Identity Management \| Schnellstart**. 

1. Klicken Sie auf dem Blatt **Privileged Identity Management \| Schnellstart** im Abschnitt **Verwalten** auf **Meine Rollen**.

1. Klicken Sie auf dem Blatt **Meine Rollen \| Azure AD Rollen** in der Liste **Berechtigte Zuweisungen** in der Zeile mit der Rolle **Globaler Leser** auf **Aktivieren**. 

1. Geben Sie auf dem Blatt **Aktivieren - Globaler Leser** im Textfeld **Grund** einen Text mit einer Begründung für die Aktivierung ein, und klicken Sie dann auf **Aktivieren**.

1. Klicken Sie auf der Symbolleiste des Azure-Portals auf das **Benachrichtigungssymbol**, und zeigen Sie die Benachrichtigung an, dass die Genehmigung Ihrer Anforderung aussteht.

    >**Hinweis**: Als Administrator für die privilegierte Rolle können Sie Anforderungen jederzeit überprüfen und abbrechen. 

1. Suchen Sie auf dem Blatt **Meine Rollen \| Azure AD-Rollen** die Rolle **Sicherheitsadministrator**, und klicken Sie auf **Aktivieren**. 

1. Klicken Sie auf die Warnung **Zusätzliche Überprüfung erforderlich. Klicken Sie, um fortzufahren**. 

1. Befolgen Sie die Anweisungen, um Ihre Identität zu überprüfen.

    >**Hinweis**: Sie müssen sich nur ein Mal pro Sitzung authentifizieren. 

1. Sobald Sie zurück in der Benutzeroberfläche des Azure-Portals sind, geben Sie auf dem Blatt **Aktivieren - Sicherheitsadministrator** im Textfeld **Grund** einen Text mit einer Begründung für die Aktivierung ein, und klicken Sie dann auf **Aktivieren**.

    >**Hinweis**: Der automatische Genehmigungsprozess sollte abgeschlossen sein.

1. Klicken Sie auf dem Blatt **Meine Rollen \| Azure AD-Rollen** auf die Registerkarte **Aktive Zuweisungen**, und beachten Sie, dass die Liste **Aktive Zuweisungen** die Rolle **Sicherheitsadministrator**, aber nicht die Rolle **Globaler Leser** enthält.

    >**Hinweis**: Sie genehmigen jetzt die Rolle „Globaler Leser“.

1. Melden Sie sich vom Azure-Portal als **aaduser2** ab.

1. Melden Sie sich am Azure-Portal als **aaduser3** an.

    >**Hinweis**: Wenn bei der Authentifizierung mit einem der Benutzerkonten Probleme auftreten, können Sie sich beim Azure AD-Mandanten anmelden, indem Sie Ihr Benutzerkonto verwenden, um seine Kennwörter zurückzusetzen oder seine Anmeldeoptionen neu zu konfigurieren.

1. Navigieren Sie im Azure-Portal zu **Azure AD Privileged Identity Management**.

1. Klicken Sie auf dem Blatt **Privileged Identity Management \| Schnellstart** im Abschnitt **Aufgaben** auf **Anforderungen genehmigen**.

1. Aktivieren Sie auf dem Blatt **Anforderungen genehmigen \| Azure AD-Rollen** im Abschnitt **Anforderungen für Rollenaktivierungen** das Kontrollkästchen für den Eintrag, der die Rollenaktivierungsanforderung für die Rolle **Globaler Leser** von **aaduser2** darstellt.

1. Klicken Sie auf **Approve**. Geben Sie auf dem Blatt **Anforderung genehmigen** im Textfeld **Begründung** einen Grund für die Aktivierung ein, beachten Sie die Start- und Endzeiten, und klicken Sie dann auf **Bestätigen**. 

    >**Hinweis**: Sie haben auch die Möglichkeit, Anforderungen zu verweigern.

1. Melden Sie sich vom Azure-Portal als **aaduser3** ab.

1. Melden Sie sich am Azure-Portal als **aaduser2** an.

1. Navigieren Sie im Azure-Portal zu **Azure AD Privileged Identity Management**.

1. Klicken Sie auf dem Blatt **Privileged Identity Management \| Schnellstart** im Abschnitt **Aufgaben** auf **Meine Rollen**.

1. Klicken Sie auf dem Blatt **Meine Rollen \| Azure AD-Rollen** auf die Registerkarte **Aktive Zuweisungen**, und überprüfen Sie, ob die Rolle „Globaler Leser“ jetzt aktiv ist.

    >**Hinweis**: Möglicherweise müssen Sie die Seite aktualisieren, um die aktualisierte Liste der aktiven Zuweisungen anzuzeigen.

1. Melden Sie sich ab, und schließen Sie das InPrivate-Browserfenster.

> Ergebnis: Sie haben das Aktivieren von PIM-Rollen mit und ohne Genehmigung geübt. 

### <a name="exercise-3---create-an-access-review-and-review-pim-auditing-features"></a>Übung 3: Erstellen einer Zugriffsüberprüfung und Überprüfen der PIM-Überwachungsfeatures

#### <a name="estimated-timing-10-minutes"></a>Geschätzte Zeit: 10 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Konfigurieren von Sicherheitswarnungen für Azure AD-Verzeichnisrollen in PIM
- Aufgabe 2: Überprüfen von PIM-Warnungen, Zusammenfassungsinformationen und detaillierten Überwachungsinformationen

#### <a name="task-1-configure-security-alerts-for-azure-ad-directory-roles-in-pim"></a>Aufgabe 1: Konfigurieren von Sicherheitswarnungen für Azure AD-Verzeichnisrollen in PIM

In dieser Aufgabe reduzieren das Risiko durch „veraltete“ Rollenzuweisungen. Hierzu erstellen Sie eine PIM-Zugriffsüberprüfung, um sicherzustellen, dass zugewiesene Rollen noch gültig sind. Insbesondere überprüfen Sie die Rolle „Globaler Leser“. 

1. Melden Sie sich mit Ihrem Konto am Azure-Portal ( **`https://portal.azure.com/`** ) an.

    >**Hinweis**: Stellen Sie sicher, dass Sie beim Azure AD-Mandanten **AdatumLab500-04** angemeldet sind. Sie können den Filter **Verzeichnis und Abonnement** verwenden, um zwischen Azure AD-Mandanten zu wechseln. Stellen Sie sicher, dass Sie als Benutzer mit der Rolle „Globaler Administrator“ angemeldet sind.
    
    >**Hinweis**: Wenn der Eintrag AdatumLab500-04 nicht angezeigt wird, klicken Sie auf den Link „Verzeichnis wechseln“, wählen Sie die Zeile AdatumLab500-04 aus, und klicken Sie auf die Schaltfläche „Wechseln“.

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Azure AD Privileged Identity Management** ein, und drücken Sie die **EINGABETASTE**.

1. Navigieren Sie zum Blatt **Azure AD Privileged Identity Management**. 

1. Klicken Sie auf dem Blatt **Privileged Identity Management \| Schnellstart** im Abschnitt **Aufgaben** auf **Azure AD-Rollen**.

1. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Schnellstart** im Abschnitt **Verwalten** auf **Zugriffsüberprüfungen**.

1. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Zugriffsüberprüfungen** auf **Neu**:

1. Geben Sie auf dem Blatt **Zugriffsüberprüfung erstellen** die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen): 

   |Einstellung|Wert|
   |---|---|
   |Name der Überprüfung|**Überprüfung „Globaler Leser“**|
   |Startdatum|Datum von heute| 
   |Häufigkeit|**Einmal**|
   |Enddatum|Ende des aktuellen Monats|
   |Rollenmitgliedschaft überprüfen|**Globaler Leser**|
   |Reviewer|**Ausgewählte Benutzer**|
   |Prüfer auswählen|Ihr Konto|

1. Klicken Sie auf dem Blatt **Zugriffsüberprüfung erstellen** auf **Start**:
 
    >**Hinweis**: Es dauert ungefähr eine Minute, bis die Überprüfung bereitgestellt wurde und auf dem Blatt **AdatumLab500-04 \| Zugriffsüberprüfungen** angezeigt wird. Möglicherweise müssen Sie die Webseite aktualisieren. Der Überprüfungsstatus ist **Aktiv**. 

1. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Zugriffsüberprüfungen** unter der Überschrift **Überprüfung „Globaler Leser“** auf den Eintrag **Globaler Leser**. 

1. Überprüfen Sie auf dem Blatt **Überprüfung „Globaler Leser“** die Seite **Übersicht**, und beachten Sie, dass in den **Statusdiagrammen** ein einzelner Benutzer in der Kategorie **Nicht überprüft** angezeigt wird. 

1. Klicken Sie auf dem Blatt **Überprüfung „Globaler Leser“** im Abschnitt **Verwalten** auf **Ergebnisse**. Beachten Sie, dass aaduser2 als zugriffsberechtigt für diese Rolle aufgeführt wird.

1. Klicken Sie in der Zeile **aaduser2** auf **Ansicht**, um ein ausführliches Überwachungsprotokoll mit Einträgen anzuzeigen, die PIM-Aktivitäten darstellen, die diesen Benutzer betreffen.

1. Navigieren Sie zurück zum Blatt **AdatumLab500-04 \| Zugriffsüberprüfungen**.

1. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Zugriffsüberprüfungen** im Abschnitt **Aufgaben** auf **Zugriff überprüfen**, und klicken Sie dann auf den Eintrag **Überprüfung „Globaler Leser“** . 

1. Klicken Sie auf dem Blatt **Überprüfung „Globaler Leser“** auf den Eintrag **aaduser2**. 

1. Geben Sie im Textfeld **Grund** eine Begründung für die Genehmigung ein, und klicken Sie dann entweder auf **Genehmigen**, um die aktuelle Rollenmitgliedschaft beizubehalten, oder auf **Verweigern**, um sie zu widerrufen. 

1. Navigieren zurück zum Blatt **Azure AD Privileged Identity Management**, und klicken Sie dann im Abschnitt **Verwalten** auf **Azure AD-Rollen**.

1. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Schnellstart** im Abschnitt **Verwalten** auf **Zugriffsüberprüfungen**.

1. Wählen Sie den Eintrag aus, der die Überprüfung **Globaler Leser** darstellt. Beachten Sie, dass das **Statusdiagramm** aktualisiert wurde, um Ihre Überprüfung anzuzeigen. 

#### <a name="task-2-review-pim-alerts-summary-information-and-detailed-audit-information"></a>Aufgabe 2: Überprüfen von PIM-Warnungen, Zusammenfassungsinformationen und detaillierten Überwachungsinformationen. 

In dieser Aufgabe überprüfen Sie PIM-Warnungen, Zusammenfassungsinformationen und detaillierte Überwachungsinformationen. 

1. Navigieren zurück zum Blatt **Azure AD Privileged Identity Management**, und klicken Sie dann im Abschnitt **Verwalten** auf **Azure AD-Rollen**.

1. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Schnellstart** im Abschnitt **Verwalten** auf **Warnungen** und dann auf **Einstellung**.

1. Überprüfen Sie auf dem Blatt **Warnungseinstellungen** die vorkonfigurierten Warnungen und Risikostufen. Klicken Sie auf einen beliebigen Eintrag, um ausführlichere Informationen zu erhalten. 

1. Kehren Sie zum Blatt **AdatumLab500-04 \| Schnellstart** zurück, und klicken Sie auf **Übersicht**. 

1. Überprüfen Sie auf dem Blatt **AdatumLab500-04 \| Übersicht** die Zusammenfassungsinformationen zu Rollenaktivierungen, PIM-Aktivitäten, Warnungen und Rollenzuweisungen.

1. Klicken Sie auf dem Blatt **AdatumLab500-04 \| Übersicht** im Abschnitt **Aktivität** auf **Ressourcenüberwachung**. 

    >**Hinweis**: Der Überwachungsverlauf ist für alle Zuweisungen und Aktivierungen privilegierter Rollen innerhalb der letzten 30 Tage verfügbar.

1. Beachten Sie, dass Sie ausführliche Informationen abrufen können, z. B. **Zeit**, **Anforderer**, **Aktion**, **Ressourcenname**, **Bereich**, **Primäres Ziel** und **Antragsteller**. 

> Ergebnis: Sie haben eine Zugriffsüberprüfung konfiguriert und Überwachungsinformationen überprüft. 

**Bereinigen von Ressourcen**

> Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen nicht verwendeter Ressourcen wird sichergestellt, dass keine unerwarteten Kosten anfallen. 

1. Legen Sie im Azure-Portal den Filter **Verzeichnis und Abonnement** auf den Azure AD-Mandanten fest, der dem Azure-Abonnement zugeordnet ist, in dem Sie die Azure-VM **az500-04-vm1** bereitgestellt haben.

>**Hinweis**: Wenn der Haupteintrag Ihres Azure AD-Mandanten nicht angezeigt wird, klicken Sie auf den Link „Verzeichnis wechseln“, wählen Sie Ihre Hauptmandantenzeile aus, und klicken Sie auf die Schaltfläche „Wechseln“.

1. Öffnen Sie im Azure-Portal Cloud Shell, indem Sie oben rechts im Azure-Portal auf das erste Symbol klicken. Wenn Sie dazu aufgefordert werden, klicken Sie auf **PowerShell** und dann auf **Speicher erstellen**.

1. Stellen Sie sicher, dass oben links im Cloud Shell-Bereich im Dropdownmenü der Eintrag **PowerShell** ausgewählt ist.

1. Führen Sie im Cloud Shell-Bereich in der PowerShell-Sitzung den folgenden Code aus, um die Ressourcengruppe zu entfernen, die Sie im vorherigen Lab erstellt haben:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB04" -Force -AsJob
    ```

1.  Schließen Sie den **Cloud Shell**-Bereich. 

1. Verwenden Sie im Azure-Portal den Filter **Verzeichnis und Abonnement**, um zum Azure Active Directory-Mandanten **AdatumLab500-04** zu wechseln.

1. Navigieren Sie zum Blatt **Azure Active Directory Premium P2 - Lizenzierte Benutzer**, wählen Sie die Benutzerkonten aus, denen Sie Lizenzen zugewiesen haben, klicken Sie auf **Lizenz entfernen**, und wählen Sie **Ja** aus, wenn Sie zur Bestätigung aufgefordert werden.

1. Navigieren Sie im Azure-Portal zum Blatt **Benutzer - Alle Benutzer**, und klicken Sie auf den Eintrag, der das Benutzerkonto **aaduser1** darstellt. Klicken Sie auf dem Blatt **aaduser1 - Profil** auf **Löschen**, und wählen Sie **Ja** aus, wenn Sie zur Bestätigung aufgefordert werden.

1. Wiederholen Sie diese Schrittfolge, um die übrigen Benutzerkonten zu löschen, die Sie erstellt haben.

1. Navigieren Sie zum Blatt **AdatumLab500-04 - Übersicht** des Azure AD-Mandanten, klicken Sie auf **Mandant verwalten** und dann auf dem nächsten Bildschirm auf **Mandant löschen**. Klicken Sie auf dem Blatt **Verzeichnis "AdatumLab500-04" löschen** auf den Link **Berechtigung zum Löschen von Azure-Ressourcen anfordern**, legen Sie auf dem Blatt **Eigenschaften** von Azure Active Directory **Zugriffsverwaltung für Azure-Ressourcen** auf **Ja** fest, und klicken Sie dann auf **Speichern**.

1. Melden Sie sich beim Azure-Portal ab, und melden Sie sich erneut an. 

1. Navigieren Sie zurück zum Blatt **Verzeichnis "AdatumLab500-04"** löschen, und klicken Sie auf **Löschen**.

> Weitere Informationen zu dieser Aufgabe finden Sie unter [https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-delete-howto](https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-delete-howto).
