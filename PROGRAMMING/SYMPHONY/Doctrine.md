---
title: "SYMFONY DOCTRINE"
date: "2020-12-28"
---

Let's talk about Doctrine! Doctrine is an ORM (Object-Relational Mapping), and it's a software that let you model and relate with your database without using a specific query language.

You can so easily swap database (ex: from POSTGRES to MySQL) without changing the code.

Doctrine will also be in charge of the coherence between the classes and the database, making sure that what is inside the code corresponds to what is inside the databse.

#### INSTALLING DOCTRINE

If you create a new FULL project in symfony, doctrine is already installed as package.

### DOCTRINE PARAMETERS

In order to parameter doctrine symfony use the .env file.  
You need to change the URL connection to the dabase :

```
DATABASE_URL="postgresql://db_user:db_password@127.0.0.1:5432/db_name?serverVersion=13&charset=utf8"
```

You can change it to the connection to your databse:

```
DATABASE_URL="musql://root:some_db_password@127.0.0.1:5432/characters?&charset=utf8"
```

### CREATING THE DATABASE

After you have set your database URL in the ".env" file it's now time to create the database. To do so we can use the Symfony console program to run doctrine code :

```
php bin/console doctrine:database:create
```

With this code our database will be created empty!

## ENTITY : What is that?

> Entities are used by doctrine but are not specific of doctrine! You can have entities even without doctrine!
> 
> Simone Panebianco

Let's talk about some concepts in Symfony that you have to understand in order to code properly a symfony program!

What is an Entity? An entity in Symfony is a type of object that is used to hold data. Each instance of an entity holds exactly one row of the targeted database table.

Let's imagine you have a school database to model, you have students, courses, etc. For each table, you create a PHP class that represents your table with getter and setter! And this is like what is an entity.

So for each table in your database you have an entity that represent it.

### Create a new Entity

```
php bin/console make:entity entityName
```

This is our symfony help us create a new Entity. The entity name will also be the table name.

If we run the code, Symfony will ask us some questions about the entity we want to create (and doing so, also the kind of table we want to create)

![](images/image-3.png)

As you can see 2 files are already generated `Student.php` and `StudentRepository.php`

Ok let's just add all asked info for 2 fields: `last_name` and `first_name`

Symfony want to know the type of data that we will insert into the DB and its size, and also if the nullabe or not.

Notice that we didn't put any `auto_increment` ID in our table, and that is because Symfony will always create an ID for each table, since this is good practice.

When we have finished, Symfony will inform us that we have updated the `src/Entity/Student.php` file.

Let's have a look :

#### STUDENT.PHP

```
<?php

namespace App\Entity;

use App\Repository\StudentRepository;
use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\Entity(repositoryClass=StudentRepository::class)
 */
class Student
{
    /**
     * @ORM\Id
     * @ORM\GeneratedValue
     * @ORM\Column(type="integer")
     */
    private $id;

    /**
     * @ORM\Column(type="string", length=50)
     */
    private $last_name;

    /**
     * @ORM\Column(type="string", length=50)
     */
    private $first_name;

    public function getId(): ?int
    {
        return $this->id;
    }

    public function getLastName(): ?string
    {
        return $this->last_name;
    }

    public function setLastName(string $last_name): self
    {
        $this->last_name = $last_name;

        return $this;
    }

    public function getFirstName(): ?string
    {
        return $this->first_name;
    }

    public function setFirstName(string $first_name): self
    {
        $this->first_name = $first_name;

        return $this;
    }
}

```

#### STUDENTREPOSITORY.PHP

```
<?php

namespace App\Repository;

use App\Entity\Student;
use Doctrine\Bundle\DoctrineBundle\Repository\ServiceEntityRepository;
use Doctrine\Persistence\ManagerRegistry;

/**
 * @method Student|null find($id, $lockMode = null, $lockVersion = null)
 * @method Student|null findOneBy(array $criteria, array $orderBy = null)
 * @method Student[]    findAll()
 * @method Student[]    findBy(array $criteria, array $orderBy = null, $limit = null, $offset = null)
 */
class StudentRepository extends ServiceEntityRepository
{
    public function __construct(ManagerRegistry $registry)
    {
        parent::__construct($registry, Student::class);
    }

    // /**
    //  * @return Student[] Returns an array of Student objects
    //  */
    /*
    public function findByExampleField($value)
    {
        return $this->createQueryBuilder('s')
            ->andWhere('s.exampleField = :val')
            ->setParameter('val', $value)
            ->orderBy('s.id', 'ASC')
            ->setMaxResults(10)
            ->getQuery()
            ->getResult()
        ;
    }
    */

    /*
    public function findOneBySomeField($value): ?Student
    {
        return $this->createQueryBuilder('s')
            ->andWhere('s.exampleField = :val')
            ->setParameter('val', $value)
            ->getQuery()
            ->getOneOrNullResult()
        ;
    }
    */
}
```

