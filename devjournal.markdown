<!-- 
vim: ft=ghmarkdown 
-->

## 2014-01-19 14:12 Sunday

This is a test for a new way to store the dev journal after loosing all my work on UbuntuOne

## 2014-01-19 14:13 Sunday

Looks like it's working.

  - Install SparkleShare from the repo (I got v0.9.0 though 1.2 is available
      on the site)

  - It creates a new key pair and puts the public one in `~/SparkleShare`

  - Install this key with Github to enable Sparkleshare.


Here, for reference, is WIP on a Vimscript version of the code that adds
time-stamp headers to this file.

``` VimL
    " Note
    "

    if exists('Note_loaded')
        delfun Note_add
    endif

    function Note_add()
        let l:date = '## ' . strftime('%F %A')
        let l:time = '### ' . strftime('%R')
        let l:lastline = line('$')
        call append(l:lastline, ['', l:time, '', ''])
        if !search(date,'w')
            call append(l:lastline, ['', l:date])
        endif
    endfunction

    let Note_loaded = 1
```

- Maybe its time to turn this into a vimscript so it can  be invoked from
  inside vim.

- Now we're using Sparkleshare, we don't use closing the editor as a trigger
  for syncing, just saving.  (Actually that was the case with Ubuntu One as
  well) but there is no reason not to edit this file from within a larger gvim
  session, since any save will result in it being saved. So the external entry
  point -- from the shell or via key-binding -- could try and open this file in
  a tab in an existing gvim server if possible. 

- In fact if it uses gvim server, we can still use the external key binding and
  if there's a bit Gvim session running somewhere it would just spawn a new tab
  with this file in. And if it already has it open, it should append the
  datestamp. Might need a bit of tuning.

# v4c

## 2014-02-06 14:29 Thursday

 * Use a backbone router -- to trigger different entry point 
   functions .

 * Create overview and results v-pages in sndeparate folder
 pages/result.js

    create the right result from the results collection
    and use it to instantiate a view to present it

 pages/overview.js

 * Main would use a router to call the main function from the
   apropriate page.

 * Main would load the relevant data from Aptivate.data into
 collections 


* in principle when creating a result, we could also create an empty
  indicator  at the server side (because result without indicator is not useful)

## 2014-02-19 20:01 Wednesday


table
    thead
    tbody
        subindicator list -- row
            subindicator container : first col, subsequent columns


## 2014-03-19 01:21 Wednesday

Load bluetooth sound devices once paired:

     pactl load-module module-bluetooth-discover 

## 2014-04-19 Saturday

### 22:09

Working on note.sh again. 
USe vim function 

### 22:15

Yay, it works?

### 22:18

* Made a vim plugin for the function
* Use Compiz command addon to bins Alt-Shift-N (Same as in Xmonad)
* Updated freshrc.bitterjug to fetch the version of note.sh from bitterjug/vim-notebook instead of the old one

## 2014-07-16 Wednesday

### 21:59

Looking at the source of Freeplane 1.4.1 alpha.  The functions called by arrow
keys to select up,down, left, right are `selectUp(...)`, `selectDown(...)`,
etc.  The bindings are defined in
`/freeplane/freeplane/src/org/freeplane/view/swing/ui/DefaultNodeKeyListener.java`,
and I expect the reason they have not been made available in the interface is because
they take a paraneter 'continuous' which appears to be se when shift is held down
to extend the selection rather than just change it. 

I think I could 

a) just hack the source to bind `hjkl` at a low level to these as well as to
the arrow keys which is a hack, but would make me happy until they change this
source.

b) Write a plugin that exposes these functions to Groovy -- maybe Groofy can
already call them?  You'd need to know if shift was being held down, I mean I'd
need to bind the shift version separately but that should be possoible.

### 22:43

The swing map view component doesn't appear in the current proxy interface
but maybe I can just call it from Groovy?


### 22:48

This appears to do what I want:

```groovy
import org.freeplane.features.mode.Controller;

mapView = Controller.getCurrentController().getMapViewManager().getMapViewComponent();

mapView.selectDown(false)
```


## 2014-10-02 Thursday

### 21:20

CMS Made Simple-- separate News categories for Events

cmsms in `~/.tmp/`

Mysql db: cmsms/cmsms

add 'cmsms.localhost' to /etc/hosts and add cmsms.conf to /etc/apache2/sites-available

Install site.

    {news number='5' category='Events'}

Note that `doc/htaccess.txt` should be installed as a `.htaccess` to enable browse mage caching.

* [ ] Add custom fields
- Day Month Year, etc.
* [ ] Create a new default template which does not render custom fields.
* [ ] Set that as default
* [ ] Create a new template that renders the events how you want them



## 2014-11-10 Monday

### 12:21

Can't get ipython to install in a ve that I just made with 

    virtualenv -p `which python3.4`

Even if I do:

    pip3.4 install ipython

When I run ipython after installing it I get 2.7.6.

    tdd~/workspace/tdd➔ which ipython
    /home/mark/workspace/tdd/ve/bin/ipython

But it sill starts 2.7.6

    ➔ ipython
    WARNING: Attempting to work in a virtualenv. If you encounter problems, please install IPython inside the virtualenv.
    Python 2.7.6 (default, Mar 22 2014, 22:59:56)
    Type "copyright", "credits" or "license" for more information.

    IPython 1.2.1 -- An enhanced Interactive Python.
    ?         -> Introduction and overview of IPython's features.
    %quickref -> Quick reference.
    help      -> Python's own help system.
    object?   -> Details about 'object', use 'object??' for extra details.

    In [1]:


### 12:27

Duh! Fixed by running:

    tdd~/workspace/tdd➔ ve/bin/ipython3

### 12:29

So now I'm running python3 I can get at _splinter_ which I installed earlier:

    506  echo `which python3.4`
    507  virtualenv -p `which python3.4` ve
    508  source ve/bin/activate
    509  python
    510  pip install splinter
      
In ipython3:

    In [1]: from splinter import Browser
    In [2]: browser = Browser()
    In [3]: browser.visit('http://google.com')

Opens a browser window and shows the google page.

### 12:46

So

    pip3 install django


### 22:07


Got BDD working with behave.

Weirdly `environment.py` has to be in the `features` directory instead of the steps.

And syntax highlighting or cucumber `.feature` files highlights 'Scenaro:' as if it were 'Scenario:'
which was frustrating.

So as of today, there is a test which fails on an assertion error if you start
a `manage.py runserver` manually because I haven't actualy written any django
yet.  So its looking for 'To-Do' in the browser title but it gets 'Welcome to
Django', or whatever.

Before doing that, however, I think it might be fun to try and get the django server running
automatically from the environment `before_all()'
