<img src="https://github.com/Supernova-Studio/sdk-typescript/blob/main/readme-icon.png?raw=true" alt="Supernova SDK" style="max-width:100%; margin-bottom: 20px;" />

# Supernova JS/TS SDK [![License](https://img.shields.io/badge/License-MIT-lightgray)](https://raw.githubusercontent.com/Supernova-Studio/sdk-typescript/main/LICENSE.md) [![SDK Status](https://img.shields.io/badge/SDK%20Status-Beta-blue)]() [![Version 1.7.0](https://img.shields.io/badge/Version-1.7.0-success)]()

The Supernova SDK provides convenient access to the [Supernova.io](https://supernova.io) platform. It allows you to build design system tooling without worrying about building entire necessary infrastructure like syncing data from Figma, Storybook, writing entire documentation systems etc. - instead, you get all of this out of the box, and you can use the data coming from your single source of truth for any other purpose.

## Features

SDK comes with all functionality (all will come with more once it reaches general release) you need to build design system tooling you previously only dreamt of:

- [x] Read API for `Tokens`, `Assets`, `Components`, `Design Components`
- [x] Read API for Account-level objects like `Workspaces`, `Design Systems`
- [x] Support for `single-branded` and `multi-branded` `Design Systems`
- [x] Read API for `Documentation`, including entire structure, content and configuration
- [x] Block or markdown `Documentation` representation
- [x] Rendering pipeline to export your assets as svg, png, pdf
- [x] Write Token API
- [ ] (coming) Version management
- [ ] (coming) Exporter management
- [ ] (coming) Integrations with awesome community tools (Figma Tokens, Style Dictionary, Raycast..)

And of course, features that make all developers happy!

- [x] Fully Typescript-enabled code
- [x] Heavily performance-optimized code and caching
- [x] Fully documented objects and attributes
- [x] Plug & play library design
- [x] Performant even for large design systems with lot of data
- [x] Extensive examples showing most of the capabilities
- [x] Unit tests

## Installing

To install, simply run:

```bash
npm install @supernovaio/supernova-sdk
```

Supernova SDK is made to run in both local (like Node.js) and remote environments (like browsers) so you can use it to develop your React sites or any kind of tool you would like to run as client-side app.

## Examples

We have prepared several examples to show you some things that are possible:

- [x] [Supernova + Gatsby Documentation Site Starter](https://github.com/Supernova-Studio/gatsby-documentation-site/)
- [x] [Supernova Gatsby Source Plugin](https://github.com/Supernova-Studio/gatsby-source-supernova)

We are working on a few more that will be available once SDK launches out of public beta. If you are interested in building something for Supernova as well, don't hesitate to [reach out](https://community.supernova.io) and talk with us!


## Obtaining API Key

Before you can use the SDK, you must obtain your personal access key from your [Supernova.io](https://supernova.io) profile. To do this, sign-in to [Supernova Cloud](https://cloud.supernova.io/), click on your profile picture > profile settings > authentication and generate a new key. [See more information](https://developers.supernova.io/getting-started#obtain-your-developer-key) if you need additional help.


## Access Rights

Usage of the SDK adheres to the [general access rights](https://learn.supernova.io/workspace/team-management/roles.html) that the account which generated the access key has. For example, if you are a viewer in a specific workspace, then you will not be able to do any change to that workspace through the SDK and error will be thrown instead. For vast majority of use-cases, being editor-level is enough however.


## Quick-start

It is time to put this SDK to good use! To start, import the main Supernova SDK object:

```typescript
import { Supernova } from "@supernovaio/supernova-sdk"
```

And instantiate it with your API key:

```typescript
const supernova = new Supernova(key, null, null) // Ignore 2 additional attributes for now
```

Supernova instance is used for several purposes, but the main one is to select specific design system you want to fetch the data from.

## How are data structured

All your design system data live under the following structure (that will match your specific setup, what versions you have created and so on):

```
Workspace 1
   Design System 1
      Version Draft
         Brand 1
         Brand 2
      Version 1.0
         Brand 1
         Brand 2
```

- `Workspace` is what you are a member of and is object that encapsulates everything, from your team to all design systems that were created under it. You can be a member of an unlimited number of workspaces, but in the majority of cases users are members of just a single one. You can have different roles and access rights under each of the workspaces you belong to.
- Every `Design System` belongs to one `Workspace` and contains things like design system configuration, integration settings and admin-related tools.
- Each `Design System` contains at minimum one active `Version` called `Shared Draft` - this version is where you write your data and is the only one which is allowed to be written to as well. 
- `Version` is data container for all design system information - like `Tokens`, `Assets`, `DesignComponents` and `Documentation`. 
- `Version` object has all methods you need to access every piece of data you have in Supernova design system.
- Each `Version` additionally contains `Brands`. As Supernova supports multi-branded design systems, `Brands` further separate data into data packs that only represent data belonging to one specific brand (for example, set of core tokens shared across all brands, but then subset of tokens specific only to that brand).
- There is always at least one `Brand` in each `Version`, even if multi-branded functionality is disabled.
- `Brand` object has all methods you need to access every piece of data you have in Supernova design system related to one specific brand.

To access the data, you first need to select which `Workspace`, `Design System` and `Version` (optionally `Brand`) you want to use.

## Using SDK

You can find full list of [what is possible, and how, here](https://github.com/Supernova-Studio/sdk-typescript/tree/main/docs).


## Resolution Caching

Supernova SDK works little bit differently than you would probably expect by default. Because of how complex the design system data is (as an example, a token can contain references to other tokens and can also be made of different types of tokens which can then be used in designComponents...) the SDK does all necessary resolution for you beforehand. 

To allow reuse of the data, the resolved results are cached and used in follow-up resolutions (for example, if you would first access your design tokens and then access documentation that uses the tokens, tokens would be reused). In many situations, this is what you want. However, there are some cases where this behavior is not what you want, like longer-running sessions:


### Longer-running sessions

If you are building a client-side app, your users might stay there for a long time without refreshing their session (like reloading a browser). This means the data will always come from the time of the first request as you are always using the same Supernova instance. To prevent this, you can disable cache layer completely:

```typescript
instance.setResolutionCacheEnabled(false)
```

With cache disabled, every request will download all necessary information for every request. This is not particulary efficient, especially for very-large design systems, but we put this option so you can control this behavior if you want to. Cache is enabled by default.


## What's next

As this is still beta, we are working hard on improving it before the first official release. The following are areas we'd like to solve before general release:

- [ ] DesignComponent API
- [ ] Write API
- [ ] Metadata API
- [ ] Automation triggers
- [ ] More examples


## Contributions

If you have additional ideas about how to make this project better, let us know by opening an issue! You can also open pull requests if you've worked on improving something yourself and would like to contribute back to the community. 

We will be reviewing feature-pull-requests on case-by-case basis, but in general, we are super open to your new ideas and we welcome them! And finally, thank you for your support! You are an amazing community.

Supernova Engineering Team