# ✨ Collaborative GatsbyJS+Stackbit+NetlifyCMS blog and docs theme ✨

[![Netlify Status](https://api.netlify.com/api/v1/badges/4cdabbc7-6c08-4d93-b66f-a930194cd871/deploy-status)](https://app.netlify.com/sites/stackbit-gatsby-theme-diy-libris/deploys)

<img src="https://themes.stackbit.com/images/diy-demo-1024x768.png" width="600">

Type: Gatsby for Stackbit starter.\
Features: Showcase + blog + docs + collaborative open authoring via NetlifyCMS.\
Content storage: git + Markdown.

This is a [Gatsby](https://gatsbyjs.com) theme using Git as a [CMS](https://en.wikipedia.org/wiki/Content_management_system) that can be imported in for [Stackbit Studio](https://www.stackbit.com?utm_source=project-readme&utm_medium=referral&utm_campaign=user_themes)'s visual editor. Every pages are visually editable with Stackbit Studio, and blog posts and docs are also editable by external contributors through GitHub via NetlifyCMS (the main repo will get a pull request for review). Stackbit studio can edit virtually any page and any content as long as its defined in stackbit.yml. Further customization through raw files editing or Gatsby plugins is possible.

This theme combines the [DIY theme](https://github.com/stackbithq/stackbit-theme-diy), which provides more extensive customization than other themes, and detailed blog posts with tags, categories, author and frontmatter image positioning, with the [Libris theme](https://github.com/stackbithq/stackbit-theme-libris)'s docs section, providing a MDX-based documentation that can also be edited in Stackbit Studio's visual editor.

This theme further includes additional plugins and edits to make the site collaborative and editable to external contributors through Git pull requests and optionally using NetlifyCMS in Open Authoring mode as a visual editor for docs and blog posts for non-technical contributors. Indeed, Stackbit Studio can visually edit anything, but invited contributors will have the same permissions as the admin, whereas with NetlifyCMS external contributions will show up as Pull Requests for further review before merging in.

It's a full Gatsby website, it can be used as a starter without Stackbit (NetlifyCMS will still work), or it can be imported into Stackbit to leverage Stackbit Studio's visual editor for the whole website.

## Preview

[Click here for a preview on Netlify](https://stackbit-gatsby-theme-diy-libris.netlify.app/).

## Install

### Locally
Install node.js (preferably version 14.15.1). Install git.

Then launch a git commandline, clone this repository somewhere, cd inside, and then type:

`npm install && npm run build && gatsby develop`

This will install gatsby, install the necessary plugins and build the gatsby website (and search index), and then launch a local development server (default port: 8000, hence open the address http://localhost:8000 to access the website locally once gatsby is running).

Note that both visual editors (Stackbit Studio and NetlifyCMS) only work when the website is deployed. Locally it's only possible to edit the raw files (but with instant feedback thanks to `gatsby develop`), although there is a [Beta feature on NetlifyCMS to allow for local git editing](https://www.netlifycms.org/docs/beta-features/#working-with-a-local-git-repository). If you try to access `/admin/`, NetlifyCMS will launch but all the pages shown will reflect the online git repository, not the local one, beware not to be bitten by this.

All content and pages are in markdown format and hence can be edited by any raw text editor. Tools such as [MarkText](https://marktext.app/) may by useful to write drafts.

If you wish to edit the website both locally and through Stackbit (i.e., other editors use Stackbit Studio visual editor and you edit the raw files locally), do not forget to work on the `preview` branch (`git checkout preview`) and push your changes there first (`git push origin preview`) so that they get reflected in Stackbit Studio. You can then `git checkout master; git merge preview; git push origin master; git checkout preview` to push the same updates to master and then get back to the `preview` branch.

### Deployment online
You can create a site on Stackbit and import this repository to kickstart your own showcase+documentation website.

If you want to allow visitors to edit the website's docs and blogs posts (via the open_authoring option combined with the editorial_workflow of NetlifyCMS), you will need to

1. host your website on GitHub, although there are alternative ways through Identity and Git-Gateway (see [here](https://www.netlifycms.org/docs/open-authoring/) for the details about open_authoring and [here to enable Netlify Identity and Git-Gateway](https://docs.netlify.com/visitor-access/git-gateway/#setup-and-settings)).
2. You also need to edit `/static/admin/config.yml` to update the `repo` field to point to your github repository's address (only the username and repo name, not the entire URL) and `site_repo` to point to the public website URL.
3. And finally, you will need to [create a new OAuth on your GitHub account](https://github.com/settings/applications/new) with the authorization callback URL set to `https://api.netlify.com/auth/done` and then input the client ID and secret key into Netlify as outlined in [this great article](https://www.stackbit.com/blog/jamstack-documentation-sites/).

If you want to transfer a Netlify instance from one repo to another (eg, relinking from a personal repository to an organization repository), then the following must be done:

1. Change the repository address in the Netlify's Site Settings > Build & Deploy > Continuous Deployment > Build settings, click on Edit settings. The organization repositories won't show up at first, you need to scroll down and click on the button to connect Netlify to a new account, then select the GitHub organization, and then the org's repositories should display. Select the appropriate one.
2. Relink the Netlify Identity providen to the new repository, by going into Site Settings > Identity > Services, then click the Disable Git Gateway button to force delinking to the old repository, then click on Create Git Gateway to create a new one that will be automatically linked to the new repository (because it syncs with the change at step 1 above).
3. Create a new OAuth app on GitHub for the Organization. For some reason, the "Application" setting doesn't show up for organizations anymore, but it's possible to access it manually via: `https://github.com/organizations/[my-org]/settings/applications` (change [my-org] to your organization's name). Here are the required settings:
    * Application Name: whatever you want to name it.
    * Homepage URL: the URL to the Netlify built website, the one visitors see. Eg, `https://my-awesome-website.netlify.app/`
    * Application description: whatever you want to remember what this app is about.
    * Authorization callback URL: `https://api.netlify.com/auth/done`
4. After creating the OAuth app, it should open the app's options (otherwise, go to `https://github.com/organizations/[my-org[/settings/applications/`) and allow to create a new clients secrets keys. Create one, and then copy it along with the ID to Netlify's Site settings > Access Control > OAuth. If there is already a GitHub entry, delete it, because only one entry per hoster can be saved, and create a new one.
4. Edit `/static/admin/config.yml` to update the `repo` field to point to your github repository's address, and `site_repo` to point to the public website URL. If necessary, change also the `branch` field if you don't deploy from the `master` branch but from another branch (eg, `main`).
5. If you were connected in the admin panel of NetlifyCMS, disconnect, clear cookies, and reconnect. NetlifyCMS should now ask you to grant a new authorization to the repository using the organization account to the OAuth app you created above, the name you chose for the OAuth app should be displayed. Accept, and you are done!

This last step is crucial, otherwise you will get an error message:

`Failed to persist entry: API_ERROR: Although you appear to have the correct authorization credentials, the `My-Org` organization has enabled OAuth App access restrictions, meaning that data access to third-parties is limited. For more information on these restrictions, including how to enable this app, visit https://docs.github.com/articles/restricting-access-to-your-organization-s-data/`

If you encounter other errors with NetlifyCMS, see [these tips](https://answers.netlify.com/t/common-issue-netlify-cms-api-error-now-what/4624).

## Additional features
Beyond the merge between the DIY and Libris themes of Stackbit to provide docs inside the DIY (showcase+blog) theme, here are a few additional features that were implemented:

* Automatic code syntax highlighting in MDX files using [gatsby-remark-highlight-code](https://www.gatsbyjs.com/plugins/gatsby-remark-highlight-code/) (Prism was disabled by default, and it required specifying the language).
* Site-wide local search engine using [gatsby-plugin-lunr](https://github.com/humanseelabs/gatsby-plugin-lunr).
* Easy collaborative editing of docs and blog posts via NetlifyCMS visual editor. If you want to disable, you can remove the netlifyCMS plugin (`gatsby-plugin-netlify-cms`) in `gatsby-config.js` and remove the "Edit this page" buttons in `src/templates/docs.js` and `src/templates/post.js`.
* Added `short_bio` field for authors to display at the end of blog posts (and is editable in NetlifyCMS, this is a workaround to the fact we can't create new template-based .md files with this CMS, so like that external contributors can create their own short bio easily). The display of short bio can be enabled on a per post basis via a boolean `show_author_bio`.
* Added `related_posts` field for blog posts.
* A new component MarkdownContent to style markdown content from arbitrary sources.
* Some slight design adjustments especially for accessibility, such as the menu icon on small screens being grey now instead of black so that is stays visible when the browser is set in night mode (black background).
* Includes a GitHub Action ([Nightly Merge Action](https://github.com/marketplace/actions/nightly-merge)) to merge all commits from master to the preview branch on every push. The preview branch is solely used by Stackbit to preview and commit its temporary changes, whereas openly authored content through netlifyCMS is pushed on the master branch, so this ensures that Stackbit always include changes done through netlifyCMS.

## Missing features

* The SectionDocs component could not be included due to some weird errors. The component is still there but it's not working and won't show up in the Stackbit Studio's visual editor.

* In the Docs section, only one level of nesting will be appropriately displayed, any deeper levels will be displayed with indent but not below their proper parent... You can play with frontmatter.weight meanwhile to give the illusion of proper nesting.

* because `gatsby-plugin-netlify-cms` uses an outdated version of NetlifyCMS without the `parent` widget type necessary for nested collections, we forcefully overwrite by copying from a custom `static/admin/index.html` in `netlify.toml` to `public/admin/index.html`. In the future, this line should be removed to use the local js files instead of outsourcing from unpkg.com .

* blog posts' categories and tags should be generated using a template instead of one md file for each that needs to be manually created. They should even be automatically generated from blog posts directly, without requiring a `data` yaml file definition.
    * For the time being, newly created tags, categories and authors through NetlifyCMS have no link to a dedicated listing/bio page, but they can be manually created by raw files editing or via the Stackbit Studio's visual editor.

* MDX ((Markdown + JSX)[https://mdxjs.com]) is not supported, although it should be possible [to migrate](https://www.gatsbyjs.com/docs/how-to/routing/migrate-remark-to-mdx/) (see also [here](https://www.aboutmonica.com/blog/thoughts-on-migrating-from-markdown-to-mdx)). However, it's possible to integrate raw HTML code inside Markdown content, it will be automatically parsed by `utils/htmlToReact.js`.

## License

All rights belong to Stackbit, this theme should be considered as under the same license as their DIY and Libris themes.

## Colophon

Generated at `2020-12-16T02:09:30.011Z` by Stackbit version `0.3.40`.
