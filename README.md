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
* from your Python using our [Python client].

> TBD: we need a docs page for zed/zq/zapi etc.

For the configuration options below, we will assume the host name we provided
you at signup is `acme.brimdata.io`.  Please substitute your actual host name.

We will have a test pool set up for you called "Welcome" with some dummy
data in it.

### The Brim App

XXX doc "Create Workspace..."

### The CLI

You can access all of the functionality exposed to the Brim App also through
the `zed api` command, or `zapi` for short.  Type `zapi -h` to get online help.

You can use the `-host` argument to point to your cloud instance or set
an environment variable:
```
export ZED_LAKE_HOST=acme.brimdata.io
```

To check that the service is working, you can run the `query` command on the
`Welcome` pool:
```
zapi -host acme.brimdata.io query "from Welcome"
```

> TBD: we should have a `zapi status` command to test the connection.

### The Python Client

XXX noah

## Support

If you run into problems or have questions, the easiest way to reach us
is on our public [Slack channel](https://www.brimdata.io/join-slack).
