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

## Documents organization

So you want to expand on the documentation, how do you do so? Easy enough, all
the documents live inside the `content/` directory and is divided in
subdirectories. The documents are related to one another through Hugo's `menu`
variable.

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

`api` and `header` are relatively simple and are defined in the `config.toml`
file.

`main` is a little more complex. it has three direct children items (admin, web,
reports) which are defined in `config.toml`, like so:

```toml
[[Languages.en.menu.main]]
weight = 40
name = "admin"
url = "/admin"
hasChildren = true

[[Languages.en.menu.main]]
weight = 50
name = "web"
url = "/web"
hasChildren = true

[[Languages.en.menu.main]]
weight = 60
name = "reports"
url = "/reports"
hasChildren = true
```
So the above toml defines that the `menu.main` is an array that contains three
items (could be viewed as objects). Each object is therefore a direct child of
`main`, and they have certain properties:
* The `weight` variable helps order the items.
* The `name` is the name of the item/category.
* The `url` is how to access said category (p.e. www.docs.kayzor.com/admin).
* The boolean `hasChildren` helps to determine if the category has
  subcategories.

A full view of how menus are organized:
```
[menu]
    │
    ├───main
    │   ├───admin
    │   │   ├───dashboard
    │   │   ├───users
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
    │       └───
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

Once we know that we can expand, reorder, or add new documents.

## Expanding documentation

Expading documents is pretty straight forward, just find the files you want to
expand.
For example, we want to expand the "Forgot Password" document inside "Main Page"
which, in turn, lives in "Web".

Go to `content/web/main-page/1.4\ Forgot\ Password.md` and edit it with your
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
weight: 40
---

### 1.4 Forgot password

System allows to restore his password if user forgot his account. In order to
restore it user must enter his email address and restore his password
```

The content between the dashes (`---`) is called **Front Matter** and you
contains metadata about the document, you can find more info about it in Hugo's
documentation, but in this example we can see:
* `title`: The title of the current document/page.
* `date`: The date of creation of the document.
* `draft`: A boolean indicating the document is not ready yet.
* `weight`: A variable that helps compare (and order) said document against
  others.
* `menu`: This is how we organize the hierarchy of the documents, in this case,
  we're saying that from the `menu` we go to `main`, and from `main`, we
  indicate that **this** document (`1.4\ Forgot\ Password.md`) is a child of
  `main-page`.

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

So now we can freely expand our document:
```diff
- System allows to restore his password if user forgot his account. In order to
- restore it user must enter his email address and restore his password
+ The systems allow the user to restore their password if they forget it. In
+ order to restore it, the user must enter their email address, which will
+ recieve an email with further information on how to restore the password.
```

Commit and push the changes:
```sh
git add content/web/main-page/1.4\ Forgot\ Password.md
git commit -m "Update web/main-page documentation"
git push origin master
```

Rebuild the documentation:
```sh
hugo -D --baseUrl=http://docs.kayzor.com/
```
And your changes should be in the `build/` directory.

### Adding documentation
Adding new documentation is very similar to expanding it.
First, we create a new document, in this case, let's add an example dashboard to
"Reports":

Create a new directory inside reports:
```sh
mkdir content/reports/example-dashboard
```
Edit a new document inside that directory:
```sh
vim content/reports/example-dashboard/_index.md
```
Add a basic Front Matter:
```
---
title: "Example Dashboard"
date: 2019-05-27T06:45:43-04:00
draft: true
menu:
  main:
    parent: "reports"
    weight: 40
weight: 40
---

### 1.4 Example Dashboard

Lorem ipsum dolor sit amet, consectetur adipiscing elit.
```

And with that, you have a new subcategory to ready to document.
