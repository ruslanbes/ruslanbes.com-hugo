---
title: Move translations from .properties file to the impex file for LocalizationEntry table
author: Ruslan Bes
type: post
date: 2019-03-09T11:16:48+00:00
url: /2019/03/09/move-translations-from-properties-file-to-the-impex-file-for-localizationentry-table/
classic-editor-remember:
  - block-editor
categories:
  - SAP Commerce (hybris)
tags:
  - hybris-6
  - impex
  - localizationentry
  - properties

---
**Problem:**&nbsp;move many translations from the&nbsp;`base_**.properties`&nbsp;file to the impex file for&nbsp;`LocalizationEntry`&nbsp;table.

Lets say in the `base_**.properties` for German language we have tons of translation like this:

`account.confirmation.address.added=Adresse erfolgreich erstellt`  
`account.confirmation.address.deleted=Adresse erfolgreich gelöscht`

and we want to move them to an impex file that creates `LocalizationEntry` for every one of them. 

`INSERT_UPDATE LocalizationEntry; code[unique = true]; translation[lang = de]`  
`; account.confirmation.address.added ; "Adresse erfolgreich erstellt"`  
`; account.confirmation.address.deleted ; "Adresse erfolgreich gelöscht"`

Why? This way we can impex them once and let customer maintain translations in backoffice and be sure we don&#8217;t overwrite them with default values when doing platform update.

We can do it in any text editor that supports Regex replace. In IntelliJ you can do it from the Replace dialog (Ctrl+R).

1. Replace `"` with `""` <figure class="wp-block-image is-resized">

[<img loading="lazy" src="https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/2017-03-27_17h09_48-1.png?resize=580%2C39&#038;ssl=1" alt="" class="wp-image-1296" width="580" height="39" srcset="https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/2017-03-27_17h09_48-1.png?w=947&ssl=1 947w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/2017-03-27_17h09_48-1.png?resize=300%2C20&ssl=1 300w, https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/2017-03-27_17h09_48-1.png?resize=768%2C52&ssl=1 768w" sizes="(max-width: 580px) 100vw, 580px" data-recalc-dims="1" />][1]</figure> 

2. In the **Regex** mode replace  
  
`^([a-zA-Z0-9._\-]*)=(.*)`   
  
with `<br>`  
`; $1 ; "$2"`  <figure class="wp-block-image">

[<img loading="lazy" width="705" height="49" src="https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/2017-03-27_17h11_20.png?resize=705%2C49&#038;ssl=1" alt="" class="wp-image-1297" srcset="https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/2017-03-27_17h11_20.png?w=869&ssl=1 869w, https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/2017-03-27_17h11_20.png?resize=300%2C21&ssl=1 300w, https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/2017-03-27_17h11_20.png?resize=768%2C53&ssl=1 768w" sizes="(max-width: 705px) 100vw, 705px" data-recalc-dims="1" />][2]</figure> 

The first command escapes quotes with double-quotes inside translated strings, the second one encapsulates them into double quotes thus escaping semicolons and other special chars. Solution based on this [answer][3] from experts.

#### Reversed process

To do a reverse operation we will need a little more work:

  1. In Regex mode replace  
    `^\s*;\s*([a-zA-Z0-9._\-]*+)\s*;\s*"(.*)"[\s;]*$`  
    with  
    `$1=$2`  
    _(this replaces translations enclosed by quotes)  
_ 
  2. Also in Regex mode replace  
    `^\s*;\s*([a-zA-Z0-9._\-]*+)\s*;\s*([^;]*)[;]?\s*$`  
    with  
    `$1=$2`  
    _(this handles the translations written in plain text without quotes)_  
    
  3. Replace `""` with`"` unescaping quotes

 [1]: https://i1.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/2017-03-27_17h09_48-1.png?ssl=1
 [2]: https://i0.wp.com/devblog.ruslanbes.com/rbes-content/uploads/2019/03/2017-03-27_17h11_20.png?ssl=1
 [3]: https://experts.hybris.com/questions/29585/impex-escape-character.html