`https://www.facebook.com/groups/223889134481096/permalink/705722699631068/`

IMO to jest bardzo złe coś. Nie dlatego, że nie ma tam dobrych zasad, tylko, że zasady dobre przeplatane są zasadami złymi, a dla zasad dobrych często podawane są złe uzasadnienia. 

No, ale żeby nie być gołosłownym (komentuję tylko tam, gdzie się czepiam): 

"Use the same vocabulary for the same type of variable"

Ponieważ PHP jest językiem (przynajmniej przed 7) beztypowym, a tutaj ktoś pominął typy, to ten przykład kompletnie nie ma sensu. Żeby miał sens, trzeby by dodać (co jest tu zrozumiałe dla tych, którzy już i tak rozumieją, o co w tej zasadzie chodzi - typowy problem źle napisanego poradnika wyjaśniającego tym, którzy i tak już rozumieją), że wszystkie te metody zwracają obiekt np. klasy UserData. No i komuś się pomyliło "same type of variable" z "same type of return value". 

"Use searchable names"

To w ogóle nie jest przykład za "use searchable names". To jest przykład za "use named constants" (albo "don't inline constants"). Przykład na "use searchable names" to byłoby np. $recordsPerPage zamiast $recppg;

"Use explanatory variables"

Zasada dobra, przykład fatalny. Serio, jak ktoś już _rozumie_, co robi ten regexp, to naprawdę ostatnią rzeczą, której potrzebuje, jest nazywanie podwyrażeń. 

"Don't add unneeded context"

Bardzo słuszne w językach statycznie typowanych, dyskutowalne w językach dynamicznie typowanych. Bo czasem lepsze jest $c->getCarName() niż $c->getName(), kiedy nie wiemy, czym jest $c. Jak już chcemy mieć bezapelacyjny przykład, lepiej wziąć jakąś metodę statyczną.

"Function arguments (2 or fewer ideally)"

Znów - słuszna zasada, fatalne uzasadnienie. Więcej niż 2 argumenty często oznacza po prostu, że parametry do funkcji naturalnie tworzą pewnego rodzaju obiekt grupujący, więc warto to opakować (kapsułkowanie!). Natomiast uzasadnienie, że "tak to się będzie ciężej testowało" jest absurdalne - jeśli funkcja logicznie zależy od 3 parametrów, to opakowanie tych parametrów w obiekt nie spowoduje, że będziemy mieli mniej przypadków, co więcej - jeśli rzeczywiście musimy liczyć zależność od 3 parametrów, to żadnym refactoringiem tego nie obejdziemy. 

"Functions should do one thing" 

Dobra zasada, idiotyczny przykład. Rozumiem, że ktoś chciał się zmieścić na jednej stronie, ale serio - wyrzucanie filtrowania listy po parametrze do oddzielnej metody to sprowadzanie dobrej reguły do absurdu. W tej regule chodzi głównie o to, żeby nie pisać wielostronicowych kobył, nie, żeby do każdej linijki kodu pisać oddzielną metodę.

"Functions should only be one level of abstraction"

Okay, ni cholery nie rozumiem, jak ten przykład ma niby ilustrować tę regułę. Wygląda bardziej jak punkt wcześniejszy ("functions should only do one thing"). 

Przydałoby się wyjaśnienie, co to znaczy, że "funkcja jest poziomem abstrakcji". Bo ja szczerze mówiąc trochę nie wiem, co poeta miał na myśli. 

"Don't use flags as function parameters"

To jest chyba najbardziej idiotyczny i szkodliwy postulat na tej całej liście. Flagi bardzo często są odzwierciedleniem tego, że dane, które dostajemy, owe flagi posiadają. Zastępujemy więc ifa w treści metody ifem w momencie wywołania - co jest naruszeniem zasady izolacji. "Fajnie" robi się dopiero w momencie, w którym flag jest kilka. Wyobraźmy sobie, że mamy użytkownika i on ma flagi requiresPasswordToLogin, isSuperAdmin, isEnabled. Już widzę te wszystkie kombinacje metod (createUserThatRequiresPasswordToLoginAndIsNotSuperAdminAndIsNotEnabled())...

"Avoid Side Effects"

