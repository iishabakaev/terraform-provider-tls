---
page_title: "tls_certificate Data Source - terraform-provider-tls"
subcategory: ""
description: |-
  Get information about the TLS certificates securing a host.
  Use this data source to get information, such as SHA1 fingerprint or serial number, about the TLS certificates that protects a URL.
---


<!-- Please do not edit this file, it is generated. -->
# tls_certificate (Data Source)

Get information about the TLS certificates securing a host.

Use this data source to get information, such as SHA1 fingerprint or serial number, about the TLS certificates that protects a URL.

## Example Usage

### URL Usage
```python
# DO NOT EDIT. Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
from constructs import Construct
from cdktf import Fn, Token, TerraformStack
#
# Provider bindings are generated by running `cdktf get`.
# See https://cdk.tf/provider-generation for more details.
#
from imports.aws.eks_cluster import EksCluster
from imports.aws.iam_openid_connect_provider import IamOpenidConnectProvider
from imports.tls.data_tls_certificate import DataTlsCertificate
class MyConvertedCode(TerraformStack):
    def __init__(self, scope, name, *, roleArn, vpcConfig):
        super().__init__(scope, name)
        example = EksCluster(self, "example",
            name="example",
            role_arn=role_arn,
            vpc_config=vpc_config
        )
        data_tls_certificate_example = DataTlsCertificate(self, "example_1",
            url=Token.as_string(
                Fn.lookup_nested(example.identity, ["0", "oidc", "0", "issuer"]))
        )
        # This allows the Terraform resource name to match the original name. You can remove the call if you don't need them to match.
        data_tls_certificate_example.override_logical_id("example")
        aws_iam_openid_connect_provider_example = IamOpenidConnectProvider(self, "example_2",
            client_id_list=["sts.amazonaws.com"],
            thumbprint_list=[
                Token.as_string(
                    Fn.lookup_nested(data_tls_certificate_example.certificates, ["0", "sha1_fingerprint"
                    ]))
            ],
            url=Token.as_string(
                Fn.lookup_nested(example.identity, ["0", "oidc", "0", "issuer"]))
        )
        # This allows the Terraform resource name to match the original name. You can remove the call if you don't need them to match.
        aws_iam_openid_connect_provider_example.override_logical_id("example")
```

### Content Usage
```python
# DO NOT EDIT. Code generated by 'cdktf convert' - Please report bugs at https://cdk.tf/bug
from constructs import Construct
from cdktf import Fn, Token, TerraformStack
#
# Provider bindings are generated by running `cdktf get`.
# See https://cdk.tf/provider-generation for more details.
#
from imports.tls.data_tls_certificate import DataTlsCertificate
class MyConvertedCode(TerraformStack):
    def __init__(self, scope, name):
        super().__init__(scope, name)
        DataTlsCertificate(self, "example_content",
            content=Token.as_string(Fn.file("example.pem"))
        )
```

<!-- 
    Schema ORIGINALLY generated by tfplugindocs,
    then manually tweaked to circumvent current limitations.

    This should be revisited, once https://github.com/hashicorp/terraform-plugin-docs/issues/66 is resolved.
-->
## Schema

### Optional

- `url` (String) The URL of the website to get the certificates from. Cannot be used with `content`.
- `content` (String) The content of the certificate in [PEM (RFC 1421)](https://datatracker.ietf.org/doc/html/rfc1421) format. Cannot be used with `url`.
- `verify_chain` (Boolean) Whether to verify the certificate chain while parsing it or not (default: `true`). Cannot be used with `content`.

### Read-Only

- `id` (String) Unique identifier of this data source: hashing of the certificates in the chain.
- `certificates` (List of Object) The certificates protecting the site, with the root of the chain first. (see [below for nested schema](#nestedatt--certificates))

<a id="nestedatt--certificates"></a>
### Nested Schema for `certificates`

Read-Only:

- `is_ca` (Boolean) `true` if the certificate is of a CA (Certificate Authority).
- `issuer` (String) Who verified and signed the certificate, roughly following [RFC2253](https://tools.ietf.org/html/rfc2253).
- `not_after` (String) The time until which the certificate is invalid, as an [RFC3339](https://tools.ietf.org/html/rfc3339) timestamp.
- `not_before` (String) The time after which the certificate is valid, as an [RFC3339](https://tools.ietf.org/html/rfc3339) timestamp.
- `public_key_algorithm` (String) The key algorithm used to create the certificate.
- `serial_number` (String) Number that uniquely identifies the certificate with the CA's system.
  The `format` function can be used to convert this _base 10_ number into other bases, such as hex.
- `sha1_fingerprint` (String) The SHA1 fingerprint of the public key of the certificate.
- `signature_algorithm` (String) The algorithm used to sign the certificate.
- `subject` (String) The entity the certificate belongs to, roughly following [RFC2253](https://tools.ietf.org/html/rfc2253).
- `version` (Number) The version the certificate is in.
- `cert_pem` (String) Certificate data in [PEM (RFC 1421)](https://datatracker.ietf.org/doc/html/rfc1421) format. **NOTE**: the [underlying](https://pkg.go.dev/encoding/pem#Encode) [libraries](https://pkg.go.dev/golang.org/x/crypto/ssh#MarshalAuthorizedKey) that generate this value append a `\n` at the end of the PEM. In case this disrupts your use case, we recommend using [`trimspace()`](https://www.terraform.io/language/functions/trimspace).

<!-- cache-key: cdktf-0.18.0 input-aa0448a429be224544a948790292f3b630d2ecf0739247bd90334feadefa8419 556251879b8ed0dc4c87a76b568667e0ab5e2c46efdd14a05c556daf05678783-->