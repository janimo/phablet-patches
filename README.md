# Porting Ubuntu for Phones patches to other Android trees

Currently the Ubuntu for Phones patches are developed against CyanogenMod. In order to port to devices that run on AOSP, CodeAurora or other custom trees, the patches may need slight adjustments or complete rewrites. These tools can help with kicking off such a port.

## Extracting patches from Ubuntu for Phones tree

Inside an Ubuntu for Phones Android tree specify where to save the commit logs and git format-patch formatted commits
describing the changes between two branches.

This exports all patches made in the saucy branches agains the original CyanogenMod branches.

`/path/to/phablet-patches-export phablet/cm-10.1 phablet/phablet-saucy patches_dir`

This exports all patches, this time against stock AOSP's android-4.2.2_r1 tag.

`/path/to/phablet-patches-export android-4.2.2_r1 phablet/phablet-4.2.2_r1 patches_dir`

## Applying patches in an output dir to an Android tree

In the destination Android tree, make sure the output of this is empty or at least expected, so you start from a known state
`repo status`

In a clean AOSP 4.2.2_r1 tree, the patches exported above should apply without conflicts.

`/path/to/phablet-patches-apply patches_dir`

An optional second argument applies the patches only to a given project, if you expect to have conflicts and want to work though the projects separately

`/path/to/phablet-patches-apply patches_dir bionic`

When git am fails on a patch you can try git apply --reject and fix it up manually.

You can always verify which projects were changed and what commits got applied to each

`repo info -o`

At this point if you have all patches applied, the tree is still not complete. You need to manually copy over uptodate copies ubuntu/ and external/gpg from the phablet branches.
