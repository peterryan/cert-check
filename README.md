# cert-check
A simple script that checks if the SSL/TLS certificate for a given domain(s)
will expire within an amount of time.

## Installation
`cert-check` is a single Bash script and can be installed by copying it to a
location in your `$PATH`. For example, I copy it to:

    /usr/local/bin

## Usage
`cert-check` _can_ be run interactively:

    $ cert-check example.com

Multiple domains can be checked:

    $ cert-check one.example.com two.example.com

By default `cert-check` will test if a certificate expires within the next 40
days, but the number of days can be changed using the `-d` option. To check if
the certificate for example.com expires within the next 14 days, use:

    $ cert-check -d 14 example.com

If you supply an email address, an email will be sent when the expiry-status
for a given domain changes. The command below will check the certificate for
the domain `example.com` and will email `foo@example.com` if the certificate
will expire (once) or if the certificate will not expire.

    $ cert-check -e foo@example.com example.com

If the expiry-status is not known, then an email will be sent. Thus, the first
time `cert-check` is run for a given domain, it will send an email notifying
the recipient of the current status. Thereafter, emails are only sent when the
status changes.

NOTE: If an email address is supplied, `cert-check` be be quiet and will
therefore work well with `cron`. When an email address is not supplied, it is
assumed the command is being run interactively and it is then slightly more
verbose.

Verbosity can be increased by adding the `-v` or `-vv` option (the latter being
for debugging purposes).

Add `cert-check` to `cron` using:

    $ crontab -e

Then add the following line (change email-address and domains as required):

    0 1 * * * /usr/local/bin/cert-check -e foo@example.com example.com

