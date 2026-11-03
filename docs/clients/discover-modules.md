# Loading modules on Discover

You can load `other/opencode` and `other/gh` manually in each interactive session, or optionally load the ones you need from your shell startup file. Follow the [NCCS shell configuration examples](https://github.com/GEOS-ESM/GEOSgcm/wiki/Suggested-NCCS-Resources#311-shell-configuration): put module commands inside the interactive-shell block, after the existing module setup. Do not replace your startup file or add a second block if you already have one; add the desired `module load` lines to the existing block instead.

For example, in `~/.bashrc`:

```bash
if [[ $- == *i* ]]
then
   # Place these after your existing module setup.
   module load other/opencode
   module load other/gh
fi
```

Or, in `~/.tcshrc`:

```tcsh
if ($?prompt) then
   # Place these after your existing module setup.
   module load other/opencode
   module load other/gh
endif
```

These blocks do not load modules in non-interactive shells. Remove either load line if you do not need that tool.
