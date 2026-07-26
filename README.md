# ASCII Art Generator - Image & Video to ASCII

Convert your images and videos into ASCII art – right in your browser.  
**No uploads to servers, no installations, no hidden costs.** 

---

## What’s Inside

This repository contains **two standalone HTML files** that work entirely offline:

| File                  | Purpose                                                                                                                                                         |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------
| `image-to-ascii.html` | Convert any image (JPG, PNG, WebP, BMP, GIF) to ASCII art with full control over width, character set, font, contrast, sharpening, and scaling.                 |
| `video-to-ascii.html` | Turn any video (MP4, WebM, OGG, AVI, MOV) into a live ASCII preview *and* export a full ASCII video (MP4) – with adaptive scaling and frame‑accurate rendering. |

> Both files are **self-contained** – just open them in any modern browser and start creating. No server needed.

---

## Features

### Image Converter
- **Upload** – drag & drop or click to browse any image.
- **Real‑time preview** – see grayscale conversion and ASCII output instantly.
- **Adjustable width** – set characters per line (8–500).
- **Multiple charsets** – from classic `standard` to detailed `vibe` (personally, i do not like this one), `blocks`, `braille`, and more.
- **Font selection** – choose from 6 monospace fonts (Courier New, Fira Code, JetBrains Mono, …).
- **Pre‑processing enhancements** – optional contrast boost and sharpen filter.
- **Scale factor** – 0.25× to 4× to control detail vs. output size.
- **Invert** – negative effect.
- **Export** – download as PNG, SVG, or plain text (TXT).
- **Copy to clipboard** – grab the ASCII art with one click.

### Video Converter
- **Upload** – drag & drop any browser‑supported video.
- **Live ASCII playback** – watch your video transformed in real‑time as it plays.
- **Full export to MP4/WebM** – download the entire ASCII‑rendered video.
- **Adaptive canvas** – automatically scales to fit your ASCII grid while keeping a safe pixel limit for performance.
- **Same controls as image converter** – width, charset, font, scale, contrast, sharpen, invert.
- **Smart framerate** – adjusts for long videos to keep file sizes manageable.
- **Single‑frame capture** – save any frame as PNG, SVG, or TXT.

---

## How to Use

### For images
1. Open `image-to-ascii.html` in your browser.
2. Click the upload area or drag an image file onto it.
3. Adjust the settings (chars/line, charset, font, scale, contrast, sharpen, invert) to your liking.
4. Press **⚡ generate** to see the ASCII art appear instantly.
5. Download as PNG, SVG, or TXT using the buttons below the output, or click **⎘ copy** to grab the text.

### For videos
1. Open `video-to-ascii.html` in your browser.
2. Upload a video file (click or drag & drop).
3. Press **play** – the live ASCII preview will appear and update in real‑time.
4. Tweak the settings while it plays – changes apply instantly to the current frame.
5. Click **⬇ video** to export the entire video as ASCII (MP4 or WebM).  
   *(The tool will adapt the framerate and canvas size to your machine’s capabilities.)*
6. Use **⏺ frame** to capture and download any single frame as PNG, SVG, or TXT.

---

## How It Works

Both tools follow the same core pipeline, adapted for still images and video frames:

### 1. Pixel access
The image or video frame is drawn onto a hidden `<canvas>`, giving raw RGBA pixel data.

### 2. Grayscale conversion
Each pixel’s brightness is computed using the luminance formula:  
`Y = 0.299·R + 0.587·G + 0.114·B` (ITU‑R BT.709 standard).  
This mimics how the human eye perceives brightness, giving a natural grayscale representation.

### 3. Resizing & aspect ratio
The image is scaled to match your chosen width × scale, while preserving the original aspect ratio. A monospace correction factor (0.52) ensures characters don’t look stretched or squashed.

### 4. Enhancement (optional)
- **Contrast** – pushes mid‑tones toward black or white using `(value - 128) × 1.5 + 128`, making dark areas darker and light areas lighter.
- **Sharpen** – applies a Laplacian kernel `[0,-1,0; -1,5,-1; 0,-1,0]` to emphasize edges, giving a crisper output.

