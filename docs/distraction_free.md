# 

[DOCUMENTATION](index.html)

Distraction Free Mode

Distraction Free Mode shows your files full screen, with nothing but text shown in the center of your monitor. All UI chrome is hidden, but accessible. Distraction Free Mode can be entered into via theView![▶](images/right.svg)Enter Distraction Free Modemenu item.

When in Distraction Free Mode, all UI chrome (side bar, minimap, status bar, etc) will be hidden. You can selectively enable parts of the UI via theViewmenu – your settings will be remembered next time you enterDistraction Free Mode.

## Customization

Certain settings will be applied when inDistraction Free Mode. The default settings (located inPackages/Default/Distraction Free.sublime-settings) are:

~~~
{
    "line_numbers": false,
    "gutter": false,
    "draw_centered": true,
    "wrap_width": 80,
    "word_wrap": true,
    "scroll_past_end": true
}

~~~

You can customize these via editingPackages/User/Distraction Free.sublime-settings, which is accessible via thePreferences![▶](images/right.svg)Settings – Distraction Freemenu item.

It's worth noting thewrap\_widthsetting, above. The value of`80`causes wrapping to happen at 80 characters. You may want to set this to a higher value, or wrap to the window width, by setting it to`0`.