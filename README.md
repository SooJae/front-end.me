[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/JaeYeopHan/gatsby-starter-bee.git)

:bulb: if you want to deploy github pages, add following script to package.json

```json
"scripts": {
    "deploy": "gatsby build && gh-pages -d public -b master -r 'git@github.com:${your github id}/${github page name}.github.io.git'"
}
```

## 🧐 Customize

### ⚙ Gatsby config

```sh
/root
├── gatsby-browser.js // font, polyfill, onClientRender ...
├── gatsby-config.js // Gatsby config
├── gatsby-meta-config.js // Template meta config
└── gatsby-node.js // Gatsby Node config
```

### ⛑ Structure

```sh
src
├── components // Just component with styling
├── layout // home, post layout
├── pages // routing except post: /(home), /about
├── styles
│   ├── code.scss
│   ├── dark-theme.scss
│   ├── light-theme.scss
│   └── variables.scss
└── templates
    ├── blog-post.js
    └── home.js
```

### 🎨 Style

You can customize color in `src/styles` directory.

```sh
src/styles
├── code.scss
├── dark-theme.scss
├── light-theme.scss
└── variables.scss
```

### 🍭 Tips (You can change...)

- Profile image! (replace file in `/content/assets/profile.png`)
- Favicon image! (replace file in `/content/assets/felog.png`)
- Header gradient! (\$theme-gradient `/styles/variables.scss`)
- Utterances repository! (replace repository address in `/gatsby-meta-config.js`)
  - ⚠️ Please check, this guide(https://utteranc.es/)

## ☕ Like it?

<a href="https://www.buymeacoffee.com/jbee" target="_blank">
  <img src="https://www.buymeacoffee.com/assets/img/custom_images/purple_img.png" alt="Buy Me A Coffee" style="height: auto !important;width: auto !important;" >
</a>

## 🤔 If...

If you are currently writing in the Medium, consider migration with [medium-to-own-blog](https://github.com/mathieudutour/medium-to-own-blog)!

## :bug: Bug reporting

[Issue](https://github.com/JaeYeopHan/gatsby-starter-bee/issues)

## 🎁 Contributing

[Contributing guide](./CONTRIBUTING.md)

## Contributors

### Code Contributors

This project exists thanks to all the people who contribute. [[Contribute](CONTRIBUTING.md)].

<a href="https://github.com/JaeYeopHan/gatsby-starter-bee/graphs/contributors">
<img src="https://opencollective.com/gatsby-starter-bee/contributors.svg?width=890&button=false" />
</a>

### Financial Contributors

Become a financial contributor and help us sustain our community. [[Contribute](https://opencollective.com/gatsby-starter-bee/contribute)]

#### Individuals

<a href="https://opencollective.com/gatsby-starter-bee"><img src="https://opencollective.com/gatsby-starter-bee/individuals.svg?width=890"></a>

#### Organizations

Support this project with your organization. Your logo will show up here with a link to your website. [[Contribute](https://opencollective.com/gatsby-starter-bee/contribute)]

<a href="https://opencollective.com/gatsby-starter-bee/organization/0/website"><img src="https://opencollective.com/gatsby-starter-bee/organization/0/avatar.svg"></a>
<a href="https://opencollective.com/gatsby-starter-bee/organization/1/website"><img src="https://opencollective.com/gatsby-starter-bee/organization/1/avatar.svg"></a>
<a href="https://opencollective.com/gatsby-starter-bee/organization/2/website"><img src="https://opencollective.com/gatsby-starter-bee/organization/2/avatar.svg"></a>
<a href="https://opencollective.com/gatsby-starter-bee/organization/3/website"><img src="https://opencollective.com/gatsby-starter-bee/organization/3/avatar.svg"></a>
<a href="https://opencollective.com/gatsby-starter-bee/organization/4/website"><img src="https://opencollective.com/gatsby-starter-bee/organization/4/avatar.svg"></a>
<a href="https://opencollective.com/gatsby-starter-bee/organization/5/website"><img src="https://opencollective.com/gatsby-starter-bee/organization/5/avatar.svg"></a>
<a href="https://opencollective.com/gatsby-starter-bee/organization/6/website"><img src="https://opencollective.com/gatsby-starter-bee/organization/6/avatar.svg"></a>
<a href="https://opencollective.com/gatsby-starter-bee/organization/7/website"><img src="https://opencollective.com/gatsby-starter-bee/organization/7/avatar.svg"></a>
<a href="https://opencollective.com/gatsby-starter-bee/organization/8/website"><img src="https://opencollective.com/gatsby-starter-bee/organization/8/avatar.svg"></a>
<a href="https://opencollective.com/gatsby-starter-bee/organization/9/website"><img src="https://opencollective.com/gatsby-starter-bee/organization/9/avatar.svg"></a>

## LICENSE

[MIT](./LICENSE)

<div align="center">

<sub><sup>Project by <a href="https://github.com/JaeYeopHan">@Jbee</a></sup></sub><small>✌</small>

</div>
