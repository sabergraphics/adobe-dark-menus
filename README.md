# Adobe Dark Menus

This Windhawk mod gives Adobe apps on Windows dark menus: real menu bars, top-bar dropdowns, context menus and dropdown lists. It also colors their standard title bars to match, darkens Photoshop's message boxes, and lets the mouse wheel scroll menus too long for the screen.

Everything happens inside each app's own process. No system colors are changed and no other application is affected.

## Supported apps

Photoshop, Illustrator, InDesign, InCopy, Dreamweaver, After Effects, Premiere Pro, Audition, Media Encoder, Bridge, Lightroom, Lightroom Classic and Acrobat.

The Beta versions of Photoshop, Illustrator, InDesign, InCopy, After Effects, Premiere Pro, Audition and Media Encoder are covered as well.

## Screenshots

**Photoshop:** a dropdown list in the options bar

![Photoshop dropdown list](https://raw.githubusercontent.com/sabergraphics/adobe-dark-menus/main/images/photoshop-dropdown-list.png)

**Illustrator:** a context menu

![Illustrator context menu](https://raw.githubusercontent.com/sabergraphics/adobe-dark-menus/main/images/illustrator-context-menu.png)

**InDesign:** the Type menu

![InDesign Type menu](https://raw.githubusercontent.com/sabergraphics/adobe-dark-menus/main/images/indesign-type-menu.png)

**After Effects:** the Window menu, with the menu bar and title bar

![After Effects Window menu](https://raw.githubusercontent.com/sabergraphics/adobe-dark-menus/main/images/after-effects-window-menu.png)

**After Effects:** the blend modes on a timeline layer

![After Effects blend modes](https://raw.githubusercontent.com/sabergraphics/adobe-dark-menus/main/images/after-effects-blend-modes.png)

**Premiere Pro:** a context menu on a timeline clip

![Premiere Pro context menu](https://raw.githubusercontent.com/sabergraphics/adobe-dark-menus/main/images/premiere-context-menu.png)

**Premiere Pro:** the Sequence menu, with the menu bar and title bar

![Premiere Pro Sequence menu](https://raw.githubusercontent.com/sabergraphics/adobe-dark-menus/main/images/premiere-sequence-menu.png)

## Related mods

These overlap with this mod. Running them together in the same app has not been tested, so use one or the other for the apps above.

- **[Photoshop Dark Menus](https://windhawk.net/mods/photoshop-dark-menus)** covers Photoshop only. This mod does everything it did and reaches the other Adobe apps, so disable it if you use this one.
- **[Dark mode context menus](https://windhawk.net/mods/dark-menus)** darkens the menus Windows draws, in every app and in fixed colors. It does not reach menus an app draws itself, which is why Photoshop stays light with it, or dropdown lists. This mod uses the same technique for the menus Windows draws, so exclude the Adobe apps from it in its Advanced tab.
- **[After Effects Theme](https://windhawk.net/mods/after-effects-theme)** and **[Premiere Pro Theme](https://windhawk.net/mods/premiere-pro-theme)** recolor the whole interface of those two apps in palettes of their own, title bar, menu bar and menus included. They answer the same menu bar messages as this mod, so if you use one of them, exclude that app from this mod in its Advanced tab.
- **[Windows 11 Custom Title Bar Colours](https://windhawk.net/mods/win11-custom-title-bar-colours)** and **[Auto Custom Titlebar Colors](https://windhawk.net/mods/auto-custom-titlebar-colors)** color title bars in every app. If you prefer one of them, turn off **Dark Title Bars** here.

## Requirements

- **[Windhawk](https://windhawk.net) 1.5 or later.** The mod uses the typed `SetFunctionHook` helper, which was added in 1.5.
- **Windows 10 1903 or later** for the menus Windows draws itself. On older builds those menus stay light, and only menus an app draws itself are darkened.
- **Windows 11** for custom title bar colors. Windows 10 2004 or later still gets a dark title bar, in Windows' own color.

There is no toolchain to install. Windhawk compiles the mod itself with its own bundled compiler.

## Installation

1. Download `adobe-dark-menus.wh.cpp` from this repository
2. Open Windhawk
3. Click **Create a new mod**
4. Replace everything in the editor with the contents of the downloaded file
5. Compile it, and Windhawk will build and install it

The apps do not need to be restarted.

## Options

The following settings can be customized in the Windhawk mod panel:

- **Menu Background Color**: Background color (Default: `#2D2D2D`).
- **Menu Text Color**: Text color for active items (Default: `#DCDCDC`).
- **Highlight Background Color**: Color when hovering over an item (Default: `#505050`).
- **Highlight Text Color**: Text color when hovering over an item (Default: `#FFFFFF`).
- **Separator Line Color**: Color for separator lines. Set to match the background color to hide them completely (Default: `#383838`).
- **Disabled Text Color**: Text color for disabled menu items (Default: `#808080`).
- **Border Color**: Color of the frame around menus an app draws itself and around dropdown lists. Leave empty to keep the system frame on those menus and to use the background color on lists (Default: `#2D2D2D`).
- **Title Bar Color**: Leave empty to match Menu Background Color. Windows 11 only: Windows 10 keeps its own dark color (Default: empty).
- **Title Bar Text Color**: Leave empty to let Windows choose, which dims the text on a window that is not active. Windows 11 only (Default: empty).
- **Theme Dropdown Lists**: Darken the dropdown lists an app draws itself, using the colors above (Default: on).
- **Darken Menus Windows Draws**: Use Windows' dark menu theme, and paint real menu bars dark (Default: on).
- **Scroll Long Menus Instead of Columns**: Open a menu that an app splits into columns as one dark column that scrolls, since Windows cannot darken a menu with columns. After Effects splits its Effect menu this way once it holds more than the screen fits. Turn it off to keep the columns, which stay light (Default: on).
- **Scroll Long Menus with the Mouse Wheel**: Windows scrolls a menu taller than the screen only with its arrows and the keyboard. With this on, the mouse wheel scrolls it too (Default: on).
- **Darken Photoshop Message Boxes**: Darken Photoshop's message boxes, such as the prompt to save changes before closing, using the colors above. Their buttons take Windows' own dark style (Default: on).
- **Dark Title Bars**: Color standard title bars to match the menus (Default: on).

Colors can be given as `#RRGGBB` or the short `#RGB` form.

The colors apply to real menu bars, to menus an app draws itself (Photoshop's) and to dropdown lists. Menus Windows draws itself keep Windows' own dark colors.

## How it works

Adobe's apps do not all draw their menus the same way, and sharing a library does not settle it: Illustrator and InDesign ship the same `AdobeOwl.dll` as Photoshop, yet Windows draws their menus and Photoshop draws its own. So the mod decides menu by menu, by whether the items are owner-drawn (`MFT_OWNERDRAW`).

| Kind of menu | Who paints it | How the mod darkens it |
| --- | --- | --- |
| Dropdowns and context menus in most apps | Windows, through its theme engine | `SetPreferredAppMode(ForceDark)` and `FlushMenuThemes` |
| Real menu bars, such as After Effects' | Windows, offered to the window first | `WM_UAHDRAWMENU` and `WM_UAHDRAWMENUITEM`, handled in a `DefWindowProc` hook |
| Scroll arrows on long menus | The Windows kernel, directly in light colors | painted over straight afterward, from a subclass on the popup |
| Long menus an app splits into columns, such as After Effects' Effect menu | Windows, without its theme, since a column break turns the theme off | the column breaks come off while the menu is open, so it is one dark column that scrolls, and go back on when it closes |
| Photoshop's dropdowns and context menus | The app, owner-drawn with plain GDI | hooks on `FillRect`, `PatBlt`, `GetSysColor` and `GetSysColorBrush`, scoped to the item being painted |
| Dropdown lists an app draws itself | The app, into the system's `ComboLBox` | the same hooks, scoped to the list's painting |
| Photoshop's message boxes, such as the prompt to save changes | Photoshop paints the background white, Windows draws the text and buttons | the box is subclassed: its background and text take the menu colors, and its buttons Windows' own dark button theme |

The system color overrides apply **only while a menu item or a dropdown list is actually being painted**, rather than for as long as a menu or list happens to be open. `TrackPopupMenu` runs a modal message loop which keeps dispatching to every other window on the thread, so a panel repainting behind an open menu would otherwise come out in menu colors.

A dropdown list whose rows Windows draws is left alone, the same rule as for menus. Windows fills it from a brush no hook can reach, so recoloring the text alone would leave light text on white. So is a list that stays open inside a dialog, such as the ones in the font picker.

Windows scrolls a menu taller than the screen only with its arrows and the keyboard, and while a menu is open it takes a wheel turn out of the input before the app or any hook in it sees it. So while a menu with scroll arrows is on screen, the mod puts up a low-level mouse hook on that menu's thread, takes a wheel turn made over the menu, and turns it into the menu's own arrow steps, as many rows a notch as Windows' wheel setting says. Every other mouse event passes straight through, and the hook comes down as soon as the menu closes.

Title bars need no drawing at all. The mod sets `DWMWA_USE_IMMERSIVE_DARK_MODE` and `DWMWA_CAPTION_COLOR` on each top-level window with a caption, and puts them back when it is disabled. A window that draws its own title bar, such as the main window of Photoshop, Illustrator or InDesign, is left alone. This is checked per window, so the dialogs in those apps are still colored. A dialog's contents stay whatever color the app or Windows paints them.

## Scope

Anything an app paints from its own palette rather than from a system color or the theme is out of reach, such as Photoshop's font browser and the Glyphs panel's character cells.

A dropdown list Windows draws itself keeps its normal colors.

A Photoshop message box holding anything beyond an icon, text and push buttons, such as a "Don't show again" check box, stays light. The buttons of a darkened one take Windows' own dark colors rather than the colors set here, and need Windows 10 1809 or later.

With **Scroll Long Menus Instead of Columns** turned off, a menu that an app splits into columns stays light, because Windows turns its theme off for any menu that has columns.

## Credits

- The dark theme technique comes from [Dark mode context menus](https://github.com/MGGSK/DarkMenus) by MGGSK.
- The menu bar messages were worked out in [win32-custom-menubar-aero-theme](https://github.com/adzm/win32-custom-menubar-aero-theme) by adzm.
- The idea of matching the title bars comes from [Windows 11 Custom Title Bar Colours](https://windhawk.net/mods/win11-custom-title-bar-colours) by Th3Fanbus.

## Reporting a bug

Open an [issue](https://github.com/sabergraphics/adobe-dark-menus/issues). Menu drawing depends on more of the environment than most bugs do, so please include:

- The Adobe app and version, and which menu or dropdown list is wrong
- Your Windows version and display scaling, for example 150%
- Your Windhawk version and the mod version
- A screenshot, which is usually worth more than a description here

## License

MIT. See [LICENSE](LICENSE).
