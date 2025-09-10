# hyprjump

`hyprjump` is a small tool which brings a few Vim-style navigation features to Hyprland, including
- Marking windows with a character with a modal binding similar to Vim's `m{a-zA-Z}`, which can then be jumped to with a binding similar to `'a`
- Traversing through windows similar to Vim's jumplist (`CTRL-I`/`CTRL-O`)

**Dependencies**:
- [`jq`](https://github.com/jqlang/jq)
- [`socat`](https://linux.die.net/man/1/socat)

## Usage
### Jumplist
If "jumplist" behavior is desired, start `hyprjump` in the background:
```
exec-once = hyprjump run
```
This allows it to track the state of the window focus history, which is vital for mimicking Vim's jumplist. From there, add calls to hyprjump which allow for window history navigation:
```
bindd = SUPER, O, Jump backward through window history, exec, hyprjump jump prev
bindd = SUPER, I, Jump forward through window history, exec, hyprjump jump next
```
###
If only the tagging behavior is desired, you can run the following to add a comprehensive set of keybinds for mark/jump mode:
```
hyprjump init 
```
For now, the generated bindings assume you are using `$mainMod` as an alias for your Super key in your Hyprland bindings (like is used in the [default configuration](https://github.com/hyprwm/Hyprland/blob/main/example/hyprland.conf#L222)). The generated binds are initialized as `$mainMod` + `M` to enter mark mode and `$mainMod` + `'` (apostrophe) to enter jump mode, much like in Vim. These bindings will be more customizable in the future; for now, if you would like a template to work off of, see [hyprjump.conf](./example-hyprland-config/hyprjump.conf).

In `mark_mode`, you can press any alpha `a`-`z` to mark the currently focused window with that letter. It uses Hyprland's tag system to first untag all windows with that character, then tags the focused window with that character.

In `jump_mode`, you can press any alpha key `a`-`z` to jump to the window marked with that character. You can also press `1`-`0` to jump to the `1st`-`10th`-most recently focused window in your history (independent of jumplist).

While in either mode, you can press `Esc` to exit early if so desired.

## To-Do List
In its current form, `hyprjump` is usable as described above. However, some minor features are WIP:
- [ ] Add `init dump` utility which dumps a complete configuration file for use with Hyprland directly
- [ ] Add option to pick which Hyprland instance to use
- [ ] Misc. options for generating bindings used by `init`
- [ ] Allow chaining of initialization and daemonization
- [ ] If the user uses single-letter tags for their own purposes, add an option for a prefix string for the tags set by `hyprjump`
- [ ] Add Nix flake, allowing for installation with Nix
- [ ] Add error messages for missing dependencies
