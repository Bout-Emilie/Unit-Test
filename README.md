<center> <h1 style='color:#6FA8DC'>Tests Unitaires </h1></center>

<p align="center"> <img src="/6a0c9d64791502a000de8a6472351a48.jpg"> </p>

Cette article a pour but d'expliquer les tests unitaires avec PhpUnit et ensuite Drupal8.

<center><h2> C'est quoi ?</h2> </center>

Un test unitaire est un procédé permettant de s'assurer du bon fonctionnement d'une unité de programme.
Lorsqu'on veut tester une application manuellement, on clique  partout on regarde si elle fonctionne. Cela devient vite rébarbatif et ennuyeux. 
On a donc créé les tests unitaires. Le but est d'automatiser les tests afin de s'assurer que des bouts de code fonctionnent comme il faut.  


<center><h2>Les avantages des tests unitaires:</h2> </center>


Mais quel est l'avantage d'un test unitaire si je dois écrire plus de code et donc perdre du temps ?

- **Gain de temps future** : 
Et comme le temps c'est de l'argent on  réduit donc les couts. Ils permettent de trouver les bugs plus facilement et donc d'éviter de compiler des bugs qui seront très difficiles à identifier par la suite.

- **Eviter les regressions** : Faciliter les changements (Update - Delete)

    Les tests unitaires détectent les modifications susceptibles de rompre un contrat de conception. Ils aident donc à maintenir et à changer le code. Les tests unitaires réduisent les défauts des nouvelles fonctionnalités ou réduisent les bugs lors du changement des fonctionnalités existantes. 

- **Fournit de la documentation** :
Les développeurs qui souhaitent savoir quelles fonctionnalités sont fournies par une unité et comment les utiliser peuvent consulter les tests d’unités pour acquérir une compréhension de base de l’interface de l’appareil.


- **Code plus modulaire** : 
Le principal prérequis au test est la modularité du code. Une idée simple : diviser pour mieux régner. Un code modulaire est bien plus robuste qu'une multitude de règles de gestion et de traitements. Plus il est modulaire, plus un code est facile à tester unitairement…

<center><h2>Caractéristiques d'un bon test unitaire </h2> </center>


- **En amont**: créé lors des développements, pendant ou avant idéalement (TDD : TEST DRIVEN DEVELOPMENT)
- **Deterministe**: c’est-à-dire qu’exécuté plusieurs fois, il devra toujours retourner le même résultat.
- **Unitaire**: Indépendant des autres tests et ne faire appel à aucun composant exterieur. Si c'est le cas, il est nécessaire de les simuler en les mockant par exemple.
- **Rapide**: Rapide à éxecuter
- **Normalisé**: Les noms des tests unitaires doivent suivre une norme : test[Comportement à tester]. Ex : testSaveFile();
- **Automatisé**: Ils ne nécessitent pas d'actions manuelles
- **Autovérification**: Le test doit pouvoir détecter automatiquement son état de réussite ou d’échec sans aucune interaction humaine.

En gros il suit la règle des `AAA`: **Arrange, Act, Assert**

- **Arranger** : Il s’agit dans un premier temps de définir les objets, les variables nécessaires au bon fonctionnement de son test (initialiser les variables, initialiser les objets à passer en paramètres de la méthode à tester, etc.).

- **Agir** : Ensuite, il s’agit d’exécuter l’action que l’on souhaite tester (en général, exécuter la méthode que l’on veut tester, etc.)

- **Auditer** : Et enfin de vérifier que le résultat obtenu est conforme à nos attentes.

 
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

 
<center><h2>L'isolation d'un test ? </h2></center>

Parfois un test a quand même besoin de faire des appels aux services, aux bases de données, fichiers ou d'autres classes. Cependant comme un test ne doit dépendre d'aucune source externes, nous avons la possibilité de mocker les données. 

Un mock est un objet qui permet de simuler un objet réel tel que la base de données, un web service…

L’écriture d’objets de type mock peut s’avérer longue et fastidieuse, les objets ainsi codés peuvent contenir des bugs comme n’importe quelle portion du code. Des frameworks ont donc été conçus pour rendre la création de ces objets fiable et rapide.

Les frameworks de mock permettent de créer dynamiquement des objets généralement à partir d’interfaces ou de classes

**Oui mais ...**

Voici un test unitaire permettant de tester la function **addToTotal** qui permet d'additionner 2 nombres ensemble.


  ```php
  public function testAddToTotal() {
    $this->addition->setTotal(200);
    $this->addition->addToTotal(166);
    $this->assertEquals(366, $this->lissabon->getTotal());
  }
  ```

