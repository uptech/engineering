[![Netlify Status](https://api.netlify.com/api/v1/badges/0a744af4-6dd8-4fc2-b83f-95909a44bca4/deploy-status)](https://app.netlify.com/sites/uptechstudio-engineering/deploys)

# Uptech Studio's Engineering Space

Welcome to our Uptech Studio's Engineering Space, [https://engineering.uptechstudio.com](https://engineering.uptechstudio.com). It houses all of standards, best practices that we have honed in on over the years as well as our engineering blog and your playbook.

This is very much a living and breathing thing that is welcome to change over time. So if you have strong opinions about anything covered or not covered here and are willing to discuss & debate to try and find the best approaches. Please open a pull request to this repository with your proposed addition/modification.

## Hosted

The Uptech Studio Engineering Space is hosted via [Netlify][] at [https://engineering.uptechstudio.com](https://engineering.uptechstudio.com).

## Contribution Process

See [how to contribute](https://github.com/uptech/engineering/blob/main/CONTRIBUTING.md)

## Content

All the content for the Uptech Studio Engineering Space is written in [CommonMark][] a formalized Markdown standard and is converted into a static site using the [Zola][] static site generator.

### Blog Posts & Docs

Our Engineering Space is split into two main sections. The Blog and our Docs. These two sections are clearly represented in our `blog` and `docs` subfolders within `content`.

#### Types of Content

Each of these sections house a few different types of content.

- **hierarchy** - this is done by adding a folder that other pages will live in. To do this simply create a folder with the slug (dashed name) you want. Then inside that folder create a `_index.md` file with the front-matter. The `content/docs/agile-development/_index.md` is an example of this type of content. It is there to add hiearchy to the organization of the information.
- **page** - this is done by creating a folder with the slug (dashed name) you want. Then inside that folder create a `index.md` file with the front-matter. The `content/docs/agile-development/demos/index.md` file is an example of this type of content. It is here to house the actual page contents in [CommonMark][] as well as the associated meta-data. This folder also houses local assets (images, etc.)

#### Images/etc.

Images and other files that you want to use in your Markdown or share should be placed in the folder for that page. You can look at the `squads-and-guilds` page folder as it has images. These images are unique to this page.

If on the other hand you have images/files that are shared across multiple pages you can put them in the top level `static` folder. Try and have some useful naming and organization there though.

Images should be optimized using [ImageOptim](https://imageoptim.com/howto.html) before adding them to the git repository.

## Test Changes Locally

Given that this repository is statically generated you will need to install the static site generator, [Zola][]. Don't worry though it is a single binary with no dependencies. So it is as easy as the following.

	brew install zola

Simply add the folders and files you want or make changes to existing files under `content` and run the following:

	zola serve

From there you can then see the site locally at [http://localhost:1111](http://localhost:1111).

It is also worth noting that `zola serve` will watch the files for changes and re-render for you. So generally you can just leave `zola serve` running and work on whatever changes you are making.

## Deployment

Build and deployment of this site will happen automatically through Netlify so you don't need to worry about it.

## License

Copyright © 2016 - 2022 UpTech Works, LLC. All rights reserved.

[CommonMark]: https://commonmark.org
[Zola]: https://www.getzola.org
[Netlify]: https://netlify.com
