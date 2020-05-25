# Germinate

[![Crates.io](https://img.shields.io/crates/v/germinate?style=flat-square&logo=rust)](https://crates.io/crates/germinate)
[![docs.rs](https://img.shields.io/badge/docs-latest-blue?style=flat-square)](https://docs.rs/germinate)
[![GitHub Workflow Status (branch)](https://img.shields.io/github/workflow/status/itmecho/germinate/CI/master?style=flat-square&logo=github)](https://github.com/itmecho/germinate/actions?query=workflow%3ACI)
![Crates.io](https://img.shields.io/crates/d/germinate?style=flat-square)

A templating library for injecting variables from multiple external sources

## Library

### Example

This is a simple example showing how to pull values from the environment

```rust
use germinate::Seed;

#[tokio::main]
async fn main() {
    std::env::set_var("NAME", "John Wick");
    let mut seed = Seed::new("Hi %env:NAME%!".into());
    let output = seed.germinate().await.unwrap();

    assert_eq!(String::from("Hi John Wick!"), output);
}
```

## Binary
Germinate provides a CLI for templating files, available from the Github releases. To run the CLI, cimply download the binary for your system and check the usage with `germinate --help`

### Example

The CLI can be used to parse an template file and output it either to `stdout` or optionally, an output file

```
# To print the parsed result to stdout
germinate myfile.txt.tmpl

# To write the output to a file
germinate myfile.txt.tmpl -o myfile.txt
```

## Sources

Currently implemented sources:

* `env` - Load values from environment variables
* `awsssm` - Load values from the [AWS Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html)
    * This source requires the `ssm:GetParameter` AWS IAM permission
* `awsec2metadata` - Load values from the [AWS EC2 Metadata Service](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instancedata-data-retrieval.html)
* `awsec2tag` - Load values from an EC2 instance's tags. This can only access tags on the instance running `germinate`
    * This source requires the instance to have the `ec2:DescribeInstances` AWS IAM permission

### Custom sources
For an example of integrating your own value source, checkout the `Seed` struct in the [docs](https://docs.rs/germinate)

## License

[GPL-3.0](https://github.com/itmecho/germinate/blob/master/LICENSE)
