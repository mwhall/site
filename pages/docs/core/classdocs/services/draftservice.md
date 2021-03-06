---
layout: page
title: DraftService
namespace: Freesewing\Services
tags: [class docs]
permalink: /docs/core/classdocs/services/draftservice
---
## Description 

The [`DraftService`](draftservice) class handles the draft service, 
which generates pattern drafts. 
It is the core business of Freesewing.

## Example

{% include coreClassdocsFigure.html
    description="An example of the draft service"
    params="?service=draft&pattern=AaronAshirt"
%}


## Typical use

The [`DraftService`](draftservice) is used to draft patterns. It's probably what
you think of when you think about Freesewing. 

## Constants

### SCALE

By default, SVG displays 90 dots per inch. 
Internally, Freesewing works with millimeters.
To align these two, the value of `SCALE` is : 90dpi/25.4 = `3.54330709`.

If we do nothing, 1mm holds 3.54330709 SVG units. 
So we wrap your entire pattern draft in an SVG transform that
scales everything up by 3.54330709, so that now 1mm holds 1 SVG unit.

## Public methods

### getServiceName

```php?start_inline=1
string getServiceName() 
```
Returns the name of the service, which is a `string`. More precisely, this returns `draft`.

### run

```php?start_inline=1
void run(
    \Freesewing\Context $context
) 
```
The `run` method drafts a pattern, sets the response and sends it.
Essentially, it takes care of the entire remainder of the request.

While doing so, it takes care of a number of things:

- It asks the [`Channel`](../channels/core/channel) wether this is a valid [`Request`](../src/request)
- It asks the [`Channel`](../channels/core/channel) to standardize the [`Model`](../src/model) measurements and options
- It adds units, a translator, [`Pattern`](../patterns/core/pattern), [`Model`](../src/model), 
[`SvgDocument`](../src/svgdocument), and [`SvgRenderbot`](../src/svgrenderbot) to the [`Context`](../src/context)
- It calls [`Pattern::draft`](../patterns/core/pattern#draft)
- It calls [`Response::send`](../src/response#send)
- It calls [`Context::cleanUp`](../src/context#cleanup)

#### Typical use
{:.no_toc}

Always called from [`Context::runService`](../src/context#runservice).

#### Parameters
{:.no_toc}

- [`Context`](../src/context) `$context` : The [`Context`](../src/context) object


## See also
{% include classFooter.html %}
* TOC - Do not remove this line
{:toc}