Tu raczej powinno być "avoid global variables". Bo o ile raczej bezdyskusyjne jest, że nie powinniśmy pisać do globalnych zmiennych, o tyle z tymi efektami ubocznymi to różnie bywa - wszak czasem np. nasz kod chce coś zapisać do jakiegoś logu. 

"Don't write to global functions"

Znów - powinno być raczej "use namespaces".

"Don't use a Singleton pattern"

_Ale że co_? Autor powołuje się na tekst z Wikipedii. Na Wiki jest napisane, że _niektórzy_ uważają singletona za antywzorzec. Zacytuję może fragment parę akapitów wyżej (z "avoid side effects"): "Don't have several functions and classes that write to a particular file. Have one service that does it. One and only one.". Hm, dla mnie brzmi jak modelowe zastosowanie singletona. Przykład też jest absurdalny - sterowniki baz danych są akurat świetnym przykładem miejsca, gdzie singleton można i często należy stosować. Owszem, singletonów się często nadużywa, ale to jest grubsza sprawa i związana z projektowaniem oprogramowania, a konkretnie identyfikowaniem singletonów na poziomie logicznym (czyli gdzie jest rzeczywiście tak, że czegoś ma być _jeden i już_, a gdzie czegoś powinno być raczej _jeden per cośtam_ albo _chwilowo jest jeden_).

"Avoid conditionals"

Głupie uogólnienie ogólnie słusznej reguły. Powinno być "avoid switch statements" - rzeczywiście, sporą część switchy daje się załatwić polimorfizmem - i zresztą tego dotyczy przykład.

"Avoid type-checking"

Jakbym był złośliwy, to bym powiedział "use a strongly-typed language instead". Asercje typowe są niezbędne w kluczowych punktach w dynamicznie typowanych językach, żeby nie polec potem na testach integracyjnych.

"Open/Closed Principle (OCP)"

Ten przykład odnosi się tylko do połowy problemu. Omawia "open for extension", ale nie tyka "closed for modification". A chodzi o to, żeby kod nie zawierał np. takich krzaczków: 

class X { 
public $kluczowaZmienna;

public function __construct() {
$kluczowaZmienna = "ToJestX";
}

public function doStuff() {
... coś co zależy od $kluczowaZmienna
}
}

tylko zamiast tego np. miał metodę getKluczowyParametr() i ją wywoływał. Kod podany w przykładzie jest o tyle zły, że tam jest coś zakodowane w if-then-else i w związku z tym ten kod też jest "closed for modification" - bo nie da się tak zmienić obiektu klasy, żeby działał. A tu chodzi o to, żeby ktoś, zamiast grzebać w obiekcie klasy X i zmieniać $kluczowaZmienna, napisał sobie podklasę klasy X.

"Liskov Substitution Principle (LSP)"

Kolega powyżej wrzucił linka do swojego tekstu na ten temat, skomentowałem tam. Przykład z kwadratem i prostokątem jest bardzo problematyczny.

"Interface Segregation Principle (ISP)"

Dobra zasada, zły przykład. Nie widać tutaj, gdzie jest zależność od interfejsu, którego się nie wykorzystuje. Przykład takiego złego "dużego interfejsu" to np.

interface WorkerInterface {
public function work();
public function manageSubemployees();
}

class NormalWorker implements WorkerInterface {
public function work() { ... } 
public function manageSubemployees() { throw new Exception("Normal workers don't have subemployees");
}

class Manager extends NormalWorker { 
public function manageSubemployees() { ... }
}

zamiast

interface WorkerInterface {
public function work();
}

interface ManagerInterface extends WorkerInterface {
public function manageSubemployees();
}

i dalej odpowiednio.

"Dependency Inversion Principle (DIP)"

Z tego fragmentu ni cholery nie da się zrozumieć, o co chodzi. Bez mechanizmów DI wyjaśnianie tego w ogóle jest raczej skazane na porażkę. No i to bardzo zaawansowana koncepcja, nie ma na niej kompletnie miejsca IMO w podstawowym poradniku.

"Use method chaining"

Pewne wzorce projektowe (głównie builder) używają method chaining, ale nie jest to koniecznie przyjęta zasada. Dyskusyjne.
