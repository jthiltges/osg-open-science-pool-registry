# registration

## Development

To run the registration server locally, run

```shell script
$ FLASK_DEBUG=true python -m registry
```

You will need a `config.py` file with settings in it, as described below.

## Installation

Clone the repository to wherever you would like to serve the application from
(e.g., `/var/www/registration`).

Template Apache configuration:
```
# TODO
```


## Configuration

Configuration options will be read out of a file named `config.py`, placed at the
root of the repository, next to this `README.md`. The file should contain
global variables with names matching the configuration options described below,
like
```python
USER_ID_ENV_VAR = "REMOTE_USER"
```

### Required

These configuration options **must** be set.
They do not have defaults.

* `SERVER_NAME` - The name of the host server.
* `OIDC_REDIRECT_URI` - The URI for the OIDC redirect.
* `USER_ID_ENV_VAR` - The request environment variable that holds the user's identity.
* `HUMANS_FILE` - The path to the file that contains information on humans.
* `ADMIN_EMAILS` - The email addresses that will receive mail when users sign up, like `ADMIN_EMAILS = "Foo Bar <foobar@university.edu>, Wiz Bang <wizbang@organization.org>"`.

### Optional

* `CONDOR_TOKEN_REQUEST_LIST` - The path to the `condor_token_request_list` executable. By default, discover it on `$PATH`.
* `CONDOR_TOKEN_REQUEST_APPROVE` - The path to the `condor_token_request_approve` executable. By default, discover it on `$PATH`.


## Humans Data Format

This application reads information on "humans" from an INI file.
This file describes each HT Phenotyping user, and in the context of the
registration flow implemented by this application, declares which source names
they are allowed to administrate.
Here is an example of the format:

```ini
# humans.ini

[User Foo Bar]
Name = foobar@university.edu
ContactName = Foo Bar
Email = foobar@company.com
Sources = University_Bar
  SomewhereElse_Bar

[User Another]
...
```

The field used by this application are described below:

* `Name` - The user's identity; whatever comes from OIDC (like their ePPN).
  These must be globally unique (though we have no way to enforce that).
* `Sources`- The sources the user is allowed to register (and therefore "owns").
  A user may have multiple sources (separated by newlines, as above), and the
  same source may be "owned" by multiple users. Source names must be globally
  unique.