So, in student.php we can see that we represent the Object itself we got the setter and getter that we need but we have no connection to the database.

We can change some value of the student object using the setters we can get the values of the object, but we don't get any database connection function.

While inside the REPOSITORY file, we arlready have set 4 functions that will be used to connect the database with the code:

- find()
- findOneBy()
- findAll()
- findBy()

#### find()

will find an entity by its ID

#### findOneBy(criteria)

It returns the first one that match the criteria

#### findAll()

Returns all the row of the table

#### findBy(criteria)

Returns all the object that follow the criteria.

You will also see that commented you have some code that show you how you can query your db!

Now we have our object model (student.php) and our repository (studentRepository.php), but there are no data inside the database, so we need to add data to the database. To do so we can use MIGRATIONS

## MIGRATION: What is that?

Migrations are the mechanisms we use in Symfony to make **STRUCTURAL** changes to the database!

So, we have created the idea of the database until now (the entity and its comments) , now we need to create the table in our database.

To do so we need to create a migration:

```
php bin/console make:migration
```

doing so, our migration is created, what that means? it means that you have a migration file in the Migrations folder.

Let's check the migration we have created :

```
<?php

declare(strict_types=1);

namespace DoctrineMigrations;

use Doctrine\DBAL\Schema\Schema;
use Doctrine\Migrations\AbstractMigration;

/**
 * Auto-generated Migration: Please modify to your needs!
 */
final class Version20201230150301 extends AbstractMigration
{
    public function getDescription() : string
    {
        return '';
    }

    public function up(Schema $schema) : void
    {
        // this up() migration is auto-generated, please modify it to your needs
        $this->addSql('CREATE TABLE student (id INT AUTO_INCREMENT NOT NULL, last_name VARCHAR(50) NOT NULL, first_name VARCHAR(50) NOT NULL, PRIMARY KEY(id)) DEFAULT CHARACTER SET utf8mb4 COLLATE `utf8mb4_unicode_ci` ENGINE = InnoDB');
    }

    public function down(Schema $schema) : void
    {
        // this down() migration is auto-generated, please modify it to your needs
        $this->addSql('DROP TABLE student');
    }
}
```

As you can see, we have here the SQL to create our table in the database (function up), if you want to modify some stuff, you can do it by hand if you want.

Even if the migration is created, there is not yet something inside the database, because the migration is not run yet :

```
php bin/console doctrine:migration:migrate
```

Let's try this immediately :

![](images/image-4-1024x82.png)

It asks us if we are sure about the modification of the database, and then it runs the modifications!

The new table is created.

#### MIGRATION: a way to GIT the database.

Migration files are very important because you can use those files to "GIT" the database structure!  
So if you work with a team, if you do some structural changes in your database those will be reflected in a new migration.

Migrations are timestamp named, (with "yyyymmdd" and time structure).

So every time you run a migration, Symfony will compare what you have in your database and what you have in your migrations and will adjust the database structure!

This means that a new dev on the team or an old dev, they will have the same database structure thanks to the migrations.

Now we have the table, but we don't have data inside this table.  
We can create some fake data to put inside the tables, so you can try your application.

## FIXTURE: What is that?

Fixtures are used to load a “fake” set of data into a database that can then be used for testing or to help give you some interesting data while you’re developing your application.

This can be very useful but this is not a core component of Symfony and you need to install it via composer :

```
composer require orm-fixture --dev
```

We use `--dev` argument because we want this package to be present only in the development version of the code, for the production version of the code this package will not be necessary.

As for the npm install of node.js, Composer will modify the `.lock` file to make an easy installation of your project.

Now in the source folder, we have a folder called "DataFixture", with a fixture inside called AppFixtures.php

The idea is that you can create a single fixture to load all your database, or you can create different fixtures for each part of the database you want to populate.

So let's create a fixture for our Students, to do so we can have a little help from Symfony using the script :

```
php bin/console make:fixtures StudentFixtures
```

