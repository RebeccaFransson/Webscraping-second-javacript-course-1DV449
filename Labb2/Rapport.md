_Rebecca Fransson_
_rf222cz_


# Säkerhetsproblem
### Manipulera kakorna / Hijacking / Kakorna förstörs ej vid utloggning
_Teori_

I denna applikationen kan användaren komma åt sin kaka och ändra den. Detta beror på att kakan är sparad eller sänd på ett osäkert sätt.
På grund av att kakorna ej förstörs vid utloggning kan en användare komma åt en kaka och vara inloggad fast användaren som kakan tillhör, har loggat ut. Detta gör att om man har någons kaka så kan man ta del av informationen på sidan utan att ha loggat in en enda gång genom att gå till /message i urlen.[1]


_Konsekvenser_

Användaren kan komma åt konton genom att ändra sin kaka och på så sätt låtsas vara någon den inte är och ta reda på personlig information om den användaren den har "hijacked".
Den icke befogade användaren har längre tid på sig att hijacka den attackerade personen då kakan inte förstörs vid utloggning.
När den icke befogade användaren fått tag på en kaka kan denna också komma åt meddelanden genom att manipulera urlen.


_Åtgärder_

Man borde spara all data i en server-cookie. Inte en klient-cookie för då kan icke befogade lätt manipulera den.
Man borde också använda sig utav HTTPS. Om man inte vill göra SSL på hela sin sida kan man välja att göra det på de känsliga/svaga sidorna, tex login. Och när användaren sedan loggat in borde en “secure cookie”(inte en “session cookie” som det är i applikationen nu) sättas.
Och se till att kakans identifierare sänds med ett krypterat protokoll!
Skriv kod så att kakan förstörs vid utlogging.
[1] [2]


### Sql-injections
_Teori_

I denna applikationen kan en användare skriva in en sql-fråga så att den passar sql-frågan som ställs vid inloggning. På så sätt kan den manipulera frågan och få ett helt annorlunda svar. Tex vid inloggning skriver  användaren frågan “om 1 är lika med 1 låt mig logga in”.[3]


_Konsekvenser_

På detta sätt kan användaren logga in utan att veta lösenordet till kontot.
Den kan också se, ändra eller förstöra innehållet i databasen.


_Åtgärder_

Programmeren kan använda sig utav lagrade procedurer och inte en query.
Eller kan programmeraren kommunicera med ett API som ger programmeraren information istället för att direkt kommunicera med databasen.[4]


### XSS
_Teori_

Användaren kan skriva in taggar och kod i denna applikationen. Detta sker när text eller ett script skickas till applikationen utan att ha blivit validerat ordentligt. Med andra ord kan en användare skicka in skadlig kod i applikationen.[5]


_Konsekvenser_

Användaren kan då manipulera koden och få applikationen att göra något helt annat. Tex skriva ut en länk som tar användaren till en annan sida och samtidigt hijackar dess sessions/cookies och samlar användarens information. Som följd utav detta kan den icke befogade användaren ta kontroll över ditt konto eller i värsta fall hela applikationen.


_Åtgärder_

“Validera indata, filtrera utdata” - som Johan sa i sin föreläsning.[7]
När det gäller att bara skriva in taggar så kan man som programmerare göra om taggarna till text så att webbläsaren inte tolkar det som kod.
Man kan också göra en whitelist - då man bara tilllåter ett visst antal tecken från användaren.
[6]

### Osäkra objekt referenser / Icke-hashade lösenord
_Teori_

I applikationen kan användaren komma åt databasen genom att skriva in /static/message.db i urlen. En icke befogad användare kan gissa sig till denna sökvägen(jag såg det i min GITBash när jag startade applikationen). Den behöver inte vara inloggad för att kunna se detta.
En användare kan också hämta mer publik data på detta viset.
I konsolen skrivs också /message/data ut. Om man går in på den länken kan man se alla meddelanden i chatten utan att vara inloggad.
Dessa fallen/riskerna kallas för "Insecure Direct Object References".[8], [9]
I databasen kunde jag se att lösenorden inte var krypterade eller hashade. Detta kunde jag också se när jag läste koden.[10]


_Konsekvenser_

Genom att en icke befogad användare kan komma åt /message/data så pass lätt, behöver den inte logga in för att ta del av meddelanden som andra användare skriver.
Med den risken att hitta databasen och icke-hashade lösenord, är det lätt för en icke befogad användare att få tag på alla andras konton och kanske till och med hela applikationen!


_Åtgärder_

