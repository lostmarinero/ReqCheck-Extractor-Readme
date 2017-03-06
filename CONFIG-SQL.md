# How To Configure ReqCheck Extractor (pulling data from MS SQL databases)

1. [The config.json File](#the-configjson-file)
2. [The Query Files](#the-query-files)
3. [The Template Files](#the-template-files)

# The config.json File

Copy `config.json.sample` to `config.json` and set the variables as described below. (or skip to [a sample config file](#sample-config-file))

- `publish_to` can be `FILE` or `LIVE`. If it's `FILE`, extracted data will be written to a local CSV file, perfect for testing the output of queries. Once queries are finalized, setting it to `LIVE` will send data to the ReqCheck web application.

- `extract_from_date` is a date in the format `month/year`, like `4/2014`. If `publish_to` is set to `FILE`, this date will be used as the start date for extraction. if `publish_to` is set to `LIVE`, this date will be ignored, and the extractor will get its start date from the ReqCheck web application.

- `reqcheck_baseurl` is the URL of the ReqCheck app, probably `https://www.projectreqcheck.org/`

- `reqcheck_username` and `reqcheck_password` are the credentials the extractor will use to authenticate with the ReqCheck app, you should receive them from the administrator.

- `reqcheck_key` is a string that'll be used to hash values that should be anonymized.

- `sql_server` is the address of the SQL server.

- `sql_server_database` is the name of the database on the server that contains the data to be extracted.

- `sql_server_username` and `sql_server_password` are the credentials the extractor will use to authenticate with the SQL database.

- `queries_dir` is the directory where `sql` files containing the database queries are kept.

- `templates_dir` is the directory where `json` files containing the data templates are kept. These templates describe how the results of the queries are processed and mapped.

- `proxy_http` and `proxy_https` are the URLs of proxy servers for http and https traffic. The `reqcheck_username` and `reqcheck_password` will be automatically added to the URLs if necessary.

- `datasets` is a list of objects that each describe a dataset to be extracted, transformed, and loaded. Each object has:

    - `name` a short, unique name that describes the data set, like "complaints"

    - `query_filename` the name of the `sql` file containing a database query, kept in the `queries_dir`. See [The Query Files](#the-query-files) for an example of what the query files should look like.

    - `template_filename` the name of the `json` file containing a dataset template, kept in the `templates_dir`. See [The Template Files](#the-template-files) for an example of what the template files should look like.

    - `endpoint` the endpoint on the ReqCheck site that the data will be sent to, like "data/complaints"

## Sample Config File

### REDACTED

# The Query Files

Each dataset requires a **query** file and a [template](#the-template-files) file. The locations and names of these files are defined in [config.json](#the-configjson-file). The query file contains a SQL query that is used to extract data from the database.

### REDACTED

Note the `?` at the end of the query. When the query is run, that `?` will be replaced with the extraction start date.

# The Template Files

Each dataset requires a [query](#the-query-files) file and a **template** file. The locations and names of these files are defined in [config.json](#the-configjson-file). The template file contains a JSON object that is used to transform data extracted from the database by the corresponding query. Here's a very simple example that transforms data extracted by the [query above](#the-query-files):

### REDACTED


The accepted values for `process_as` are:

- `date` - this value will be treated as a date
- `boolean` - this value will be treated as a boolean value
- `anonymous`- this value will be anonymized (used for id numbers)
- `string` - this value will be treated as a string value. Numerical values should also be processed as strings. If a value isn't a `date`, `boolean`, or `anonymous` value, it should be a `string`.

If you have a value that you want to anonymize *within the context of a single incident*, you can also use the `process_with` parameter as shown above. Using the unique incident ID (`opaqueId`) to anonymize the unique resident ID (`residentId`) will keep a single resident from tracked across incidents.

The template defines how an incident will look when it's written to disk, or posted to a web service. A CSV file generated using the above query and template would look like this:

### REDACTED
