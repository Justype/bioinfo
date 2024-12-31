#linux #tools #env-manager

<img alt="Lmod logo" height=32 src="https://lmod.readthedocs.io/en/latest/_static/Lmod-4color@2x.png" /> **Lmod** is a lua and enhanced version of [[Environment Modules]].

They have the same usage. But their module files are different.

Go to check my GitHub repository: [Justype/modules](https://github.com/Justype/modules)

# Quick Examples

- `module avail`
- `module load software`
- `module unload software`
- `module purge` (unload all)

# Custom lua script

The most important part is:

1. `prepend_path("PATH", pathJoin(app_root, "bin"))` (add `bin` to `$PATH`)
2. `setenv` (environmental variable, in the machine)

My `template.lua` example.

```lua
-- Lmod modulefile.lua
-- Almost all of them are automated, you can modify whatis, ModulesHelp, and APP_HOME as needed.
-- Search TOCHANGE to find the targets.
-- You can also load other dependency modules by depends_on().
--     depends_on("java/17.0.12")

local abs_path = myFileName()                   -- absolute path of module, /somewhere/modules/modulefiles/app/x.y.z
local app_full_name = myModuleFullName()        -- full name of module, app/x.y.z
local app_name = app_full_name:match("^(.+)/")  -- app name, app
local app_version = myModuleVersion()           -- app version, x.y.z
local module_root = abs_path:match("^(.+)/.+/.+/")    -- abs path of modules, /somewhere/modules
local app_root = pathJoin(module_root, "apps", app_full_name) -- abs path of target app, /somewhere/modules/apps/app/x.y.z
local current_datetime = os.date("%Y-%m-%d %H:%M:%S") -- Record current datetime

-- TOCHANGE: A brief description using whatis
whatis("Loads " .. app_name .. " version " .. app_version)

-- TOCHANGE: Detailed help section
help([[Placeholder for the usage of this module.]])

-- Display a message when loading the module
if (mode() == "load") then
    io.stderr:write("[" .. current_datetime .. "] Loading module " .. app_full_name .. "\n")
    -- # only display message if not in SLURM
    -- if os.getenv("SLURM_JOB_ID") == nil then
    -- end
-- Display a message when unloading the module
elseif (mode() == "unload") then
    -- io.stderr:write("[" .. current_datetime .. "] Unloading module " .. app_full_name .. "\n")
end

-- Modify environment variables if the directories exist
if (isDir(pathJoin(app_root, "bin"))) then
    prepend_path("PATH", pathJoin(app_root, "bin"))
elseif (mode() == "load" and os.getenv("SLURM_JOB_ID") == nil) then
    io.stderr:write("WARNING: No bin directory found in " .. app_full_name .. "\n")
end
if (isDir(pathJoin(app_root, "lib"))) then
    prepend_path("LD_LIBRARY_PATH", pathJoin(app_root, "lib"))
end
if (isDir(pathJoin(app_root, "share/man"))) then
    prepend_path("MANPATH", pathJoin(app_root, "share/man"))
end

-- TOCHANGE: Set custom environment variables and aliases
-- setenv("APP_HOME", app_root)
-- set_alias("app", "command")

-- Handle conflicts with other versions
conflict(app_name)

```

# References

- [https://lmod.readthedocs.io/en/latest/index.html](https://lmod.readthedocs.io/en/latest/index.html)
- [https://lmod.readthedocs.io/en/latest/100_modulefile_examples.html](https://lmod.readthedocs.io/en/latest/100_modulefile_examples.html)