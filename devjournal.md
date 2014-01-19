

## 2014-01-19 14:12 Sunday

This is a test for a new way to store the dev journal after loosing all my work on UbuntuOne

## 2014-01-19 14:13 Sunday

Looks like it's working.

  - Install SparkleShare from the repo (I got v0.9.0 though 1.2 is available
      on the site)

  - It creates a new key pair and puts the public one in `~/SparkleShare`

```VimL
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
