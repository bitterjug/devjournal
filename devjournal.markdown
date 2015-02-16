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
automatically from the environment `before_all()`

## 2014-11-11 Tuesday

### 21:32

Or maybe that's not the way to do it. We have `django_behave` which is supposed
to handle this.  It wants us to add itself to installd apps. And create
`features` directories in the apps.  So lets give that a try.

### 21:42

After the fiile wrangling we get na interesting exception:


    tdd~/workspace/tdd/superlists➔ ./manage.py test superlists
    Traceback (most recent call last):
      File "./manage.py", line 10, in <module>
        execute_from_command_line(sys.argv)
      File "/home/mark/workspace/tdd/ve/lib/python3.4/site-packages/django/core/management/__init__.py", line 385, in execute_from_command_line
        utility.execute()
      File "/home/mark/workspace/tdd/ve/lib/python3.4/site-packages/django/core/management/__init__.py", line 377, in execute
        self.fetch_command(subcommand).run_from_argv(self.argv)
      File "/home/mark/workspace/tdd/ve/lib/python3.4/site-packages/django/core/management/commands/test.py", line 50, in run_from_argv
        super(Command, self).run_from_argv(argv)
      File "/home/mark/workspace/tdd/ve/lib/python3.4/site-packages/django/core/management/base.py", line 284, in run_from_argv
        parser = self.create_parser(argv[0], argv[1])
      File "/home/mark/workspace/tdd/ve/lib/python3.4/site-packages/django/core/management/commands/test.py", line 53, in create_parser
        test_runner_class = get_runner(settings, self.test_runner)
      File "/home/mark/workspace/tdd/ve/lib/python3.4/site-packages/django/test/utils.py", line 144, in get_runner
        test_module = __import__(test_module_name, {}, {}, force_str(test_path[-1]))
      File "/home/mark/workspace/tdd/ve/lib/python3.4/site-packages/django_behave/runner.py", line 152
        except ParserError, e:
                          ^
    SyntaxError: invalid syntax

Which looks like pre python 3 syntax. So is _django_behave_ python 3 compatible?

### 21:55

https://github.com/django-behave/django-behave says its tested with Python3.4, so what's going on?

### 22:01

Perhaps a different version from pypi?
Installed: django_behave-0.1.2-py3.4.egg-info

So pip uninstalled and tried again with pip3.4 which gave the same error on install.
So I'm fetching the version from the git repo:

    pip3.4 install git+https://github.com/django-behave/django-behave.git

Seems to have worked after second attempt.

Once I added 'superlists' to INSTALLED_APPS it complained that my app didn't have a models module
so created empty `models.py` and the test runs.

Interestingly, I don't get a server to play with out of the box. 

Do I want a server? The Django test runner creates some kind of fake server.
Maybe I still want some code in `environment.py` to start a test server.
You can learn how to do this here: https://pythonhosted.org/behave/django.html?highlight=django


## 2014-11-12 Wednesday

### 21:45

