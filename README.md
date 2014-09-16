MyFirstRest
===========
# 1.	Opis problema
Cilj ovog projekta predstavlja izrada aplikacije koja će se koristiti za ekstakciju, struktuiranje i integraciju podataka o kursevima. Osnovna funkcijonalnost koju mora da obezbedi aplikacija odnosi se na ekstrakciju podataka sa sajta gde se nalaze kursevi. Podaci su  raspoloživi sa https://www.coursera.org/ cousera i https://www.udacity.com/ sajta, koji podatke čini dostupnim u JSON formatu. Korišćenjem njihovih API-jeva potrebno je preuzeti podatke, proveriti njihovu ispravnost, zatim konvertovati ih u RDF i sačuvati ih u RDF repozitorijum. Svaki podatak ekstrakovan iz odgovarajućeg API-ja se mapira kao odgovarajući element LRMI  vokabulara. LRMI specifikacija je kolekcija propertija za opisivanje obrazovnog sadržaja. Ona predstavlja proširenje Schema.org  vokaulara. Pretežno koristi propertije klase schema:CreativeWork koji su definisani u  Schema.org vokabularu kao i propertije koji su definisani u LRMI vokabularu, a koji su specifični za resure za učenje. Takođe, LRMI uvodi i neke nove klase koje su specifične za resurse za učenje, kao što su
klase AlignmentObject
i EducationalAudience.
Radi kreiranja aplikacije potrebno je bilo ispuniti sledeće zahteve:
*kreiranje web parsera koji prikuplja podatke o kursevima koristeci postojeće API-je
*kreiranje RDF baze i čuvanje ekstrahovanih podataka u skladu sa LRMI vokabularom
*omogućiti pristup podacima u bazi pomoću odgovarajućih REST servisa

Sačuvane RDF podatke potrebno je učiniti dostupnim korisnicima. Implementacija REST servisa koji će biti dostupan na Web-u predstavlja rešenje koje korisnicima pruža API sa dostupnim operacijama za pristup tim podacima. Pristupajući različitim URL-ovima koje nudi REST API, korisnik šalje upite i dobija željene podatke u JSON formatu. 



