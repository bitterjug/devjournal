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