Followed along with [this chapter](http://chimera.labs.oreilly.com/books/1234000000754/ch03.html#_the_unit_test_code_cycle)
creating unit tests but using my BDD as the functional test.

Im concerned that we still haven't added the `lists` module as an `INSTALLED_APPS` entry.



## 2014-11-15 Saturday

### 21:39

Copying the functional tests from chapter 4, we find the browser object from splinter has different methods.
Notably:


      AttributeError: 'WebDriver' object has no attribute 'find_element_by_tag_name'

### 21:44

As explained in the [splinter documentation](http://splinter.cobrateam.info/en/latest/finding.html), the available options are:

    browser.find_by_css('h1')
    browser.find_by_xpath('//h1')
    browser.find_by_tag('h1')
    browser.find_by_name('name')
    browser.find_by_id('firstheader')
    browser.find_by_value('query')



### 21:46

Oh! I got ahead of myself, Should have done the refactor to use template first!

## 2014-11-20 Thursday

### 20:09

Updating a lost wordpress password.

    update wp_users set user_pass = MD5('new-pw') where id = 1;

### 22:45

Here's how to  zip up a wordpress theme.
From inside the theme's git repo directory:

    git archive --format=zip --output ../Xenonium.zip HEAD --prefix=Xenonium/

## 2014-11-30 Sunday

### 18:54

Notes on DArk Themes
Installed Ambiance Dark via nooblabs ppa

      sudo add-apt-repository ppa:noobslab/themes
      sudo apt-get update
      sudo apt-get install ambiance-dark-red
 
This looks pretty good.  We still have a problem with the software centre,
there are css fixes for this but for the moment I'm trying out appgrid which
doens't seem to have the problems.

Firefox tabs were a bit odd until I restarted.

Duck Duck Go had light text  on a white background but this is solved with a
dark DuckDuckGo theme.

Vimperator looks much better with zenburn theme selected.This isn't set as
default yet; which I might change. I wonder how do that.  Proably in
`.vimperatorrc`.

    colorscheme zerburn 

Did the trick. But it assumes zerburn theme is available. Might need to check
my version of that into version control.

## 2014-12-29 Monday

### 17:51

How to do mDNS lookups with DIG:

    dig @224.0.0.251 -p 5353  musicbox.localhost

Awesome!

## 2015-01-03 Saturday

### 09:29

Ideas about virtualising and isolating my laptop from dev environment on new pc.
* [VAGRANT](http://docs.vagrantup.com/v2/provisioning/docker.html)
* [Docker](https://www.docker.com/)
* [Fig](http://www.fig.sh/django.html)

## 2015-01-18 Sunday

### 16:36

- [ ] Make 'gst' Git status (using vim) alias more robust when not in a git repository.

### 17:43

Fix for Bootstrap Navbar borders: add `navbar-fixed-top` to the top one to keep
it not scrolling, and `navbar-satic-bottom` to the bottom one to fix it to the
bottom of the page. See [The docs](http://getbootstrap.com/components/) for details.

## 2015-01-21 Wednesday

### 21:33

__Meslo LG L DZ__ Is a great looking font for programming with even more vertial space than Source Code Pro

## 2015-01-24 Saturday

### 08:25

Unite for vim
http://eblundell.com/thoughts/2013/08/15/Vim-CtrlP-behaviour-with-Unite.html

### 20:49

# MPD config on Revo

It won't work with pulseaudio out of the box because it runs as system service. 
So I want to make it run as user level service.

- how to disable the system service? I only know how to use BUM.  I'm going to
  run Bum (remotely via ssh -Y)

  Find mpd in the list and uncheck the box. Press apply and choose yes to apply changes.

  And for good measure, as per [instructions](https://help.ubuntu.com/community/MPD#Configuring_MPD_to_run_as_a_user_service)

    sudo update-rc.d mpd disable

And here is a [list of alternatives](http://askubuntu.com/questions/19320/how-to-enable-or-disable-services/20347#20347)
for starting and stopping services. `jobs-admin` appears to be the winner.

Now, how to make it start when `mark` logs in?

### 21:24

I already have a `.mpd/` directory with some settings in.

### 21:36

Create a new config file as ~/.mpd/mpd.conf by extrating the example one:

    gunzip -c /usr/share/doc/mpd/examples/mpdconf.example.gz  > mpd.conf

And uncoment pulse audio (And other settings).


### 21:49

Turns out the services that start for me as a user, in ubuntu, are desktop
files stored in:

    ~/.config/autostart/xcape.desktop

Now, does that work on mythbuntu?

### 22:52

Fucking about with the config I keept geting:

    server reports failed to open audio output

Guess what! Mythbuntu doesn't install pulse.

Hmm. So do I want mpd to play through pulse on Revo?..



## 2015-01-25 Sunday

### 10:43

michelle  at key travel 1640 e


### 20:53

Tried to install [beets](http://beets.readthedocs.org/en/v1.3.10/guides/main.html)
the apt-get repo has 1.3.1 but 1.3.10 is current. Pip install beets gave
errors. So something is broken, try again.

### 21:59

Turns out the version of `six` was too old. Installed now. Could try and organize 
the revo music with it.

## 2015-01-29 Thursday

### 23:32

Having switched to lightline from powerline, I'm kinda wondering how to 
get the right components of the vimfiler window to collapse.

## 2015-01-30 Friday

### 09:09

Having just added Tmuxline.vim to vim (!!!) I'm wondering about using
`NeoBundleLazy` to load all the markdown and other weird types. Maybe
only load pymode for python files, imagine!



### 09:10

And for collapsing the _vimfiler_ status bar coponents, I think thebest
solution was custom `percent` and `lineinfo` parts that are empty 
for _vimfiler_, and then setting them to their defalt valus otherwise,
but then we need to register those new keys as reqiring expansion
to insert the details.

## 2015-02-13 Friday

### 22:26

Set up tmuxline settings for tmux permanently with a command from vim:

:TmuxlineSnapshot[!] [file]                              *:TmuxlineSnapshot*
  Create a file holding tmux statusline configuration. The file can be sourced
  by your tmux, typically in your tmux.conf. The command must be executed
  after |tmuxline| has set tmux's statusline, i.e. after executing
  |:Tmuxline|. The file will not be overwritten unless bang [!] is given.


## 2015-02-16 Monday

### 21:21


Note
    "NeoBundle 'Rykka/riv.vim' "Restructured text

They reverted the terrible clickable thing, go check it out again.

### 21:51

Here's something else to try out: Vim in the browser

    https://github.com/ardagnir/pterosaur#vim-info