### 5. Brightness → character mapping
Each grayscale value (0–255) is linearly mapped to a character in the selected charset.  
- Darker areas become dense characters (like `@`, `#`, `8`).  
- Brighter areas become sparse characters (like `.`, `'`, or spaces).  
- **Invert** reverses the mapping for a negative effect.

### 6. Rendering & export
The resulting 2D character grid is displayed as text. For exports:
- **PNG** – drawn onto a canvas with the selected monospace font.
- **SVG** – wrapped in an SVG `<text>` element – resolution‑independent.
- **TXT** – saved as a plain text file.
- **Video** – frames are captured during playback and encoded using `MediaRecorder` with adaptive bitrate.

> All processing happens entirely **client‑side** – your files never leave your computer.

---

## Technical Notes

- **Browser compatibility** – Works on on Brave 100% (others were not tested).
- **No external dependencies** – the HTML files include everything they need (except Google Fonts, which are loaded for the font selector; they work offline too with fallback fonts).
- **Video export** – Uses `canvas.captureStream()` and `MediaRecorder`. The canvas size is adaptively capped to prevent memory issues (4M pixels max, or scaled down further if needed).
- **Performance** – For very large images or long videos, the tools automatically adjust to keep your browser responsive.

---

## Support: DeepSeek

```
#@@S?+++*?%SSS%++?%SS###%*+*%##SSSS##%????????%?%SS%%S######S%SS?*S##@@@@@@@@@@@@@@@@@@@@@@@###########@@@%;:,::::::::,,,.,,,,,,,,,::::::,,;S##%?S##S%%%S##S%%??
#@@#*:++?%%S##S:;S%SS@@@S;:+S@@%%S#S@%+?????*%%:@@@@+S##SSS@#*#@?,S@@##@@@@@@@@@@@@@@@@@###@#@@##@@@@@@@@@S#* ?%,::;::,,...,,::,,,,:,:;;;:. ?@#*?#@@%. ;S@@?:;:+
#@@S*;;;?%?S@#S,;S%SS@@#?::+S@@%%S###?+%S%??*SS;*SS?%%###SS@@*S@%:?@@@@@@@@@@@@@@@@@*%@@@S#@@@@@@@@@@#%@@@:#@@@:,::::,,,,. ,,::,,,,,::;:,:: .@?*?@#@@*?#@#@S+,.;
#@@S?:;+?%%S##%,+SSSS#@@%:;*#@@%%#S@#%+??%?**%S;:+:;%S###SS@@*S@S:?#@@@@@@@@@@@@%*%S#S@@@#@#@@@@@@@@@@@SS%.%@@+ ::+::,,,...,,::,.,,,,::;:::. +#+?S@@#%+#@@#@@?;+
#@@S*:;;*%%S#@?.*SSSS@@@%:;+S@#%%S#@S*;...+%*S#%%?%S%%S@SS%@@*S@%:?#@@#@@@@@@@@@@@@@?#@@@@@##@@@@@@@S#@@@@S@@S .::+::,,,,...,:,:,,,,::::;:,: ,%?+S@#@@? +@@#@@@%
#@@S*;;+*?%S@@?,*S%SS#@@%:;*S@#%?##@%;+*, ;?*S#?*@@S+%###S%@@*#@S,?@@@@@@@@@@@@@@@@%%@@@@@@@S#@@@@@@#%S@@@@@@@: :::;::,,....,:::,,,,:,:;;::,, ?#+*#@##@?::%@@SS%
#@@S?;;+?%?#@#*.?S%%S#@@?:;+S@#%%S##*;+++*%?+%@S.S@S*S###S%@@#@@@*%@@@@@@@@@@@@@@@@@@@@##@@@@#@@@@@@S%S#@@ %@@@+,::;:,,,,..,,:,::,,,,,,:;;::,. #S*%#@@@@#%?**S#%
#@#S%:;+??%###*,*#%S%#@@?:;*S@#%%S#S*,*@@?***%@%+@@@*S@@@@@@@+.:?*S;  ,+   ?@@@@@@@@@@@?:*@@##S%?%S#S#S@@@ S@  ;:::;:,,,,.,.,:,:,,,,,,,::;::,: .?:,*@@@@@@#?+: +
#@@#%;;;??%#@#?.?SSS%#@@%:;*#@#S%##S,*?#@?%?*%#%:#@#%@@@    :S:, . ,:     .      @@@@@@@S@@@@?S#SSSS#S#@S @# ,::;;::,,,.....:,:,:,,,,,,:;;,:,. %#;:.*@@@@@@@##?
#@@#%+,;*?%S##;,?S%S?@@@?::%@@@S%SS?S@S##%#@??@* .*@* ,@@?,                  .:.     .@@@%+#@@@@@@S##SS#@# :,.,:::;;:::,,...,,,::,,,,,,,::;:::: ,##*;:.+?S###SS?
#@@#S+:;+?%S##;:%S%S%#@@*:;?@@#%SS??@#+: +@@?%S+:+@#+%+      .,  ::  .         .. ;:     @@@###@@#####SS@? .,,:;::;;:,:,,.,..,,,,:,,,,,,:;;;:,:. ?@%%S?+;+*+;,:*
#@@#S+::+%%S##::?S%S?S@#%::S@@@%S%?@@*?;.@@?;##;*@ ,@* ,:       @  ::    , . .+..*: ;   ;.+@S##SS####@##@?.,:,::::;;:::,,,..,,,::::,:,,,,:;;;:::  @@?*+;::*S@@@?
#@@@S*;:;??#@S::%S%%%#@@*:;#@@#%%%#@*+%*@@#;+?%?;##*       :?    *  ;%.: :;.;. .,; .? :+, +@S#SSSS#SS@@@@S ,:,:;;;:::.,,,,...,,,:::,,,,,,,:;;;::: +@@?++::   ,:;
#@@#S+::;?%S@S,:%SS%%S@#+;;S@@S%?S@@,:+@@#**??;: @@.,.::,:.  ,,.:  .     :   :       ?, %*@+@*@@@@S%%++;*, :,,.:::, ,:,.,,...,,,:::,,,,,,::;;;,::, %@#%+*?%*   :
#@@@#*,;:?%#@S,;%SS%%S@#*::#@#%%S@@+ *@@@**?;S*@@                                     ,  + @@@@@@@@@@@@@@@ ,,;+   ,@@@,.,,..,,,,:,,,,,,,,,,;::;::,  @@%%*%#@@S%+
S@@@#?:::*?S@S:;%S%%%S@#+,+@@#?%#@% ,@@#?+?:@*:@%, ,   ,   .,?S%#%SS##??:+;            :  S. @@@@@@@@@@@@# ,,:@@?S@@@* ,,,...,,,::::,,,,,,,::;;;,:, .@@?*?SSSSS%
S@@@#?;::+%S@S::?%%%%S#S+:*@#S?%@@+.#@@?%?*S???%      ,.;*?@#SS%S#S@S###@@@S?:;**+:.   ;   . * @@@@@@@@@@# ,: ?@@@@@S  ,::...,,,:,,:,,,,,,,:::;::::, *@@##S#%%%%
S@@@@S;::+?S@S:;%%%%%S@S+,?@#S?@@%,S@@*%%?*@  * @@.  . :;*S%*??%%*SS?%?%%%%#@@##%S#%#:@@; :: S;@@@@@@@@@@# ,: ,#@@@@@#+   .,...,,,:,::,,,,.:,:;;;:::. S@@#SS%%%%
S@@#@S+,:+%S@#;:%S%%?#@S;.#@#?@@S+@@@+?SS%**.#@+     .,+;+?*+*#%??%%%S%?%#@SS#S%++*?+@@@@@ ;@ #,@@@@@@@@@# ,, #@@@@@@@@@@; ,..,,,,:,:,,,,,,,,::;;;::;  @@#SS%%%?
S@@@@#+,;;?S@#;;?S%%?@@S:,@@?S@#+#@@;;%#S?*SS@+ :  .. ,,++????;*%*%S%*?*%*%#%%?*?%?+##*@@@,   %.:@@@@@@@@@ ,.*@@@@@@;,,.++:.,,,,,,,:::,,,,,,,::;;;:::,  #@%%????
S@@@@@+,:;?S@#;:?%%%?##S:;@#S@@*#@#;,S#S?++?@# .:,      .+?;;;??+*;?*+;?*????%%%%+;.;@:*@@@ ,+,# @@@@@@@@@ ,,,  ,@@S..,...,,...,,,,,:::, . ,,,,::;;:::, :%%?SS%*
S@@@@#*:::?S@#:;?%%?%##S,?@S@@?#@S:.*##%*#@# +@#     .?%+*;+;?%%@#@@@@@S###*SSSS#?**; @@@@    :@ S@@@@@@@S,,,,,, %@#.;;:::,,,,,.,,,:,:, :#+ ,,,,:;;::::.;#%+,,.:
S@@@@#*:::?%@S:;%%%?%##S,?@@###@SS::%@?*@   S: S   ;??+%*+?+*???*;.:*@@@@@@%%#SS#@@#%%;@@@S. ;;;,@SSSS%%??.,:::::.?*,;:::,,,,,.,.,    .. ?@# .,,,::;;;::,.  ::,,,
S@@@@#+:::?S@S;;?%%?%##?,?@@#@@%S%;:?%?S? %@@S; ;*;;++***+??*+;:;.+;     :*%+?%%#%??##S: @@   . ,@***+++*+,,::::;:,,;+:::,:,,,,.,S@%;. ;S@#+  .,,::;;;::,:. ,  :
S@@@##*:::*S@S,+?%%?%@#? %@@@#**#%:;++%@ ?@+ #@   ,?**+;.?:.;S#@S@@@##%+::,:**;++:        %@ :. %*;;;;;++:.::,:::;;:;;;:::.,,.,,. ,@@@@@@##@S:    ,:;:::,:, ;??+
S@@@#S?:::*S@%:+%?%?%SS:S@@@S;+%@%::*SS@.@  .#+ +??++*++?S%#*   .   :     ;S@S:  ,#@@@@@@; @@ %@#SSSS###@#:,:::::;;;:;;::::,,,,..,.  %S####%@@@@@#,:::;;:::, %S+
S@@@##*:::*S#?,*%???S#*S@@#?+?S#@*,+?@#S?:*@#* @?;++*++*##S##@@+%@@@@##%S@.#@S :@       .@#,@ @@@@@@@@@@@@?.::::,;;;;;;::::,,,,,,,,, .%@##@SS%+.  .,::;;:;:,: :;
S@@@##*;:;?#@*:*????S%S@@S++SSS##+,*##S#;*:#S %*:+%*+*S?%%%%#@@#S*+:*?SS#;S#@#::@@%@@@@@@   @#@@@@@@@##@@@? ::::::;;;:;;:::,,,,.,,,..?@@@@@@@#. .,,,::;;;::::. :
S@@@##*:,;%##*:*%??*%#@@* ?@#SS#%;,?##S@;%* +?#:::,;;+*?S%S##??SS*@@##@SS%%*#S#.?#%*;.   @@S@+?%SSS%%S?%%%*.::::::;;;;;;::::,,,,,...;@@*:   @@@: ,,,,,,;;;::::,,
#@@@#S;:,+%##::%%*;#@@S, ,#@SSS#%,:%S#S@%@@+:%@;,,:,:+.;??S###@##@SSSS@S#@%%SS% +?*%SS#@??#@@@S%*#@@@@@##@%,,:::::;;;;;;;:::,,,,,,.,     ,,.  #@+ ,,,,,:;;:::,::
#@@##S+.:*S@?,*?+?@@@*:: +@@SS#S*,;?###?@%%*.%@:.,:::.:,:;??@@#@@@@@%?+;%%?@##?@S ?@SS%%S?+;@@@@**SS#%S*?*?:.:::::;;;:;;;;::,,,,...,.,.,,,,,:,  ::,,,,,:;:;;::::
#@@@@S;.:?@#%  ;#@@%;+?; *@#SS#S+:+%###S*@,*,S@S;:;:++,:,;;:+*S*??. :?S@@@%+@@@?@?+:*S##%%*@#@+?*+*??**????:,::::,:;+;;;;;:::,,:.,,.,,,,,,,::,:,..,,,,,,,:;;;:::
#@@#@%: ;%@#. %@@#?;?#%:,S@@SS#%,,*S#SS%;?@ @@ ;;;;:;,,    ..,+; *@@ :##@@@@#@#@@@: ***?++ @%SS#S@@##@@@@@% ,:::::;;;;;;;;::,,,,,...,.,,,,,,::::,,,,,,,,:::;;::
#@@##S, +##;,@@#??+;#@?,,@@###S+,++S#SS%?;@? @@:,;;++*;::::,:**+?@@%@@            .S?  .:;  @+@#@#@#@@@@@@@@,,::::::+;;:;;;:::,,,,,..,.,,,,,,:,,::,,,,,,,,::;;;:
@@##@? .+%*#@@%%%+:+@#?.;#@@##%;:;?S##S%?;@,:@+.;;;:;**;?S%%+?#@@##@#@@@@@S:  +#@@#*?@?,   % @##@#@######@@#+.::::::;+;;::;::::,:,,,..,,,,,,,::::,:,,,,,,,,,;:;:
#####?  ;@@@%S%%+:*S@@+.*@@@##?::+?##SS%*,@ :%:,;;;++?S,?*??%*%#S?SS#S%%%S@;@@#*?*%%%*%S.+@+%@S######@####@@?.,::::::;;;;;;:::::,:,,,,...,,,,,:,::::,,,,..,,::::
#####. S@@S##%*+;*%@@#; ?@@#SS+:;+%#SSS?,S# @*+.;++;*%%+;?S%?*;.    +?#@@?@@@@%#@@S++%*%,@@:@SS#####@@@@@@@@S,,:::::::+;:;;;::::,,,,,,..,,,,,,,,,::::,,,,,,,,:::
###S?S@@S#@%:++;?%?S@#:.S@##%%+:;*%##S%S*@; ,?;::+:++*++:?*;?+;@#@@?*               :;%;?@ S@#####@@#@@@@@@@@;.::::::;+;;;;;;:;::,,,:,.,...,,,,,,:::::,,,,,,,,,:
#SS@@@##@S. ++:?%%S@@S,,@@@S#%;;+?S#S@@ @@@   *::.;;*?*.**;+;S@%S?S@%@@%@@@@@@#?*#@?%%? @ ?@S######@###SS#S#S*,:::::::;;;;;;;+:::::,,,,...,,,,,,,,,::::,,,.,,,,,
##@S#@@S**::;;*?%%%@@? *@@##S. ;%S#@@% @@#@,+:  .,,:*?+,**%**S%%S%**?: ,  ,   .;+%?%:? @ .@S%??*+*++*++***???*.:::::::;;;;;;;;:::::,,:,,,,..,,,,,,,::,::,,,,,,,,
##S@@S .?*:,;+SSS%S@@? ?@@@S @@  ;@S.  @@@S.::*;     :; ;?**;+%%%?*?%%??;., ,:%**,+#+ , .#*++*?%%%#@@@@@@@@@@%;,:::::::;;;;:;;;:::::,,,,,.,,.,,,,,,,,,:::,,,,,,,
#@@%.  +%*,;?%%%%S#@@,.@S+?S@@@@+      *@@@ ,;;+%+     ::;%**+?%%%S??%S%##@@SS??+%@?  ,@@@@@@@@@@@S%?**;;;:..,:,::::::::;+;;;;;;:::,:,,,,...,,,,,,,,:::::,,,,.,
#?##* :@#:;?%?%%?S@@% ;@%@@#,    ..     @@#@  .+;:%; .  ,;;;++??%@S#?%%@@@#SS%%*%S@  @@@@@@@#**;;:,:::;;;;+??+;:::::::::;;;;;;+;;::;::,,:,,,...,,,,,,,,,,::,,,.,
#@@S .%@%,+%?S%*#@?@@@?     ..:,,. .     @@#@@;  +;+% ,  ,;+*+%??%%@@%S%S??#S%?%%@,*%         ,%#@@@@@@@@@@@@#?:.::::::::;;;;;:;;::::::,,,,,,..,,,,,,,,,,:::,,,,
@@@? .S#*,+?+%#@@#,       ,.              @@@@@@@: , %,:   +*++S*%S*@@?%*?%%*%%?%#@ @*;*+*++.             ,#@@@#;++;;::::;;;;:;;;;::::,,:,,,,,,,,,,,,,,,,,,:,,,,
@@@* :S%: ?@@@                             @@@@@@@@@%   :;   ,%?S*?S*@SS@@%S#S?,.@   #,;****;;,.                 ..:*??**+**+;;;:;;;::::,,,,,,..,...,,,,,::::,,,
#@S, :#@@@@,                                :@?#S#@@@@@@    :     .:?*S%+::    .@*    %;,, .?*++,.        ,              . ,**??***;;::::,,,,,.,,,,.,,,,,,:,::,,
#@%@@@%,                                     :@SSS?S@@@@@@@@    ,           @@@@      .**,.      ;++,                             ,:?+;,,,::,,,.,.,,,,,,,,,,:::,
S*%:.              ..     .                   +@@@@@S%@@@@@@@@@@++*%+@@@@@@@@@@.@       +*+,:.       ,:  ..                     ..   . %?:*:,,,,,.,,,,,,,,,,:::,
+;                    . .                      *@@@@@@@#@@@@@  ,.%*;*S   @@@@@%S#@      .:;;,;,.:,   ,  ,                         .,,,, :; :# .,,...,.,,,,,,,::,
.     .                                         ;@@@@@@@@@   @S*%+%*.;@@     @@@@@#@      ,:,,,,....  .            ,                ..:;:.   @+.,,,,,.,,,,,,,,,,
                        .                         @@@@@@       +SSS:@*% S@@@@***#@@ @     .,:,.,....              .:               . ,,:;;    @,.,,,,,,,,,,,,:::
                                                   @@@:;@;@@@@@  .,,. %@@@S@@@@@@@#@ @      ,;,,,.   .             ,                  ,;+,    ,@ ,,,,,,,,,,,,,,:
                                                    @*@@@@?%?@@ .S%?#*.*@@S@@@@@@@@;@@       ,:,.,..               :                 . :+?     S+ ,,,,,,,,,,,,:,
                                                     @#@@@@@@@ *,*;;:@ *@@S@@@@@@@S%#@@       .,...                .                   .:#,   .?:,,,,,,,,,,,,,:,
                                                      @?@#@%#S ?*;%S:?S,.S@#@@@@%S@@@@@+     ..... .               .                    ,#%    ?.,,,,,,,,,,,,,,,
                                                       @@@@@?*;*@ @:%.@.% +@@@@S#@@@@@S@      , .,                 ,                     @     ?; ,,,,,.,,,,,,:,
                                                        @@@@**:%S*:#;?+#.@  @@@#@@@@@@*@@     ....                 ,            .. .    .@    ,*@ .,,,,,,,,,,,,,
                                                         @@@+?,S;S,@***#;*?: %@@@@@@@@@S@       . .                              ...,   .@   .*%%# ,,..,,,,,,,:,
                              .                           :@@@:;%#:+%,S+S:@:?  @@@@@,?**S+       ....            .,           . .,,,,::,;*   ?%: @ ,,,,,,,,,,,,,
                                .                           ;%@ %+%,@?,#*?+%;?:  @@+@@#*@@                        .         ,:;;;::,.. .@  ;%+  +* ;,.,,,,,,,,,,
                                                             + @*???,@,SS;%*?+*:. @@@@@?:@;                             .,,.        ,,  @ ,:   :?S  ;.,,,,,,,,,,
                                                               ;??*#*+S,@;S*+%+%+; @@@@@;*@                            ;               .,     .:?@  S..,,,,,,,:,
                                                               .++?**??*??*?+?***++;S@@@#%S;                            ;              ,      .:*+, .:,,,,,,,,,,
```

---

**Enjoy turning your media into ASCII art!**  
If you like this project, give it a ⭐ on GitHub, follow me – and feel free to fork or contribute.
