# dnsimple_cli

A cli for the DNSimple v2 API. This is not a complete API implementation. It should be great to use as a simple dynamic dns client.

## Install

There is no install, it's just `bash`! Well, that's not true.

### Dependencies

1. [curl](https://curl.haxx.se/)
2. [jq](https://stedolan.github.io/jq/)
3. dig
    * Part of OS X
    * On Linux it's usuall the `dnsutils` package
4. bash version >= 4
    * For an old version of OS X, use [Homebrew](http://brew.sh/)
    * For an old version of Linux, use [Linux Brew](http://linuxbrew.sh/)
    * If you're really stuck with bash v3.something, let me know and I'll take all the love and joy out of the script for you. ;)

## Setup

1. Login to [DNSimple](https://dnsimple.com)
2. Go to your *Account* page (_not_ your Settings)
3. Go to *Access Tokens*
4. Create an account access token.
5. Add that to your `.bashrc`, `.zshenv`, etc...
    * `export DNSIMPLE_ACCOUNT_TOKEN="The token from DNSimple"`
6. Reload your shell: `exec bash`, `exec zsh`, etc...

## Usage

### Help

```
$ ./dnsimple -h
Usage: dnsimple <command> [options] [command specific options]
  OPTIONS:
  -h, --help            This.

  --dat,                Specify a $DNSIMPLE_ACCOUNT_TOKEN, in lieu of or to
  --account-token       override the setting from your environment.
                        You can create 'User access token' on your dnsimple.com
                        User Settings page.

  --dai,                Specify a $DNSIMPLE_ACCOUNT_ID to override the
  --account-id          wildcard of '_' or to override the setting from your
                        environment. Specifying "auto" will use the API to
                        figure out what the account id really is. Some
                        commands do not support the wildcard.

  --api                 Override the default v2 API URL. The default is:
                        https://api.dnsimple.com/v2

  COMMANDS:
    whoami              Prints info for your $DNSIMPLE_ACCOUNT_TOKEN.

    zones               Prints available zones, requires a real
                        $DNSIMPLE_ACCOUNT_ID.

    zone_info           Prints info for a specific zone.
      -z, --zone <zone name>

    zone_records        Prints records for a specific zone.
      -z, --zone <zone name>

    zone_record         Prints info for a specific zone record.
      -z, --zone <zone name>
      -r, --record <record name>

    zone_record_id      Prints the id for a specific zone record.
      -z, --zone <zone name>
      -r, --record <record name>

    update_a_record     Creates/Updates the zone record with an IP address.
                        Specifying an IP address of "auto" will detect and use
                        the current public internet IP.

      -z, --zone <zone name>
      -r, --record <record name>
      -i, --ip <IP or "auto">

  You need to have 'dig', 'curl', and 'jq' installed and in your $PATH.
  https://curl.haxx.se/
  https://stedolan.github.io/jq/
```


### `whoami`

```
$ ./dnsimple whoami
{
  "data": {
    "user": null,
    "account": {
      "id": ***Your Account ID***,
      "email": "***Your Email Address***",
      "created_at": "2010-11-16T14:19:28.219Z",
      "updated_at": "2016-02-22T11:04:09.404Z"
    }
  }
}
```

### `zones`

```
$ ./dnsimple zones --account-id auto

# Pretty JSON of your available zones.
```

### `update_a_record`

Read that as "update an A record" which is really my goal use case, as a dynamic DNS update client.

```
$ ./dnsimple update_a_record --zone chorn.net --record derp3 --ip auto
{
  "data": {
    "id": 5544144,
    "zone_id": "chorn.net",
    "parent_id": null,
    "name": "derp3",
    "content": "173.84.105.46",
    "ttl": 3600,
    "priority": null,
    "type": "A",
    "system_record": false,
    "created_at": "2016-04-05T11:43:39.691Z",
    "updated_at": "2016-04-12T10:50:59.576Z"
  }
}
```


## Todo

1. Pagination
2. Update other record types.

