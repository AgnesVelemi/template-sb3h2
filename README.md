# Spring Boot 3.4 + H2 + JPA Starter Template (Java 21)
### 💡 Kifejezetten IntelliJ IDEA Community Edition-höz és Állásinterjús Házi Feladatokhoz!

Ez a projekt egy minimális, sziklaszilárd, működőképes Spring Boot 3.4 sablon. Mintaként szolgál új backend mikroszervizek és állásinterjún kapott házi feladatok gyors, hibamentes elindításához.

---

## 🎯 Gyorstalpaló: Állásinterjún kapott üres git repó felhúzása a sablonból

Gyakori helyzet: a cég ad egy üres (vagy csak egy `README.md`-t és `.gitignore`-t tartalmazó) git repository-t (pl. `trial-recruiting-backend-work-agnessmiths`), amit leklónozol a gépedre, és neked kell rá felhúzni egy működő Spring Boot projektet.

Kövesd az alábbi **apró lépéseket**:

### 1. LÉPÉS: Fájlok átmásolása a sablonból az új mappába

Nyisd meg a Windows Intézőt (vagy parancssort):

**Amit MÁSOLJ ÁT a sablonból (`recruit-be` vagy `sb3start-h2-be`) az új interjús mappába (`trial-recruiting-backend-work-agnessmiths`):**
* `pom.xml`
* `.mvn/` mappa (benne a rejtett wrapper beállítások)
* `mvnw` és `mvnw.cmd` fájlok
* `src/` teljes mappa (Java forráskód és properties)
* `.gitignore` (ha a kapott repóban nincs, vagy másold be a tartalmát)

**Amit SZIGORÚAN TILOS ÁTMÁSOLNI:**
* ❌ `.git/` mappa *(NE másold át, mert az felülírná a cégtől kapott git repó előzményeit!)*
* ❌ `.idea/` mappa *(NE másold át, mert az a régi projekt belső IntelliJ cache-e)*
* ❌ `target/` mappa *(NE másold át, mert az régi fordítási maradvány)*

---

### 2. LÉPÉS: Megnyitás IntelliJ IDEA Community-ben

1. Indítsd el az IntelliJ IDEA Community-t.
2. Kattints: **File** $\rightarrow$ **Open...**
3. Keresd meg a klónozott interjús mappát (`C:\ws\2027\trial-recruiting-backend-work-agnessmiths`), és kattints az **OK** gombra.
4. Válaszd a **Trust Project** lehetőséget.
5. Várd meg, amíg az alsó kék sávban befejeződik az indexelés.

---

### 3. LÉPÉS: Névadás és beállítások testreszabása a `pom.xml`-ben

Nyisd meg a `pom.xml` fájlt a projekt gyökerében, és írd át az alábbi mezőket:

#### Milyen elnevezési szabályokat kövess?
* **`<groupId>`**: A céges vagy saját névtér fordított domain formátumban, csupa kisbetűvel, pontokkal elválasztva.  
  *Példa:* `com.agnessmiths.recruiting` vagy `com.trial.recruiting`
* **`<artifactId>`**: A projekt/mikroszerviz pontos neve (kisbetűk, kötőjelek). Egyezzen meg a kapott mappa / repo nevével!  
  *Példa:* `trial-recruiting-backend-work-agnessmiths`
* **`<version>`**: Hagyhatod standard `0.0.1-SNAPSHOT`-on.
* **`<name>`**: Ugyanaz, mint az `artifactId`.

#### A `pom.xml` felső része így nézzen ki:
```xml
    <groupId>com.agnessmiths.recruiting</groupId>
    <artifactId>trial-recruiting-backend-work-agnessmiths</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>trial-recruiting-backend-work-agnessmiths</name>
    <description>Recruiting backend interview homework</description>
```

#### Maven újraszinkronizálás:
A mentés után kattints a jobb felső sarokban megjelenő kis kék lebegő Maven ikonra (**Load Maven Changes**), vagy nyomd le a billentyűzeten a <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>O</kbd> kombinációt!

---

### 4. LÉPÉS: Csomagnév (Package) és Főosztály átnevezése (Refactor)

Hogy szép és professzionális legyen a kód, a Java csomagnév kövesse a `groupId`-t:

#### A) Csomag átnevezése:
1. A bal oldali **Project** sávban nyisd ki a mappafát: `src/main/java`.
2. Látni fogsz egy csomagot (pl. `com.recruit.backend` vagy `com.example.starter`).
3. Kattints a csomagra jobb egérgombbal $\rightarrow$ **Refactor** $\rightarrow$ **Rename...** (vagy állj rá és nyomj <kbd>Shift</kbd> + <kbd>F6</kbd>-ot).
4. Válaszd a **Rename package** (vagy *Rename directory structure*) opciót.
5. Írd be az új nevet: `com.agnessmiths.recruiting`.
6. Kattints a **Refactor** gombra. *(Az IntelliJ minden importot automatikusan átír!)*
7. Ismételd meg ugyanezt a `src/test/java` mappa alatt is!