A new file is created "StudentFixtures.php", and in here we create the Students, and we insert the data in the database, this is the code when we create the fixture :

```
<?php

namespace App\DataFixtures;

use Doctrine\Bundle\FixturesBundle\Fixture;
use Doctrine\Persistence\ObjectManager;

class StudentFixtures extends Fixture
{
    public function load(ObjectManager $manager)
    {
        // $product = new Product();
        // $manager->persist($product);

        $manager->flush();
    }
}
```

Now we need to add some Entities :

In order to use the entity class we need to "USE" the namespace :

```
use App\Entity\Student;
```

Then we can create a new student inside the load function :

```
public function load(ObjectManager $manager)
    {
        $new_student = new Student();
        $new_student->setFirstName("Simone")
                    ->setLastName("Panebianco");
```

(We can chain more function because the object return itself in those functions)

Now that we have set info we need to say that we want to PERSIST those data :

```
$manager->persist($new_student);
```

Doing so we have added to the list of objects to persist (transfer to the database) our new object. We can do this one more time :

```
$new_student_2 = new Student();
$new_student->setFirstName("Lidia")
            ->setLastName("Panebianco");
```

Ok, so we have persisted 2 objects, but we need to launch the script that will transform those objects in data inside the database :

```
$manager->flush();
```

Now everything is ready to be executed :

```
php bin/console doctrine:fixtures:load
```

This is the command that we need to run, but **IMPORTANT**, this will **ERASE** your database and create a new set of data!  
if you want to "append" the data with the existing ones, you need to passe the argument `--append` to the code:

```
php bin/console doctrine:fixtures:load --append
```

Here is the finale code of the page :

```
<?php

namespace App\DataFixtures;

use App\Entity\Student;
use Doctrine\Bundle\FixturesBundle\Fixture;
use Doctrine\Persistence\ObjectManager;

class StudentFixtures extends Fixture
{
    public function load(ObjectManager $manager)
    {
        $new_student = new Student();
        $new_student->setFirstName("Simone")
            ->setLastName("Panebianco");
        $manager->persist($new_student);

        $new_student_2 = new Student();
        $new_student_2->setFirstName("Lidia")
            ->setLastName("Panebianco");
        $manager->persist($new_student_2);
        $manager->flush();
    }
}
```

If you don't want to create 10 differents variables you can do somthing like that using the "clone()" command:

```
class StudentFixtures extends Fixture
{
    public function load(ObjectManager $manager)
    {
        $new_student = new Student();
        $new_student->setFirstName("Simone")
            ->setLastName("Panebianco");
        $manager->persist(clone ($new_student));

        $new_student = new Student();
        $new_student->setFirstName("Lidia")
            ->setLastName("Panebianco");
        $manager->persist(clone ($new_student));
        $manager->flush();
    }
}
```

And also of course you can create a loop into an array, you know how to make your code better...

## LOADING STUDENTS

All this is beautiful, but useless unless we charge our info in the code!  
So let's have a look on how you retrieve all the info from the database.

We start with something easy, list all students.

