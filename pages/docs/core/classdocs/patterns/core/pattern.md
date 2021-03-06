---
layout: page
title: Pattern
namespace: Freesewing\Patterns\Core
tags: [class docs]
permalink: /docs/core/classdocs/patterns/core/pattern
---
## Description 

The [`Pattern`](pattern) class is an abstract class to be extended by patterns.

It defines two abstract public methods that must be implemented by all classes
extending it. They are:

- [`Pattern::sample`](pattern#sample) 
- [`Pattern::draft`](pattern#draft) 

## Constants

### PI

`PI` is defined as 3.1415 because we don't need more precision than that.

When you need PI, you can use this.

## Public methods overview

This [`Pattern`](pattern) class has a lot of public methods.

To help you make sense of it all, I've grouped similar methods together. 
In addition to grouping the methods, here are some general guidelines that will help you make sense of them all:

### Public methods patterns must implement
{:.no_toc}

This [`Pattern`](pattern) class is an abstract class, and these are the methods you MUST implement
when creating your own pattern by extending this class:

- [`Pattern::sample`](pattern#sample) : Sample a pattern
- [`Pattern::draft`](pattern#draft) : Draft a pattern

### Public methods for pattern designers
{:.no_toc}

These are the methods a pattern designer should be familiar with:

- [`Pattern::setOption`](pattern#setoption) : Set an option
- [`Pattern::setOptionIfUnset`](pattern#setoptionifunset) : Set an option unless it's already set
- [`Pattern::setValue`](pattern#setvalue) : Set a value
- [`Pattern::setValueIfUnset`](pattern#setvalueifunset) : Set a value unless it's already set
- [`Pattern::getOption`](pattern#getoption) : Returns a given option 
- [`Pattern::o`](pattern#o) : Alias for [`Pattern::getOption`](pattern#getoption)
- [`Pattern::getValue`](pattern#getvalue) : Returns a given value
- [`Pattern::v`](pattern#v) : Alias for [`Pattern::getValue`](pattern#getvalue)
- [`Pattern::t`](pattern#t) : Translate a string
- [`Pattern::stretchToScale`](pattern#stretchToScale) : Take a stretch percentage and returns a scale percentage
- [`Pattern::clonePoints`](pattern#clonepoints) : Clone points from one part into another
- [`Pattern::newPart`](pattern#newpart) : Add a new part to the pattern
- [`Pattern::requested`](pattern#requested) : Returns the full class name of the pattern requested by the user
- [`Pattern::msg`](pattern#msg) : Add a message to the pattern
- [`Pattern::dbg`](pattern#dbg) : Add a debug message to the pattern

### Internal public methods
{:.no_toc}

- [`Pattern::__construct`](pattern#__construct) : The constructor
- [`Pattern::__clone`](pattern#__clone) : A PHP magic method called when cloning a pattern
- [`Pattern::getConfig`](pattern#getconfig) : Returns the pattern configuration
- [`Pattern::getUnits`](pattern#getunits) : Returns the pattern units
- [`Pattern::getHeight`](pattern#getheight) : Returns the pattern height
- [`Pattern::getWidth`](pattern#getwidth) : Returns the pattern width
- [`Pattern::getMessages`](pattern#getmessages) : Returns the pattern messages
- [`Pattern::getDebug`](pattern#getdebug) : Returns the pattern debug messages
- [`Pattern::getReplacements`](pattern#getreplacements) : Returns the pattern replacements
- [`Pattern::getClass`](pattern#getclass) : Alias for [`Pattern::requested`](#requested)
- [`Pattern::getClassChain`](pattern#getclasschain) : Returns the pattern class chain
- [`Pattern::getTranslationFiles`](pattern#gettranslationfiles) : Returns the pattern translation files
- [`Pattern::getSamplerModelConfig`](pattern#getsamplermodelconfig) : Returns the sampler model config
- [`Pattern::setPartMargin`](pattern#setpartmargin) : Sets the pattern part margin
- [`Pattern::setTranslator`](pattern#settranslator) : Sets the pattern translator
- [`Pattern::setUnits`](pattern#setunits) : Sets the pattern units
- [`Pattern::setPaperless`](pattern#setpaperless) : Sets whether or not it's a paperless pattern
- [`Pattern::addOptions`](pattern#addoptions) : Adds options to the pattern
- [`Pattern::loadParts`](pattern#loadparts) : Adds parts listed in the pattern config to the pattern
- [`Pattern::cleanUp`](pattern#cleanup) : Is called before wrapping up, does nothing by default
- [`Pattern::layout`](pattern#layout) : Handles the layout of the pattern parts
- [`Pattern::replace`](pattern#replace) : Adds a search/replace pair

## Methods patterns must implement

### sample

```php?start_inline=1
void sample(
    \Freesewing\Context $context
)
```

This is an abstract method, to be implemented by all childred of [`Pattern`](pattern).

#### Example
{:.no_toc}

{% include coreClassdocsFigure.html
    description="Sample should generate a minimal version of the pattern useful for sampling measurments and options"
    params="service=sample&pattern=AaronAshirt&mode=measurements"
%}

#### Typical use
{:.no_toc}

This will be called from [`SampleService::run`](../../services/sampleservice#run) to sample a pattern.

This will typically also be called form [`Pattern::draft`](pattern#draft) as a starting point 
for a complete pattern draft.

#### Parameters
{:.no_toc}

- [`Context`](../../src/context) `$context` : The full Freesewing context object

#### Return value
{:.no_toc}

This returns nothing. Changes will be made inside the [`Pattern`](pattern) object.

### draft

```php?start_inline=1
void draft(
    \Freesewing\Context $context
)
```

This is an abstract method, to be implemented by all childred of [`Pattern`](pattern).

#### Example
{:.no_toc}

{% include coreClassdocsFigure.html
    description="Draft should generate a full pattern draft with all bells and whistles"
    params="service=draft&pattern=AaronAshirt"
%}

#### Typical use
{:.no_toc}

This will be called from [`DraftService::run`](../../services/draftservice#run) to draft a pattern.

#### Parameters
{:.no_toc}

- [`Context`](../../src/context) `$context` : The full Freesewing context object

#### Return value
{:.no_toc}

This returns nothing. Changes will be made inside the [`Pattern`](pattern) object.

## Methods for pattern designers

### setOption

```php?start_inline=1
void setOption(
    string $name,
    mixed $value
)
```

Sets the option `$name` to `$value` in the options property, which is an `array` of name/value pairs.

> ##### Use [`pattern::setOptionIfUnset`](pattern#setoptionifunset) instead
>
> While you can use this method to set an option, it's typically better to use 
> the [`pattern::setOptionIfUnset`](pattern#setoptionifunset) method instead.
>
> The reason is that (future) patterns may extend your pattern and choose to set options
> to a different value. 
>
> This method will blindly override their choices (typically not a good thing)
> whereas [`pattern::setOptionIfUnset`](pattern#setoptionifunset) will only set the option if 
> it hasn't already been set (which is better).
{:.tip}

#### Typical use
{:.no_toc}

Used in patterns to set options that aren't depending on user input, and by the [`OptionsSampler`](../../src/optionssampler).

#### Parameters
{:.no_toc}

- `string` `$name` : Name of the option to set
- `mixed` `$value` : The value to set

### setOptionIfUnset

```php?start_inline=1
void setOptionIfUnset(
    string $name,
    mixed $value
)
```

Sets the option `$name` to `$value` in the options property 
(which is an `array` of name/value pairs)
, unless it already has an assigned value.

#### Typical use
{:.no_toc}

Used in patterns to set options that aren't depending on user input.

#### Parameters
{:.no_toc}

- `string` `$name` : Name of the option to set
- `mixed` `$value` : The value to set

### setValue

```php?start_inline=1
void setValue(
    string $name,
    mixed $value
)
```

Sets the value `$name` to `$value` in the values property, which is an `array` of name/value pairs.

> ##### Use [`pattern::setValueIfUnset`](pattern#setvalueifunset) instead
>
> While you can use this method to set a value, it's typically better to use 
> the [`pattern::setValueIfUnset`](pattern#setvalueifunset) method instead.
>
> The reason is that (future) patterns may extend your pattern and choose to set different values.
>
> This method will blindly override their choices (typically not a good thing)
> whereas [`pattern::setValueIfUnset`](pattern#setvalueifunset) will only set the value if 
> it hasn't already been set (which is better).
{:.tip}

#### Typical use
{:.no_toc}

Used in patterns to set values.

#### Parameters
{:.no_toc}

- `string` `$name` : Name of the value to set
- `mixed` `$value` : The value of the value to set

### setValueIfUnset

```php?start_inline=1
void setValueIfUnset(
    string $name,
    mixed $value
)
```

Sets the value `$name` to `$value` in the values property 
(which is an `array` of name/value pairs)
, unless it already has an assigned value.

#### Typical use
{:.no_toc}

Used in patterns to set values.

#### Parameters
{:.no_toc}

- `string` `$name` : Name of the value to set
- `mixed` `$value` : The value to set

### getOption

```php?start_inline=1
mixed getOption(
    string $name
)
```

Returns the option `$name`.

#### Typical use
{:.no_toc}

Used in patterns to retrieve an option, although it's more commonly used 
through its alias [`Pattern::o`](pattern#o).

#### Return value
{:.no_toc}

Whatever is stored in the option, typically an `int`, `float` or `string`.

### o

```php?start_inline=1
mixed o(
    string $name
)
```

An alias of [`Patterns::getOption`](pattern#getoption).

### getValue

```php?start_inline=1
mixed getValue(
    string $name
)
```

Returns the value `$name`.

#### Typical use
{:.no_toc}

Used in patterns to retrieve a value, although it's more commonly used 
through its alias [`Pattern::v`](pattern#v).

#### Return value
{:.no_toc}

Whatever is stored in the value, typically an `int`, `float` or `string`.

### v

```php?start_inline=1
mixed v(
    string $name
)
```

An alias of [`Patterns::getValue`](pattern#getvalue).

### t

```php?start_inline=1
string t(
    string $message
)
```

Returns the translation of `$message`.

This is just a front for the trans method in the Symfony Translator.

#### Typical use
{:.no_toc}

Called from a pattern when adding text.

#### Parameters
{:.no_toc}

- `string` `$message` : The string to translate

#### Return value
{:.no_toc}

Returns a translated `string`.

### stretchToScale

```php?start_inline=1
void stretchToScale(
    float $stretch
)
```

The way people measure stretch intuitively is different from the way I handle stretch in code.

When people say _25% stretch_ they mean that 10cm fabric should get stretched to 12.5cm fabric.
In code, that means a scale of 80% needs to be applied.

This method does that calcuation, it's as simple as:

```php?start_inline=true
return ( 1 / ( 1 + $stretch ) );
``` 


#### Typical use
{:.no_toc}

Used in patterns that use negative ease, aka stretch.

#### Parameters
{:.no_toc}

- `float` `$stretch` : The percentage of stretch as a number between 0 and 1.

### clonePoints

```php?start_inline=1
void clonePoints(
    string $from,
    string $into
)
```

Clones points from [`Part`](../../src/part) `$from` into [`Part`](../../src/part) `$into`.

Note that both parts need to exists in the `parts` property, which is an array of [`Part`](../../src/part) objects.

#### Typical use
{:.no_toc}

Used in patterns when building a part on the basis of another part.

#### Parameters
{:.no_toc}

- `string` `$from` : The [`Part`](../../src/part) object under key `$from` in the `parts` property to clone points from.
- `string` `$into` : The [`Part`](../../src/part) object under key `$into` in the `parts` property to clone points into.

### newPart

```php?start_inline=1
void newPart(
    string $name
)
```

Creates a new [`Part`](../../src/part) and adds it to the `parts` property under `$name`.

The `parts` property is an `array` of name/[`Part`](../../src/part) object pairs.

#### Typical use
{:.no_toc}

Called from [`Pattern::loadParts`](pattern#loadparts).

#### Parameters
{:.no_toc}

- `string` `$name` : The name of the part in the `parts` array.

### requested

```php?start_inline=1
string requested()
```

Returns the full namespaced name of the class/pattern requested by the user.

#### Typical use
{:.no_toc}

Called from patterns to determine whether we are in the pattern requested, or in a (grand)father pattern.

This is typically used for conditionale execution of code by comparing it to PHP's magic `__CLASS__` variable.
See [this blog post](/blog/where-are-we/) for more info.

### msg

```php?start_inline=1
void msg(
    string $message
)
```

Stores the message `$message` in the `messages` property.

#### Typical use
{:.no_toc}

Messages stored in the pattern are (by default) included in the SVG output as comments.
This is a handy way to include extra information in the pattern.

The default theme includes this information as comments at the end of the SVG file.

#### Example
{:.no_toc}

Here is a sample from the Simon Shirt:

```html
<!--

    After 22 attemps, collar opening is 0.1mm off.
    After 4 attemps, the sleeve head is -0.4mm off.
    After 28 attemps, the collar stand is 0.4mm off.
    After 3 attemps, the collar is -0.1mm off.
    
  -->
```

#### Parameters
{:.no_toc}

- `string` `$message` : The message to include

### dbg

```php?start_inline=1
void dbg(
    string $message
)
```

Stores the message `$message` in the `debug` property.

This behaves exactly as [`Pattern::msg`](pattern#msg), the difference is
in whether or not it will be included in the SVG file.

#### Typical use
{:.no_toc}

This is used to store debug messages in the pattern.
It is up to the theme designer to includ them SVG output as comments or not.

#### Example
{:.no_toc}

Here is a sample from the Simon Shirt:

```html
<!--

        DEBUG OUTPUT
    
    Sleeve tweak run 1. Sleeve head is -7.7649917638344mm off
    Sleeve tweak run 2. Sleeve head is -5.265807909828mm off
    Sleeve tweak run 3. Sleeve head is -2.8190108251431mm off
    
  -->
```

This debug information is useful for designers tweaking the way the 
sleeve is tweaked (in this example).

#### Parameters
{:.no_toc}

- `string` `$message` : The message to include as debug.

## Internal methods

### __construct

```php?start_inline=1
string getServiceName() 
```
If a pattern configuration file is present, this will load it into the `config` property.

This standard version will also load the following values into the `replace` property (an `array`):

- `__TITLE__` : **info &raquo; name** in the config file
- `__VERSION__` : **info &raquo; version** in the config file
- `__COMPANY__` : **info &raquo; company** in the config file
- `__AUTHOR__` : **info &raquo; author** in the config file
- `__DATE__` : The current date

If you'd like to replace more/different things, you can either implement your own constructor, 
or use [`Patterns::replace`](pattern#replace) to do so.

This also calls [`Patterns::loadParts`](pattern#loadparts) which will add all parts in the 
config file to the [`Pattern`](pattern) object.

#### Return value
{:.no_toc}

This is a constructor, so it returns a [`Pattern`](pattern) object.

### __clone

```php?start_inline=1
void __clone()
```

This is one of PHP's magic methods. When an [`Pattern`](pattern) object
is cloned, this method will be called on the freshly cloned object.

We use it to remove all parts in the pattern.

#### Typical use
{:.no_toc}

This is used by the sample service.

### getConfig

```php?start_inline=1
array getConfig()
```
Returns the pattern `config` property, which is an `array`.

#### Typical use
{:.no_toc}

The [`InfoService`](../../services/infoservice) uses this, as does [`Sampler`](../../src/sampler).

#### Return value
{:.no_toc}

An `array` with the data from the pattern configuration file.

### getUnits

```php?start_inline=1
string getUnits()
```

Returns the units of the pattern.

#### Return value
{:.no_toc}

A `string`, either `metric` or `imperial`.

### getHeight

```php?start_inline=1
float getHeight()
```

Returns the `height` property.

#### Typical use
{:.no_toc}

Used by the [`DraftService`](../../services/draftservice).

#### Return value
{:.no_toc}

A `float` which is the height of the pattern in mm.

### getWidth

```php?start_inline=1
float getWidth()
```

Returns the `width` property.

#### Typical use
{:.no_toc}

Used by the [`DraftService`](../../services/draftservice).

#### Return value
{:.no_toc}

A `float` which is the width of the pattern in mm.

### getMessages

```php?start_inline=1
string getMessages()
```

Returns the messages stored in the `messages` property.

The `messages` property is an array of messages. They will be imploded together with a newline character.

#### Typical use
{:.no_toc}

This is used to retrieve messages stored in the pattern. 
Typically for the theme to include them as comments in the SVG document.

#### Return value
{:.no_toc}

A `string` of all messages joined together by a newline character.

### getDebug

```php?start_inline=1
string getDebug()
```

Returns the debug messages stored in the `debug` property.

The `debug` property is an array of messages. They will be imploded together with a newline character.

#### Typical use
{:.no_toc}

This is used to retrieve debug messages stored in the pattern. 
Typically for the theme to include them as comments in the SVG document.

#### Return value
{:.no_toc}

A `string` of all debug messages joined together by a newline character.

### getReplacements

```php?start_inline=1
array getReplacements()
```

Returns the `replacements` property, which is an `array` of search/replace pairs.

#### Typical use
{:.no_toc}

Called from the [`DraftService`](../../services/draftservice) to replace strings in the SVG code.

#### Return value
{:.no_toc}

Returns an `array` of search/replace pairs.

### getClass

```php?start_inline=1
string getClass()
```

Alias for [`Pattern::requested`](#requested).

### getClassChain

```php?start_inline=1
array getClassChain()
```

Returns an array of classes between the actual pattern, and the abstract pattern class.
         
#### Typical use
{:.no_toc}
If a pattern extends another pattern (and another, and ...) this
will construct the class chain up to the abstract pattern class.
This is needed for the theme to load template files hierarchically
In other words, when you extend a pattern, this makes sure the templates
are extended too.

#### Return value
{:.no_toc}

Returns an `array` of directory paths.

### getTranslationFiles

```php?start_inline=1
array getTranslationFiles(
    string $locale,
    string $altloc
)
```

Returns an `array` of translation files.
         
#### Typical use
{:.no_toc}

Called from the [`Context`](../../src/context) class to add all translation
files to the translator.

#### Parameters
{:.no_toc}

- `string` `$locale` : The primary locale, a string like `en` or `de`
- `string` `$altloc` : The fallback locale, a string like `nl` or `it`

#### Return value
{:.no_toc}

Returns an `array` of translation files.

### getSamplerModelConfig

```php?start_inline=1
array getSamplerModelConfig()
```

Returns the sample model configuration.

Note that this will merge the sampler config of the pattern with its
(grand)parent pattern(s).
         
#### Typical use
{:.no_toc}

Called from the services classes.

#### Return value
{:.no_toc}

Returns an `array` with the sample configuration.

### setPartMargin

```php?start_inline=1
string setPartMargin(
    float $margin
)
```

Sets the `partMargin` property to `$margin`.

The `partMargin` property determines how much space is left between pattern parts.

#### Typical use
{:.no_toc}

This is used by the service classes to set the part margin to the value stored 
in the theme configuration file.

#### Parameters
{:.no_toc}

- `float` `$margin` : The part margin in mm.

### setTranslator

```php?start_inline=1
void setTranslator(
    \Symfony\Component\Translation\Translator $translator
)
```

Sets the `translator` property to `$translator`.

#### Typical use
{:.no_toc}

Called from a service classes to set the translator.

#### Parameters
{:.no_toc}

- `$translator` : A [Symfony translator](http://symfony.com/doc/current/translation.html) object.

### setUnits

```php?start_inline=1
void setUnits(
    array $units
)
```

Sets the `units` property to `$units`.

#### Typical use
{:.no_toc}

Called from a service classes to set the units.

#### Parameters
{:.no_toc}

- `array` `$units` : An array of units, like this: 

```php?start_inline=1
[
    'in' => 'metric', 
    'out' => 'imperial'
] 
```

### setPaperless

```php?start_inline=1
array setPaperless(
    bool $bool
)
```

Sets the `isPaperless` property to `$bool`.
         
#### Typical use
{:.no_toc}

This is used to determine whether to include the papeless-specific
stuff in the pattern.

#### Parameters
{:.no_toc}

- `bool` `$bool` : A boolean, either `true` or `false`.

### addOptions

```php?start_inline=1
void addOptions(
    array $options
)
```

Addes the contents of `$options`, an array of name/value pairs, to the `options` property.

This will not simply assign `$options` to the `options` property, but rather add every
element of `$options` to the `options` array.

This way, options that are already present will be preserved, unless they are also present
in `$options`, in which case they will be overwritten.

#### Typical use
{:.no_toc}

Called from the service classes to add user-supplied options to a pattern.

#### Parameters
{:.no_toc}

- `array` `$options` : An array of name/value pairs.

### loadParts

```php?start_inline=1
void loadParts()
```

Add parts listed in the pattern's config file to [`Pattern`](pattern) object.

This prevents you from having to manually add all parts.
Note that if there are parts you don't need
(depending on options for example) you could override this function. 
Or, you can simple call `setRender(false)` on them to keep them from being rendered.

_Add_ means, create a [`Part`](../../src/part) object, and add it to the `parts` 
property, which is an array of all parts.

This will also auto-set the part title and units for you.

#### Typical use
{:.no_toc}

Called from [`Pattern::__contruct`](pattern#__construct), the pattern constructor.

### cleanUp

```php?start_inline=1
void cleanUp()
```

Does nothing by default.

The `cleanUp` method is called on the [`Theme`](../../themes/core/theme), [`Pattern`](pattern), 
and [`Channel`](../../channels/core/channel) object before terminating a request.
It's a way to tie up any loose ends you may have, like open database connections and such things.

By default, it does nothing though.

#### Typical use
{:.no_toc}

Called from [`DraftService::run`](../../services/draftservice#run), [`SampleService::run`](../../services/sampleservice#run),
or [`CompareService::run`](../../services/compareservice#run).

### layout

```php?start_inline=1
void layout()
```

Creates a layout, fitting all parts on the pattern with the help of a [`Packer`](../../src/packer).

#### Typical use
{:.no_toc}

Called from the service classes prior to rendering.

### replace

```php?start_inline=1
void replace(
    string $search,
    string $replace
)
```

Adds a `$search`/`$replace` pair to the `replacements` property.

#### Typical use
{:.no_toc}

Replacements are used to replace certain strings late in the pipeline.
It's typically used for including things only known at runtime (like a date) 
into data included form disk, like theme template data.

#### Parameters
{:.no_toc}

- `string` `$search` : The string to look for
- `string` `$replace` : The string to replace it with

## See also
{% include classFooter.html %}
* TOC - Do not remove this line
{:toc}
