# SASS -> LUCRURI BASIC:
1. Ce este SASS?
2. Sintaxa SASS
3. Variabile
4. Maps
5. Nesting
6. Partials
7. Functii 
8. Exemple de mixin
9. Extend
10. Operatii matematice

## 1. CE ESTE SASS?
Sass (sau cum se prezinta pe site-ul oficial, "Syntactically Awesome StyleSheets") este o extensie a limbajului CSS. Sintaxa SASS e de doua tipuri: cea indented, adica cea originala, asemanatoare cu Haml, si cea noua care e Sassy CSS. Ai extensii diferite pentru SASS si SCSS. Poti sa-i mai spui "CSS with Superpowers". Este mult mai organizat si mai logic decat CSS. Se zice ca o data ce inveti SASS nu o sa mai folosesti CSS :)) 

De ce ai nevoie ca sa scrii SASS?

1. Un IDE (in cazul meu Visual Studio Code)
2. Un compilator de SASS, pentru ca browserul tau nu stie sa citeasca SASS, deci codul trebuie compilat in plain CSS. Exista mai multe metode de compilare, fie cu nodeJS, fie cu extensie Live Sass Compiler de pe VS Code. (Eu extensia o voi folosi)
3. Un Live Server (sa nu tot dai reload la pagina i guess).

O mica recomandare: sa nu uiti, daca folosesti extensia din VS Code de SASS, sa iti faci setarile cum vrei tu la compiler (Sa ii zici unde sa puna fisierele compilate, in cazul meu, in /dist/css)

## 2. SINTAXA SASS
Sass accepta doua tipuri de sintaxe:
1. Sintaxa SCSS: Foloseste extensia ".scss". E un Superset al CSS-ului, adica orice CSS valid este si SCSS valid. Din cauza acestei similaritati, este foarte populara si se foloseste cel mai mult.
    ```scss
        @mixin button-base(){
            @include typography(button);
            display: inline-flex;
            position:relative;
            height: $button-height;
            vertical-align:middle;

            &:hover{cursor:pointer;}
        }
    ```
2. Sintaxa "Indented": Foloseste identarea in loc de paranteze si nu mai folosesti ;
    ```sass
        @mixin button-base()
            @include typography(button)
        display:inline-flex
        position:relative
        height:$button-height
        border:none
        vertical-align:middle
    ```

## 3. VARIABILE
- sintaxa unei variabile: $numeVariabila: valoare;
- In momentul in care tu folosesti variabile in SASS, ele nu se vor compila in variabile in CSS, ele se vor compila cu valoarea implicita a variabilei (adica tu daca vrei sa pui de exemplu o culoare backgroundului, in SCSS vei avea ```background:$numeVariabila```, dar in CSS compilat vei avea ```background:valoareaVariabilei;```)

## 4. MAPS
- sintaxa unui map: ``` $numeMap: (
    "cheie": valoare,
    "cheie2": altaValoare,
    "cheie3": altaValoare2
);```
- daca vrei sa folosesti ce ai in map, ai o functie pentru asa ceva. De exemplu, am facut un map pentru font-weights, si avem o cheie ```bold``` cu o anumita valoare. Daca vrem sa mergem in body si sa modificam font-weight astfel incat valoarea acestuia sa fie bold-ul din map, scriem astfel: ```font-weight:map-get($font-weights, bold)```, unde ```map-get()``` este functia pentru a lua valoarea din map cu cheia data ca parametru.

## 5. NESTING
- intr-o clasa daca doresti sa modifici un tag doar din interiorul ei, in SCSS poti sa faci un lucru pe nume "nesting". De exemplu:
    ```scss
    .main {
        /*ceva proprietati*/
        p {
            /*ceva proprietati*/
        }
        /*sau poti sa faci o clasa speciala in interiorul .main care sa modifice doar ce ii zici tu sa modifice*/
        .main__paragraph{
            /*ceva proprietati */
        }
        /*o alta solutie este folosirea ampersantului, ca sa nu mai scrii main, si atunci ar fi  &__paragraph, dar apare o alta problema acum, faptul ca in css nu va mai mai fi nested cu .main, ci va fi el singur, ca o clasa izolata. Pentru a rezolva asta trebuie sa folosim interpolarea, deci nu vrem doar .main__paragraph, vrem totul de dinainte, iar pentru asta facem #{&}__paragraph, ca sa iei si parintele de deasupra, si in css sa fie .main .main__paragraph */
        #{&}__paragraph{
            /*proprietati*/

            /* daca vreau sa mai adaug un hover pe paragraph, atunci folosesc doar &*/
            &:hover {
                color: pink;
            }
        }
    }
    ```
## 6. PARTIALS
- In SASS putem sa cream snippeturi mici de CSS pe care sa le includem in alte fisiere SASS. Asta ajuta in momentul in care vrem sa modularizam codul in cazul in care lucram la un proiect mai mare. Un "partial" este un fisier SASS al carui nume are un _ in fata. Compilatorul va ignora fisierele care incep cu _ (de exemplu, _menu.scss);
- Includerea acestor fisiere se face cu ```@import 'pathul-catre-fisier';```

## 7. FUNCTII
- functiile asemanatoare cu cele din javascript. O sa pun aici un exemplu de functie
    ```scss
    @function weight($weight-name){
        @return map-get($font-weights, $weight-name);
    }
    ```
- apelarea functiei se face normal, ca in orice limbaj: ```weight(bold)```

## 8. MIXINS
- Daca nu vrei sa mai scrii inca o data niste proprietati care se aplica pe alte clase / taguri / id-uri, poti folosi mixins, e ca si cum tu deja ti-ai facut un set de proprietati pe care doar le aplici peste mai multe clase. Sau ai mai multe clase care ar trebui sa aiba aceleasi proprietati + cateva chestii in plus (pentru partea a doua poti folosi si extend)
- Sintaxa:
    ```scss
    @mixin flexCenter {
        /*proprietati*/
    }
    ```
- Ca sa le aplici pe o clasa, in interiorul clasei din SCSS, incluzi acel mixin: ```@include flexCenter;```
- La mixin mai ai posibilitatea sa ii dai si parametru. De exemplu:
    ```scss
    @mixin flexCenter($parametru){
        /*proprietati in care folosesti si parametrul asta eventual, de exemplu flex-direction:$parametru;*/
    }
    ```
- La includerea mixin-ului, efectiv ii pasezi un parametru (parametrul ala poate fi de exemplu o anumita valoare pentru o proprietate).

## 9. EXTENDS
- Ajuta la extinderea unor style-uri de la o clasa la alta. Acest lucru poti sa il faci folosind ```@extend numele-clasei-pe-care-vrei-sa-o-extinzi```, iar in continuarea extendului poti sa pui alte atribute sau sa schimbi proprietatile din clasa extinsa dupa bunul plac.
    ```scss
    #{&}_paragraph1 {
        color: pink;
        &:hover {
            color: black;
        }
    }
    #{&}_paragraph2 {
        @extend .main_paragraph1;
        /*pot sa i schimb hoverul*/
        &:hover{
            color: white;
        }
    }
    ```

## 10. OPERATII MATEMATICE
- In CSS exista functia de ```calc()```, iar in paranteze puneam ce voiam noi sa calculam. In SASS poti sa renunti la asta. In schimb, ceea ce ar trebui sa fie de tinut minte aici este ca daca puteai in CSS sa faci smecherii de tipul ```calc(40% - 200px)```, in SASS nu prea mai e valabil acest lucru, pentru ca operanzii trebuie sa fie de acelasi tip. Cand este compilat, CSS-ul va pune rezultatul.




