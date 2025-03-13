---
title: "Symfony Forms"
date: "2021-08-04"
---

This is another big step in learning Symfony because one of the most important things in a web app is filling forms (for CRUD operations).

Symfony has it philosophy when it comes to creating and manipulating forms, they get you all the tools you may need to create a form and to handle it. We will start using their method, and then I will attempt to understand how to use normal HTML forms and how to handle everithing.

## THE SYMFONY WAY

The recommended workflow with Symfony forms is :

1. **Build the form** in a Symfony controller or using a dedicated form class;
2. **Render the form** in a template so the user can edit and submit it;
3. **Process the form** to validate the submitted data, transform it into PHP data and do something with it (e.g. persist it in a database).

Forms are linked to the entities so that if you want to create a form you just say for what entities the form is, and it will automatically create all the needed code.

We will try to create a form to modify a filiere entity (inside the databse). This table/entity has un ID (standard) a UID (just a string) and a name (String)

The command to create a form is `php bin/console make:form`  
Then it will ask us the name of the form (normally you use the entity name + "Type") and the entity that we want to use for this form.  
In my case I'm trying to create a form to fill a filiere table.

If you look at your files now, you have a file inside `src/Form/` called `FiliereType.php`

As you will see, you can not modify the ID of an entity, this is not possible by default.
