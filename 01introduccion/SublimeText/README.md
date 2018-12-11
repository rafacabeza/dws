# ¿Qué es Sublime text?

Sublime Text es un editor de texto y editor de código fuente está escrito en C++ y Python para los plugins.1 Desarrollado originalmente como una extensión de Vim.

Vamos a configurar Sublime Text de acuerdo a este [tutorial](https://manuais.iessanclemente.net/index.php/Tutorial_sobre_editor_Sublime_Text_3).

## Paquetes interesante para PHP

* "Package Control"
* "SideBarEnhancements"
* "View In Browser"
* "Emmet"
* "BracketHighlighter"
* "Laravel Blade Highlighter"
* "PHP Getters and Setters"
* "PHP Constructors"
* "SublimeCodeIntel"
* "SublimeLinter" y "SublimeLinter-php"

## Configuracion de usuario

* Preferences &gt; Settings – User.

```
{
    "default_line_ending": "unix",
    "ensure_newline_at_eof_on_save": true,
    "font_size": 13,
    "rulers":
    [
        120
    ],
    "tab_size": 4,
    "translate_tabs_to_spaces": true,
    "trim_trailing_white_space_on_save": true,
    "word_wrap": "true"
}
```

## Atajos

* Preferences -&gt; Key Bindings \(user\)
  ```
  [
    { "keys": ["shift+f6"],
        "command": "side_bar_open_in_browser" ,
        "args":{"paths":[], "type":"production", "browser":""}
    },
    { "keys": ["ctrl+shift+r"],
        "command": "reindent" ,
        "args": { "single_line": false }
    },
    { "keys": ["ctrl+shift+s"],
        "command": "delete_trailing_spaces"
    },
    { "keys": ["ctrl+alt+s"],
        "command": "save_all"
    }
  ]
  ```



