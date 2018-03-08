## About
These are slides for a talk on building a full JavaScript GraphQL stack using
the [Apollo GraphQL](https://apollographql.com/) tools.

You can [view them online](https://metaspora.org/apollo-graphql) or locally.

The slides are written in
[GitHub Flavored Markdown](https://github.github.com/gfm/) (see `slides.md`) and
presented through [Remark.js](https://remarkjs.com/).

### Abstract
This talk provides a quick introduction to the [GraphQL](http://graphql.org/)
language itself, the pub-sub and request-response RPC architectures behind it,
as well as schema exploration and visual documentation through GraphiQL.

While enumerating some use cases, the slides suggest a possible integration with
existing services and showcase a set of tooling all based on JavaScript,
featuring static analysis through linting, both unit and integration tests, as
well as a proposal for logging and performance monitoring.

### Related Talks
* [Eric Baer - The Evolution of API Design: From RPC to GraphQL](https://www.youtube.com/watch?v=PmWho45WmQY)
* [Guillaume Diallo-Mulliez - My React (Native) app speaks GraphQL](http://slides.com/guitoof/my-react-app-speaks-graphql-with-apollo-client/fullscreen#/)

## Installation
You will need Bower, and for development with live reload, also Node.js, npm
and Gulp.

### Run the slides

To install runtime dependencies:
```bash
bower install
```

To serve from a local webserver, you need to have `gulp-cli` in your system.
Then:
```bash
npm install && gulp
```

The slides will be running on `http://localhost:8012`.

Alternatively, you can serve `index.html` from any other webserver, e.g. nginx.
Images are in `img/`, JavaScript sources in `bower_components` and `slides.js`.

## Feedback
If you have any questions or suggestions, please reach out to me, create PRs or
issues, or simply write me an email. I'll be happy to help! 🐢
