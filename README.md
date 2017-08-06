# syncthing-dedupe

When you move or otherwise rename files in a versioned Syncthing folder, they get copied to `.stversions`. That's because Syncthing [sees renames](https://forum.syncthing.net/t/why-does-rename-move-put-file-s-in-stversions-dir/2757) as separate delete and create operations.

syncthing-dedupe is a quick and dirty script for purging `.stversions` of all files that actually still exist in the Syncthing folder.

## Usage

```shell
$ ./syncthing-dedupe /path/to/sync/folder
```

You will be asked for confirmation before deleting files.

## Deduplication strategy

File equivalence is based on byte size and SHA-256 checksum.

If the checksum of an `.stversions` file equals those of multiple "living" files, it's impossible to determine which (if any) of the living files is the true original. In that case, the `.stversions` file will be spared.

Files with size 0 are also spared.
