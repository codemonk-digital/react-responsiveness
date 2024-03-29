<div class="center">
<h1>React Responsiveness</h1>
</div>

**What** - Tiny plugin for working with responsiveness intervals, developed with a focus on ease of use (DX) and runtime performance.  
**Why** - I am a bit obsessed with both performance and ease of use. See [how it works](#how-it-works)

<p>
<a href="https://www.npmjs.com/package/react-responsiveness"><img src="https://img.shields.io/npm/dt/react-responsiveness.svg?color=f9d342&style=flat-square" alt="Total Downloads"></a>
<a href="https://www.npmjs.com/package/react-responsiveness"><img src="https://img.shields.io/npm/v/react-responsiveness.svg?color=f9d342&style=flat-square" alt="Latest Release"></a>
<a href="https://github.com/codemonk-digital/react-responsiveness/blob/main/LICENSE.MD"><img src="https://img.shields.io/npm/l/react-responsiveness.svg?color=f9d342&style=flat-square" alt="License"></a>
<a href="https://github.com/codemonk-digital/react-responsiveness/blob/main/package.json#L27"><img src="https://img.shields.io/badge/dependencies-0-f9d342.svg?style=flat-square" alt="Dependencies" /></a>
<a href="https://unpkg.com/react-responsiveness"><img src="https://img.badgesize.io/https://unpkg.com/react-responsiveness.svg?compression=gzip&label=umd:minzip&color=f9d342&style=flat-square" alt="unpkg umd min:gzip size" /></a>
<img src="https://img.shields.io/badge/SSR-ready-f9d342?style=flat-square" alt="SSR compatibility status"/>
<a href="https://makeapullrequest.com"><img src="https://img.shields.io/badge/PRs-welcome-f9d342.svg?style=flat-square" alt="PRs Welcome"/></a>
</p>

## Installation

#### yarn

```terminal
yarn add react-responsiveness
```

#### npm

```terminal
npm i react-responsiveness
```

## Demo

[codesandbox](https://codesandbox.io/p/sandbox/react-responsiveness-lx3789?file=/src/App.tsx).

## Usage

#### A) Add provider
<details>
    <summary>Example</summary>

```tsx
import { ResponsivenessProvider, Presets } from "react-responsiveness";

function App() {
    // ...
}

const WithResponsiveness = () => (
  <ResponsivenessProvider breakpoints={Presets.Bootstrap_5}>
    <App />
  </ResponsivenessProvider>
);
export default WithResponsiveness;
```

</details>

#### B) Use in any component
<details>
    <summary>Example</summary>

```tsx
import { useResponsiveness } from "react-responsiveness";

const { isMin, isMax, isOnly, currentInterval } = useResponsiveness()

return (<>
   <div>Current interval {currentInterval}</div>
   {isMin('md') && (
       // @media (min-width: 768px)
       <div>content...</div>
   )}
   {isMax('md') && (
       // @media (max-width: 991.9px)
       <div>content...</div>
   )}
   {isOnly('md') && (
       // @media (min-width: 768px) and (max-width: 991.9px)
       <div>content...</div>
   )}
</>)
```
</details>

## Available presets:

`Bootstrap_3`, `Bootstrap_4`, `Bootstrap_5`, `Bulma`, `Chakra`, `Foundation`, `Ionic`, `Material_Design`, `Materialize`, `Material_UI`, `Quasar`, `Semantic_UI`, `Skeleton`, `Tailwind_CSS`, `Windi_CSS`

_Notes:_ 
 - Default breakpoints value is **_already set_** to Bootstrap 5's [responsiveness breakpoints](https://getbootstrap.com/docs/5.3/layout/breakpoints/#available-breakpoints) preset.
<details>
    <summary>Preset details</summary>

```tsx
Presets.Bootstrap_5 = {
  xs: 0,
  sm: 576,
  md: 768,
  lg: 992,
  xl: 1200,
  xxl: 1400,
};
```
</details>

- If you maintain a CSS framework (or use one often) and want its preset added, [open an issue](https://github.com/codemonk-digital/react-responsiveness/issues) or a PR.
- If you spot any inaccuracy in [presets](https://github.com/codemonk-digital/react-responsiveness/blob/main/lib/presets.ts) (either typo or due to library update), please, let me know, I'll correct it.

## Can I add my own intervals? Of course:

```tsx
<ResponsivenessProvider
  breakpoints={{
    small: 0,
    medium: 777,
    large: 1234,
  }}
>
  // ...
</ResponsivenessProvider>
```
... can then be used as:
```tsx
import { useResponsiveness } from "react-responsiveness";

const { isOnly } = useResponsiveness()

return (<>
    {isOnly('medium') && (
        // @media (min-width: 777px) and (max-width: 1233.9px)
        <div>content...</div>
    )}
</>)
```
## F.A.Q.
<details>
    <summary>My IDE doesn't pick up the types for the package. Is there anything I can do about it?</summary>

I've noticed this weird problem in some codesandbox.io instances. 

I don't know why it happens but here's what you can do to fix it: create a `react-responsiveness.d.ts` file in `src/` folder, with the following content:

```ts
declare module "react-responsiveness" {
    export * from "react-responsiveness" 
}
```
This seems to fix TS.  
Another fix which sometimes works is to re-start the TS service.
</details>

## How it works:

- uses the native [`window.matchMedia(queryString)`](https://developer.mozilla.org/en-US/docs/Web/API/Window/matchMedia) and only reacts to changes in the query's `matches` value. It's the same API powering CSS media queries
- listeners are placed on the `MediaQueryList` instances, meaning they are garbage collected as soon as the app is unmounted, without leaving bound events behind on `<body>` or `window` object
- no global pollution
- in terms of memory and/or CPU consumption, listening to `window.matchMadia` 'change' events is a few hundred times lighter than using the _"traditional"_ `resize` event listener method

## Is the package working well for you?
- star the repo
- help [this answer](https://stackoverflow.com/a/76999879/1891677) get higher in the list

## Got issues?

[Let me know!](https://github.com/codemonk-digital/react-responsiveness/issues)