to do so we create a new controller and a new view (we can call it "StudentController"), for now, we just focus on the controller part.  
(you can see the tutorial [here](http://www.simonepanebianco.fr/symfony-4/))

So, in our controller, in the main route `/students` we need to connect the controller with our database, and what class i created for this purpose?  
The Repository class

This part for now I will learn it by memory, (because I just don't understand how this works)

First, you use an internal method ( probably extended from "AbstractController" ) to retreive the doctrine object

```
$repository = $this->getDoctrine()
```

Now from the doctrine Object we need to get the Repository object (not just the PHP file) that we use to "call" the right repository:

```
$repository = $this->getDoctrine()->getRepository(Student::class)
```

Doing so we get the repository object (not the class itself) and we can call the methods of the repository class. So we can do :

```
$students = $repository ->findAll();
```

And pass `$students` to the front to be show.

Don't forget to USE the class inside the code or you can not add the class Student.

```
use App\Entity\Student;
```

### SHOWING PRIVATE VARIABLES?

There is a special mechanism that is particular to the system that we have used until now! If you remember, the variables of the Student class are all private, so, in theory not accessible from outside, but when we pass the `$students` variable to the front, Twig is able to access those variables!

That's because when Symfony will look for a specific variable in the Entity, like "`Firstname`", and it will not find it, it will also look for a getter function (so "`getFirstname()`"). What does this mean? It means that you could invent new variables and pass them as getter (in theory) like "`Fullname`" that return `Lastname` \+ `Firstname`, even if `Fullname` is not a database column!  
But this also mean that you cannot call the function at your wish, you must respect the "getPropertyname" format!

**I have tested this an IT WORKS :)**

### DEPENDENCY INJECTION and the SERVICE CONTAINER

To understand the dependency injection you need to understand another concept before, the Service Container.

Your application is _full_ of useful objects: a “Mailer” object might help you send emails while another object might help you save things to the database. Almost _everything_ that your app “does” is actually done by one of these objects. And each time you install a new bundle, you get access to even more!

In Symfony, these useful objects are called **services** and each service lives inside a very special object called the **service container** (one obj for all). The container allows you to centralize the way objects are constructed. It makes your life easier, promotes a strong architecture and is super fast!

Now, the moment you start a Symfony app, your container _already_ contains many services. These are like _tools_: waiting for you to take advantage of them. In your controller, you can “ask” for a service from the container by type-hinting an argument with the service’s class or interface name. Want to [log](https://symfony.com/doc/current/logging.html) something? No problem:

```
// src/Controller/ProductController.php 
namespace App\Controller; use Psr\Log\LoggerInterface; 
use Symfony\Component\HttpFoundation\Response; 
class ProductController { 
/**
 *
 @Route("/products")
 */
 public function list(LoggerInterface $logger): Response
 {
 $logger->info('Look, I just used a service!');
 // ...
  }
}
```

So, Symfony know that passing a variable inside the controller (with the kind of the same name of the object), it's like loading the service and passing it to the variable ("$logger" in this case).

This is what we call a **dependency injection**!

More infos here:

[https://symfony.com/doc/current/service\_container.html](https://symfony.com/doc/current/service_container.html)

[https://symfony.com/doc/current/components/dependency\_injection.html](https://symfony.com/doc/current/components/dependency_injection.html)

So now that we have a better IDEA of what is an dependency injection we change the code to make it like this:

```
namespace App\Controller;

use App\Repository\StudentRepository;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class StudentController extends AbstractController
{
    /**
     * @Route("/students", name="students")
     */
    public function allStudents(StudentRepository $repository): Response
    {
        $students = $repository->findAll();
        return $this->render('student/index.html.twig', [
            'controller_name' => 'StudentController', 'students' => $students
        ]);
    }
}
```

And this is how it was before :

```
namespace App\Controller;

use App\Entity\Student;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class StudentController extends AbstractController
{
    /**
     * @Route("/students", name="students")
     */
    public function allStudents(): Response
    {
        $repository = $this->getDoctrine()->getRepository(Student::class);
        $students = $repository->findAll();
        return $this->render('student/index.html.twig', [
            'controller_name' => 'StudentController', 'students' => $students
        ]);
    }
}

```

## DATABASE RELATION - 1 TO MANY

When building a database (especially a relational one) it is important to make relations between tables, for example we may want to create a class for our students, and for each student there is a class, so we have a relation 1 to Many.

We need to make Doctrine understand that when we build the table.

First, we create the "Course" entity (I prefer use Session instead of class because of the same name representing the Class of an object).  
To do so we use the usual console of Symfony

```
php bin/console make:entity Course
```

For each course, we are going to have just a label for the name of the course (string, 255 not nullable) and then the relation field that will call "students"

Since this will be a "relation" field, the kind of the property (string, integer) will be "relation".

![](images/image.png)

When using the relation type a wizard will be called to help us create the relation, the first thing it will ask us is the entity we need to relate to, so in this case student

![](images/image-1.png)

Then it will ask us the kind of relation we want: "OneToMany" , "ManyToMany", etc.

![](images/image-2.png)

In our case is "OneToMany" because each Course can have many Student, but each Student relates to one Course

![](images/image-3.png)

Since it is going to modify the Entity of Student, it want to know what it will be the new name of that property (default is course) and that is good to me

![](images/image-4.png)

Next, it will ask us if we want the "student.course" to be nullable; well, this is a tricky question, because this could make some problems if you already have data in your database. If you already have students in your database and those students do not have a course yet, you will have a problem during migrations.  
For the moment i set no, but im counting on erasing all datas before insert new data in migrations

Now it will ask us if we want to delete all the Entity Student that don't have a Course, we say No! It update Course and Student entities... Let's have a look:

In "course.php" we have this :

```
public function __construct()
{
    $this->students = new ArrayCollection();
}
```

An `ArrayCollection` is an object created by the framework, that add more functionalities to a simple array.  
Also, **a collection is an array containing objects**!

In our entity "Student.php" we have this :

```
    /**
     * @ORM\ManyToOne(targetEntity=Course::class, inversedBy="students")
     * @ORM\JoinColumn(nullable=false)
     */
    private $course;
```

This is the new part for the entity, and this will be the "foreign key" that wil make the link with our "course" table

But we also have this :

```
public function addStudent(Student $student): self
    {
        if (!$this->students->contains($student)) {
            $this->students[] = $student;
            $student->setCourse($this);
        }

        return $this;
    }
public function removeStudent(Student $student): self
    {
        if ($this->students->removeElement($student)) {
            // set the owning side to null (unless already changed)
            if ($student->getCourse() === $this) {
                $student->setCourse(null);
            }
        }

        return $this;
    }
```

With this code we can add or remove a student to a course, this could be handy when making fixtures :)

Now we can make a migration! (but first I will delete all data in database)

```
 php bin/console make:migration
```

A new migration file is generated, now we can migrate :

```
php bin/console doctrine:migration:migrate
```

Now all the changes are in the database.

We need to create now some fixtures to add the info... I will not write here all the fixtures for this, just one to make you understand how you connect a student to a class.

```
$new_student = new Student();
        $frontEndCourse = new Course();
        $frontEndCourse->setLabel("front end");
        $manager->persist($frontEndCourse);
$new_student->setFirstName("Simone")
            ->setLastName("Panebianco")
            ->setCourse($frontEndCourse);
```

## DATABASE RELATION - MANY TO MANY

Now that we have a course for each student, we want to add another kind of relation in our database: "many to many"! This is the kind of relationship you have for example if you consider professors: each student have more than on professor, and each professor have more than one student :)

Let's have a look, on how we want to implement this scenario inside Symfony!

For this scenario we will need two more tables, one table to list all the professors and another table to connect the professor to a student (literally 2 columns inside this table "professor\_id" and "student\_id")

As before the first thing we want to do is create a new Entity for our Profressors list:

```
php bin/console make:entity Professor
```

Our professors will have just a single column that will be "name" (String 255 not nullable).

The next column of our Entity will be the relationship column, and I'll call it "students" (plural) because this will contains all the list of student liked to our professor.

So we say that we want a relation, the wizard will pop up, we then say we want to connect to the class Student, and then we select ManyToMany.

It will ask us if we want to be able to access to "`getProfessors()`" a function that will return us all the professor of a student, and of course we want this option, so we say YES!

Then it will ask how should the parameter be called inside the student class, we leave it named professors

Our entities have been generated, we have some new functions :

```
//Entity Professor
public function addStudent(Student $student): self
    {
        if (!$this->students->contains($student)) {
            $this->students[] = $student;
        }

        return $this;
    }

    public function removeStudent(Student $student): self
    {
        $this->students->removeElement($student);

        return $this;
    }
```

```
 /**
     * @return Collection|Professor[]
     */
    public function getProfessors(): Collection
    {
        return $this->professors;
    }

    public function addProfessor(Professor $professor): self
    {
        if (!$this->professors->contains($professor)) {
            $this->professors[] = $professor;
            $professor->addStudent($this);
        }

        return $this;
    }

    public function removeProfessor(Professor $professor): self
    {
        if ($this->professors->removeElement($professor)) {
            $professor->removeStudent($this);
        }

        return $this;
    }
}
```

So now we can add a student to a professor but also add a professor to a student :)

So now we make a migration, and then run it!

```
php bin/console make:migration
php bin/console doctrine:migrations:migrate
```

Let's have a look at our database:

![](images/image-5.png)

We have 5 tables, one more table that is for our many to many relationship, the table is called "professor\_student" :

![](images/image-6.png)

Now we need to create the fixtures for this, but we have a little dilemma here: what do we create first ? Professors or Student ? Both are correct this is up to you, you can create first the professors and then create the students and add to the students the professors or vice versa.

```
$alberto = new Professor();
$alberto->setName("Alberto");
$manager->persist($alberto);

$luca = new Professor();
$luca->setName("Luca");
$manager->persist($luca);

$new_student = new Student();

$new_student->setFirstName("Simone")
            ->setLastName("Panebianco")
            ->addProfessor($luca)
            ->addProfessor($alberto);
```

This way we can create the database!
