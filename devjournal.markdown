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

## 2015-02-24 Tuesday

### 10:02


# TDD

Going back to this project after some time, how the hell do you run the tests?
Maybe I should have written some documentation?

### 10:10

There's a readme now. What to put in it?

There is a django app in here somewhere.

First I activat the virtual env in /tdd/ve

In 
    /tdd/superlists

I did

    ./manage.py test superlists

Which runs the functional tests.

They fail. They were'nt set up properly. Having shuffled the directories about
a bit we find they run when behave can find all the steps and stuff. And then
they complain becuse there is no server running for them to connect to.

### 10:50

I want to use
[LiveServerTestCase](https://docs.djangoproject.com/en/1.7/topics/testing/tools/#django.test.LiveServerTestCase)
to fix this. 

Ther's a kind of race condition here because in order to get a
live server you have to have done some django setup first. I can see how I the
book this isn't apropriate but for me I thnk its okay if we steal  a working
django system and get tests to run. I'll steal from my old project.

Could try and do it the hard way folowing this [example from behve docs](http://pythonhosted.org//behave/django.html?highlight=django)

### 11:06

Or maybe not. LiveServerTestCase might not be the best solution.  And behave
has its own test runner.

In a package called `django-behave` which apparently we have installed in the
app.

### 11:14

There is a file [here](https://github.com/django-behave/django-behave/blob/master/django_behave/splinter.steps_library.py)
with steps for setting up Splinter! Let's have a look at those.

They're not as interesting as we hoped.

### 12:39

Set up the `django-behave` test runner.

This internally uses `LiveServerTestCase` !

And it sets the `server_url` in the Behave Configuration object.  Which our tests
can access from the steps becausethe `configuration` object is set on the
`context` object. So in my `environment.py`  in `before_all`, I'm setting

    context.home_url = context.config.server_url

Now the browser starts, it finds the test server set up for the test case and
we get a url not found error. Which is probably correct because I haven't copid over the
url config from the old project  yet.

- [x]  copy over old url config

### 12:45

- [ ] Should these journals be linked to or contained within the current project instead of global?
  - better for  keepingthe thread of deelopment together
  - worse for having all your wisdom collected togther in the same place

  - Maybe some kind of compromise using, e.g. RIV that can remember the last project specific file 
  we were working on and open that by default when we hit the note taking command key.

## 2015-02-28 Saturday

### 22:17

Try out Vim [ControlSpace](https://github.com/szw/vim-ctrlspace#the-idea)

### 22:39

Checkout using `augroup` in vim
see https://github.com/szw/dotfiles/blob/master/vimrc

## 2015-03-01 Sunday

### 20:58

Here's a plugin to make vim jump to the step for a line in celery like behave file

    https://gitorious.org/cucutags/vim-behave/

### 21:31


One thing about using the double loop TDD approach is that it is kinda okay to
insert some bloody silly code that makes a unit test pass, because the
functional test should still be failing. Actually, I'm not sure what the reason
for wanting to insert a trivial pass code is, because you're going to have to
take it out anyway. Perhaps to understand exactly how the test works and what
it tests for (and what it doses not).


### 22:52

Bastart Splinter doesn't appear to handle keystrokes the same way as selnium.
So I have no idea how you type really complex stuff, but to type "Enter"
atthe end of in put where I Wanted that action to submit the form, I finally
tried ading "\n" to the input string, and it worked.

## 2015-03-02 Monday

### 21:06

The other reason for wanting to make quick changes to make the tests pass
is the Red-Green-Refactor thing. You can make it pass then refactor to a
more reliable soltion you're going green to green.

### 21:39

[Here][1]'s an interesting tip for getting your virtual env active inside vim (as
opposed to activating it before you start vim.)

[1]:http://www.sontek.net/blog/2011/05/07/turning_vim_into_a_modern_python_ide.html#virtualenv

### 22:13


" VimFiler
    noremap <Leader>f :VimFilerExplorer -toggle<cr>
    "Open  if no file argumentsNERDTree
    autocmd VimEnter * if !argc() | VimFiler | endif
    " use inplace of netrw
	let g:vimfiler_as_default_explorer = 1
    " Ignore dotfiles and .pyc files.
    let g:vimfiler_ignore_pattern = '^\%(\..*\|.*\.pyc\)$'
	" Like Textmate icons.
	let g:vimfiler_tree_leaf_icon = '╰─'
	let g:vimfiler_tree_opened_icon = '─▾'
	let g:vimfiler_tree_closed_icon = '─▷'
	let g:vimfiler_file_icon = '─╴'

    let g:vimfiler_readonly_file_icon ='─╸'
	let g:vimfiler_marked_file_icon = ' ✔'
    let g:vimfiler_tree_indentation = 3
	" let g:vimfiler_tree_closed_icon = '▸'
	"let g:vimfiler_file_icon = '╼'

### 22:15



## 2015-03-09 Monday

### 23:16

- [ ] Make neocomplete use Jedi completion as, e.g. omnicomplete function

## 2015-03-10 Tuesday

### 00:04

Would it be possible to use `LiveServerTestCase` to roll back the transactions between scenarios?

## 2015-03-13 Friday

### 10:39

# TDD in Python

Page 83 of the TDD in Python book; REST would have us use a PUT request to create a new list.
[Turns out](http://stackoverflow.com/questions/1856996/doing-a-http-put-from-a-browser) 
you can't easily send POST requests from HTML forms, but you can with Javascript/AJAX.

### 11:12

- [ ] How about a tmux binding for ^a-k that takes us into edit mode and does a k=up action?:

### 11:16

- [ ] Fork django-behave and add the current test case object to context so I
  can access it in the steps to use assertXxxxx methods

### 11:19

# TDD in Python

Page 85: Use new browser session for new user.  How to do thisin Behave? Should
we quite the browser between scenarios?  And make thisa new scenario. It
doesn't feel like a new feature to me.

The previously unused "Given a User" step now opens a new browser instance.
And `after_feature()` tries to close any open `context.browser` .

### 11:45

# Vim

- [ ] Hide ve files and ropeproject from Unite Ctrl-P file list

### 12:10

- [ ] My laptop seemsto have forgotten to open my ssh key when I log in.
How do I add that to the keyring again?

## 2015-03-17 Tuesday

### 19:58

# Vim

Is `vim-css-color`  plugin broken?

    /home/mark/.vim/bundle/vim-css-color/after/syntax/vim.vim, line 6
    Vim(call):E118: Too many arguments for function: css_color#init
    Error occurred while executing "open" action!
    Press ENTER or type command to continue

No. Just me expecting new stuff to work without restarting vim :(

### 21:00

Note: We can probably make the percent and line info collapse
for vimfiler windows by using `component_expand` instead/as well as
`component_function` in lightline config



### 22:01

# Exaile

Could make it an option whether to apply the moodbar to the main or secondary
output device?

To make moodbar work on the secondary playback device, the moodbar needs to
know about:

- The progress bar ui component to replace (stored as `setlf.pr`) in the
  `ExModbar` object. For example: `ExModbar.changeBarToMod()` refers to
  `self.pr` for the progress bar. Which is set in `setupUi()` in reference to
  `self.exaile`, assuming there to be a single `progress_bar`:

    self.pr=self.exaile.gui.main.progress_bar

  Perhaps `setupUi()`, or an explicit setter method, should take a parameter
  and set up the progress bar instance to attach to.

- The player object whose events to respond to.  It refers explicityly to the
  primary player when, for example adding callbacks for track start and end:

``` python
    event.add_callback(ExaileModbar.play_start, 'playback_track_start', player.PLAYER)
    event.add_callback(ExaileModbar.play_end, 'playback_player_end', player.PLAYER)
```

  In `_enable()`, and similarly when removing them in `disable()`

  Perhaps the `ExModbar()` object should have a method to do these two calls,
  referring to the player object for the instance of the plugin.

  There is currently a `set_ex(exaile)` method which sets `self.exaile` which
  is used _only_ to refer to the progress bar.

  So I propose: 

   - remove `set_ex and` replace with params to init

``` python


    def enable(exaile):
        global ExaileModbar
        ExaileModbar=ExModbar(
            progress_bar=exaile.gui.main.progress_bar,
            player=player.PLAYER
        )
    ...

    def _enable(eventname, exaile, nothing):
        global ExModbar
        ExaileModbar.readMod('')
        ExaileModbar.setupUi()
        ExaileModbar.setCallbacks()  

    ...

    ExaileModbar...

        def setCallbacks(self) 
            event.add_callback(self.play_start, 'playback_track_start', self.player)
            event.add_callback(self.play_end, 'playback_player_end', self.player)

```

  Inside the `ExModbar` object there are several references to `player.PLAYER`
  that could be replaced with references to, e.g. `self.player`.


## 2015-03-21 Saturday

### 20:40

- [ ] Add .pyo to hidden files for vimfiler


### 21:47

Exaile blows up.  0.3.4 beta2, up to my current version. (i.e. it did it before I started hacking it)
Start a track runing, go to Preferences Plugins Gui, and turn on and off the moodbar a couple of times.
Then close the prefs plugin.  Endlessly repeating:

    Original exception was:
    RuntimeError: maximum recursion depth exceeded
    Error in sys.excepthook:
    RuntimeError: maximum recursion depth exceeded

What's going on? How can I get more data?

### 21:51



Meanwhile stage one of my refactor doesn't seem to have broken anything.
The next step is going to be a strategy for adding it to the preview device.

There seem to be two dependencies: the

#### player object. 
 - The default exaile one is: `player.PLAYER`
 - The preview one is: `previewdevice.PREVIEW_PLUGIN.player` IF the preview
   plugin is activated.  and 

#### progress bar within the gui
- the default exaile one is `exaile.gui.main.progress_bar` which is a
  `SeekProgressBar` object.
- the preview device plugin creates one of these and, as of my recent change,
  stores it in `previewdevice.PREVIEW_PLUGIN.progress_bar`


### 23:51

Ouch! What a headache?
Apparetly at some point I installed a user-local copy of exaile and got local copies of the plugins dirs
in `~/.local/share/exaile/plugins`. Deleting this directory means that whe I run `./exaile` in 
`workspace/exaile` it tries to run the version of the plugins I have been messing with, not the
ones from before.


## 2015-03-22 Sunday

### 00:06

So we should now be able to make the setup do install on defalt player, 
Check if the preview device is loaded. If so install on that too.

Then we need to 
- [ ] add setup and tear down events to the preview device,  something like:


    def enable(exaile):
        global ExaileModbar
        ExaileModbar = ExModbar(
            player=player.PLAYER
            progress_bar=exaile.gui.main.progress_bar,
        ) 
        import previewdevice ## do we need to check if this is defined?
        if previewdevice.PREVIEW_PLUGIN:
            global ExailePreviewModbar
            ExailePreviewModbar = ExModbar(
                player=progress_bar.PREVIEW_PLUGIN.player,
                progress_bar=previewdevice.PREVIEW_PLUGIN.progress_bar
            )

- [ ] and then attach the moodbar to those to attach when it gets created.
- [ ] check for external dependancies like the file name the command line tool
  writs to.

We need to be able to creat the preview moodbar object lazily 

### 19:51

# Vim

- [ ] Enable spell checking by default in vim windows of type `gitcommit`

### 19:52

# Exaile

So now Preview Device sends signals when it is enabled or disabled.

Next is to make the moodbar attch to it.  Based on last night's ideas, I'm
currently thinking:

    
    def _enable(exaile):
        enableMainMoodbar(exaile)

        event.add_callback(
            enablePreviewMoodbar,
            'previewdevice_enabled'
        )

        event.add_callback(
            disablePreviewMoodbar,
            'previewdevice_disabled'
        )


        

### 20:09

# Vim

- [ ] `set formatoptions+=t` in mardown buffers

### 21:00

# Exaile

### 21:00

To get the preview plugin at any point in time, I'm thinking something like this:


    try:
        import previewdevice
        preview_plugin = previewdevice.PREVIEW_PLUGIN
    except ImportError:
        preview_plugin = None

    if preview_plugin:
        enable_preview_moodbar(preview_plugin)

### 23:49

So the thing seems to be working, except that if you manually enable the
preview device after the moodbar, it doesn't get decorated. I wonder why. I
have the feeling the callbacks aren't getting called, which is annoying because
they were back when all they did was call logging messages.  What changed?

Maybe write to the list and ask for some help.


## 2015-03-30 Monday

### 21:43

Configuring UPNP Streaming pulseaidio .,

1) install Rygel

2) Use `paprefs` NetworkServer, make local sound devices available as DLNA/UpNP Media server
  Create separate audio device for media streaming

3) From [Stack Overflow][http://stackoverflow.com/questions/15548586/streaming-media-files-via-dlna-upnp]
   use this to get the device name:

    pactl list | egrep -A2 '^(\*\*\* )?Source #' | grep 'Name: .*\.monitor$' | awk '{print $NF}' | tail -n1

   Whic for me was `upnp.monitor`.


4) in ~/.config/rygel.conf:

    [GstLaunch]
    enabled=true
    launch-items=mypulseaudiosink

    mypulseaudiosink-title=Audio on @HOSTNAME@
    mypulseaudiosink-mime=audio/flac
    mypulseaudiosink-launch=pulsesrc device=upnp.monitor ! flacenc

Except that it doesn't seem to work

### 22:01

From [here](https://wiki.gnome.org/Projects/Rygel/Pulseaudio):

    $ sudo apt-get install rygel-gst-launch

D'oh! Didn't have that installed. 

### 22:22

Finally change the first line of the config to:

    [GstLaunch]
    enabled=true

And we can ding "Audio on Fuji" in the file manager on Kodi

# Exaile

## 2015-04-01 Wednesday

### 22:21

## Plan for blog

Transforming this... into this...

Exaile is a wonderfl media player but it doesn't flaunt its features.
Originally inspired by Amarok, it now distinguishes itself with some unique and
powerfl features. Many of these features are provided as plugins and are not
enabled by default. Some require some configuration.

Even the [screen shots on the Exaile website][...] don't
hint at its flexibility and power.

The area where Exaile excells is making playlists on the fly. 
- Drag and drop between multiple playlists open at one time
- Split playlist view to make drag and drop even easier
- Support for previewing tracks on one sound card while playing on another
- Integrated BPM tapper, for main and preview devices
- Moodbar/waveform view, to get a visual preview of the upcoming tracks



## 2015-04-16 Thursday

### 20:47

Installed [`pulseaudio-dlna`](https://github.com/masmu/pulseaudio-dlna) in a
virtual env as per the instructions, in `~/workspace/pulseaudio-dlna`.
Run with:

     bin/pulseaudio-dlna  --encoder flac

To get lossless audio directly. Requires Revo running. Then select from 
pulse audio control device.

## 2015-04-17 Friday

### 09:10

Thinking about writing an indicator applet for `pulseaudio-dlna` I started
to look into Gobject Introspection.

Here's a great [Arcive of documentation](https://lazka.github.io/pgi-docs/#Indicate-0.7/classes/Indicator.html#methods)
generated from the C source. This is what the python bindings use to 
provide the objects you interact with to talk to GTK/ Indicate, etc.

So you can say:

```python
    from gi.archive import Indicate

```

### 09:34

[Here](https://github.com/Kagee/pa-stream-sink-selector/blob/master/pa-stream-sink-selector.py) is an exampe
app. And [tutorial](http://python-gtk-3-tutorial.readthedocs.org/en/latest/search.html?q=app&check_keywords=yes&area=default)

## 2015-04-26 Sunday

### 22:49

- [ ] For some reason I'm not getting False, Ture and None highlighted in python sources in vim
