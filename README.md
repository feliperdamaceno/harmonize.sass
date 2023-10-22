# Harmonize.Sass

The Harmonize.Sass is a personal project aimed at providing a centralized and efficient way to manage design tokens, generate utility classes, and work with media queries in your Sass-based projects. This library streamlines your design system and improves the maintainability of the styles.

Inspired by [Andy Bell's Gorko](https://github.com/Andy-set-studio/gorko).

## Table of contents

- [Installation](#installation)
- [Token Configuration](#token-configuration)
- [Functions and Mixins](#functions-and-mixins)
- [Generators](#generators)
- [Folder Structure](#folder-structure)
- [License](#License)

## Installation

1. Install Harmonize.sass via npm:

```bash
npm install harmonize.sass
```

2. Import it into your main Sass file:

```scss
@use 'harmonize.sass';
```

## Token Configuration

Below is the default configuration of the library. Use it as a base for your own configuration. The idea is to have a foundation to start your project.

```scss
/*
  BASE SIZE:
  All calculations are based on this value. The default is 1.6rem because on 
  the root 1rem is set to be equal to 10px, so that, 1.6rem is equivalent to 
  the default 16px.
*/
$base-size: 1.6rem !default;

/*
  DEFAULT SCALES:
  These are the default scales that could be used to empower all the sizing in 
  the library. It could be extended as required.
*/
$default-scales: (
  linear: (
    100: 0.2rem,
    200: 0.4rem,
    300: 0.8rem,
    400: 1.6rem,
    500: 3.2rem,
    600: 6.4rem,
    700: 12.8rem,
    800: 2.56rem,
    900: 5.12rem
  ),
  minor: (
    100: $base-size * 0.579,
    200: $base-size * 0.694,
    300: $base-size * 0.833,
    400: $base-size,
    500: $base-size * 1.2,
    600: $base-size * 1.44,
    700: $base-size * 1.728,
    800: $base-size * 2.074,
    900: $base-size * 2.488
  ),
  major: (
    100: $base-size * 0.512,
    200: $base-size * 0.64,
    300: $base-size * 0.8,
    400: $base-size,
    500: $base-size * 1.25,
    600: $base-size * 1.563,
    700: $base-size * 1.953,
    800: $base-size * 2.441,
    900: $base-size * 3.052
  )
) !default;

/*
  BASE SCALE:
  These settings empower all the utilities related to (font-size, margin,  
  padding, etc). Configure based on the scale options above.
*/
$base-scale: map-get($default-scales, 'major') !default;

/*
  GLOBAL COLORS:
  Colors that are going to be used throughout utilities like backgrounds, 
  texts, borders, etc. It can be extended as required.
*/
$base-colors: (
  primary: (
    100: hsl(204, 94%, 94%),
    200: hsl(201, 94%, 86%),
    300: hsl(199, 95%, 74%),
    400: hsl(198, 93%, 60%),
    500: hsl(200, 98%, 39%),
    900: hsl(202, 80%, 24%)
  ),
  secondary: (
    100: hsl(149, 80%, 90%),
    200: hsl(152, 76%, 80%),
    300: hsl(156, 72%, 67%),
    400: hsl(158, 64%, 52%),
    500: hsl(160, 84%, 39%),
    900: hsl(164, 86%, 16%)
  ),
  neutral: (
    100: hsl(240, 5%, 96%),
    200: hsl(240, 6%, 90%),
    300: hsl(240, 5%, 84%),
    400: hsl(240, 5%, 65%),
    900: hsl(240, 6%, 10%)
  )
) !default;

/*
  GLOBAL TOKENS:
  These settings control all the custom properties and utilities that could be 
  generated.
*/
$global-tokens: (
  font-family: (
    value: (
      regular: (
        system-ui,
        -apple-system,
        BlinkMacSystemFont,
        'Segoe UI',
        Roboto,
        Oxygen,
        Ubuntu,
        Cantarell,
        'Open Sans',
        'Helvetica Neue',
        sans-serif
      )
    ),
    prefix: (
      font: 'font-family'
    ),
    property: 'font-family'
  ),
  font-size: (
    value: $base-scale,
    prefix: (
      text: 'font-size'
    ),
    property: 'font-size'
  ),
  font-weight: (
    value: (
      light: 300,
      regular: 400,
      medium: 500,
      semibold: 600,
      bold: 700
    ),
    prefix: (
      font: 'font-weight'
    ),
    property: 'font-weight'
  ),
  letter-spacing: (
    value: (
      narrower: -0.05em,
      narrow: -0.025em,
      regular: 0em,
      wide: 0.025em,
      wider: 0.05em
    ),
    prefix: (
      font: 'letter-spacing'
    ),
    property: 'letter-spacing'
  ),
  line-height: (
    value: (
      heading: 1.1,
      regular: 1.5
    ),
    prefix: (
      leading: 'line-height'
    ),
    property: 'line-height'
  ),
  text-align: (
    value: (
      start: left,
      center: center,
      end: right
    ),
    prefix: (
      text: 'text-align'
    ),
    property: 'text-align'
  ),
  text-transform: (
    value: (
      uppercase: uppercase,
      lowercase: lowercase,
      capitalize: capitalize
    ),
    prefix: (
      text: 'text-transform'
    ),
    property: 'text-transform'
  ),
  measure: (
    value: (
      compact: 45ch,
      ideal: 66ch,
      longform: 75ch
    ),
    prefix: (
      measure: 'max-width'
    ),
    property: 'max-width'
  ),
  padding: (
    value: $base-scale,
    prefix: (
      p: 'padding',
      pt: 'padding-top',
      pb: 'padding-bottom',
      pl: 'padding-left',
      pr: 'padding-right',
      px: 'padding-inline',
      py: 'padding-block'
    ),
    property: 'padding'
  ),
  margin: (
    value: map-merge(
        $base-scale,
        (
          auto: auto
        )
      ),
    prefix: (
      m: 'margin',
      mt: 'margin-top',
      mb: 'margin-bottom',
      ml: 'margin-left',
      mr: 'margin-right',
      mx: 'margin-inline',
      my: 'margin-block'
    ),
    property: 'margin'
  ),
  gap: (
    value: map-get($default-scales, 'linear'),
    prefix: (
      gap: 'gap'
    ),
    property: 'gap'
  ),
  border-width: (
    value: map-get($default-scales, 'linear'),
    prefix: (
      border: 'border-width'
    ),
    property: 'border-width'
  ),
  border-style: (
    value: (
      solid: solid,
      dashed: dashed,
      dotted: dotted
    ),
    prefix: (
      border: 'border-style'
    ),
    property: 'border-style'
  ),
  border-radius: (
    value: map-merge(
        map-get($default-scales, 'linear'),
        (
          full: 999rem
        )
      ),
    prefix: (
      rounded: 'border-radius'
    ),
    property: 'border-radius'
  ),
  shadow: (
    value: (
      xs: 0 1px 2px 0 var(--shadow-color),
      sm: (
        0 1px 3px 0 var(--shadow-color),
        0 1px 2px -1px var(--shadow-color)
      ),
      md: (
        0 4px 6px -1px var(--shadow-color),
        0 2px 4px -2px var(--shadow-color)
      ),
      lg: (
        0 10px 15px -3px var(--shadow-color),
        0 4px 6px -4px var(--shadow-color)
      ),
      xl: (
        0 20px 25px -5px var(--shadow-color),
        0 8px 10px -6px var(--shadow-color)
      )
    ),
    prefix: (
      shadow: 'box-shadow'
    ),
    property: 'box-shadow'
  ),
  transition-timing: (
    value: (
      linear: linear,
      in: cubic-bezier(0.4, 0, 1, 1),
      out: cubic-bezier(0, 0, 0.2, 1),
      in-out: cubic-bezier(0.4, 0, 0.2, 1)
    ),
    property: 'transition-timing-function'
  ),
  breakpoints: (
    value: (
      sm: 36em,
      md: 48em,
      lg: 62em
    )
  )
) !default;
```

### Base size

`$base-size`

All calculations are based on this value. The default is 1.6rem because on the root, 1rem is set to be equal to 10px, so 1.6rem is equivalent to the default 16px.

### Default scales

`$default-scales`

These are the default scales that could be used to empower all the sizing in the library. It could be extended as required.

These values can be accessed via the `useSize` hook function.

### Colors

`$base-colors`

Colors that are going to be used throughout utilities like backgrounds, texts, borders, etc. It can be extended as required.

These values can be accessed via the `useColor` hook function.

### Breakpoints

The `breakpoints` map is used in the `useMediaQuery` mixin.

```scss
breakpoints: (
  value: (
    sm: 36em,
    md: 48em,
    lg: 62em
  )
);
```

## Functions and Mixins

In order to use the helper functions and mixins, just use the `@use` rule to import helpers in your SCSS file.

```scss
@use 'harmonize.sass/helpers' as *;
```

Then use one of the following options:

`useColor('color', 'shade')`

This function allows you to retrieve and return colors from the token configuration. Provide a color key as an argument to get the corresponding color value.

#### Example:

```scss
body {
  color: useColor('primary', 400);
  background-color: useColor('neutral', 100);
}
```

`useSize('size')`

This function allows you to retrieve and return one of the base sizes configured in the token configuration. Accept the size as a parameter and return the corresponding size value.

#### Example:

```scss
.rectangle {
  width: useSize(800);
  height: useSize(400);
}
```

`useValue('property', 'value')`

Use this function to retrieve a value by specifying a property name from the token configuration.

#### Example:

```scss
button {
  padding-block: useValue('padding', 200);
  padding-inline: useValue('padding', 400);
}
```

`useProperty('property', 'value')`

This mixin returns the CSS property and value associated with the specified property name in the token configuration. This is only a more pragmatic way to use the token values.

#### Example:

```scss
.card {
  // Other Styles...
  @include usePropery('shadow', 'md');
}
```

#### Output:

```css
.card {
  /* Other Styles... */
  box-shadow: 0 4px 6px -1px var(--shadow-color), 0 2px 4px -2px var(--shadow-color);
}
```

`useMediaQuery('breakpoint')`

The `useMediaQuery` mixin accepts a parameter with the corresponding breakpoint from the tokens. It returns a media query that you can use in your styles.

#### Example:

```scss
@include useMediaQuery('lg') {
  body {
    font-size: useValue('font-size', 500);
  }
}
```

## Generators

Generators are mixins that loop through the given property from the tokens configuration and return the corresponding utility classes or custom properties.

In order to use the generator, just use the `@use` rule to import helpers into your SCSS file.

```scss
@use 'harmonize.sass/helpers' as *;
```

### Prefix

The `prefix` setting is used to name the classes you're generating from; it also takes as a value on the configuration token the name of the corresponding property. The naming convention of the prefixes is free to be used; just make sure to separate them by a hyphen `-` character, and you should be good to go.

#### Examples

Below is the configuration used for the `padding` property. As you can see, it has a list of prefixes that are used to generate a bunch of utility classes.

```scss
padding: (
  value: $base-scale,
  prefix: (
    p: 'padding',
    pt: 'padding-top',
    pb: 'padding-bottom',
    pl: 'padding-left',
    pr: 'padding-right',
    px: 'padding-inline',
    py: 'padding-block'
  ),
  property: 'padding'
);
```

### Utilities Generator

Using the `generateUtilities` mixin, you can generate a list of utility classes by passing the property you want to generate the classes from.

#### Example:

```scss
@include generateUtilities('padding');
```

#### Example Outputs:

```css
.p-400 {
  padding: 1.6rem;
}

.pt-400 {
  padding-top: 1.6rem;
}

.py-400 {
  padding-block: 1.6rem;
}
```

### Color Utilities Generator

Similar to `generateUtilities`, the `generateColorUtilities` mixin generates the custom utility classes based on the colors set up in the `$base-colors` map under tokens configuration.

However, this mixin takes two arguments: the first one being the `prefix` you want to use in your class names, and the second one is the `property` you want to generate the colors for.

#### Examples:

```scss
@include generateColorUtilities('text', 'color');

@include generateColorUtilities('bg', 'background-color');
```

#### Example Outputs:

```css
.text-primary-400 {
  color: hsl(198, 93%, 60%);
}

.bg-secondary-400 {
  background-color: hsl(158, 64%, 52%);
}
```

This allows you to have more control in creating your color utilities.

### Custom Properties Generator

The `generateCustomProperties` mixin accepts the `prefix` and the `property` parameters to generate custom properties. For that, it is recommended to use the corresponding mixin inside the `:root` pseudo-class in the `root.scss` file in your base folder.

#### Example:

```scss
@include generateCustomProperties('fs', 'font-size');
```

#### Output:

```css
:root {
  --fs-100: 0.8192rem;
  --fs-200: 1.024rem;
  --fs-300: 1.28rem;
  --fs-400: 1.6rem;
  --fs-500: 2rem;
  --fs-600: 2.5008rem;
  --fs-700: 3.1248rem;
  --fs-800: 3.9056rem;
  --fs-900: 4.8832rem;
}
```

### Color Custom Properties Generator

Like the previous mixin, the `generateColorCustomProperties` generates custom properties from the `$base-colors` map in the tokens configuration. It accepts only one argument, which is the `prefix` to be used in the naming convention.

#### Example:

```scss
@include generateColorCustomProperties('clr');
```

#### Output:

```css
:root {
  --clr-primary-100: hsl(204, 94%, 94%);
  --clr-primary-200: hsl(201, 94%, 86%);
  --clr-primary-300: hsl(199, 95%, 74%);
  --clr-primary-400: hsl(198, 93%, 60%);
  --clr-primary-500: hsl(200, 98%, 39%);
  --clr-primary-900: hsl(202, 80%, 24%);

  --clr-secondary-100: hsl(149, 80%, 90%);
  --clr-secondary-200: hsl(152, 76%, 80%);
  --clr-secondary-300: hsl(156, 72%, 67%);
  --clr-secondary-400: hsl(158, 64%, 52%);
  --clr-secondary-500: hsl(160, 84%, 39%);
  --clr-secondary-900: hsl(164, 86%, 16%);

  --clr-neutral-100: hsl(240, 5%, 96%);
  --clr-neutral-200: hsl(240, 6%, 90%);
  --clr-neutral-300: hsl(240, 5%, 84%);
  --clr-neutral-400: hsl(240, 5%, 65%);
  --clr-neutral-900: hsl(240, 6%, 10%);
}
```

## Folder Structure

This library follows a very simple folder structure, based on a mix of the [CUBE CSS](https://cube.fyi) methodology and personal preference, making it quite opinionated.

### base

Inside this folder, you'll find the `base.scss` file, which is intended to create a foundation for your CSS code, where you define your `<h1>` sizes, basic `<button>` styles, and so on.

There is also the `reset.scss` file, which contains a highly opinionated CSS reset based on the [Modern CSS Reset](https://andy-bell.co.uk/a-modern-css-reset) with some personal modifications.

Finally, there is our `root.scss` file, where you create your custom CSS properties to be used throughout your whole application.

### blocks

This is where the CUBE CSS methodology comes into play. This folder is intended to create your block styles, essentially classes like `.card`, `.pill`, `.modal`, etc.

For a more in-depth explanation of this topic, please read the [Block Section](https://cube.fyi/block.html) in the CUBE CSS documentation.

### compositions

Similar to blocks, this folder was created based on the CUBE CSS ideals. Compositions are where you create your single principle utilities that take care of the overall layout of your website/application.

By default, there are some base compositions already created based on the [Every Layout](https://every-layout.dev) book by Andy Bell and Heydon Pickering, which you can read to learn more about what each of those compositions does.

### helpers

This folder should not be touched at any time. Here you'll find the backbone of the library: generators, functions, mixins, and much more. You'll import this folder very often if you want to use such helpers via the `@use` keyword.

### utilities

This folder is where you create your utility files containing your generated utility classes. The idea is to separate each type of utility by file and then `@forward` them in the `index.scss` file.

Inside this folder, you'll find, by default, a bunch of pre-made utilities for you to use. However, feel free to remove and create your own utilities as desired.

Keep in mind that generating a bunch of classes without using them could cause your final `.css` file to be gigantic. To avoid this, make sure to use some kind of CSS processor like `PostCSS` to remove unused classes.

## License

This is an open-source library and is available under the [**MIT License**](LICENSE). You are free to use, modify, and distribute the code in accordance with the terms of the license.

## Contributors

Contributions are highly appreciated! If you encounter any issues or have suggestions for improvements, please feel free to open an issue or submit a pull request.

[feliperdamaceno](https://github.com/feliperdamaceno)

## Contact me

Linkedin: [feliperdamaceno](https://www.linkedin.com/in/feliperdamaceno)