- `$this->addition->setTotal(200);` permet de changer la valeur de notre "objet" addition à 200
- `$$this->addition->addToTotal(166);` permet d'ajouter la valeur 166 à "l'objet " addition 
-`$this->assertEquals(366, $this->addition->getTotal());` permet de comparer la valeur obtenu avec la methode `addToTotal` avec le resultat voulue 366. 


Un problème se pose : nous avons dit qu'un test unitaire d'une function était isolé des autres méthodes, or ici nous avons utilisé : 

- des accesseurs
- un constructeur



Il a donc certaines limitations qui ne pourront être mockés (mais autorisé à utiliser si nous n'avons pas d'autres choix).

- les classes finales,
- les enums,
- les méthodes final,
- les méthodes static,
- les méthodes privées.
- les méthodes du style str_replace.


<center><h2> De  la théorie à la pratique : PhpUnit avec Drupal</h2></center>

<h3>Installation de PhpUnit</h3>

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

<h3>Les bonnes pratiques avec PhpUnit :</h3>

- Les classes test doivent être dans un dossier Unit. 
- Les classes test doivent heriter de la classe UnitTestCase.
- Les classes test doivent avoir comme patern *Test.php 
- Les fonctions et attributs de la classe Test doivent etre public 
- Les méthodes doivent avoir comme patern test*, ex : testSetName().




    <center><img src="/file.png"></center>


<h3>Petit exemple avec Php</h3>

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
  - `tearDown()`:  Il est appelé après chaque méthode de test et permet de "nettoyer" l'objet après chaque fonction de tests.
  - `testSetLenght()` : C'est la méthode qui permet de tester la méthode setLenght()
  - `testGetLenght()` : C'est la méthode qui permet de tester la méthode getLenght()
  - `assertNotEquals` et `assertEquals` : Il s'agit d'`Assertions` ( voir $paragraphe Assertion)


<h3>Lancer nos tests</h3>

  **Configuration de `PhpUnit.xml.dist`**
    Pour  cela copier coller le fichier PhpUnit.xml.dist present dans le dossier core et renommer en PhpUnit.xml. Copier coller le code ci-dessous.

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
Ci-dessus on a configuré pour pouvoir éxecuter tous les tests present dans custom. 


``` xml
<testsuite name="toto">
                <directory> modules/custom/toto/tests </directory>
</testsuite>
<testsuite name="titi">
                <directory> modules/custom/titi/tests </directory>
</testsuite>
```

Ci-dessus, on a configuré pour pouvoir executer tous les tests presents dans titi ou dans toto. 


**Lancer les tests**
  A la racine du projet ou dans le dossier `core` pour Drupal, 
  
  - pour éxecuter tous les tests
```shell
vendor/bin/phpunit -c web/core/phpunit.xml
```
-  pour éxecuter uniquement les tests de toto 
```shell
vendor/bin/phpunit -c web/core/phpunit.xml --testsuite=custom
```

