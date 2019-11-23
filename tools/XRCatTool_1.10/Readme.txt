X Rebirth Catalog Tool, version 1.10
Copyright (c) 2016 EGOSOFT

Usage (command console version):

XRCatTool -in <paths> -out <path> [-diff <paths>] [-include <patterns>] [-exclude <patterns>] [Flags]

<path>               A folder name or catalog name (e.g. c:\xr, c:\xr\01.cat). Catalog names must end with '.cat'.
<paths>              A list of path names
-in <paths>          Input paths. Files from later paths overwrite files from earlier paths.
-out <path>          One output path.
-diff <paths>        Base input for which a diff will be created (only new/changed files and deletions remain).
-include <patterns>  Include all files matching the following regular expression(s).
-exclude <patterns>  Exclude all files matching the following regular expression(s) from the already included files.
                     Even for files that are not included, this can speed up the scanning, e.g.: -exclude "^assets/"

Flags:
-append              If output path is an existing catalog, append output to it instead of overwriting it.
-x3cat               Write output catalog in old X/X2/X3 catalog format (note: format is auto-detected on -append).
                     An appended file entry overrides all previous occurrences of the same file in the catalog.
-dump                Dump the list of resulting file names.

For regular expressions, see http://www.arglist.com/regex/regex7

--------------------------------------------------

Usage (GUI version):

XRCatToolGUI [<catalog> [<catalog2> ...]]

<catalog>            Optional catalog to be imported
<catalog2>           Additional catalog(s), overwriting files from earlier catalogs. They are imported in the provided order.

GUI Functions:
- Diff selection:    Switch between "Old" and "New" content via the radio button. All other functionality (import, save etc.)
                     only affects the selected content, leaving the other content unchanged. Default content is "new".
- Create diff:       Compare "Old" and "New" content and replace the "New" content with the difference.
                     Files that are identical in both contents are removed, and missing files are marked as deleted.

- Folder import:     Via button or drag&drop of folders. Previously imported files can be overwritten.
- File import:       Via button or drag&drop of files. Previously imported files can be overwritten.
- Catalog import:    Via button or drag&drop of .cat files. Previously imported files can be overwritten.
                     When importing multiple catalogs simultaneously, they are imported in alphabetical order.
                     When dragging catalogs together with other files and folders, they are treated as normal files.
- Root folder:       Required for import of folders and files via button. When importing files or folders from sub-folders
                     of the root folder, the folder hierarchy will be respected. This also applies to drag&drop.

- Extract:           Write the selected entries to disk as single files.
                     If "Keep folder hierarchy" is checked the appropriate sub-folders will be created.
- Remove entries:    Remove the selected entries via Remove button or Delete key.
- Clear:             Clear all entries in the current content.

- Extract all:       Write all entries of the current content to disk as single files, creating appropriate sub-folders.
					 NOTE: The program will be unresponsive until the operation has completed.
- Save as catalog:   Write all entries of the current content to a catalog (using X/X2/X3 catalog format if selected).
                     WARNING: Do not overwrite a catalog that was imported and is currently used as source.
					 NOTE: The program will be unresponsive until the operation has completed.
- Append to catalog: Append all entries of the current content to an existing catalog (format is auto-detected).
                     WARNING: The content of the existing catalog is not respected at all, the content is simply added.
					 NOTE: The program will be unresponsive until the operation has completed.

--------------------------------------------------

Notes for modders on catalog usage in X Rebirth:

At first, the game load catalogs of the form ##.cat (## = 01..99). Loading stops at the first missing file.

In each enabled extension, the game then tries to load the following files:
- subst_##.cat (starting with subst_01.cat)
- subst_v###.cat (### = current game version, e.g. subst_v130.cat)
- ext_##.cat (starting with ext_01.cat)
- ext_v###.cat (### = current game version, e.g. ext_v130.cat)

The files in the ext_* catalogs are interpreted as part of the extension.
This is the recommended way of altering the game, it allows for a high degree of compatibility between mods.

The files in the subst_* catalogs are interpreted as relative to the game root (adding/replacing base game files).
This should only be used if there is no other way to adjust the game assets. Please make sure that players are aware of the
potential incompatibilities with other mods.

The version catalogs can be used when a new version of the game is available as a beta. In case your mod becomes incompatible
because of game changes in a new version, or in case you want to use features of the new version, you can use the beta phase to
make the necessary changes and provide support for both game versions simultaneously: Files for one game version are in normal
ext_## catalogs, while the ext_v### applies the necessary changes for the other version. You can use the diff feature of the
catalog tool to generate the version catalog automatically, based on two input folders.

When using a version catalog, it is up to you whether the version catalog represents the "old" (released) version or the "new"
(beta) version. But although it seems natural to use it for the beta content (catalogs being applied chronologically), it makes
sense to use it for the released version instead. This prevents problems in the future when the game gets a higher version
number but no updated mod is available at that time (the new content would not be loaded because the version does not match).

--------------------------------------------------

Version History:

v1.10 (2016-12-13)
- Added support for catalog format from earlier X games (Note: In general, files in pre-XR catalogs have unknown file dates)
- Auto-detect format of existing catalog (X/X2/X3 vs X Rebirth) when used as input or when appending
- When importing X/X2/X3 catalogs, file timestamps are derived from actual file content (only for *.pck, *.pbd, *.pbb, *.obj)

v1.09a (2015-03-13)
- Updated GUI icon

v1.09 (2014-02-26)
- Fixed errors when comparing many single files
- Ignore filename case mismatch when creating a diff

v1.08
- Added GUI
- Keep original file date when extracting files
- Detect identical files with different timestamps when creating a diff
- Added optional argument -append
- Fixed output catalog names being converted to lower case

v1.07
- Fixed import of joined catalogs
- Fixed redundant empty file definitions carried over in diff catalogs

v1.06
- Now allows file dates to differ by +3600 or -3600 to work around a daylight savings time issue.

v1.05
- Minor optimisation to folder scanning - folders that match the exclude folder are no longer recursively scanned.

v1.04
- Support regular expressions.

v1.02
- Fixed upercase/lowercase issue - the pathname key to match filenames in a filedb is now always lowercase
