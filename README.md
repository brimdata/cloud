# Brim Cloud

Welcome to Brim Cloud!

We've spent the past few years building the open-source
[Zed project](https://github.com/brimdata/zed)
and are now building a cloud service to provide an easy-to-use
[Zed lake](https://github.com/brimdata/zed/tree/main/docs/lake).

Let us know if you would like to be a pre-alpha tester, free of charge.
You will be helping us build out our cloud product and your participation
will be greatly appreciated.
To sign up for the pre-alpha test, please email us at cloud-alpha@brimdata.io
and we'll get the conversation going.

## Signing Up

When you sign up, you will provide us your preferred email
and we will enable your access via [auth0](https://auth0.com/).
You will get a standard account registration email from auth0
to establish your account.

We will provide a custom DNS name pointing to a dedicated EC2 instance
to sign into the service via your auth0 credentials from one of
our clients.

Your Zed data will be stored in one of our S3 buckets, which we can make
available to you for direct migration or backup.  You can copy all of the lake data
to your own domain (e.g., your own S3 bucket or local filesystem) and run your
own instance of `zed serve` to precisely recreate the lake from
the Brim Cloud.

## Using the Zed Lake

You can access your Zed lake in the Brim cloud using
* from your desktop using [the Brim app](#the-brim-app)
* from the command-line using [`zed` CLI command](#the-cli)
* from your Python using our [Python client](#the-python-client)

For the configuration options below, we will assume the host name we provided
you at signup is `<custom>.lake.brimdata.io`.  You should replace
`<custom>` with the DNS name we assigned you.

When you first connect, there will be a test pool set up for you called "Welcome"
with some sample Zed data.

### The Brim App

The Brim App can communicate with and switch between different instances
of Zed lakes, including the local Zed lake it automatically launches
on `localhost`.  Lake connections are a bit like Slack teams, where Slack
lets you easily switch between teams.

To connect to the cloud from the Brim app, click on the lake dropdown
in the upper left area of the window where you see the down carrot
and click on _Add Lake..._.  Once the cloud connection has been created,
you can use this dropdown to switch between your local lake and the cloud
(as well as other cloud lakes or lakes that you set up on your own server
with `zed serve`).

Under the _Name_ input, provide a name for the lake connection that you would like to
appear in listing of available lakes.  Under the Host input, provide the URL
of the lake service, in this case, `https://<custom>.lake.brimdata.io`

You can import some sample queries relevant to the Welcome pools
into the app's query library by
downloading [queries.json](./queries.json?raw=1).
You can simply drag this file onto the **Queries** panel in the Brim app
and this will add a query folder called "Welcome".
You may also import this file by clicking on the `+` button
in the **Queries** panel and selecting _Import from JSON_.

### The CLI

The `zed` command lets you access all of the functionality exposed to the Brim
App on the command line. Type `zed -h` to get online help.

The quickest way to get the `zed` command on macOS, Linux, or Windows is to [download
a pre-built release](https://github.com/brimdata/zed/releases).

To use the Brim cloud with `zed`, you can use the `-lake` command-line option
but for all the examples here, we will use presume you set the
following environment variable to point at your cloud instance:
```
export ZED_LAKE=https://<custom>.lake.brimdata.io
```
> Note that the service is available on the default SSL port rather than
> the `zed serve` default port of 9867.

Authenticate your lake connection using `zed auth login`:
```
zed auth login
```
This will launch your browser with an authentication dialogue pointing
at the auth0 service for Brim and command will print a confirmation code
on the terminal.  Once you confirm in the auth0 web dialogue, `zed auth login`
will report success and you will be logged in.

Note that `zed auth login` will store your access credentials in `$HOME/.zed/credentials.json`.

Once authenticated, check that the service is working
by running a `query` command on your "welcome" data:
```
zed query "from Welcome"
```
> Note that you can delete the `Welcome` pool at any time.
> It is just here to help you get started.  You can always
> reproduce it by creating the pool and copying
> [the data](welcome.zson) from this repo and optionally dragging the
> [Welcome queries](queries.json) into your app's query library.

### The Python Client

To access your lake via the Python client, first authenticate as described
above using `zed auth login` as the Python client requires access to your login
credentials stored in `$HOME/.zed/credentials.json`.

If you haven't installed the Python `zed` module, run
```
pip3 install "git+https://github.com/brimdata/zed#subdirectory=python/zed"
```
Run a test by creating a pool, loading data from Python, and reading it back:
```
import zed
import os
c = zed.Client(os.environ.get('ZED_LAKE'))
c.create_pool('test')
c.load('test', '{a:1}{a:2}{a:3}')
for v in c.query('from test'):
  print(v)
```

## Performance Caveats

If you do many small loads, the queries will run slow because the current
lake implementation does not yet implement the planned design.  See
[issue 2936](https://github.com/brimdata/zed/issues/2936).

Compaction is coming soon.

If you pull very large result sets down into the Python client, the network
transfer using ZJSON will run much slower than ZNG.  (We plan to add native
ZNG support to the Python client but it's not yet implemented.)  If you want
to retrieve large data sets from your Zed lake it's best to use `zed query`,
which defaults to the ZNG format (unless you send the output to a terminal,
in which case the textual ZSON format is default).

## Support

If you run into problems or have questions, please reach out to us
on our public [Slack channel](https://www.brimdata.io/join-slack).