Il est possible de faire de nombreuses configurations dans ce fichier : [Documentation officielle PHPUnit](https://phpunit.readthedocs.io/fr/latest/configuration.html)

**Les états possibles des tests**

- `Ok`, tout va bien 

![alt text](/resultat_requete_ok.png "Resultat requete ok")

- `Faillure`, il y a eu une erreur

![alt text](/resultat_requete_faillure.png "Resultat requete faillure")

- `Risky`, peut s’agir de tests inutiles, le temps d'exécution peut être trop long ...


![alt text](/resultat_requete_risky.png "Resultat requete risky")

<h3>Les assertions </h3>

Voici quelques méthodes utiles d'assertions lors des tests unitaires.

  - **assertEquals(var1,var2)** : return false si les 2 variables ne sont pas    égales.
  - **ssertEmpty(var1)**: return false si var1 n'est pas vide.
  - ***assertSame(var1,var2)** : return false si les 2 variables ne sont pas égales et du meme type, - égalité stricte-
  - **assertInstanceOf(var1,var2)** : return false si les var2 n'est pas une instance de var1
  - **assertArrayHasKey(key , array)** : return false si key n'est pas présent dans array
  - **assertCount(count, array)** : return false si le nombre d'élement dans  array ne correspond pas a count. 
  - **assertNull(var1)**: return false si var1 n'est pas null.


[Documentation officielle PHPUnit sur les assertions](https://phpunit.readthedocs.io/fr/latest/assertions.html)
<h3> Les fixtures </h3>

Imaginons que nous devons créer des tests pour la même classe. Tous ces tests vont dans un premier temps instancier notre classe. 
Dans de nombreux framwork de test , il existe des fixtures qui sont appelées à des moments bien précis lors d'un appel de test et qui vont nous permettrent de gagner du temps. 

Par exemple on a deux fonctions de tests ou on instancie notre classe Toto  : 

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
Pour éviter de répéter cette étape trop souvent, nous pouvons nous servir d'une fixture : 

  -  `setUp()` : Il est appelé avant chaque méthode de test et permet d'initialiser tous les éléments comme nos objets, notre base de données qui sera utilisé dans tous les tests.

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


Une autre fixture très utile et à faire obligatoirement: 

  - `tearDown()`:  Il est appelé après chaque méthode de test et permet de "nettoyer" les  éléments après chaque fonction de tests.

  
<h3>Les annotations </h3>


Les annotations sur PHPUnit doivent  être placées dans les blocs de commentaire.  Un « doc comment » en PHP doit commencer par /** et se terminer avec */. Les annotations se trouvant dans des commentaires d’un autre style seront ignorées.
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

Ici on va donc tester la méthode Add avec 4 jeux de données différents, ce test va nous retourner une erreur avec le dernier essai : 

```shell
There was 1 failure:

1) DataTest::testAdd with data set #3 (1, 1, 3)
Failed asserting that 2 is identical to 3.
```



<h3> Un peu plus loin : Les mocks (doublures)</h3>

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
Il est aussi possible de maîtriser le retour d'une de ses méthode grâce à la méthode `willReturn`

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

<h3>La base de données </h3>

Comme Drupal est fortement dépendant de la base de données, il est parfois difficile de créer des tests unitaires sans cette dernière. 

A éviter au maximun car difficile a maintenir.

**Les 4 étapes lors de l'utilisation d'une base de données**
- Nettoyer la base de données
- Configurer les fixtures
- Exécuter les tests, vérifier les résultats 
- Nettoyer la base de données



Dans un premier temps il faut configurer notre environnement de test de base de données. 

Pour cela on va utiliser 2 méthodes : `getConnection()` et `getDataSet()` et notre fichier `PhpUnit.xml` . 

- Notre fichier de test
```php 
<?php
use PHPUnit\Framework\TestCase;
use PHPUnit\DbUnit\TestCaseTrait;

abstract class Generic_Tests_DatabaseTestCase extends TestCase
{
    use TestCaseTrait;

    // only instantiate pdo once for test clean-up/fixture load
    static private $pdo = null;

    // only instantiate PHPUnit_Extensions_Database_DB_IDatabaseConnection once per test
    private $conn = null;

    final public function getConnection()
    {
        if ($this->conn === null) {
            if (self::$pdo == null) {
                self::$pdo = new PDO( $GLOBALS['DB_DSN'], $GLOBALS['DB_USER'], $GLOBALS['DB_PASSWD'] );
            }
            $this->conn = $this->createDefaultDBConnection(self::$pdo, $GLOBALS['DB_DBNAME']);
        }

        return $this->conn;
    }
}
?>
```
- Notre fichier de PhpUnit.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<phpunit>
    <php>
        <var name="DB_DSN" value="mysql:dbname=myguestbook;host=localhost" />
        <var name="DB_USER" value="user" />
        <var name="DB_PASSWD" value="passwd" />
        <var name="DB_DBNAME" value="myguestbook" />
    </php>
</phpunit>
```

`getConnection` permet de fournir un accès à une connexion de base de données abstraite via la bibliothèque PDO. 



La méthode `getDataSet()` définit à quoi doit ressembler l'état initial de la base de données avant que chaque test ne soit exécuté . Elle est lancée lors du setUp. 

Il existe plusieurs facons de définir l'état initial de la base de données( en xml, yaml ou csv). 

- Exemple de getDataSet() avec xml. 

```php
<?php
use PHPUnit\Framework\TestCase;
use PHPUnit\DbUnit\TestCaseTrait;

class MyTestCase extends TestCase
{
    use TestCaseTrait;

    public function getDataSet()
    {
        return $this->createFlatXmlDataSet('myFlatXmlFixture.xml');
    }
}
?>
```

- Voici le fichier xml : 
```xml
<?xml version="1.0" ?>
<dataset>
    <table name="guestbook">
        <column>id</column>
        <column>content</column>
        <column>user</column>
        <column>created</column>
        <row>
            <value>1</value>
            <value>Hello buddy!</value>
            <value>joe</value>
            <value>2010-04-24 17:15:23</value>
        </row>
        <row>
            <value>2</value>
            <value>I like it!</value>
            <null />
            <value>2010-04-26 12:14:20</value>
        </row>
    </table>
</dataset>
```

- Exemple de getDataSet() avec yaml

``` yaml
guestbook:
  -
    id: 1
    content: "Hello buddy!"
    user: "joe"
    created: 2010-04-24 17:15:23
  -
    id: 2
    content: "I like it!"
    user:
    created: 2010-04-26 12:14:20
```

```php
<?php
use PHPUnit\Framework\TestCase;
use PHPUnit\DbUnit\TestCaseTrait;
use PHPUnit\DbUnit\DataSet\YamlDataSet;

class YamlGuestbookTest extends TestCase
{
    use TestCaseTrait;

    protected function getDataSet()
    {
        return new YamlDataSet(dirname(__FILE__)."/_files/guestbook.yml");
    }
}
?>
```
- Exemple de getDataSet() avec csv

```csv
id,content,user,created
1,"Hello buddy!","joe","2010-04-24 17:15:23"
2,"I like it!","nancy","2010-04-26 12:14:20"
```
```php
<?php
use PHPUnit\Framework\TestCase;
use PHPUnit\DbUnit\TestCaseTrait;
use PHPUnit\DbUnit\DataSet\CsvDataSet;

class CsvGuestbookTest extends TestCase
{
    use TestCaseTrait;

    protected function getDataSet()
    {
        $dataSet = new CsvDataSet();
        $dataSet->addTable('guestbook', dirname(__FILE__)."/_files/guestbook.csv");
        return $dataSet;
    }
}
?>
```

Une fois notre schéma de base de données configuré nous pouvons créer nos assertions: 
Voici plusieurs exemples de quelques assertions: 

- Sur le nombre de ligne dans une table 

```php
<?php
use PHPUnit\Framework\TestCase;
use PHPUnit\DbUnit\TestCaseTrait;

class GuestbookTest extends TestCase
{
    use TestCaseTrait;

    public function testAddEntry()
    {
        $this->assertEquals(2, $this->getConnection()->getRowCount('guestbook'), "Pre-Condition");

        $guestbook = new Guestbook();
        $guestbook->addEntry("suzy", "Hello world!");

        $this->assertEquals(3, $this->getConnection()->getRowCount('guestbook'), "Inserting failed");
    }
}
?>
```

- Sur le résultat de nos requêtes

```php
<?php
use PHPUnit\Framework\TestCase;
use PHPUnit\DbUnit\TestCaseTrait;

class ComplexQueryTest extends TestCase
{
    use TestCaseTrait;

    public function testComplexQuery()
    {
        $queryTable = $this->getConnection()->createQueryTable(
            'myComplexQuery', 'SELECT complexQuery...'
        );
        $expectedTable = $this->createFlatXmlDataSet("complexQueryAssertion.xml")
                              ->getTable("myComplexQuery");
        $this->assertTablesEqual($expectedTable, $queryTable);
    }
}
?>
```

<center><h2> Et sous Drupal </h2></center>


Il existe différents types de tests dont on pourrait  avoir besoin  sous Drupal: 

  - UnitTestCase
  - KernelTestBase
  - Functional 
    - Browser TestBase
    - JavascriptTestBase


Comment les choisir: 

En général UniTest sert à tester les méthodes propres d'une classe.

KernelTest sert à tester des API ou des tests d'integrations.

Browser TestBase et JavsacripTestBase sont plus des test functionels.



<h3> Les tests UnitTest </h3>


Les tests unitaires se trouvent dans le répertoire: `modules/my_module/tests/src/Unit`.
Il recquiet un **namespace** : `Drupal\Tests\my_module\Unit`.
Il a besoin **d'annotation** : 
  - @coverDefaultClass: qui specifie la classe a tester
  -  @group : qui specifie le groupe du test, c'est souvent le nom du module
  
Ce sont les tests que nous avons vus précédemment 

<h3> Les tests KernelTestBase</h3>


Se trouve dans le **repertoire**: `modules/my_module/tests/src/Kernel/`

Attention on doit toujours avoir une base de données configurée dans notre phpunit.xml.dist

On doit spécifier les dépendances du module comme ceci :

``` 
 public static $modules = ['block', 'apod', 'system', 'user'];
``` 

Ici nos tests vont dépendre du module block, apod , system et user. 

Voici un exemple d'un test d'une API : 

```php
class ApodBlockTest extends KernelTestBase {
  /**
   * {@inheritdoc}
   */
  public static $modules = ['block', 'apod', 'system', 'user'];
  /**
   * {@inheritdoc}
   */
  public function setUp() {
    parent::setUp();
    $this->installSchema('system', ['sequence']);
    $this->installEntitySchema('user');
  }
  /**
   * Test that the apod module provides a block plugin definition.
   */
  public function testEnablingApodModuleCreatesBlock() {
    /** @var \Drupal\Core\Block\BlockManagerInterface $block_manager */
    $block_manager = $this->container->get('plugin.manager.block');
    $plugin_id = 'apod_block';
    $this->assertTrue($block_manager->hasDefinition($plugin_id));
  }
 
  public function testNonEmptyBlock() {
    // Mock the ApodClient object to return a known success response.
    $mockApodClient = $this->prophesize(ApodClient::class);
    $mockApodClient->getAstronomyPictureOfTheDay()
      ->willReturn($this->getMockResponseBody());
    $this->container->set('apod.client', $mockApodClient->reveal());
    /** @var \Drupal\Core\Block\BlockManagerInterface $block_manager */
    $block_manager = $this->container->get('plugin.manager.block');
    $block = $block_manager->createInstance('apod_block');
    $render = $block->build();
    $this->assertArrayHasKey('#theme', $render);
    $this->assertEquals('apod_block', $render['#theme']);
    $this->assertArrayHasKey('#title', $render);
    $this->assertEquals('Foo!', $render['#title']);
    $this->assertArrayHasKey('#image', $render);
    $this->assertArrayHasKey('#content', $render);
    $this->assertNotEmpty($render['#content']);
  }
  
  protected function getMockResponseBody() {
    return (object) [
      'copyright' => 'foo.com',
      'date' => '2017-11-12',
      'explanation' => 'Foo FTW.',
      'hdurl' => 'https://foo.com/hd/foo.jpg',
      'media_type' => 'image',
      'service_version' => 'v1',
      'title' => 'Foo!',
      'url' => 'https://foo.com/foo.jpg',
    ];
  }
```


<h3> Les tests BrowserTest</h3>

On peut en avoir besoin pour : 
- Savoir si un utilisateur authentifié à les bonnes permissions
- Naviguer sur une page donnée
- Tester les rules
- Tester l'installation de son module (crée par defaut avec la console lors de la création d'un module)

Se trouve dans le répertoire: `modules/my_module/tests/src/Funtional`

A pour namespace : `Drupal/Tests/my_module/Functional`

Comme pour les KernelTest on a besoin de spécifier les dependances du modules: 
```php 
public static $modules = ['block', 'imagecache_external', 'apod'];
```

Il a besoin d'annotation : 
  -  @group : qui spécifie le group du test, c'est souvent le nom du module
  

Exemple d'un test d'une installation de module: 

```php
class LoadTest extends BrowserTestBase {

  /**
   * Modules to enable.
   *
   * @var array
   */
  public static $modules = ['laberline_ticket'];

  /**
   * A user with permission to administer site configuration.
   *
   * @var \Drupal\user\UserInterface
   */
  protected $user;

  /**
   * {@inheritdoc}
   */
  protected function setUp() {
    parent::setUp();
    $this->user = $this->drupalCreateUser(['administer site configuration']);
    $this->drupalLogin($this->user);
  }

  /**
   * Tests that the home page loads with a 200 response.
   */
  public function testLoad() {
    $this->drupalGet(Url::fromRoute('<front>'));
    $this->assertSession()->statusCodeEquals(200);
  }

}
```


<h3> Les tests Javascript</h3>

On peut en avoir besoin pour:
  - tester les fonctionnalités en Ajax.


Se trouve dans le **module** `modules/my_module/tests/src/FunctionalJavascript`.

Il a besoin pour **namespace** : `Drupal\Tests\my_module\FunctionalJavascript`

Il a besoin **d'annotation** : 
  -  @group : qui specifie le group du test, c'est souvent le nom du module

Attention il requiert de PhantomJs pour se lancer.


<center><h2>Réferences</h2></center>

Les lectures possibles:
- http://www.kamibarut.de/pdf/practical-php-testing.pdf
- https://www.drupal.org/docs/8/phpunit/running-phpunit-tests
- http://mile23.com/sites/default/files/PHPUnit%20and%20Drupal%208.pdf
- https://github.com/danielnitschelc/unit-testing-d8/tree/master/web/modules/custom/quadrupaler
- https://phpunit.de/manual/4.8/en/writing-tests-for-phpunit.html

Exemple de tests unitaire dans un module
- https://github.com/ebeyrent/apod
- https://github.com/nuez/connect_four
