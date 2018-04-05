## STR

1. Clone this repo & `cd` into it
2. `mkvirtualenv dj111-py2 -p python2.7`
3. `pip install Django==1.11.12`
4. `./manage.py squashmigrations testapp 0002 --noinput`
5. `./manage.py migrate`
6. `mkvirtualenv dj111-py3 -p python3.6`
7. `pip install Django==1.11.12`
8. `./manage.py migrate`

## Expected

The migrate command at step 8 completes successfully, or else a less misleading error message shown.

## Actual

Step 8 results in:

```
CommandError: Conflicting migrations detected; multiple leaf nodes in the migration graph:
(0002_some_change, 0001_squashed_0002_some_change in testapp).
To fix them run 'python manage.py makemigrations --merge'
```

Running the suggested `manage.py makemigrations --merge` fails too (and it shouldn't be necessary anyway):

```
Traceback (most recent call last):
  ...
  File ".../django/core/management/commands/makemigrations.py", line 272, in handle_merge
    raise ValueError("Could not find common ancestor of %s" % migration_names)
ValueError: Could not find common ancestor of {'0001_squashed_0002_some_change', '0002_some_change'}
```

## Additional notes:
* If the bytestring prefix (`b'...'`) is removed from the migration names specified in `replaces` in the squashed migration, the migration succeeds.
* The above was using Python 2.7.14 and Python 3.6.4.
