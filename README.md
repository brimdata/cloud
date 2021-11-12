# Brim Cloud

Welcome to Brim Cloud!

We've spent the past few years building the open-source
[Zed project](https://github.com/brimdata/zed)
and are now building a cloud service to provide an easy-to-use
[Zed lake](https://github.com/brimdata/zed/tree/main/docs/lake).

Let us know if you would like to be a pre-alpha tester, free of charge.
You will have the noble reputation of helping us build out our cloud product.
If so, please email us at cloud@brimdata.io and we'll get the conversation going.

## Signing Up

When you sign up, you will provide us your prefered email
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
* from your desktop using the [Brim app](https://github.com/brimdata/brim),
* from the command-line using [zapi](https://github.com/brimdata/zed/blob/main/cmd/zed/api/command.go), or
* from your Python using our
[Python client](https://github.com/brimdata/zed/tree/main/python/zed).

For the configuration options below, we will assume the host name we provided
you at signup is `acme.brimdata.io`.  Please substitute your actual host name.

We will have a test pool set up for you called "Welcome" with some dummy
data in it.

### The Brim App

To connect to the cloud from the Brim app, click on the

XXX doc "Create Workspace..."

### The CLI

You can access all of the functionality exposed to the Brim App also through
the `zed api` command, or `zapi` for short.  Type `zapi -h` to get online help.

You can use the `-host` argument to point to your cloud instance or set
an environment variable:
```
export ZED_LAKE_HOST=https://acme.brimdata.io
```
Note that the service is available on the default SSL port rather than
the `zed lake serve` default port of 9867.

Authenticate your lake connection using `zed auth`:
```
zed auth login
```
Note that `zed auth` will store your secret credentials in `$HOME/.zed/credentials.json`.

```
zapi -host acme.brimdata.io query "from Welcome"
```

Once authenticated, check that the service is working
by running a `query` command on your "welcome" data:
```
zapi -host acme.brimdata.io query "from Welcome"
```
> Note that you can delete the `Welcome` pool at any time.
> It is just here to help out new users and you can always
> reproduce it by creating the pool and copying
> [the data](welcome.zson) from this repo and optinally dragging the
> [Welcome queries](queries.json) into your app's query library.

> TBD: we should have a `zapi status` command to test the connection.

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
c = zed.Client('https://acme.brimdata.io')
c.create_pool('test')
c.load('test', '{a:1}{a:2}{a:3}')
for v in c.query('from test'):
  print(v)
```

## Support

If you run into problems or have questions, please reach out to us
on our public [Slack channel](https://www.brimdata.io/join-slack).
