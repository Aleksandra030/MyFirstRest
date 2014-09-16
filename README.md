MyFirstRest
===========
# 1.	Opis problema
Cilj ovog projekta predstavlja izrada aplikacije koja će se koristiti za ekstakciju, struktuiranje i integraciju podataka o kursevima. Osnovna funkcijonalnost koju mora da obezbedi aplikacija odnosi se na ekstrakciju podataka sa sajta gde se nalaze kursevi. Podaci su  raspoloživi sa https://www.coursera.org/ cousera i https://www.udacity.com/ sajta, koji podatke čini dostupnim u JSON formatu. Korišćenjem njihovih API-jeva potrebno je preuzeti podatke, proveriti njihovu ispravnost, zatim konvertovati ih u RDF i sačuvati ih u RDF repozitorijum. Svaki podatak ekstrakovan iz odgovarajućeg API-ja se mapira kao odgovarajući element LRMI  vokabulara. LRMI specifikacija je kolekcija propertija za opisivanje obrazovnog sadržaja. Ona predstavlja proširenje Schema.org  vokaulara. Pretežno koristi propertije klase schema:CreativeWork koji su definisani u  Schema.org vokabularu kao i propertije koji su definisani u LRMI vokabularu, a koji su specifični za resure za učenje. Takođe, LRMI uvodi i neke nove klase koje su specifične za resurse za učenje, kao što su
klase AlignmentObject
i EducationalAudience.
Radi kreiranja aplikacije potrebno je bilo ispuniti sledeće zahteve:
* kreiranje web parsera koji prikuplja podatke o kursevima koristeci postojeće API-je
* kreiranje RDF baze i čuvanje ekstrahovanih podataka u skladu sa LRMI vokabularom
* omogućiti pristup podacima u bazi pomoću odgovarajućih REST servisa

Sačuvane RDF podatke potrebno je učiniti dostupnim korisnicima. Implementacija REST servisa koji će biti dostupan na Web-u predstavlja rešenje koje korisnicima pruža API sa dostupnim operacijama za pristup tim podacima. Pristupajući različitim URL-ovima koje nudi REST API, korisnik šalje upite i dobija željene podatke u JSON formatu. 
#2.	Domenski model
Klasa CreativeWork opisuje strukturu kursa, obuhvatajući instruktore, univerzitete i same sesije kursa ukoliko ih ima. Za svaki kurs definisan je jezik, kao i samo trajanje kursa.

Klasa Person predstavlja insturtktora datog kursa i u modelu je definisan samo atribut koji predstavlja ime. 

Organizacija u modelu prestavlja univerzitet odnosno samog organizatora ovog događaja.

Klasa Duration prestavlja vremenski period trajanja samog kursa.
Mapiranje odgovarajucih elemenata  u odgovarajuće elemente RDF vokabulara LRMI Grafički prikaz

Prvi korak je transformisanje podataka iz JSON formata u odgovarajuće RDF triplete. Podaci se potom smeštaju u lokalnu RDF bazu. Aplikacija omogućava pristup sačuvanim podacima korišćenjem RESTful servisa. Radi transformisanja izvornih podataka izvršeno je parsiranje JSON dokumenta.

#3.	Rešenje
Coursera
Coursera je online platforma nekih od najprestižnijih američkih, kanadskih i australijskih univerziteta, koji pružaju potpuno besplatne kurseve svakome ko se registruje. Ovo omogućavama bilo kome, sa bilo koje tačke na planeti da dobija nova znanja i informacije. Kursevi koji se mogu pohađati su iz oblasti fizike, elektronike, hemije, medicine, biologije, sociologije, matematike, informatike, ekonomije itd.

Coursera API 
Kursevi sa Coursera sajta dostupni su preko njihovog API-ja i dati su u JSON formatu i primer jednog takvog kursa možemo videti na slici. API katalog sadrži listu kurseva, instruktora i univerziteta dostupnih na Coursera platformi. API  je dostupan javno bez autentifikacije preko Interneta. Osnovne URL adrese API-ja su:
*	Kursevi:  https://api.coursera.org/api/catalog.v1/courses
*	Univerziteti: https://api.coursera.org/api/catalog.v1/universities
*	Kategorije: https://api.coursera.org/api/catalog.v1/categories
* Instrukori: https://api.coursera.org/api/catalog.v1/instructors
* Sesije: https://api.coursera.org/api/catalog.v1/sessions

Primer zahteva
Slanje GET metode do osnovne adrese servisa će vratiti celu kolekciju datih resursa (kurseva, instruktora). Da bi se dobili podaci pojedinacnih elemenata, potrebno je dodati id samog resursa u putanju. Sledeća dva primera poziva su ekvivalentna:
``` 
* 1	curl https://api.coursera.org/api/catalog.v1/courses/2
* 2 curl https://api.coursera.org/api/catalog.v1/courses?id=2
``` 
Po defaultu, samo minimalni skup polja su uključeni u objektima odgovora. Da bi odgovor sadržao više polja, uključuju se njihova imena u upitu polja parametar. Na primer, sledeći zahtev obuhvata jezik i kratak opis u odgovoru.
``` 
curl https://api.coursera.org/api/catalog.v1/courses?fields=language,shortDescription
``` 
U samom linku kursa mogu biti uključeni više objekata. Na primer, sledeći zahtev će vratiti kurseve, kao i osnovne informacije o sesijama: 
``` 
curl https://api.coursera.org/api/catalog.v1/courses?includes=sessions
``` 

Primer JSON formata jednog od kurseva
``` json
{
"elements":
	[
		{
			"id":2,
			"shortName":"ml",
			"name":"Machine Learning",
			"language":"en",
			"previewLink":"https://class.coursera.org/ml-005/lecture/preview",
			"shortDescription":"Learn about the most effective machine learning techniques, and gain practice implementing them and getting them to work for yourself.","targetAudience":1,"instructor":"Andrew Ng, Associate Professor",
			"links":{
				"universities":[1],
				"instructors":[1244],
				"sessions":[
					16,152,970311,971489,972224,972303,972304
				]
			}
		}
	],
"linked":{
	"universities":[
		{
			"id":1,
			"shortName":"stanford"
		}
	],
	"instructors":[
		{
			"id":1244,
			"firstName":"Andrew",
			"lastName":"Ng"
		}
	],
	"sessions":[
		{	
			"id":152,
			"homeLink":https://class.coursera.org/ml-2012-002/
		},
``` 


