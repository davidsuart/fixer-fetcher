# fixer-fetcher

A proof-of-concept to fetch currency (EUR) base rates from the fixer.io API and store/display from a local database

## Usage

1. Provide a fixer.io API key within `[server.js]` :: `###STRINGREPLACEAPIKEY###`
2. Swap out the database placeholder strings with your secrets/credentials within `[mysql.js]`:
   * database host/username/password/dbname :: `'###STRINGREPLACESQL[HOST/USER/PASS/DATA]###'`
3. Run the app: `$ node server.js`
4. View the data: `http://<your-nodejs-instance>:8000/`
5. Profit!

## How it works

- On startup, the local database is connected and content dropped if any is present
- The local tables are created (Or re-created) as per the schema in mysql.js
- A recurring database connectivity check and fixer.io 'rategetter' are spawned
  - Every X minutes the rategetter will pull the rates from the fixer API and store in the local database
- The web server listener is launched
  - The web UI pulls the rates from the local DB, presenting a warning if their timestamps are stale

## Troubleshooting

The fixer API may pass a range of error codes back to you. These will be caught and handled in `fnGetFixerRates`;
- Error 101 is explicitly handled as this is most frequently triggered by reaching your API limit. At a 5 minute interval you are likely to hit the free API limit within a few days (1000/hits month at time of writing).
- For all other errors the API provided error code/message will be gracefully displayed in a header at the top of the resultant web page

For full error code and message details, see: https://fixer.io/documentation#errors

## Contributing

Spotted an error? Something functional to add value? Send me a pull request!

1. Fork it (<https://github.com/yourname/yourproject/fork>)
2. Create your feature branch (`git checkout -b feature/foo`)
3. Commit your changes (`git commit -am 'Add some foo'`)
4. Push to the branch (`git push origin feature/foo`)
5. Create a new Pull Request

## License

MIT license. See [LICENSE](LICENSE) for full details.
