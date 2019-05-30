# Kayzor docs

## Installation instructions

Clone this repo **recursively**:
```sh
git clone --recursive https://github.com/KayzorPlatform/docs
```
Change the working directory to the repo:
```sh
cd docs
```
Run `hugo` to build the project:
```sh
hugo -D --baseUrl=http://docs.kayzor.com/
```

The built project it's in the `public/` directory.


### Why does it have to be recursively?
Because the theme it uses is a
[fork](https://github.com/dreamtigers/dot-hugo-documentation-theme) of
[dot-hugo-documentation-theme](https://github.com/themefisher/dot-hugo-documentation-theme/)
and it's added as a git submodule.


## Dependencies

This project depends on the `hugo` SSG, which you can get (in Debian based
distros) with:

```sh
sudo apt-get install hugo
```

## Organization

Easy enough, all the documents live inside the `content/` directory, which is
divided in subdirectories. The documents are related to one another through
Hugo's `menu` variable.

inside the `menu` variable there can be any number of items, defined like a
key-value dictionary. So the menu variable contains:
```
[menu]
    ├───main
    ├───api
    └───header
```

* The `main` item (or key) contains the order in which documentation is
  distributed.
* The `api` key contains the items displayed in the API section of the footer.
* The `header` item contains the fields displayed in the header of the site.

`api` and `header` are simple and are defined in the `config.toml` file.

In Hugo we have **(sub)categories** and **documents**:
* **categories** are data structures defined in `config.toml`, that can
  contain other subcategories or documents.
* **documents** are just .md files that have a **Front Matter** at the start of
  each document, with metadata about them. In the Front Matter is where we
  define to what category/subcategory the document belongs to.

**Categories** are defined in the `config.toml` file, like so:
```toml
[[Languages.en.menu.main]]
weight = 40
name = "admin"
url = "/admin"
hasChildren = true
```
If you're defining a **sub**category, you have to specify it's **parent**:
```toml
[[Languages.en.menu.main]]
parent = "admin"
name = "users"
URL = "/admin/users"
weight = 10
```

The categories have certain properties:
* The `weight` variable helps order the categories.
* The `name` is the name of the category.
* The `url` is how to access said category (p.e. www.docs.kayzor.com/admin).
* The boolean `hasChildren` determines if the category has
  subcategories.

A full view of how menus are organized:
```
[menu]
    ├───main
    │   ├───admin		<- Category
    │   │   ├───dashboard
    │   │   ├───users		<- Subcategory
    │   │   │   ├───2.1 List	<- Document
    │   │   │   └───...
    │   │   ├───roles
    │   │   ├───tickets
    │   │   ├───events
    │   │   ├───finance
    │   │   └───settings
    │   │       └───currencies
    │   ├───web
    │   │   ├───main-page
    │   │   └───user-panel
    │   └───reports
    │       ├───betting-dashboard
    │       ├───finance-dashboard
    │       └───user-dashboard
    ├───api
    │   ├───admin
    │   └───web
    └───header
        ├───home
        ├───admin
        ├───web
        ├───reports
        └───contacts
```

## Expanding documentation
Once we know that, we can:
* [Expand a Document]().
* [Add a Document]().
* [Add a Category]().

### Add a Category
For example, assume we have the current structure.
```
[menu]
    └───main
        └───admin             <- Category
            ├───dashboard     <- Document
            └───users         <- Subcategory
                ├───2.1 List  <- Document
		└───...
```
Let's say we want to add the category `example` as a child of `admin`:

1. Create a new directory inside `admin`:
```sh
mkdir content/admin/example
```
2. Edit a new document inside that directory:
```sh
vim content/reports/example/_index.md
```
Add a basic Front Matter:
```
---
title: "Example"
date: 2019-05-27T06:45:43-04:00
draft: true
---
```
3. Add the category to `config.toml`:
```toml
[[Languages.en.menu.main]]
parent = "admin"
name = "example"
URL = "/admin/example"
weight = 80
```

With that, we have a new category.

### Expand a Document
Expading documents is pretty straight forward, just find the files you want to
expand.

For example, we want to expand the "Forgot Password" document of "Main Page",
which is a child of `web`.

1. Go to `content/web/main-page/1.4\ Forgot\ Password.md` and edit it with your
   favorite text editor, you'll see:
```
---
title: "Forgot Password"
date: 2019-04-26T06:45:43-04:00
draft: true
menu:
  main:
    parent: "main-page"
    weight: 40
---

### 1.4 Forgot password

System allows to restore his password if user forgot his account. In order to
restore it user must enter his email address and restore his password
```

The content between the dashes (`---`) is called **Front Matter** and contains
metadata about the document, you can find more info about it in Hugo's
documentation, but in this example we can see:
* `title`: The title of the current document/page.
* `date`: The date of creation of the document.
* `draft`: A boolean indicating the document is not ready yet.
* `menu`: This is how we organize the hierarchy of the documents, in this case,
  we're saying that from the `menu` we go to `main`, and from `main`, we
  indicate that **this** document (`1.4\ Forgot\ Password.md`) is a child of
  `main-page`.
* `weight`: A variable that helps compare (and order) said item against others.

Where is `main-page` defined?

Because `main-page` is a subcategory, it is defined inside `config.toml`:
```toml
# WEB children
[[Languages.en.menu.main]]
parent = "web"
name = "main-page"
URL = "/web/main-page"
weight = 10
```
There we can see it's name, and it's parent.

2. So now we can freely expand our document:
```diff
- System allows to restore his password if user forgot his account. In order to
- restore it user must enter his email address and restore his password
+ The systems allow the user to restore their password if they forget it. In
+ order to restore it, the user must enter their email address, which will
+ recieve an email with further information on how to restore the password.
```

3. Commit and push the changes:
```sh
git add content/web/main-page/1.4\ Forgot\ Password.md
git commit -m "Update web/main-page documentation"
git push origin master
```

4. Rebuild the documentation:
```sh
hugo -D --baseUrl=http://docs.kayzor.com/
```

And your changes should be in the `build/` directory.

### Add a Document
Adding new documentation is very similar to expanding it.

Let's say we want to add a document about Deleting a user:
```
[menu]
    └───main
        └───admin
            ├───dashboard
            └───users
                ├───2.1 List
		├───...
                └───2.5 Delete user <- This is what we want to create
```
1. Create a new document inside the directory of it's parent:
```sh
vim content/admin/users/2.5\ Delete\ user.md
```

2. Add a basic Front Matter:
```
---
title: "Delete user"
date: 2019-05-27T06:45:43-04:00
draft: true
menu:
  main:
    parent: "admin"
    weight: 50
---

### 2.5 Delete user

Lorem ipsum dolor sit amet, consectetur adipiscing elit.
```

And with that, you have a new document.
