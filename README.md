# hyprjump

Hyprjump is a small Lua script which brings a few Vim-style navigation features to Hyprland, including:

- Traversing through window history in a similar to Vim's jumplist (`CTRL-I`/`CTRL-O`)
- Marking windows with a character with a modal binding similar to Vim's `m{a-zA-Z}`, which can then be jumped to with a binding similar to `'{a-zA-Z}`

## Usage

Install `hyprjump.lua` in your Hyprland config directory (likely `$XDG_CONFIG_HOME/hypr/`). Then, require it in your config and use the relevant features' setup functions.

### Jumplist

```lua
local jump_list = require("hyprjump").jump_list.setup() -- Setup initializes the jump list
hl.bind("SUPER + O", jump_list.jump_down, { description = "Jump backward through window history" })
hl.bind("SUPER + I", jump_list.jump_up, { description = "Jump forward through window history" })
```

**Note**: If mouse focus is enabled, occasionally a focus call on a specific window will rearrange items in the focus history in an unexpected way as the mouse focuses windows between the current window and the target. Hyprjump's jump functions account for this, but if you use `hl.dsp.focus()` elsewhere in your binds, then be aware that it can rarely make Hyprjump appear broken.

### Mark Mode

```lua
local mark_mode = require("hyprjump").mark_mode.setup() -- Setup creates the submaps and takes some options to customize the modes; see the annotated param type.
-- Enter mark mode with SUPER + M; in this mode, pressing a-z will mark a window with that character, then immediately exit
hl.bind("SUPER + M", mark_mode.enter_mark_mode, { description = "Enter mark mode" })
-- Enter mark mode with SUPER + apostrophe ('); in this mode, pressing a-z will jump to a window with that character.
-- You can also press 1-0 to jump to the 1st-10th most recently focused window. You can change the normalization behavior (i.e. 0-key maps to 1st, 1 to 2nd, and so on) in the setup options.
hl.bind("SUPER + apostrophe", mark_mode.enter_jump_mode, { description = "Enter jump mode" })
-- Press Escape in either mode (customizable with the setup options) to exit the submap without marking or jumping
```
