# Bug reproduction

This reproduction is based on Next i18n-routing official example.
A redirection is added in getStaticProps to a different locale, which creates a bug.

It seems that the inter-locale redirections are not supported in the getStaticProps/getServerSideProps: the same api for redirects than in next.config.js with `locale: false` would be needed.

## Reproduction steps

- `yarn install`
- `yarn build`
- `yarn start`

### getStaticProps

- Go to http://localhost:3000/fr/gsp/first
- You should be successfully redirected to http://localhost:3000/nl/gsp/first
- Click the `fr` link
- Check the error message in the console:

  ```
  Uncaught (in promise) Error: The provided `as` value (/fr/nl/gsp/first) is incompatible with the `href` value (/gsp/[slug])
  ```

- Go to http://localhost:3000/fr
- Click the `To dynamic getStaticProps page` link
- Same error as above

### getServerSideProps

- Go to http://localhost:9000/fr/gssp
- You should be successfully redirected to http://localhost:3000/nl/gssp
- Click the `fr` link
- You should be redirected to http://localhost:9000/fr/nl/gssp (note the 2 locales)
- Go to http://localhost:3000/fr
- Click the `To getServerSideProps page` link
- Same errored redirection as last one