Som programmerare kan man skriva kod som gör att en icke befogad användare inte kommer åt dessa sökvägar om den inte har tillgång till det.[8]
Det är också viktigt att hasha lösenord innan man lägger in dem i databasen. Inte kryptera, för krypterar man ett ord kan man alltid kryptera tillbaka det. Så när användaren sedan loggar in så hashas det inskriva lösenordet och jämnförs det med det sparade hashade lösenordet.


# Prestandaproblem
### Inline
Man kan hitta css och javascript inline i html-koden, det är aldrig bra och mycket lätt att byta ut och lägga i egna filer. Att skriva kod och css inline gör bara lata programmerare.

### Script-länkar
Script-länkar länkas in i headern tillsammans med css-länkarna. Dock borde dessa script-länkarna länkas in i slutet av html-filen. Detta för att de ej skall störa renderingen utav resten av sidan. Då all rendering av sidan stannar upp när applikationen laddar igenom ett script.[11]

### Nytt meddelande
När ett nytt meddelande skrivs, läggs det till i en json-fil. Sedan raderas alla meddelanden i messageArea och alla meddelanden i jsonfilen läggs till. Istället kan man koda så att det nya meddelandet läggs till ovanpå dem andra, så att applikationen slipper skriva ut alla meddelanden igen.

### Kaka fastän användaren ej inloggad
En kaka skapas när användaren försöker logga in, fast än den inte lyckades.

### Tabort-knapp
Tabort-knapp för admin fungerar ej, dock finns koden för funktionen men den används inte.

### HTTP
I applikationen sker det väldigt många HTTP-anrop, vilket är negativt för applikationen.[11]
Det är också flera anrop applikationen inte kommer åt, tex Materialize.js. Även om det är små saker hade detta påverkat applikationen om den hade varit större.

# Egna övergripande reflektioner
### Applikationen
Förutom det uppenbara som jag skrivit i ovanstående rubriker tycker jag att det fattas rätt och fel meddelande i applikationen. "Logout"-knappen visar sig när man både är inloggad och utloggad. Knappen borde bara visas när man är inloggad. Annars blir det förvirrande.
Det är alldeles för många app_moduler som finns i applikationen som inte används.

### Laborationen
Laborationen har varit rolig och lärorik. Det var skönt men samtidigt svårt att byta lärosätt(att inte programmera). Jag tror dock att jag lärt mig mycket mer genom denna laborationen än om vi skulle provat programmera allt detta. Det hade dessutom tagit mycket längre tid. Vi har dock aldrig gått igenom säkerhet så här detaljerat så det var mycket att ta till sig. Med denna laborationen kunde jag också koppla teorin till praktiken och det är alltid ett bra sätt att lära sig på tycker jag!
Informationen jag tagit till sig under denna laborationen kommer jag att ha stor glädje av resten utav min karriär som webbprogrammerare. Säkerhetwn är livsviktig för en hållbar applikation!


# Referenser
[1] The Open Web Application Security Project, "OWASP Top 10 The ten most critical web application security risks”, s. 8.

[2] The Open Web Application Security Project, "OWASP Periodic Table of Vulnerabilities", Tillgänglig: [Cookie Theft/Session Hijacking](https://www.owasp.org/index.php/OWASP_Periodic_Table_of_Vulnerabilities#Periodic_Table_of_Vulnerabilities).

[3] The Open Web Application Security Project, "OWASP Top 10 The ten most critical web application security risks”, s. 7.

[4] OWASP, "OWASP Periodic Table of Vulnerabilities", Tillgänglig: [SQL Injections](https://www.owasp.org/index.php/OWASP_Periodic_Table_of_Vulnerabilities_-_SQL_Injection).

[5] The Open Web Application Security Project, "OWASP Top 10 The ten most critical web application security risks”, s. 9.

[6] The Open Web Application Security Project, "OWASP Periodic Table of Vulnerabilities", Tillgänglig: [Cross.Site Scripting](https://www.owasp.org/index.php/OWASP_Periodic_Table_of_Vulnerabilities_-_Cross-Site_Scripting_(XSS)).

[7] Johan Leitet, Tillgänglig: “[Webbteknik II - HT13 - Webbsäkerhet](https://www.youtube.com/watch?v=Gc_pc9TMEIk)”.

[8] The Open Web Application Security Project, "OWASP Top 10 The ten most critical web application security risks”, s. 11.

[9] The Open Web Application Security Project, Tillgänglig: “[Improper Filsystem Permissions](https://www.owasp.org/index.php/OWASP_Periodic_Table_of_Vulnerabilities_-_Improper_Filesystem_Permissions).

[10] The Open Web Application Security Project, "OWASP Top 10 The ten most critical web application security risks”, s. 12.

[11] Akamai, Tillgänglig: [Font end optimazation](https://www.akamai.com/us/en/resources/front-end-optimization-feo.jsp)