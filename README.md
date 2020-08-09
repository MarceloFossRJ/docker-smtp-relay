# [Docker SMTP relay container](https://hub.docker.com/r/marcelofossrj/smtp_relay)

SMTP relay with local queue based on the [Tecnativa postfix relay](https://hub.docker.com/r/tecnativa/postfix-relay).

The container retains an email queue in a volume under `/var/spool/postfix`, so
in case the network fails or your real SMTP server is down for maintenance
or whatever, the queue will be sent when network connection is restored.

This way your app can send emails faster, forget about possible temporary
network failures, and concentrate on its business.

## Configuration

Environment variables:

- `MAILNAME`: The default host for cron job mails.
- `MAIL_RELAY_HOST`: The real SMTP server (e.g. `smtp.mailgun.org`).
- `MAIL_RELAY_PORT`: The port in `MAIL_RELAY_HOST`. Depending on the port,
  a specific security configuration will be used.
- `MAIL_RELAY_USER`: The user to authenticate in `MAIL_RELAY_HOST`.
- `MAIL_RELAY_PASS`: The password to authenticate in `MAIL_RELAY_HOST`.
- `MAIL_CANONICAL_DOMAINS`: A space-separated list of domains that are
  considered [canonical][].
- `MAIL_NON_CANONICAL_DEFAULT`: A domain that should be found in the list of
  `MAIL_CANONICAL_DOMAINS`, which will be used as the replacement domain when
  a non-[canonical][] message comes in. Leave it empty to skip that
  replacement system.
- `MAIL_CANONICAL_PREFIX`: Defaults to `noreply+`, and it is what will be
  prefixed to replaced non-[canonical][] sender addresses.
- `MESSAGE_SIZE_LIMIT` in bytes, defaults to 50MiB. Most generous servers offer
  a limit of 25iMB (Gmail, Mailgun...), so by defaulting to 50MiB, basically
  we are forcing the remote server to fail in case of a big email, instead of
  making the local relay to fail. Change at will if you prefer a different
  behavior.
- `ROUTE_CUSTOM` space separated list of subnets in the CIDR standard notation
  (e.g 192.168.0.0/16).
