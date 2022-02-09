# Engineering Playbook

Welcome to our Engineering Playbook. It houses all of standards and best practices that we have honed in on over the years.

This is very much a living and breathing thing that is welcome to change over time. So if you have strong opinions about any covered or not covered here and are willing to discuss & debate to try and find the ideal path. Please open a Pull Request to this repository with your proposed addition/modification.

## Contribution Process

It is simple you just open a Pull Request with your proposed change and debate and discussion happens around your proposed change until a conclusion is made. It might be your exact change that makes it in, or it might be a varient of your change that makes it in, or maybe your change gets rejected with an explanation of why. You are always welcome to continue discussions / re-open topics that have been rejected if you discover more information or learnings that help support your position.

## Content

All the content for the Engineering Playbook is written in [CommonMark][] a formalized Markdown standard and is converted into a static site using the [Zola][] static site generator.

In terms of content contribution you should only have to worry about the `content` directory and the folders and files within it.

### Types of Content

There are a few different types of content.

- **hierarchy** - this is done by adding a folder that other pages will live in. To do this simply create a folder with the slug (dashed name) you want. Then inside that folder create a `_index.md` file with the front-matter. The `content/TODO/_index.md` is an example of this type of content. It is there to add hiearchy to the organization of the information.
- **page** - this is done by creating a folder with the slug (dashed name) you want. Then inside that folder create a `index.md` file with the front-matter. The `content/git-commit/index.md` file is an example of this type of content. It is here to house the actual page contents in [CommonMark][] as well as the associated meta-data.

### Images/etc.

Images and other files that you want to use in your Markdown or share should be placed in the folder for that page. You can look at the `squads-and-guilds` page folder as it has images. These images are unique to this page.

If on the other hand you have images/files that are shared across multiple pages you can put them in the top level `static` folder. Try and have some useful naming and organization there though.

Images should be optimized using [ImageOptim](https://imageoptim.com/howto.html) before adding them to the git repository.

## Test Changes Locally

Given that this repository is statically generator you will need to install the static site generator, [Zola][]. Don't worry though it is a single binary with no dependencies. So it is as easy as the following.

	brew install zola

Simply add the folders and files you want or make changes to existing files under `content` and run the following:

	zola serve

From there you can then see the site locally at [http://localhost:1111](http://localhost:1111).

It is also worth noting that `zola serve` will watch the files for changes and re-render for you. So generally you can just leave `zola serve` running and work on whatever changes you are making.

## Deployment

Build and deployment of this site will happen automatically through Netlify so you don't need to worry about it.

[CommonMark]: https://commonmark.org
[Zola]: https://www.getzola.org
