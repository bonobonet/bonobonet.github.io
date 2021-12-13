Botty
=====

**Botty** is the official BonoboNET bot written by [rany TODO link](), it's quite a fun bot with a modular design that allows
for the easy addition of new commands. The source code is available [here TODO link]().

## Commands

There are quite a few of commands and this section aims to describe them all.

### Translation service

Botty has the ability to translate sentences you give it to any language of choice or to guess the language of the setenece provided
and then to translate it to English.

The following translates the given sentence to English by guessing the origin language:

```
~deavmi: .tr My naam is Koolerboks
```

The reply:

```
+botty: deavmi, My name is Koolerboks
```

The following let's you translate a given sentence (TODO: I presume english semantics) to the requested language, here Arabic:

```
~rany: .tr:ar ya like jazz?
```

The reply:

```
+botty: rany, هل تحب موسيقى الجاز؟
```

### Deavmi Comedy

If you wish to express the <pre>a e t h e t i c s</pre> in your text then this command is for your:

```
~rany: .deavmicomedy OKAY!
```

The reply:

```
+bottyL rany, O K A Y !
```

### Rot 13

Rot 13 does a simple scrambling of the words. For more information see [ROT13](https://en.wikipedia.org/wiki/ROT13).

Here we rotate the text "THIS IS A SECRET... ROMAN SECURITY":

```
~rany: .rot13 THIS IS A SECRET... ROMAN SECURITY
```

The reply:

```
botty: rany, GUVF VF N FRPERG... EBZNA FRPHEVGL
```

### Urban Dictionary lookup

Botty provides the ability to look up the meaning of words according to the world's true source of information, _Urban Dictionary_.

Here we use the command to lookup the definition of "penis":

```
~rany: .ub penis
```

The reply:

```
botty: rany, Definition:
botty:  
botty: [the thing] that [justin bieber] [doesnt] have
botty:  
botty: rany, Example:
botty:  
botty: [haha] he [doesnt] have [a penis]
botty:  
botty: rany, Author: helloim345
botty: rany, Permalink: http://penis.urbanup.com/5049121
```

### Text reversal

You can reverse text too:

```
deavmiYgg: .rev Hello world, this is tristan
```

The reply:

```
botty: deavmiYgg, natsirt si siht ,dlrow olleH
```

### Seraching the web

Botty provides the ability to search the web using DuckDuckGo and grabbing the first result. Here we search for the definition of _"Little Endian"_:

```
deavmiYgg: .ddg Little Endian
```

The reply:

```
botty: deavmiYgg, <section class="prog__container"><p>From Swift: someone who eats eggs little end first. Also used of computers that store the least significant byte of a word at a lower byte address than the most significant byte. Often considered superior to big-endian machines. See also big-endian.</p></section>
```