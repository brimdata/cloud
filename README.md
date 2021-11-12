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
own instance of `zed lake serve` to precisely recreate the lake from
the Brim Cloud.

## Using the Zed Lake

You can access your Zed lake in the Brim cloud using
* from your desktop using [the Brim app](#the-brim-app)
* from the command-line using [zapi CLI command](#the-cli)
* from your Python using our [Python client](#the-python-client)

For the configuration options below, we will assume the host name we provided
you at signup is `yourlake.brimdata.io`.  You should substitute the DNS name
we provided during your actual signup for `yourlake`.

When you first connect, there will be a test pool set up for you called "Welcome"
with some sample Zed data.

### The Brim App

To connect to the cloud from the Brim app, click on the lake dropdown
in the upper left area of the window where you see the down carrot
and click on _Add Workspace..._.

> NOTE we are changing this to "Add Lake"

Under the _Name_ input, provide a name for the lake connection that you would like
appear in listing of available lakes.  Under the Host input, provide the URL
of the lake service, in this case, `https://yourlake.brimdata.io`

You can import some sample queries relevant to the Welcome pools
into the app's query library by
downloading [queries.json](./queries.json).
To import these queries into your query library, clieck on the `+` button
in the **Queries** header in the lefthand panel and select _Import from JSON_.
This will bring up a file finder and you can import the file you downloaded.

> XXX Do we support drag yet?

### The CLI

The `zed api` command (or `zapi` for short) lets you
access all of the functionality exposed to the Brim App on the command line.
Type `zapi -h` to get online help.

To use the Brim cloud with `zapi`, just use the `-host` argument
to point to your cloud instance or set an environment variable:
```
export ZED_LAKE_HOST=https://yourlake.brimdata.io
```
> Note that the service is available on the default SSL port rather than
> the `zed lake serve` default port of 9867.

Authenticate your lake connection using `zed auth`:
```
zed auth login
```
Note that `zed auth` will store your secret credentials in `$HOME/.zed/credentials.json`.

Once authenticated, check that the service is working
by running a `query` command on your "welcome" data:
```
zapi -host yourlake.brimdata.io query "from Welcome"
```
> Note that you can delete the `Welcome` pool at any time.
> It is just here to help you get started.  You can always
> reproduce it by creating the pool and copying
> [the data](welcome.zson) from this repo and optionally dragging the
> [Welcome queries](queries.json) into your app's query library.

### The Python Client

To access your lake via the Python client, first authenticate as described
above using `zapi auth` as the Python client requires access to your login
credentials stored in `$HOME/.zed/credentials.json`.

If you haven't installed the Python `zed` module, run
```
pip3 install "git+https://github.com/brimdata/zed#subdirectory=python/zed"
```
Run a test by creating a pool, loading data from Python, and reading it back:
```
import zed
c = zed.Client('https://yourlake.brimdata.io')
c.create_pool('test')
c.load('test', '{a:1}{a:2}{a:3}')
for v in c.query('from test'):
  print(v)
```

## Support

If you run into problems or have questions, please reach out to us
on our public [Slack channel](https://www.brimdata.io/join-slack).
