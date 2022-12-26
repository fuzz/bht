# bht
Keep your bits free from rot!

At the moment:
* create a directory
* symlink your bin and dotfiles into above directory
* move all your data not managed by other apps (eg not your Photos library)
  into above directory, symlink things--or not--as you prefer

Now all the data you care about backing up is in one directory. Note this
directory is exempt for the macOS's protections for your Desktop and Documents
folders--this makes backing up and otherwise working with files a bit easier at
the cost of reduced security. If you have data you'd prefer to be protected
that way by macOS leave it in those directories. Also note that some apps may
insist on their files being installed in a specific directory and some commands
may not tolerate their dotfiles being symlinks.

## Example
Let's say your directory is called Wardrobe and one of your backup drives is
called BlackSSD. You might do something like this:
```
mkdir ~/.bht
mtree -c -k md5digest -p Wardrobe > ~/.bht/wardrobe-current.mtree
ditto ~/Wardrobe ~/Volumes/BlackSSD/Wardrobe
mtree -c -k md5digest -p /Volumes/BlackSSD/Wardrobe > ~/.bht/blackssd-wardrobe-current.mtree
```

Then you could validate and track changes with eg:
```
diff ~/.bht/wardrobe-current.mtree ~/.bht/blackssd-wardrobe-current.mtree
```

And periodically validate your backup drives with eg:
```
mtree -k md5digest -f ~/.mtree/wardrobe-current.mtree -p /Volumes/BlackSSD/Wardrobe
```
Which will also cause those blocks to be read periodically--good for SSDs that
aren't always powered on.

This will get the job done, albeit in a barebones way.

## What about Photos, etc?
Don't use symlinks but otherwise treat them the same.

## What about Time Machine, Backblaze, etc?
Those are great, but they don't catch data corruption or bit rot. They'll
happily back up the new, corrupted file, and in time the original will be
lost--30 days in the case of the standard Backblaze plan.

## Cool, but I don't see myself futzing around with mtree and diffs
Excellent! You're in the right place; you've just arrived a bit early.
