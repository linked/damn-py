
# DAMN! (Directory And Markup Nesting!)

DAMN is just folders and YAML files, used to compose one big, mega YAML of all
the children. It's for when you have too much YAML to write by hand, and a
hundred lines of code just to read it all back. DAMN will let you split it up,
organize it into folders, and read it back with a simple
``damn.load('config/')``

DAMN is not-your-dad's structured file storage. It is a superset of YAML, and therefore JSON. If you are using YAML and JSON already, there is probably no reason to switch, but you can do so seamlessly. Use DAMN next time you get a headache.

DAMN is good for managing complex configurations that would get too-big-to-manage in other markups and object notations. It does this by allowing you to use directories and filenames all whilly-nilly to organize all your data.

For example's sake, let's redunantly store the same data twice, like this --

```bash
# We could have one big nested object
$ echo "{nested: {message: {hi: mom}}}" > ./__.json

# Or we could split it up into directories and sub-files
$ echo "{hi: mom}" > ./nested/message.damn
```
Now we can retrieve it like this --

```python
# Open a flatfile ('__' objects will get loaded into the parent's namespace)
damn.load('__.json')

# Guess a valid extension (.json|.yml|.yaml|.damn)
damn.load('__')

# Load from nested directories, automagically
damn.load('.') == {'nested': damn.load('nested/')}
damn.load('__.json') == {'nested': damn.load('nested/message.damn')}

# Load from a string
damn_obj = damn.loads("{nested: {message: {hi: mom} } }")

# Dump it all back into one big damn string
# or json string or yaml string
damn.dumps(damn_obj, render='json')

```

In other words, any time nested dictionaries are too much for you to handle, you can fall back to using directories full of files.

A file named 'a/b/c.yml' will populate into a dictionary called `data['a']['b']['c']`, just like that, easy-peasy.

Need examples? I HAVE FIVE.

Here's my directory structure --

![alt tag](https://raw.github.com/linked/damn-py/master/damn_screenshot.png)

```python

# Look, it might as well be YAML!
yaml_abc = yaml.load(yml)
damn_abc = damn.load(FIXT('abc'))
self.assertEqual(damn_abc, yaml_abc)

# Check the tests/ directory to see how these files are structured
mom = {'hi': 'mom'}
self.assertEqual(damn.load(FIXT('dir/and/markup/nest')), mom)

# Or you can probably figure this out
self.assertEqual(damn.load(FIXT('dir')), {'and': {'markup': {'nest': mom}}})

# It's not a science rocket
self.assertEqual(damn.load(FIXT('foo')), {'bar': damn.load(FIXT('bar'))})

# if 'bar.yml' and 'bar/__.yml' both exist, directories take precedence over files
self.assertEqual(damn.load(FIXT('foo/bar')), damn.load(FIXT('bar')))

```

# Was another markup notation really necessary?
Hey shut up, structuring my config files like this drastically saved me a ton of
time and effort in one of my recent projects, is anything that makes us happy ever really necessary?

# That's it!
I'm tired, I'm going to bed, hot damn
