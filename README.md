# Rust na AWS

Para saber mais sobre Rust na AWS, acesse o [AWS Developer Center](https://aws.amazon.com/developer/language/rust/).

## AWS Rust SDK

- [AWS Rust SDK](https://aws.amazon.com/sdk-for-rust/)
- [Developer Guide](https://docs.aws.amazon.com/sdk-for-rust/latest/dg/welcome.html)
- [GitHub repo](https://github.com/awslabs/aws-sdk-rust)
- [Exemplos de código](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview)
- [Roadmap](https://github.com/orgs/awslabs/projects/50/views/1)

## AWS Lambda Rust Runtime

1. Install [cargo-lambda](https://github.com/cargo-lambda/cargo-lambda)

```shell
brew tap cargo-lambda/cargo-lambda
brew install cargo-lambda
```

2. Create a function

```shell
cargo lambda new FUNCTION_NAME
```

3. Build

```shell
cargo lambda build --release
cargo lambda build --release --arm64
```

4. Deploy

```shell
cargo lambda deploy \
  --iam-role arn:aws:iam::XXXXXXXXXXXXX:role/your_lambda_execution_role
```

5. Test

```shell
cargo lambda invoke --remote \
  --data-ascii '{"command": "hi"}' \
  --output-format json \
  FUNCTION_NAME
```

- [Rust Runtime for AWS Lambda](https://github.com/awslabs/aws-lambda-rust-runtime)
- [cargo-lambda](https://github.com/cargo-lambda/cargo-lambda)

## Links

- [Rust on AWS](https://aws.amazon.com/developer/language/rust/)

### Vídeos

- [AWS re:Invent 2021 - Building with the new AWS SDKs for Rust, Kotlin, and Swift](https://youtu.be/Nhk1K1AjYvg)

### Blogs

- [Innovating with Rust](https://aws.amazon.com/blogs/opensource/innovating-with-rust/)
- [A New AWS SDK for Rust – Alpha Launch](https://aws.amazon.com/blogs/developer/a-new-aws-sdk-for-rust-alpha-launch/)
- [Why AWS loves Rust, and how we’d like to help](https://aws.amazon.com/blogs/opensource/why-aws-loves-rust-and-how-wed-like-to-help/)
- [Rust Runtime for AWS Lambda](https://aws.amazon.com/blogs/opensource/rust-runtime-for-aws-lambda/)
