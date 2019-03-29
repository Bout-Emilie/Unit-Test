# Tests Unitaires 

Cette article a pour but d'expliquer les tests unitaires avec PhpUnit.

## C'est quoi ? 

Un test unitaire est un procédé permettant de s'assurer du bon fonctionnement d'une unité de programme.
Lorsqu'on veut tester une application manuellement, on clique  partout on regarde si elle fonctionne. Cela devient vite rebarbatif et ennuyeux. 
On a donc créé les tests unitaires. Le but est d'automatiser les tests afin de s'assurer que des bouts de code fonctionnent comme il faut.  

## Les avantages des tests unitaires:


Mais quel est l'avantage d'un test unitaire si je dois écrire plus de code et donc perdre du temps ?

- **Gain de temps future** : 
Et comme le temps c'est de l'argent on  reduit donc les couts. Ils permettent de trouver les bugs plus facilement et donc d'éviter de compiler des bugs qui seront très difficiles à identifier par la suite.

- **Eviter les regressions** : Faciliter les changements (Update - Delete)

    Les tests unitaires détectent les modifications susceptibles de rompre un contrat de conception. Ils aident donc à maintenir et à changer le code. Les tests unitaires réduisent les défauts des nouvelles fonctionnalités ou réduisent les bugs lors du changement des fonctionnalités existantes. 

- **Fournit de la documentation** :
Les développeurs qui souhaitent savoir quelles fonctionnalités sont fournies par une unité et comment les utiliser peuvent consulter les tests d’unités pour acquérir une compréhension de base de l’interface de l’appareil.

- **Rend le process Agile** :
Unit testing really goes hand-in-hand with agile programming of all flavors because it builds in tests that allow you to make changes more easily. In other words, unit tests facilitate safe refactoring

- **Code plus modulaire** : 
Le principal pré-requis au test est la modularité du code. Une idée simple : diviser pour mieux régner. Un code modulaire est bien plus robuste qu'un imbroglio de règles de gestion et de traitements. Plus il est modulaire, plus un code est facile à tester unitairement…

## Caractéristiques d'un bon test unitaire 


- **En amont**: crée lors des développements, pendant ou avant idéalement ( TDD : TEST DRIVEN DEVELOPMENT)
- **Deterministe**: c’est-à-dire qu’exécuté plusieurs fois, il devra toujours retourner le même résultat.
- **Unitaire**: Independant des autres tests et ne faire appel a aucun composant exterieur. Si c'est le cas, il est nécessaire de les simuler en les mockant par example.
- **Rapide**: Rapide a executer
- **Normalisé**: Les noms des tests unitaires doivent suivre une norme : test[Comportement a tester]. Ex : testSaveFile();
- **Automatisé**: Ils ne necessitent pas d'actions manuelles
- **Autovérification**: Le test doit pouvoir détecter automatiquement son état de réussite ou d’échec sans aucune interaction humaine.

En gros il suit la règle des `AAA`: Arrange, Act, Assert

- Arranger : Il s’agit dans un premier temps de définir les objets, les variables nécessaires au bon fonctionnement de son test (initialiser les variables, initialiser les objets à passer en paramètres de la méthode à tester, etc.).

- Agir : Ensuite, il s’agit d’exécuter l’action que l’on souhaite tester (en général, exécuter la méthode que l’on veut tester, etc.)

- Auditer : Et enfin de vérifier que le résultat obtenu est conforme à nos attentes.

 
```php
$x=2;       \\ Arrange
$y = racine(4); \\ Act
if ($x==$y) { \\ Assert
  return true;
}
else{
  return false;
}
```


## Les avantages des tests unitaires:


Mais quel est l'avantage d'un test unitaire si je dois écrire plus de code et donc perdre du temps ?

- **Gain de temps future** : 
Et comme le temps c'est de l'argent -> reduction des couts. Permet de trouver les bugs plus facilement et donc d'éviter de compiler des bugs qui seront très difficiles à identifier par la suite.

- **Eviter les regressions** : Facilite les changements (Update - Delete)

    Les tests unitaires détectent les modifications susceptibles de rompre un contrat de conception. Ils aident donc à maintenir et à changer le code. Les tests unitaires réduisent les défauts des nouvelles fonctionnalités ou réduisent les bugs lors du changement des fonctionnalités existantes. 

- **Fournit de la documentation** :
Les développeurs qui souhaitent savoir quelles fonctionnalités sont fournies par une unité et comment les utiliser peuvent consulter les tests d’unités pour acquérir une compréhension de base de l’interface de l’appareil (API).