#### B) Főosztály átnevezése:
1. A csomagon belül kattints jobb gombbal a belépési osztályra (pl. `RecruitBeApplication.java`).
2. **Refactor** $\rightarrow$ **Rename...** (<kbd>Shift</kbd> + <kbd>F6</kbd>).
3. Új név: `TrialRecruitingApplication` (vagy `RecruitingBackendApplication`).
4. Ugyanígy a teszt mappában: `RecruitBeApplicationTests.java` $\rightarrow$ `TrialRecruitingApplicationTests`.

---

### 5. LÉPÉS: Konfiguráció beállítása (`application.properties`)

Nyisd meg: `src/main/resources/application.properties`

```properties
# 1. Alkalmazás neve
spring.application.name=trial-recruiting-backend-work-agnessmiths

# 2. Port beállítása (Figyeld a feladat leírást! Ha 3000-et kérnek, akkor 3000, ha nincs megadva, 8080)
server.port=3000

# 3. H2 In-Memory Adatbázis
spring.datasource.url=jdbc:h2:mem:trialdb;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

# 4. H2 Konzol (elérhető: http://localhost:3000/h2-console)
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console
spring.h2.console.settings.web-allow-others=true

# 5. JPA / Hibernate
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
```

---

### 6. LÉPÉS: Lombok bekapcsolása (IntelliJ Community kötelező lépés!)

Ha a feladatban Lombokot használsz (`@Data`, `@Getter`, `@Setter`, `@Builder`, `@RequiredArgsConstructor`), az IntelliJ Community Editionben be kell kapcsolnod az Annotation Processinget:

1. Nyisd meg a beállításokat: **File** $\rightarrow$ **Settings...** (<kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>S</kbd>).
2. Keresd meg: **Build, Execution, Deployment** $\rightarrow$ **Compiler** $\rightarrow$ **Annotation Processors**.
3. Pipáld be: **Enable annotation processing**.
4. Kattints az **Apply** $\rightarrow$ **OK** gombra.

---

### 7. LÉPÉS: Tesztelés és Futtatás (Apró lépésenként a terminálban)

Nyisd meg az IntelliJ beépített terminálját: <kbd>Alt</kbd> + <kbd>F12</kbd>.

#### 1. Ellenőrizd a POM és a függőségek épségét:
```powershell
.\mvnw.cmd clean validate
```
*Elvárt kimenet:* `BUILD SUCCESS` (kb. 1-2 másodperc alatt lefut).

#### 2. Futtasd le a beépített tesztet (ellenőrzi, hogy a Spring ApplicationContext sikeresen betölt-e):
```powershell
.\mvnw.cmd test
```
*Elvárt kimenet:*  
`[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0`  
`[INFO] BUILD SUCCESS`

#### 3. Indítsd el az alkalmazást:
```powershell
.\mvnw.cmd spring-boot:run
```
*Elvárt kimenet:*  
Megjelenik a Spring ASCII banner a terminálban, a logok végén pedig kiírja:  
`Started TrialRecruitingApplication in X.XXX seconds (process running for Y.YYY)`.

#### 4. Ellenőrzés böngészőből:
* Nyisd meg a böngészőt: `http://localhost:3000/h2-console`
* **JDBC URL:** `jdbc:h2:mem:trialdb`
* **User Name:** `sa`
* **Password:** *(hagyd üresen)*
* Kattints a **Connect** gombra: be kell jönnie a H2 webes táblázatkezelőjének!

#### 5. Alkalmazás leállítása:
A terminál ablakban nyomd meg a <kbd>Ctrl</kbd> + <kbd>C</kbd> billentyűkombinációt (majd szükség esetén `Y` + Enter).

---

### 8. LÉPÉS: Első tiszta állapot elmentése a Gitbe (Interjús commit)

Mielőtt elkezdenéd az üzleti logikát megírni, rögzítsd a működő alapvázat a Gitben:

```bash
git status
git add .
git commit -m "feat: initial spring boot 3.4 project skeleton with h2 and jpa"
git push origin main
```
*(Ha a kapott ág `master`, akkor `git push origin master`.)*

Mostantól a repódban van egy működő, stabil, zöld tesztekkel rendelkező alap, amire magabiztosan építheted fel a feladatban kért vezérlőket (Controller), szervizeket (Service) és entitásokat!
