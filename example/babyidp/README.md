# babyidp

A miniscule IDP server that can actually respond to SAML requests.  Based on the existing [`../idp/idp.go`](../idp/idp.go) example, but extended to be useable right away as an IDP server.

- idp key and idp cert and configurable via environment variables (`IDP_KEY=key.pem` and `IDP_CERT=cert.pem`)
- users configurable via [`users.json`](./users.json)
- SAML client application configurable using `-service http://url.to/saml/metadata.xml`

## Usage example (with Grist)

- start [Grist](https://github.com/gristlabs/grist-core) locally:

	```bash
	$ docker run -it --rm --env DEBUG=1 -p 8484:8484 \
		--env GRIST_SAML_SP_HOST=http://localhost:8484 \
		--env GRIST_SAML_IDP_LOGIN=http://localhost:8000/sso \
		--env GRIST_SAML_IDP_LOGOUT=http://localhost:8484 \ # fake logout, babyidp does not support it
		--env GRIST_SAML_IDP_UNENCRYPTED=1 \
		--env GRIST_SAML_SP_KEY=/key.pem \
		--env GRIST_SAML_SP_CERT=/cert.pem \
		--env GRIST_SAML_IDP_CERTS=/cert.pem \
		-v $PWD/cert.pem:/cert.pem -v $PWD/key.pem:/key.pem gristlabs/grist
	```
- run the idp server `./babyidp -idp http://localhost:8000 -service http://localhost:8484/saml/metadata.xml`

And then login with `alice@example.com` and password `hunter2` at <http://localhost:8484> in Grist.