- **Rend le process Agile** :
Unit testing really goes hand-in-hand with agile programming of all flavors because it builds in tests that allow you to make changes more easily. In other words, unit tests facilitate safe refactoring

- **Code plus modulaire** : 
Le principal pré-requis au test est la modularité du code. Une idée simple : diviser pour mieux régner. Un code modulaire est bien plus robuste qu'un imbroglio de règles de gestion et de traitements. Plus il est modulaire, plus un code est facile à tester unitairement…

## L'isolation d'un test ? 

Parfois un test a quand meme besoin de faire des appels aux services, aux bases de données, fichiers ou d'autre classe.Cependant comme un test ne doit dépendre d'aucune sources externes, nous avons la possibilité de mocker les données. 

Un mock est un objet qui permet de simuler un objet réel tel que la base de données, un web service…

L’écriture d’objets de type mock peut s’avérer longue et fastidieuse, les objets ainsi codés peuvent contenir des bugs comme n’importe quelle portion du code. Des frameworks ont donc été conçus pour rendre la création de ces objets fiable et rapide.

Les frameworks de mock permettent de créer dynamiquement des objets généralement à partir d’interfaces ou de classes

**Oui mais ...**

Voici un test unitaire permettant de tester la function addToTotal(


  ```php
  public function testAddToTotal() {
    $this->addition->setTotal(200);
    $this->addition->addToTotal(166);
    $this->assertEquals(366, $this->lissabon->getTotal());
  }
  ```

- `$this->addition->setTotal(200);` permet de changer la valeur de notre "objet" addition a 200
- `$$this->addition->addToTotal(166);` permet d'ajouter la valeur 166 à "l'objet " addition 
-`$this->assertEquals(366, $this->addition->getTotal());` permet de comparer la valeur obtenu avec la methode `addToTotal` avec le resultat voulue 366. 


Un problème se pose : nous avons dis qu'un test unitaire d'une function etait isolé des autres tests, or ici nous avons utilisé : 

- des accesseurs
- un constructeur



Il a donc certaines limitations qui ne pourront etre mockes (mais autorisé a utiliser si nous avons pas d'autres choix).

- les classes finales,
- les enums,
- les méthodes final,
- les méthodes static,
- les méthodes privées.
- les méthodes du style str_replace.


## De  la théorie à la pratique : PhpUnit avec Drupal

###Installation de PhpUnit

```shell
composer require --dev phpunit/phpunit;
```

**Verification de l'installation** : 
- A la racine du projet :
```shell
vendor/bin/phpunit --version
```
- Resultat attentu
```shellk
PHPUnit 8.0.0 by Sebastian Bergmann and contributors.
```

###Les bonnes pratiques avec PhpUnit :

- Les classes test doivent etre dans un dossier Unit. 
- Les classes test doivent heritées de la classe UnitTestCase.
- Les classes test doivent avoir comme patern *Test.php 
- Les functions et attributs de la classe Test doivent etre public 
- Les methodes doivent avoir comme patern test*, ex : testSetName().



    ![alt text](/file.png "List des shemas")


###Petit exemple avec Php

La classe `Toto` : 


```php
 <?php
                  namespace Drupal\phpunit_example;
                  /**
                   * Defines a Toto class.
                   */
                  class Toto {
                    private $length = 0;
                    /**
                     * @param int $length
                     */
                    public function setLength(int $length) {
                      $this->length = $length;
                    }
                    /**
                     * @return int
                     *   The length of the unit.
                     */
                    public function getLength() {
                      return $this->length;
                    }
                  }
```              
                
La classe `TotoTest` : 

```php
<?php
                    use Drupal\Tests\UnitTestCase;
                    use Drupal\projetTest\Toto;
                    require __DIR__ . "/../../../src/Toto.php";
                    
                    class TotoTest extends UnitTestCase {
                      protected $toto;
                     
                      public function setUp() {
                        $this->toto = new Toto();
                      }
                      
                      public function tearDown() {
                        unset($this->toto);
                      }
                      
                      public function testSetLenght() {
                        $this->assertEquals('0', $this->toto->getTotal());
                        $this->toto->setTotal(366);
                        $this->assertEquals(366, $this->toto->getTotal());
                      }
                      
                      public function testGetLenght() {
                        $this->toto->setTotal(366);
                        $this->assertNotEquals(200, $this->toto->getTotal());
                      }
                    }
``` 


**Explications** : 

  - `setUp()` : Il est appelé avant chaque méthode de test et permet d'initialiser l'objet $toto qui sera utilisé dans tous les tests .
  - `tearDown()`:  Il est appelé après chaque méthode de test et permet de "nettoyer" l'objet apres chaque fonction de tests.
  - `testSetLenght()` : C'est la méthode qui permet de tester la méthode setLenght()
  - `testGetLenght()` : C'est la méthode qui permet de tester la méthode getLenght()
  - `assertNotEquals` et `assertEquals` : Il s'agit d'`Assertions` ( voir $paragraphe Assertion)


### Lancer nos tests

  **Configuration de `PhpUnit.xml.dist`**
    Pour  cela copier coller le fichier PhpUnit.xml.dist present dans le dossier core et renommer en PhpUnit.xml. Copier coller le code ci dessous.

``` xml
    <?xml version="1.0" encoding="UTF-8"?>

                    <phpunit bootstrap="tests/bootstrap.php"
                           backupGlobals="false"
                           colors="true"
                           verbose="true"
                           beStrictAboutTestsThatDoNotTestAnything="true"
                           beStrictAboutOutputDuringTests="true"
                           beStrictAboutChangesToGlobalState="true">
                    <php>
                        <env name="SIMPLETEST_BASE_URL" value="http://laberline.local"/>
                    </php>
                    <testsuites>
                      <testsuite name="toto">
                        <directory>../modules/custom/</directory>
                      </testsuite>
                    </testsuites>
                    </phpunit>
```
- L’élément `<testsuites>` et son ou ses enfants `<testsuite>` peuvent être utilisés pour composer une série de tests.
Ci dessus on configure pour pouvoir executer tous les tests present dans custom. 

- Vous avez besoin de configurer votre variable SIMPLETEST_BASE_URL avec la valeur de votre url de base. 



``` xml
<testsuite name="toto">
                <directory> modules/custom/toto/tests </directory>
</testsuite>
<testsuite name="titi">
                <directory> modules/custom/titi/tests </directory>
</testsuite>
```

Ci dessus, on configure pour pouvoir executer tous les tests presents dans titi ou dans toto. 


**Lancer les tests**
  A la racine du projet , pour executer tous les test
```shell
vendor/bin/phpunit -c web/core/phpunit.xml
```

A la racine du projet, pour executer uniquement les tests de toto 
```shell
vendor/bin/phpunit -c web/core/phpunit.xml --testsuite=custom
```

Il est possible de faire de nombreuses configurations dans ce fichier : 
[Documentation officielle PHPUnit](https://phpunit.readthedocs.io/fr/latest/configuration.html)

###Les états possibles des tests

- `Ok`

![alt text](/resultat_requete_ok.png "Resultat requete ok")

- `Faillure`

![alt text](/resultat_requete_faillure.png "Resultat requete faillure")

- `Risky`

![alt text](/resultat_requete_risky.png "Resultat requete risky")

### Les assertions 

Voici quelques méthodes utiles d'assertions lors des tests unitaires.

  - assertEquals(var1,var2) : return false si les 2 variables ne sont pas    égales.
  - assertEmpty(var1): return false si var1 n'est pas vide.
  - assertSame(var1,var2) : return false si les 2 variables ne sont pas égales et du meme type, - égalité stricte-
  - assertInstanceOf(var1,var2) : return false si les var2 n'est pas une instance de var1
  - assertArrayHasKey(key , array) : return false si key n'est pas présent dans array
  - assertCount(count, array) : return false si le nombre d'élement dans  array ne correspond pas a count. 
  - assertNull(var1): return false si var1 n'est pas null.


[Documentation officielle PHPUnit sur les assertions](https://phpunit.readthedocs.io/fr/latest/assertions.html)
### Les fixtures 

Imaginons que nous devons creer des tests pour la meme classe. Tous ces tests vont dans un permier temps instancier notre classe. 
Dans de nombreux framwork de test , il existe des fixtures qui sont appellees a des moments bien precis lors d'un appel de test et qui vont nous permettrent de gagner du temps. 

Par exemple on a deux fonctions de test ou on instancie notre classe Toto  : 

```php
<?php
class TotoTest extends UnitTestCase {
 public function testSomething()
 {
  toto = new Toto();
 // act, assert...
 }
 public function testOtherFeature()
 {
 $toto = new Toto();
 // act, assert...
 }
}
```
Pour eviter de repeter cette etape trop souvent, nous pouvons nous servir d'une fixture : 

  -  `setUp()` : Il est appelé avant chaque méthode de test et permet d'initialiser  tous les elements comme nos objets, notre base de donnees qui sera utilisé dans tous les tests .

```php

<?php
class TotoTest extends UnitTestCase
{
 private $toto;
 public function setUp()
 {
 $this->toto = new Toto();
 }
 public function testSomething()
 {
 // act on $this->_iterator, assert...
 }
 public function testOtherFeature()
 {
 // act on $this->_iterator, assert...
 }
}
```


Une autre fixture tres utile : 

  - `tearDown()`:  Il est appelé après chaque méthode de test et permet de "nettoyer" les elements apres chaque fonction de tests.

  
### Les annotations 


Les annotations sur PHPUnit doivent etre placé dans les blocs de commmentaire.  Un « doc comment » en PHP doit commencer par /** et se terminer avec */. Les annotations se trouvant dans des commentaires d’un autre style seront ignorées.
Il est de la forme @annotation


Il existe de nombreuses annotations , en voici les principales: 

 - `@after`: Peut être utilisée pour spécifier des méthodes devant être appelées après chaque méthode de test.

 - `@before` : Peut être utilisée pour spécifier des méthodes devant être appelées avant chaque méthode de test.

 - `@expectedException` : Permet de tester si une exception est levée dans le code. 
  
  Par exemple si on ce code : 

  ```php
  public function testException()
 {
 try {
 $iterator = new ArrayIterator(42);
 $this->fail();
 } catch (InvalidArgumentException $e) {
 }
 }
  ```

On peut le remplacer par : 
```php
/**
 * @expectedException InvalidArgumentException
 */
 public function testMethodRaiseExceptionAgain()
 {
 $iterator = new ArrayIterator(42);
 }
```

-  `@dataProvider` : Ceux-ci permettent via une fonction de fournir à une fonction de tests un jeu de données.

Par exemple on a ce code : 

```php
?php
use PHPUnit\Framework\TestCase;

class DataTest extends TestCase
{
    /**
     * @dataProvider additionProvider
     */
    public function testAdd($a, $b, $expected)
    {
        $this->assertSame($expected, $a + $b);
    }

    public function additionProvider()
    {
        return [
            [0, 0, 0],
            [0, 1, 1],
            [1, 0, 1],
            [1, 1, 3]
        ];
    }
}

```

Ici on va donc tester la méthode Add avec 4 jeux de données differents, ce test va nous retourner une erreur avec le dernier essais : 

```shell
There was 1 failure:

1) DataTest::testAdd with data set #3 (1, 1, 3)
Failed asserting that 2 is identical to 3.
```
### Les stubs (bouchons)


### Un peu plus loin : Les mocks (doublures)

``` php
        use Drupal\Tests\UnitTestCase;
        use Drupal\projetTest\Toto;
        require __DIR__ . "/../../../src/Toto.php";
                    
        class TotoTest extends UnitTestCase {
              protected $toto;
                     
              public function testExemple() {

                $client = $this->createMock('Titi');
              }
               
```
Admettons que dans la fonction testExemple nous ayons besoin de l'objet Titi. 
Nous allons creer un mock de Titi grace a la methode : `createMock`
Il est aussi possible de maîtriser le retour d'une de ses methode grace a la methode `willReturn`

``` php
        use Drupal\Tests\UnitTestCase;
        use Drupal\projetTest\Toto;
        require __DIR__ . "/../../../src/Toto.php";
                    
        class TotoTest extends UnitTestCase {
              protected $toto;
                     
              public function testExemple() {

                $response=0

                $client = $this->createMock('Titi');
                $client->method('get')->willReturn($response);

                $this->assertEqual(0,$client->get());
              }
               
```

```php
{
 $coordinates = array('latitude' => '42N', 'longitude' =>
'12E');
 $googleMapsMock = $this->getMock('GoogleMaps',
array('getLatitudeAndLongitude'));
 $googleMapsMock->expects($this->once())
 ->method('getLatitudeAndLongitude')
 ->with('Rome')
 ->will($this->returnValue($coordinates));
 $service = new GeolocationService($googleMapsMock);
 $user = new User;
 $user->location = 'Rome';
 $service->locate($user);
 $this->assertEquals('42N', $user->latitude);
 $this->assertEquals('12E', $user->longitude);
 }
```

[Documentation officielle PHPUnit sur les doublures](https://phpunit.readthedocs.io/fr/latest/test-doubles.html)

###La base de données 

Comme Drupal est fortement dependant de la base de données, il est parfois difficile de creer des tests unitaires sans cette derniere. 

A éviter au maximun car difficile a maintenir.

**Les 4 étapes**
- Nettoyer la base de données
- Configurer les fixtures
- Exécuter les tests, vérifier les résultats 
- Nettoyer la base de données






sudo -u www-data vendor/bin/phpunit -c web/core/phpunit.xml
